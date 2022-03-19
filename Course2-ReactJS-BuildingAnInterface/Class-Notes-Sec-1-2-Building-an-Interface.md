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
  - Example: creating a ___Search___ and a ___DropDown___ component
    - Create new file named `Search.js` in the `components` folder
      - The File includes a newly created `Search` function, that, for ease and convenience of passing parameters in the empty `(...)` parentheses, we use the _arrow_` form of defining a function:
        ```
        const Search = () => {
            return(
            //... JSX code will go here - copied from: https://github.com/LinkedInLearning/react-interface-2880067/wiki/Search-Component-JSX
            )
        }
        ```
      - We need to import icons. Here is the copied JSX code plus the necessary import and export statements, `Search.js`:
        ```
        import { BiCaretDown, BiSearch} from "react-icons/bi"

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
                    </div>
                  </div>
                </div>
              </div>        
            )
        }
        export default Search        
        ```
        - Note 1: We have to import the _BoxIcons_ that we use in the `Search function
        - Note 2: We have to export `Search` as the default function/component from this file, so that it can be called in the `App.js` file
        - Note 3: The `DropDown` component is a child component to the `Search`. Therefore, we included the `DropDown` function/component in the `Search.js` file.
          - The `DropDown` function creates dropdown menu of checkble options which we can choose to organize our search results.
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
