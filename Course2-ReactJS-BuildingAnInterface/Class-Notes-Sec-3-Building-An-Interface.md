# Course 2: React.js: Building an Interface
## Class Notes / Section 3

### Section 3: Sort and Search
- Passing Data to a Component
  - In this section, we re-factor our code to break out the appointment information code  from the `App.js` component and into a separate component named `AppointmentInfo.js`
  - This will require passing information from the `App` component into the `AppointmentInfo` component
  - Recall that the `App` component receives/imports all of the scheduled appointment information through the import statement: `import appointmentList from "./data.json"`
  - `App` then processes the appointment data into an unordered list with the following code block:
    ```
    <ul className="divide-y divide-gray-200">
        {appointmentList
          .map(appointment => (
            <li className="px-3 py-3 flex items-start">
              <button type="button"
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
          ))
        }
      </ul>
      ```
  - This code block loops through each item (i.e., appointment)in the `AppointmentList` array, and inserts it into a list item.
  - Re-factoring: We will extract the `<li>` code from `App.js` and place it in a newly created component named `AppointmentInfo`.
    - This will simplify the `App.js` code and make it more readable, while placing a logically contained and functional code block into its own component.
    - However, this will require us to pass data between the parent `App` component to the child `AppointmentInfo` component
  - Creation of new `AppointmentInfo.js` component file looks like this:
    ```
    import { BiTrash } from "react-icons/bi";

    const AppointmentInfo = ({ appointment }) => {
        return (
            <li className="px-3 py-3 flex items-start">
                <button type="button"
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

    export default AppointmentInfo
    ```
    - Note 1: Data is passed from the parent component, `App`, to the `AppointmentInfo` child component though the `appointment` variable in the declaration statement : `const AppointmentInfo = ({ appointment }) => { ... }`
      - `appointment` is defined in the `App.js` file's `map` statement.
      - Recall that `appointment` is an object of the form:
        ```
        {
          "id": "1",
          "petName": "Rio",
          "ownerName": "Philip Ransu",
          "aptNotes": "Rio is up for his next round of vaccinations",
          "aptDate": "2018-11-28 10:15"
        }
        ```
    - Note 2: we reference the key-value pairs four times in the above code block to construct a single list item: `{appointment.petName}`, `{appointment.aptDate}`, `{appointment.ownerName}`, `{appointment.aptNotes}`
  - Modification of `App.js` component file looks like this:
    ```
    import { BiCalendar } from "react-icons/bi";
    import Search from "./components/Search";
    import AddAppointment from "./components/AddAppointment";
    import AppointmentInfo from "./components/AppointmentInfo";
    import appointmentList from "./data.json"

    function App() {
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
    - Note 1: The import statements are modified to reference the correct icons as well as to import/call `AppointmentInfo`
    - Note 2: We pass the `Appointment` object to the child `AppointmentInfo` component with the `map (appointmentItem => ( ... ) code block
    - Note 3: Breaking apart the following line:
      ```
      {appointmentList
        .map(appointmentItem => ( // Note 3A
          <AppointmentInfo 
            key={appointmentItem.id}  // Note 3B
            appointment={appointmentItem} // Note 3C
          />
        ))
      }
      ```
      - Note 3A: `appointmentItem` here is a variable of an iterated item/object from the `appointmentList` array
      - Note 3B: we add a key to the list item to quash warning statements
      - Note 3C: `appointment` is the named attribute - and will be passed to the child `AppointmentInfo` component - and `{appointmentItem}` is the actual iterated item/object passed through the `appointment` property
        - In other words, `appointment` is the passed `props` (or properties) parameter variable (which we saw in the "React Essentials" Eve Porcello training class) and it has been set to equal `appointmentitem`.
- The useState Hook and Conditional Classes
  - In the following code we modify `AddAppointment.js` and the `AddAppointment` componentso that we can toggle the display of the form for adding an appointment.
  - We utilize the useState hook which allows us to use the state of the component to drive its behavior - and more specifically the component/UI's appearance
  - Recall that `useState` accepts an initial state and returns two values: 1. The current state, and 2. A function that updates the state.
  - Modified code in `AddAppointment.js`:
    ```
    
    ```
    - Note 1: we import `useState`: `import { useState } from "react";`
    - Note 2: we create the state variable and set function: `let [toggleForm, setToggleForm] = useState(false);`
      - the state variable, `toggleForm` is initialized to be `false` - that is, we don't want the form to display initially unless the button is clicked
    - Note 3: the Conditional expression to toggle the input form has this structure: { toggleForm && ... //Code for form ... }
      - This is effectively a shorthand way to write an if statement
      - However, there is a division in opinion as to whether this pattern is good, readable code -- see <https://stackoverflow.com/questions/12664230/is-boolean-expression-statement-the-same-as-ifboolean-expression-stat>
      - Dissenters believe a traditional `if` statement or ternary `if` statement should be used instead
    - Note 4: we activate the "Add Appointment" button to toggle the form by adding the following code"
      ```
      <button onClick={ () => {setToggleForm(!toggleForm)} }
            ...
          </button>
      ```
    - Note 5: we format the bottom corners of the button to round depending on whether the form is showing (i.e, has been toggled on or off --  true or false)
      - We want the button to have `className = rounded-md` when false, and `className = rounded-t-md` (only top is rounded) when true (i.e, form is toggled on or showing)
      - To do this we'll set the button's `classname` to equal a JavaScript expression
        ```
        className={
              `bg-blue-400 text-white px-2 py-3 w-full text-left
              ${toggleForm ? 'rounded-t-md' : 'rounded-md' }`
            }
        ```
        - Note 5A: the JavaScript expression is inserted between the curly braces `{ ... }`
        - Note 5B: a template literal is inserted between the back ticks - \`...\`
        - Note 5C: within the template literal, we insert a JavaScript expression using `${ ... }`:
          - `${toggleForm ? 'rounded-t-md' : 'rounded-md' }`
          - For more information on `${ ... }`, see: <https://discuss.codecademy.com/t/what-does-this-syntax-do/432913> and <https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals>
- Passing State and Hiding Components
  - The following code toggles the display of the searchbar dropdown that provides the different sorting options for the returned appointments
  - This requires us to pass state information from the parent `Search` component to the child `Dropdown` component - both of which re locatedin `Search.js`
  - This code is contained in the `Search` component in `Search.js`:
    ```
    import { BiCaretDown, BiSearch, BiCheck } from "react-icons/bi"
    import { useState } from "react";

    const DropDown = ( {toggle} ) => {
      if (!toggle) {
        return null;
      }  
      return(
            <div className="origin-top-right absolute right-0 mt-2 w-56
            rounded-md shadow-lg bg-white ring-1 ring-black ring-opacity-5">
                <div className="py-1" role="menu" aria-orientation="vertical" aria-labelledby="options-menu">
                    <div
                        className="px-4 py-2 text-sm text-gray-700 hover:bg-gray-100 hover:text-gray-900 flex justify-between cursor-pointer"
                        role="menuitem">Pet Name <BiCheck /></div>
                    <div
                        className="px-4 py-2 text-sm text-gray-700 hover:bg-gray-100 hover:text-gray-900 flex justify-between cursor-pointer"
                        role="menuitem">Owner Name  <BiCheck /></div>
                    <div
                        className="px-4 py-2 text-sm text-gray-700 hover:bg-gray-100 hover:text-gray-900 flex justify-between cursor-pointer"
                        role="menuitem">Date <BiCheck /></div>
                    <div
                        className="px-4 py-2 text-sm text-gray-700 hover:bg-gray-100 hover:text-gray-900 flex justify-between cursor-pointer border-gray-1 border-t-2"
                        role="menuitem">Asc <BiCheck /></div>
                    <div
                        className="px-4 py-2 text-sm text-gray-700 hover:bg-gray-100 hover:text-gray-900 flex justify-between cursor-pointer"
                        role="menuitem">Desc <BiCheck /></div>
                </div>
          </div>        
        )
    }

    const Search = () => {
      let [toggleSort, setToggleSort] = useState(false);  
      return(
            <div className="py-5">
            <div className="mt-1 relative rounded-md shadow-sm">
              <div className="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none">
                <BiSearch />
                <label htmlFor="query" className="sr-only" />
              </div>
              <input type="text" name="query" id="query" value=""
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
    export default Search
    ```
    - Note 1: We need to import the `useState function and declare and initialize the state variable in the parent `Search` component: `let [toggleSort, setToggleSort] = useState(false);`
    - Note 2: We need to activate the Search dropdown button with an `onClick` event: `onClick={() => { setToggleSort(!toggleSort) }}`
    - Note 3: We need to pass the `toggleSort` state variable to the child `DropDown` component - we do this by creating a property, named `toggle`, for the `<DropDown />` tag and setting it equal to the `toggleSort` state variable: `<DropDown toggle={toggleSort} />`
    - Note 4: The `DropDown` component receives the `toggleSort` state variable through the passed `toggle` property: `const DropDown = ( {toggle} ) => { ...}`
    - Note 5 : if the `toggle` property equals `false` then we return `null` - the dropdown menu is not returned - and we exit the `DropDown` function/component. Otherwise, the rest of the `DropDown` code is processed and we return the dropdown.
    ```
    if (!toggle) {
      return null;
    }  
    return(
          <div className="origin-top-right absolute right-0 mt-2 w-56
          rounded-md shadow-lg bg-white ring-1 ring-black ring-opacity-5">
          ....
          </div>        
    )
    ```
