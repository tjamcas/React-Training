# Course 1: React.js Essential Training
## Class Notes

### Section 1: what is React

- To add React Developer Tools extension to Chrome:
    - <https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi?hl=en>
- To add React Developer Tools extension to FIrefox:
    - <https://addons.mozilla.org/en-US/firefox/addon/react-devtools/>
- React learning (docs and tutorials) resource:
    - <https://reactjs.org/>
- Shortcut to open React Developer Tools in Chrome:
    - Mac: `Command+Option+J`
- Installing Node and NPM on your Mac:
    - Node.js® is a JavaScript runtime built on Chrome's V8 JavaScript engine.
    - node is a framework that can run JavaScript code on your machine while npm is a package manager. Using npm we can install and remove javascript packages also known as node modules.
    - go to <https://nodejs.org/en/> for latest version download


### Section 2: Intro to React Elements
- To create and run a package on your computer, run the following in Terminal:
    - `npx create-react-app appName`
    - `npx` is the package runner
    - `create-react-app` is the tool to create and build the project
- Once the package is built, you can:
    - `cd appname` to go to the directory where you created the package named `appName`
    - `npm start` to run the package
        - this will open `localhost:3000` in the Chrome browser
        - this is just a local web server in your browser that runs the package files
- Alternatively, you can run the package in `React-CodeSandbox`:
    - enter `react.new` in the Chrome browser
    - Your browser will go to the site: <https://codesandbox.io/s/new?utm_source=dotnew> where you can experiment with new code
- `create-react-app` builds the following file structure:
    - `appName`
        - `package.json`
            -  `"dependencies"` provides details of React version, testing libraries, and `react-scripts`
            -  `react-scripts` sets up the tooling
        -  `appName/src`
            -  contains all the code files that we write for an app
        -  `appName/public`
            -  contains assets like an index.html and logos files that will be used to build the app
-  Creating React elements
    -  `index.js`:    
        ```
        import React from "react";
        import ReactDOM from "react-dom";
        import "./index.css";

        ReactDOM.render(
          React.createElement(
            "h1",
            { style: { color: "blue" } },
            "Heyyy Everyone!"
          ),
          document.getElementById("root")
        );
        ```
    - the `ReactDOM.render(arg1, arg2)` function has two arguments:
        - `arg1`is the element that you want create using `React.createElement(argA, argB, argC)`
        - `arg2` specifies the location where you want to insert the new element using `document.getElementById(argD)`
    - `React.createElement(argA, argB, argC)`
        - `argA` is the type of HTML element/tag that you want to add
        - `argB` contains properties for the HTML element -- if none then specify `null`
        - `argC` is the element value -- in this case, the text for the `h1` header
    - `document.getElementById(argD)`
        - `argD` specifies where you want to insert the newly created element -- in this case the newly created `h1` header is to be inserted into an element with an id of "root"
    - _Note:_ in this example code snippet we have removed `React.StrictMode` which generates warnings if any code is outside of a coding best practice.
- Refactoring elements using JSX
    - JSX - Javascript as XML - is a javaScript language extension that allows you to write HTML tags directly in JavaScript
    - For example:
    ```
    import React from "react";
    import ReactDOM from "react-dom";
    import "./index.css";

    ReactDOM.render(
      <ul>
        <li>Monday</li>
        <li>Tuesday</li>
        <li>Wednesday</li>
      </ul>,
      document.getElementById("root")
    );
    ```    
    - JSX is the code between and including the `<ul>` and `</ul>` start and end tags.
    - When you use `create-react-app`, the Babel compiler is used to correctly interpret JSX code that it finds embedded in the JavaScript code.
    - For more information on the Babel JavaScript compiler, go to: <https://babeljs.io/>

### Section 3: React Components
- Components are pieces or building blocks of UI that we use to describe one part of an application. _More specifically, components are functions that return UI._
- A component is described in a JavaScript file, `componentName.js`
    - In the JavaScript file we define a function that returns JSX
    - At the end of the `ComponentName.js` file you export `ComponentName`, 
        - `export default ComponentName;`
    - and ...
    - ... at the beginning of the `index.js` file you import `componentName`:
        - `import ComponentName from "./ComponentName"`
- Example
    - `index.js:`
        ```
        import React from "react";
        import ReactDOM from "react-dom";
        import "./index.css";
        import App from "./App";

        ReactDOM.render(<App />, document.getElementById("root"));
        ```
        - The last code line, `ReactDOM.render(<App />, document.getElementById("root"));`, specifies that the "App" component will be rendered to the DOM and inserted into the "root" element
    - `App.js`
        ```
        import React from "react";
        import "./App.css";    

        function Header() {
          return (
            <header>
              <h1>Eve's Kitchen</h1>
            </header>
          );
        }    

        function Main() {
          return (
            <section>
              <p>We serve the most delicious food around.</p>
            </section>
          );
        }    

        function Footer() {
          return (
            <footer>
              <p>It's true.</p>
            </footer>
          );
        }    

        function App() {
          return (
            <div className="App">
              <Header />  {/* Refers to Header() function defined above */}
              <Main />  {/* Refers to Main() function defined above */}
              <Footer />  {/* Refers to Footer() function defined above */}
            </div>
          );
        }    

        export default App;
        ```
        - The `App.js` file contains the `App()` function as well as the `Header()`, `Main()`, and `Footer()` functions
        - The `App()` function returns to the `index.js` file the `Header()`, `Main()`, and `Footer()` functions to be rendered into the DOM at element named "root"
        - The `App.js` file organizes and returns the components for rendering
- Adding Component Properties
    - Every React component has access to the object called `props`. 
        - 'props' is passed into functions.
        - The `props` object holds all of the different properties for our component.
    - Think of props as being a mini-container - like a little backpack - that you can place information in for every single component.
        - When the component is rendered, we pass the properties into the component, and then display them using dot notation.
    - Example:    
        ```
        import React from "react";
        import "./App.css";

        function Header(props) {
          return (
            <header>
              <h1>{props.name}'s Kitchen</h1>
            </header>
          );
        }

        function Main(props) {
          return (
            <section>
              <p>
                We serve the most {props.adjective} food around.
              </p>
            </section>
          );
        }

        function Footer(props) {
          return (
            <footer>
              <p>Copyright {props.year}</p>
            </footer>
          );
        }

        function App() {
          return (
            <div className="App">
              <Header name="Horacio" />
              <Main adjective="amazing" />
              <Footer year={new Date().getFullYear()} />
            </div>
          );
        }

        export default App;
        ```
        - `props` is added as an argument to the `Header()`, `Main()`, and `Footer()` functions
            - `function Main(props) { ... }`
        - Hot spots are designated in the `Header()`, `Main()`, and `Footer()` functions with double curly braces - { }
            - `<p> We serve the most {props.adjective} food around. </p>`
            - Inside the curly braces we use a dot notation with `props`to define the property's name.
            - Here, we're creating the `adjective` property with the `Main` component
        - In the `App` function, we can reference the newly defined component property and assign it a value
            - `<Main adjective="amazing" />`
            - Here, the `App` component refers to the `Main` component for which we have defined a property named `adjective` which has a value of "amazing".
            - Inside the curly braces we use a dot notation with `props`to define the property's name.
- Working with lists(i.e.,sending array data to components)
    - So far in the exaamples above we've passed properties to components in the form of string and functions (i.e., the date function in the last example). in the next example, we will pass array data into a component tree.
    - Example, `App.js`:
        ```
        import React from "react";
        import "./App.css";

        function Header(props) {
          return (
            <header>
              <h1>{props.name}'s Kitchen</h1>
            </header>
          );
        }

        function Main(props) {
          return (
            <section>
              <p>
                We serve the most {props.adjective} food around.
              </p>
              <ul style={{ textAlign: "left" }}>
                {props.dishes.map(dish => (
                  <li>{dish}</li>
                ))}
              </ul>
            </section>
          );
        }

        function Footer(props) {
          return (
            <footer>
              <p>Copyright {props.year}</p>
            </footer>
          );
        }

        const dishNames = [
          "Macaroni and Cheese",
          "Salmon",
          "Tofu with Vegetables"
        ];

        function App() {
          return (
            <div className="App">
              <Header name="Horacio" />
              <Main adjective="amazing" dishes={dishNames} />
              <Footer year={new Date().getFullYear()} />
            </div>
          );
        }

        export default App;
        ```
        - In the `Main(props)` function, we created the `props.dishes` property
            - `<ul style={{ textAlign: "left" }}> {props.dishes.map(dish => ( <li>{dish}</li> ))} </ul>`
        - When the `App()` function calls the `Main()` function, it specifies that the property`dishes={dishNames}`
        - `dishNames` is a defined array constant
        - In the return statement of the `App()` function and inside the `ul` tags, we 1. map the `dishNames` array to a new array that takes each item in `dishNames` and sandwiches it between `li` tags, and then 2. passes this new array through the `props` parameter
        - We reference the `props.dishes` property in the `App()` function
            - `<Main adjective="amazing" dishes={dishes} />`
        - _For more information on the `map()` method see_ <https://www.w3schools.com/jsref/jsref_map.asp>
        - _For more information on the `=>` function see_ <https://www.w3schools.com/js/js_arrow_function.asp>
- Adding Keys to List Items:
    - In the previous example, the console raises a warning that each child in a list must have a unique key property. This is because `dishNames` is an array of strings without keys
    - To remedy this we can map `dishNames`, which is a simple array of strings, to an array of objects with each object having a key and value pair.
        ```
        import React from "react";
        import "./App.css";

        function Header(props) {
          ....
        }

        function Main(props) {
          return (
            <section>
              <p>
                We serve the most {props.adjective} food around.
              </p>
              <ul style={{ textAlign: "left" }}>
                {props.dishes.map(dish => (
                  <li key={dish.id}>{dish.title}</li>
                ))}
              </ul>
            </section>
          );
        }

        function Footer(props) {
          ....
        }

        const dishNames = [
          "Macaroni and Cheese",
          "Salmon",
          "Tofu with Vegetables"
        ];

        const dishNamesObjects = dishNames.map((dish, i) => ({
          id: i,
          title: dish
        }));

        function App() {
          return (
            <div className="App">
              <Header name="Horacio" />
              <Main adjective="amazing" dishes={dishNamesObjects} />
              <Footer year={new Date().getFullYear()} />
            </div>
          );
        }

        export default App;
        ```
    - To create the array of objects, we write a short transformation function as seen in the code snippet directly below. This function will `map` over the `dishNames`, and for each `dish`, it returns an object (___note:___ whenever we return an object from a function like this, in line, we need to wrap it in parenthesis). So the `id` will be `i` and the `title` will be dish. 
        - `{props.dishes.map(dish => ( <li key={dish.id}>{dish.title}</li> ))}`
