# Course 2: React.js: Building an Interface
## Class Notes / Section 1-2

### Section 1: Setup and Install
- React Installation
  - To access the slides for this course, go to: <https://raybo.org/slides_reactinterface>
  - To access the files for this course, go to: <https://github.com/planetoftheweb/reactinterface>
    - Choose the branch that correlates with the video, e.g., branch 02_01e is the end state of the code worked on in Chapter video 1.
  - To access the JSX code snippets and data in examples go to the instructor's GitHub wiki at <https://github.com/LinkedInLearning/react-interface-2880067/wiki>
- Using React Icon Components
  - React icons gives you access to thousands of icon libraries like Font Awesome, Bootstrap Icons, and CSS.gg.
    - React Icons only inserts the code for the icon that you need -- not the entire library (e.g. just the FontAwesome icons you need and not there full library of hundreds of icons
  - To install: `npm install react-icons --save`
  - To use in an app: `import { ICONNAME } from "react-icons/LIBNAME";`
    - where `LIBNAME` is usually a two letter abbreviation, for example, `BS` for Bootstrap
    - In your JS file, create an element: `<ICONNAME />`, where `ICONNAME` will be the name of your icon
  - you can search for an icon at <https://react-icons.github.io/react-icons>
  - Here is an example where we inserted an icon from the Bootstrap Icon library into the header by editing `App.js`:
    ```
    import './App.css';
    import { BiArchive } from "react-icons/bi";

    function App() {
      return (
        <div className="App">
          <h1><BiArchive />Your Appointments</h1>
        </div>
      );
    }

    export default App;
    ```
- Installing Tailwind CSS in React
  - Installation instructions can be found at: <https://tailwindcss.com/docs/guides/create-react-app>
  - To install Tailwind CSS:
    - In Terminal:
      - `npm install -D tailwindcss postcss autoprefixer`
      - `npx tailwindcss init -p`
      - --> `npm install @tailwindcss/forms` <-- ___Not in Tailwind instruction guide (the link above) but this is specifically called out in the course's setup instructions.___ __MUST DO!__
    - Modify the `.../reactinterface/tailwind.config.js` file
      - ___Did not perform this step from the Tailwind instruction guide (the link above) - the `content` line was not added in the coursework instructions___:
        - Add the paths to all of your template files in your `tailwind.config.js` file:
          ```
          module.exports = {
          content: ["./src/**/*.{js,jsx,ts,tsx}",],
          ```
      - Tailwind will install all of the styles, even if we don't use them. Thankfully, we can use a plugin called purgeCSS to get rid of what we don't need. We'll change the generated `tailwind.config.js` config file, so that it takes care of this. Modify the `purge` setting in the config file to this: `purge: ['./src/**/*.{js,jsx,ts,tsx}', './public/index.html'],`
      - Modify the `plugins` setting to this: `plugins: [ require('@tailwindcss/forms')]`
      - Add the Tailwind CSS Forms library to the `plugins` setting: `plugins: [require('@tailwindscss/forms')],`
      - __Final__ form of `.../reactinterface/tailwind.config.js`:
        ```
        module.exports = {
          purge: ['./src/**/*.{js,jsx,ts,tsx}', './public/index.html'],
          darkmode: false,
          theme: {
            extend: {},
          },
          plugins: [require('@tailwindcss/forms')]
        }
        ```
    - Add the `@tailwind` directives for each of Tailwind’s layers to your `./src/index.css` file. Replace everyting in the file with the following:
      ```
      @tailwind base;
      @tailwind components;
      @tailwind utilities;
      ```
    - Start using Tailwind in your project: `npm run start`

### Section 2: Getting Started
- Your First Component
  - We will place all our components in a newly created folder. `./src/components`
  - Example: creating a ___Search___ component with its child ___DropDown___ component
    - Create new file named `Search.js` in the `components` folder
      - The File includes newly created `Search` and `DropDown` functions, in which we use, for ease and convenience of passing parameters in the empty `(...)` parentheses, the _arrow_ `() => { ... }` form of defining a function:
        ```
        const Search = () => {
            return(
            //... JSX code will go here - copied from: https://github.com/LinkedInLearning/react-interface-2880067/wiki/Search-Component-JSX
            )
        }
        ```
      - We need to import icons for the new parent and child components, `Search` and `DropDown`. Here is the copied JSX code for the new `Search` and `DropDown` components plus the necessary import and export statements, `Search.js`:
        ```
        import { BiCaretDown, BiSearch, BiCheck } from "react-icons/bi"

        const DropDown = () => {
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
                        className="justify-center px-4 py-2 bg-blue-400 border-2 border-blue-400 text-sm text-white hover:bg-blue-700 focus:outline-none focus:ring-2 focus:ring-offset-2 flex items-center" id="options-menu" aria-haspopup="true" aria-expanded="true">
                        Sort By <BiCaretDown className="ml-2" />
                      </button>
                      <DropDown />
                    </div>
                  </div>
                </div>
              </div>        
            )
        }
        export default Search    
        ```
        - Note 1: We have to import the _BoxIcons_ that we use in the `Search` function
        - Note 2: We have to export `Search` as the default function/component from this file, so that it can be called in the `App.js` file
        - Note 3: The `DropDown` component is a child component to the `Search`. Therefore, we included the `DropDown` function/component in the `Search.js` file.
          - The `DropDown` function creates dropdown menu of checkable options which we can choose to organize our search results.
    - Modify the  `app.js` file in the `src` folder to reference the `Search.js` file containing the `Search` component/function
      ```
      import { BiArchive } from "react-icons/bi";
      import Search from "./components/Search";

      function App() {
        return (
          <div className="App container mx-auto mt-3 font-thin">
            <h1 className="text-5xl">
              <BiArchive className="inline-block text-red-400 align-top"/>Your Appointments</h1>
            <Search />
          </div>
        );
      }

      export default App;
      ```
      - Note: we reference/call the `Search` component through the `<Search />` tag
- Getting Appointments and Debugging
  - In the following code, we add another component that will allow us to add appointments into our user interface
    - We create a new file `./components/AddAppointment.js` which exports the function/component, `AddAppointment`:
      ```
      import { BiCalendarPlus } from "react-icons/bi";

      const AddAppointment = () => {
          return(
              <div>
              <button className="bg-blue-400 text-white px-2 py-3 w-full text-left rounded-t-md">
                <div><BiCalendarPlus className="inline-block align-text-top" />  Add Appointment</div>
              </button>
              <div className="border-r-2 border-b-2 border-l-2 border-light-blue-500 rounded-b-md pl-4 pr-4 pb-4">
                <div className="sm:grid sm:grid-cols-3 sm:gap-4 sm:items-start  sm:pt-5">
                  <label htmlFor="ownerName" className="block text-sm font-medium text-gray-700 sm:mt-px sm:pt-2">
                    Owner Name
                  </label>
                  <div className="mt-1 sm:mt-0 sm:col-span-2">
                    <input type="text" name="ownerName" id="ownerName"
                      className="max-w-lg block w-full shadow-sm focus:ring-indigo-500 focus:border-indigo-500 sm:max-w-xs sm:text-sm border-gray-300 rounded-md" />
                  </div>
                </div>

                <div className="sm:grid sm:grid-cols-3 sm:gap-4 sm:items-start  sm:pt-5">
                  <label htmlFor="petName" className="block text-sm font-medium text-gray-700 sm:mt-px sm:pt-2">
                    Pet Name
                  </label>
                  <div className="mt-1 sm:mt-0 sm:col-span-2">
                    <input type="text" name="petName" id="petName"
                      className="max-w-lg block w-full shadow-sm focus:ring-indigo-500 focus:border-indigo-500 sm:max-w-xs sm:text-sm border-gray-300 rounded-md" />
                  </div>
                </div>

                <div className="sm:grid sm:grid-cols-3 sm:gap-4 sm:items-start  sm:pt-5">
                  <label htmlFor="aptDate" className="block text-sm font-medium text-gray-700 sm:mt-px sm:pt-2">
                    Apt Date
                  </label>
                  <div className="mt-1 sm:mt-0 sm:col-span-2">
                    <input type="date" name="aptDate" id="aptDate"
                      className="max-w-lg block w-full shadow-sm focus:ring-indigo-500 focus:border-indigo-500 sm:max-w-xs sm:text-sm border-gray-300 rounded-md" />
                  </div>
                </div>

                <div className="sm:grid sm:grid-cols-3 sm:gap-4 sm:items-start  sm:pt-5">
                  <label htmlFor="aptTime" className="block text-sm font-medium text-gray-700 sm:mt-px sm:pt-2">
                    Apt Time
                  </label>
                  <div className="mt-1 sm:mt-0 sm:col-span-2">
                    <input type="time" name="aptTime" id="aptTime"
                      className="max-w-lg block w-full shadow-sm focus:ring-indigo-500 focus:border-indigo-500 sm:max-w-xs sm:text-sm border-gray-300 rounded-md" />
                  </div>
                </div>

                <div className="sm:grid sm:grid-cols-3 sm:gap-4 sm:items-start  sm:pt-5">
                  <label htmlFor="aptNotes" className="block text-sm font-medium text-gray-700 sm:mt-px sm:pt-2">
                    Appointment Notes
                  </label>
                  <div className="mt-1 sm:mt-0 sm:col-span-2">
                    <textarea id="aptNotes" name="aptNotes" rows="3"
                      className="shadow-sm focus:ring-indigo-500 focus:border-indigo-500 mt-1 block w-full sm:text-sm border-gray-300 rounded-md" placeholder="Detailed comments about the condition"></textarea>
                  </div>
                </div>

                <div className="pt-5">
                  <div className="flex justify-end">
                    <button type="submit" className="ml-3 inline-flex justify-center py-2 px-4 border border-transparent shadow-sm text-sm font-medium rounded-md text-white bg-blue-400 hover:bg-blue-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-blue-400">
                      Submit
                    </button>
                  </div>
                </div>
              </div>
            </div>        
          )
      }

      export default AddAppointment      
      ```
    - We need to import the new component into `App.js` and add it to the `App` function's JSX:
      ```
      import { BiCalendar } from "react-icons/bi";
      import Search from "./components/Search";
      import AddAppointment from "./components/AddAppointment";

      function App() {
        return (
          <div className="App container mx-auto mt-3 font-thin">
            <h1 className="text-5xl mb-3">
              <BiCalendar className="inline-block text-red-400 align-top"/>Your Appointments</h1>
            <AddAppointment />
            <Search />
          </div>
        );
      }

      export default App;
      ```
      - Note 1: we call the `AddAppointment` component/function with the `<AddAppointment />` tag
      - Note 2: a minor change where we changed the icon from `BiArchive` to `BiCalendar`
- Importing JSON Data as a Variable      
  - We create a file in the `src/data.json` that has all the appointment data and import it into App.JS. When we import it, we give it the variable name, `appointmentList`.
  - `data.json` has the format:
    ```
    [
        {
          "id": "0",
          "petName": "Pepe",
          "ownerName": "Reggie Tupp",
          "aptNotes": "It's time for this rabbit's post spaying surgery checkup",
          "aptDate": "2018-11-28 13:30"
        },
        {
          "id": "1",
          "petName": "Rio",
          "ownerName": "Philip Ransu",
          "aptNotes": "Rio is up for his next round of vaccinations",
          "aptDate": "2018-11-28 10:15"
        },
        // ...
        {
          "id": "24",
          "petName": "Nugget",
          "ownerName": "Darla Branson",
          "aptNotes": "This little fish Nugget, has a rash on his stomach area",
          "aptDate": "2018-12-2 9:00"
        }
      ]
    ```
  - `App.js` is modified to look like:
    ```
    import { BiCalendar, BiTrash } from "react-icons/bi";
    import Search from "./components/Search";
    import AddAppointment from "./components/AddAppointment";
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
        </div>
      );
    }

    export default App;    
    ```
    - Note: `import appointmentList from "./data.json"` imports the file `data.json` into the array variable `appointmentList`
    - Note: we use the `.map` function to map each item from the `appointmentList` array into a JSX list item, and we use an expression that is placed inside `{...}` curly braces to refer to the appropriate JSON key-value pair in `appointmentList`, e.g., `{appointment.aptNotes}`.
