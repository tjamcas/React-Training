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
        - Note 5B: the string literal is inserted between the back ticks ` `...` `
