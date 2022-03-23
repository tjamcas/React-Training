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
    - Note 1: First, we will edit the `<button> tag associated with the `<BiTrash/>` icon to add an `onClick` event which will call a function named `onDeleteAppointment` (and which is defined in the parent `App` component).
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
    - Note 1: First, we define the `onDeleteAppointment` property/function and add it in the `<AppointmentInfo /> tag. More specifically:
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
