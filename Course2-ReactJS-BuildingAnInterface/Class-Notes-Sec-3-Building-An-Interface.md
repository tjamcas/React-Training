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
