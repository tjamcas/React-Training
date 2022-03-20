# Course 2: React.js: Building an Interface
## Class Notes / Section 3

### Section 3: Sort and Search
- Passing Data to a Component
  - In this section, we re-factor our code to break out the appointment information code  from the `App.js` component and into a separate component named `AppointmentInfo.js`
  - This will require passing information from the `App` component into the AppointmentInfo` component
  - Recall that the `App` component receives/imports all of the scheduled appointment information through the import statement: `import appointmentList from "./data.json"`
  - `App` then processes the appointment data with the following code block:
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
    - This will simplify the `App.js~ code and make it more readable, while placing a logically contained and functional code block into its own component.
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
              .map(appointment => (
                <AppointmentInfo 
                  key={appointment.id}
                  appointment={appointment}
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
    - Note 2: We pass the `Appointment` object to the child `AppointmentInfoo` component with the `map (appointment => ( ... ) code block
    - Note 3: Breaking apart the following line:
      ```
      {appointmentList
        .map(appointment => ( // Note 3A
          <AppointmentInfo 
            key={appointment.id}  // Note 3B
            appointment={appointment} // Note 3C
          />
        ))
      }
      ```
      - Note 3A: `appointment` here is a variable of an iterated item from the `appointmentList` array
      - Note 3B: we add a key to the list item to quash warning statements
      - Note 3C: `appointment` is the named attribute - we could call it anything??? - and `{appointment}` passes the iterated item to the child `AppointmentInfo` component
