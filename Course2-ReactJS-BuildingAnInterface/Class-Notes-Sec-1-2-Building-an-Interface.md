# Course 2: React.js: Building an Interface
## Class Notes / Section 1-2

### Section 1: Setup and Install
- React Installation
  - To access the slides for this course, go to: <https://raybo.org/slides_reactinterface>
  - To access the files for this course, go to: <https://github.com/planetoftheweb/reactinterface>
    - Choose the branch that correlates with the video, e.g., branch 02_01e is the end state of the code worked on in Chapter video 1.
- Using React Icon Components
  - React icons gives you access to thousands of icon libraries like Font Awesome, Bootstrap Icons, and CSS.gg.
    - React Icons only inserts the code for the icon that you need -- not the entire library (e.g. just the FontAwesome icons you need and not there full library of hundreds of icons
  - To install: `npm install react-icons --save`
  - To use in an app: `import { ICONNAME } from "react-icons/LIBNAME";`
    - where `LIBNAME` is usually a two letter abbreviation, for example, `BS` for Bootstrap
    - In your JS file, create an element: `<ICONNAME />`, where `ICONNAME` will be the name of your icon
  - you can search for an icon at <https://react-icons.github.io/react-icons>
  - Here is an example where we inserted an icon from the Bootstrap Icon library into the header:
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
      - ??? `npm install @tailwindcss/forms` ???
    - Add the paths to all of your template files in your `tailwind.config.js` file:
      ```
      module.exports = {
      content: [
        "./src/**/*.{js,jsx,ts,tsx}",
      ],
      ```
    - Add Tailwind CSS Forms library `.../reactinterface/tailwind.config.js`:
      ```
      module.exports = {
        content: [],
        theme: {
          extend: {},
        },
        plugins: [require('@tailwindscss/forms')],
      }
      ```
    - Add the `@tailwind` directives for each of Tailwind’s layers to your `./src/index.css` file. Replace everyting in the file with the following:
      ```
      @tailwind base;
      @tailwind components;
      @tailwind utilities;
      ```
    - Start using Tailwind in your project: `npm run start`
