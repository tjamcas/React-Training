# Course 2: React.js: Building an Interface
## Class Notes / Section 4

### Section 4: Mutating Data
- useEffect and useCallback Hooks
  - The following code simulates fetching data from an external source. 
  - In the past examples, `./src/data.json` has resided in the `src` folder, but our data will more likely come fro an external API or will the results of a SQL query.
  - To simulate data received from an external source, we will move `data.jsn` to the `/public` folder and use `fetch` function to retrieve it
    - Note 1: files in the public folder will be included in an application build
    - Note 2: in our source code we can reference files that reside in the `/public` folder as if they resided in the same `/src` folder as our source code -- i.e., we will reference `/public/data.json` as `./data.json`
  - Side Effects: Data fetching, setting up a subscription, and manually changing the DOM in React components are all examples of side effects. In React class components, the render method itself shouldn’t cause side effects. It would be too early — we typically want to perform our effects after React has updated the DOM. . . . By using the `useEffect` Hook, you tell React that your component needs to do something after render. React will remember the function you passed (we’ll refer to it as our “effect”), and call it later after performing the DOM updates. The effect could be to perform data fetching or call some other imperative API. For more explanation, see: <https://reactjs.org/docs/hooks-effect.html>
  - Here is our revised `App.js` code:
    ```
    import { useState, useCallback, useEffect } from "react";
    import { BiCalendar } from "react-icons/bi";
    import Search from "./components/Search";
    import AddAppointment from "./components/AddAppointment";
    import AppointmentInfo from "./components/AppointmentInfo";

    function App() {
      let [appointmentList, setAppointmentList] = useState([]);

      const fetchData = useCallback(
        () => {
          fetch('./data.json')
            .then(response => response.json())
            .then(data => {
              setAppointmentList(data)
            })
        }, []
      )

      useEffect(() => {
        fetchData()
      }, [fetchData]);

      return (
        <div className="App container mx-auto mt-3 font-thin">
          <h1 className="text-5xl mb-3">
            <BiCalendar className="inline-block text-red-400 align-top"/>Your Appointments</h1>
          <AddAppointment />
          <Search />

          <ul className="divide-y divide-gray-200">
            {appointmentList
              .map(appointmentItem => (
                <AppointmentInfo 
                  key={appointmentItem.id}
                  appointment={appointmentItem}
                />
              ))
            }

          </ul>
        </div>
      );
    }

    export default App;
    ```
    - Note 1: we need to import `useState`, `useEffect`, and `useCallback` from the React library
    - Note 2: We are no longer going to import the `data.json` file, and so we remove the line:    
      `import appointmentList from "./data.json"`
    - Note 3: We create a state variable named  `appointmentList` and initialize it as an empty array:   
      `let [appointmentList, setAppointmentList] = useState([]);`
    - Note 4: We retrieve our data by creating a function, `fetchData()`, that uses the `useCallback` hook. After we have retrieved the data, `useCallback` will monitor any changes that happen to that data.
      ```
      const fetchData = useCallback(
        () => {
          fetch('./data.json')
            .then(response => response.json())
            .then(data => {
              setAppointmentList(data)
            })
        }, []
      )
      ```
      - The `fetch` is a promise that says our data will be retrieved the server (in this case, from our file directory). 
      - The first `.then` says that we will receive a `response` from the server and we'll use an arrow function to take that response from the server and convert it from text to JSON.
      - The second `.then` is another promise in which we create a temporary variable, `data`, where all the data is received and we pass it into the function `setAppointmentList`
      - The empty array `[]` is where we could specify variables that we want to track
        - For an example of the `fetch` API that will explain/translate the above shorthand code, go to <https://www.w3schools.com/js/js_api_fetch.asp>
        - For more information on the Fetch API, go to <https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch>
        - For more information on the Promise object and asynchronous requests, go to the web documents on the Mozilla Developer Network at: <https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise>
    - Note 5: we use `useEffect` to make sure that it is tracking the data and any changes to the data as it comes in.
      ```
      useEffect(() => {
        fetchData()
      }, [fetchData]);
      ```
      - useEffect takes two parameters. The first one is a function and second one is an array (it is also optional). In short, useEffect is invoking the function based on the second parameter. An explanation of useEffect from: <https://medium.com/swlh/react-hooks-how-to-use-useeffect-4013db2cec44>
- Deleting Records
  - We will create the functionality to delete an appointment record by clicking on the trash icon next to a record in the appointment list. The parent `App` component is managing the data, therefore, the `AppointmentInfo` component will have to communicate to the parent `App` component using the appointment id parameter.
  - Recall that our `data.json` fiile was an array of appointment objects, and the appointment objects had the form of:
    ```
    {
      "id": "3",
      "petName": "Nadalee",
      "ownerName": "Krystle Valerija",
      "aptNotes": "This dog is coming in for his monthly nail trim and grooming",
      "aptDate": "2018-11-28 16:00"
    }
    ```
  - So, we will be able to reference an appointment record with the `id` key
  - We begin by modifying the `AppointmentInfo.js` component:
    ```
    import { BiTrash } from "react-icons/bi";

    const AppointmentInfo = ({ appointment, onDeleteAppointment }) => {
        return (
            <li className="px-3 py-3 flex items-start">
                <button onClick={ () => onDeleteAppointment(appointment.id) } type="button"
                className="p-1.5 mr-1.5 mt-1 rounded text-white bg-red-500 hover:bg-yellow-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-blue-500">
                <BiTrash /></button>
                <div className="flex-grow">
                <div className="flex items-center">
                    <span className="flex-none font-medium text-2xl text-blue-500">{appointment.petName}</span>
                    <span className="flex-grow text-right">{appointment.aptDate}</span>
                </div>
                <div><b className="font-bold text-blue-500">Owner:</b> {appointment.ownerName}</div>
                <div className="leading-tight">{appointment.aptNotes}</div>
                </div>
            </li>
        )
    }
    ```
    - Note 1: First, we will edit the `<button>` tag associated with the `<BiTrash/>` icon to add an `onClick` event which will call a function named `onDeleteAppointment` (and which is defined in the parent `App` component).
    - Note 2: Second we need to modify the `AppointmentInfo` passed properties to also include the `onDeleteAppointment` function
  - Next, we modify the `App.js` parent component to define the `onDeleteAppointment` function:
    ```
    import { useState, useCallback, useEffect } from "react";
    import { BiCalendar } from "react-icons/bi";
    import Search from "./components/Search";
    import AddAppointment from "./components/AddAppointment";
    import AppointmentInfo from "./components/AppointmentInfo";

    function App() {
      let [appointmentList, setAppointmentList] = useState([]);

      const fetchData = useCallback(
        () => {
          fetch('./data.json')
            .then(response => response.json())
            .then(data => {
              setAppointmentList(data)
            })
        }, []
      )

      useEffect(() => {
        fetchData()
      }, [fetchData]);

      return (
        <div className="App container mx-auto mt-3 font-thin">
          <h1 className="text-5xl mb-3">
            <BiCalendar className="inline-block text-red-400 align-top"/>Your Appointments</h1>
          <AddAppointment />
          <Search />

          <ul className="divide-y divide-gray-200">
            {appointmentList
              .map(appointmentItem => (
                <AppointmentInfo 
                  key={appointmentItem.id}
                  appointment={appointmentItem}
                  onDeleteAppointment={
                    (appointmentId) => 
                      setAppointmentList(appointmentList.filter(appointment => 
                        appointment.id !== appointmentId))
                  }
                />
              ))
            }

          </ul>
        </div>
      );
    }

    export default App;
    ```
    - Note 1: We define the `onDeleteAppointment` property/function inside the `<AppointmentInfo />` tag. More specifically:
      ```
      {appointmentList
        .map(appointmentItem => (
          <AppointmentInfo 
            key={appointmentItem.id}
            appointment={appointmentItem}
            onDeleteAppointment={
              (appointmentId) => 
                setAppointmentList(appointmentList.filter(appointment => 
                  appointment.id !== appointmentId))
            }
          />
        ))
      }
      ```
      - When the `onClick` event occurs, the `appointment.id` is passed along with the `onDeleteAppointment` function. Therefore, we can reference `appointment.id` in a temporary variable named `appointmentId`, and use `appointmentId` when we define the `onDeleteAppointment` function using arrow format
      - `onDeleteAppointment` function calls the state setter function, `setAppointmentList`, and uses the `filter` method to remove any appointments whose `id` matches `appointmentId`
- Searching with a filtered array
  - In the next set of code, we will modify the application that activates the search input on the appointment list. 
    - The `Search.js` component will need to be modified to accept the input from the search field and send it to the `App.js` parent component. Search.js` will also need to reference the search function, `onQueryChange`, that resides in `App.js` .
    - The `App.js` component will be modified to accept the search string and to create a function, `onQueryChange`, that queries whether the search criteria can be found in one or records within in the `petName`, `ownerName` or `apptNotes` fields. We want to leave the `AppointmentList` unchaged, and so its contents will be duplicated to a second array.
  - Here is the modified code for the `Search` component inside the `Search.js` file:
    ```
    const Search = ({ query, onQueryChange }) => {
      let [toggleSort, setToggleSort] = useState(false);  
      return(
            <div className="py-5">
            <div className="mt-1 relative rounded-md shadow-sm">
              <div className="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none">
                <BiSearch />
                <label htmlFor="query" className="sr-only" />
              </div>
              <input type="text" name="query" id="query" value={query}
                onChange={(event) => { onQueryChange(event.target.value) }}
                className="pl-8 rounded-md focus:ring-indigo-500 focus:border-indigo-500 block w-full sm:text-sm border-gray-300" placeholder="Search" />
              <div className="absolute inset-y-0 right-0 flex items-center">
                <div>
                  <button type="button"
                    onClick={() => { setToggleSort(!toggleSort) }}
                    className="justify-center px-4 py-2 bg-blue-400 border-2 border-blue-400 text-sm text-white hover:bg-blue-700 focus:outline-none focus:ring-2 focus:ring-offset-2 flex items-center" id="options-menu" aria-haspopup="true" aria-expanded="true">
                    Sort By <BiCaretDown className="ml-2" />
                  </button>
                  <DropDown toggle={toggleSort} />
                </div>
              </div>
            </div>
          </div>        
        )
    }
    ```
    - Note 1: We trigger an event when someone types something into the search box. In this code snippet we use the onChange event:    
      `onChange={(event) => { onQueryChange(event.target.value) }}`
      -  An onChange event occurs when the value of an element, in this case the search input, has been changed and the element loses focus (when the user clicks or tabs to somewhere else).
      -  For more information on `onChange`, see <https://www.w3schools.com/jsref/event_onchange.asp>.
      -  For more information on events, see <https://www.w3schools.com/js/js_events.asp>.
    -  Note 2: we need to track the value of the input by assigning the `<input>`'s `value` property to a javascript variable named `query`:    
        `<input type="text" name="query" id="query" value={query} ... />`
    - Note 3: The `Search` component now has two passed parameters : 1. the changed value inside the search box and the `onQueryChange` function (that is defined in `App.js`). Therefore we have to change the `Search` component's function declaration to include these two parameters:    
  - Here is the modified code for the `App` component inside the `App.js` file:
    ```
    import { useState, useCallback, useEffect } from "react";
    import { BiCalendar } from "react-icons/bi";
    import Search from "./components/Search";
    import AddAppointment from "./components/AddAppointment";
    import AppointmentInfo from "./components/AppointmentInfo";

    function App() {
      let [appointmentList, setAppointmentList] = useState([]);
      let [queryState, setQueryState] = useState("");

      const filteredAppointments = appointmentList.filter(
        item => {
          return(
            item.petName.toLowerCase().includes(queryState.toLowerCase()) ||
            item.ownerName.toLowerCase().includes(queryState.toLowerCase()) ||
            item.aptNotes.toLowerCase().includes(queryState.toLowerCase())
          )
        }
      )

      const fetchData = useCallback(
        () => {
          fetch('./data.json')
            .then(response => response.json())
            .then(data => {
              setAppointmentList(data)
            })
        }, []
      )

      useEffect(() => {
        fetchData()
      }, [fetchData]);

      return (
        <div className="App container mx-auto mt-3 font-thin">
          <h1 className="text-5xl mb-3">
            <BiCalendar className="inline-block text-red-400 align-top"/>Your Appointments</h1>
          <AddAppointment />
          <Search query={queryState}
            onQueryChange = { myQuery => setQueryState(myQuery) } />

          <ul className="divide-y divide-gray-200">
            {filteredAppointments
              .map(appointmentItem => (
                <AppointmentInfo 
                  key={appointmentItem.id}
                  appointment={appointmentItem}
                  onDeleteAppointment={
                    (appointmentId) => 
                      setAppointmentList(appointmentList.filter(appointment => 
                        appointment.id !== appointmentId))
                  }
                />
              ))
            }

          </ul>
        </div>
      );
    }

    export default App;
    ```
    - Note 1: We need a new state variable for the search query (which is named `query` and initialized to an empty string), which we can create with the `useState` hook:    
      `let [queryState, setQueryState] = useState("");;`
    - Note 2: We change the properties for the <Search/> child component. The `query` property (our first passed parameter/property) will be set to the local `queryState` variable, and `onQueryChange` will receive `myQuery` from the event and then use the setQuery method and pass along what we received:
      ```
      <Search query={queryState}
        onQueryChange = { myQuery => setQueryState(myQuery) } />
      ```
     - Note 3: we create a copy of the original `AppointmentList` array named `filteredAppointments` because we don't want to change the original but we do want to change the copy so it only includes records that match our search (query) criteria:
      ```
      const filteredAppointments = appointmentList.filter(
        item => {
          return(
            item.petName.toLowerCase().includes(queryState.toLowerCase()) ||
            item.ownerName.toLowerCase().includes(queryState.toLowerCase()) ||
            item.aptNotes.toLowerCase().includes(queryState.toLowerCase())
          )
        }
      )
      ```
    - Note 4: Now, we want the unorder list to be base on the `filteredAppointments` array variable rather than the full `appointmentList` array variable:
      ```
      <ul className="divide-y divide-gray-200">
        {filteredAppointments
          .map(appointmentItem => 
          ...
            />
      ```
