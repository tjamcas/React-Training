# Course 1: React.js Essential Training
## Class Notes / Section 4 - 6

### Section 4: React State in the Component Tree
- Conditional Rendering
  - A React component tree can render components conditionally.
  - Example:
    - `index.js`:
      ```
      import React from "react";
      import ReactDOM from "react-dom";
      import "./index.css";
      import App from "./App";

      ReactDOM.render(
        <App authorized={true} />,
        document.getElementById("root")
      );
      ```
    - `app.js`:
      ```
      import React from "react";
      import "./App.css";

      function SecretComponent() {
        return (
          <h1>Secret information for authorized users only</h1>
        );
      }

      function RegularComponent() {
        return <h1>Everyone can see this component.</h1>;
      }

      function App(props) {
        return (
          <>
            {props.authorized ? (
              <SecretComponent />
            ) : (
              <RegularComponent />
            )}
          </>
        );
      }

      export default App;
      ```
    - We can refactor the code to simplify the `if` statement in the `App` function:
      ```
      function App(props) {
        return (
          <>
            { props.authorized ? <SecretComponent /> : <RegularComponent /> }
          </>
        )
      }
      ```
      - the `?` format used in the refactored code above is the ___JavaScript conditional (ternary) operator___ and more information on its use can be found at <https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Conditional_Operator>.
      - the conditional ternary operator is available in Java as well.
- Destructuring arrays and objects
  - In JavaScript arrays and objects can be destructured. Destructuring simplifies the syntax to reference an element in an array or object.
  - More information on destructuring can be found at W3Schools at <https://www.w3schools.com/react/react_es6_destructuring.asp> or mozilla at <https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment>
  - Array destructuring is used when handling component state in React
  - Object destructuring is often used in React with the `props` object.
  - Object destructuring example:
    ```
    function App({ authorized }) {
      return (
        <>
          {authorized ? (
            <SecretComponent />
          ) : (
            <RegularComponent />
          )}
        </>
      );
    }
    ```
    - Rather than having the declaration `function App(props) {...}`,    
      we reference the specific key in `props` that we want like this:    
      `function App({ authorized }) { ... }.`     
      And then we can reference `authorized` again in the conditional ternary operator
    - This can simplify and improve readability of your code when it refers to many key-value pairs in the `props` object.
- Understanding the Use State Hook
  - Managing state in a React application is important. The most modern way to handle state variables in an app is to use a function called `useState`.
  - Supplemental information on the `useState` function can be found on W3Schools at <https://www.w3schools.com/react/react_usestate.asp>
  - The `useState(arg)` function returns an array with two items.
    - The first item is the state variable - it provides the state of the component function - and it can be any type: boolean, string, integer, etc.
    - The second item is a function that updates the state variable
    - the argument `arg` is the initial state 
  - Example:
    ```
    import React, { useState } from "react";
    import "./App.css";

    function App() {
      const [emotion, setEmotion] = useState("happy");
      return (
        <>
          <h1>Current emotion is: {emotion}.</h1>
          <button onClick={() => setEmotion("frustrated")}>
            Frustrate
          </button>
          <button onClick={() => setEmotion("enthusiastic")}>
            Enthuse
          </button>
        </>
      );
    }

    export default App;
    ```
      - Looking closer at this line in the `App()` function, `const [emotion, setEmotion] = useState("happy");`
        - We are using array deconstruction to name the state variable to `emotion`, and
        - to name the state updater function to `setEmotion`
          - It is convention to name the state updater function `setState-name` -- in this case state name is 'emotion' and the updater is, then, `setEmotion`.
          - The state updater function can update the value of its state - e.g., `SetEmotion("angry")` changes `emotion` to have value "angry".
          - If you don't provide the state update function with a value, then it returns the current value of the state - e.g., `setEmotion` returns the current value of `emotion`.
      - ___Note:___ For a refesher on the use of the JavaScript `=>` arrow function, see <https://www.w3schools.com/js/js_arrow_function.asp>. The arrow `=>`function is shorthand syntax to declare a JavaScript function.
- Working with useEffect
  - From W3Schools at <https://www.w3schools.com/react/react_useeffect.asp>:
    - The `useEffect` Hook allows you to perform side effects in your components, like: console messaging, animations, fetching data, directly updating the DOM, and timers.
    - useEffect accepts two arguments. The second argument is optional:
      - `useEffect(<function>, <dependency>)`
    - `useEffect` runs on every render - i.e., whenever the DOM (is it on the DOM changing or a value in the DOM or are these the same thing?) changes causing a render to happen.
    - You can control how side effects run based on how you use the second argument:
      - Don't pass the second dependency argument, and the side effect runs every time the component is rendered
        ```
        useEffect(() => {
        //Runs on every render
        });
        ```
      - Pass an empty array, and the side effect only runs on the first render:
        ```
        useEffect(() => {
        //Runs only on the first render
        }, []);
        ```
      - Pass `props` or state values, and the side effect runs on every render:
        ```
        useEffect(() => {
        //Runs on the first render
        //And any time any dependency value changes
        }, [prop, state]);
        ```
  - ___Side note___ on using curly braces when importing React hooks, and, in general, when to use curly braces in an import statement.
    - For a good explanation, see the StackOverflow article, <https://stackoverflow.com/questions/41337709/what-is-use-of-curly-braces-in-an-es6-import-statement>
    - The article also covers default and named exports.
  - ___Side note___ on using ` `S{...}` `in the console.log - AKA 'Template literals' AKA 'Template strings'.
    - For a good explanation, see the StackOverflow article, <https://stackoverflow.com/questions/35835362/what-does-dollar-sign-and-curly-braces-mean-in-a-string-in-javascript> as well as <https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals>
  - Examples:
    - The following example is basic use of `useEffect` to log the `emotion` state to the console
      ```
      import React, { useState, useEffect } from "react";
      import './App.css';

      function App() {
        const [emotion, setEmotion] = useState("happy");

        useEffect(() => {
          console.log(`It's ${emotion} around here!`)
        });

        return (
          <>
            <h1>Current emotion is: {emotion}.</h1>
            <button onClick={() => setEmotion("happy")}>
              Happy
            </button>
            <button onClick={() => setEmotion("frustrated")}>
              Frustrate
            </button>
            <button onClick={() => setEmotion("enthusiastic")}>
              Enthuse
            </button>
          </>
        );
      }

      export default App;
      ```
    - The following snippet revises the example above by 1. creating two state variables (`emmotion` and `secondary`), and 2. adding two `useEffect` statements - each with dependency arrays - in order to log/track the two state variables (`emotion` and `secondary`) in the console.
      ```
      function App() {
        const [emotion, setEmotion] = useState("happy");
        const [secondary, setSecondary] = useState("tired");

        useEffect(() => {
          console.log(`It's ${emotion} around here!`)
        },[emotion]);

        useEffect (() => {
          console.log(`It's ${secondary} around here also!`)
        },[secondary]);

        return (
          <>
            <h1>Current emotion is: {emotion} and {secondary}.</h1>
            <button onClick={() => setEmotion("happy")}>
              Make Happy
            </button>
            <button onClick={() => setSecondary("crabby")}>
              Make Crabby
            </button>
            <button onClick={() => setSecondary("tired")}>
              Make Tired
            </button>
            <button onClick={() => setEmotion("frustrated")}>
              Frustrate
            </button>
            <button onClick={() => setEmotion("enthusiastic")}>
              Enthuse
            </button>
          </>
        );
      }
      ```
- Incorporating useReducer
  - The `useReducer` function is similar, yet an upgrade, to `useState`
    - Syntax: `useReducer(<reducer>, <initialState>)`
    - The `useReducer` function takes in the component's state and returns a new state.
    - More specifically, the useReducer Hook returns an array with the first element being the current state and the second element being the reducer function AKA dispatch method.
  - Example - here is the more complex code to track the state of a check box:
    ```
    import React, {useState} from "react";
    import './App.css';

    function App() {
      const [checked, setChecked] = useState(false);

      return (
        <>
          <input 
            type= "checkbox" 
            value={checked} 
            onChange={() => setChecked((checked) => !checked)}
          />
          <p>{checked ? "checked" : "not checked"}</p>
        </>
      );
    }

    export default App;
    ```
  - Example - here  we create a ___reducer___ function and name it `toggle`. A reducer function takes in a component's state, and changes it.
    ```
    import React, {useState} from "react";
    import './App.css';

    function App() {
      const [checked, setChecked] = useState(false);

      function toggle() {
        setChecked((checked) => !checked);
      }

      return (
        <>
          <input 
            type= "checkbox" 
            value={checked} 
            onChange={toggle}
          />
          <p>{checked ? "checked" : "not checked"}</p>
        </>
      );
    }

    export default App;
    ```
  - Example - here is the final, simplified and reduced code version of the same functionality that implements the `useReducer` React hook:
    ```
    import React, { useReducer } from "react";
    import "./App.css";

    function App() {
      const [checked, toggle] = useReducer(
        checked => !checked,
        false
      );
      return (
        <>
          <input
            type="checkbox"
            value={checked}
            onChange={toggle}
          />
          <p>{checked ? "checked" : "not checked"}</p>
        </>
      );
    }

    export default App;
    ```
    - in the code line, 
      ```
      const [checked, toggle] = useReducer(
        checked => !checked,
        false
      );
      ```
    - `checked => !checked` is the first argument of the `UseReducer` hook - i.e., the ___reducer___ function. And using array deconstruction, the reducer function has been named `toggle`, which is referenced in the `<input>` tag's `onChange={toggle}` property.
    - `false` is the second argument -- i.e., the initial state
    - __Remember:__ the useReducer Hook returns an array with the ___first___ element being the current state and the ___second___ element being the reducer function AKA dispatch method.

### Section 5: Asynchronous React
- Fetching Data with Hooks
  - In the following example we use the JavaScript `fetch` web API
    - A Web API is an application programming interface for the Web.
    - The `fetch` API interface allows web browser to make HTTP requests to web servers. <https://www.w3schools.com/js/js_api_fetch.asp>
    - We can then attach the `then` function to the returned results and cleanup the output. <https://stackoverflow.com/questions/3884281/what-does-the-function-then-mean-in-javascript>  
  - In the following example we fetch and render some raw data from the Github users API <https://api.github.com/users>. However, the rendered data is displayed in a very raw format:
    - `index.js`
      ```
      import React from "react";
      import ReactDOM from "react-dom";
      import "./index.css";
      import App from "./App";

      ReactDOM.render(
        <App login = "tjamcas" />, document.getElementById("root")
      );
      ```
    - `app.js`
      ```
      import React, { useState, useEffect } from "react";
      import './App.css';

      // https://api.github.com/users/tjamcas

      function App({login}) {
        const [data, setData] = useState(null);

        useEffect ( () => {
          fetch(`https://api.github.com/users/${login}`)
          .then((response) => response.json())
          .then(setData);
        }, []);

        if (data) {
          return (
            <div>
              {JSON.stringify(data)}
            </div>
          );
        }

        return (
          <div>
            No user available
          </div>
        );
      }

      export default App;
      ```
- Displaying data from an API
  - In this example, we select specific JSON data elements to display and render that selected data with more readable HTML formatting:
  - Update `app.js` snippet:
    ```
    if (data) {
        return (
          <div>
            <h1>{data.name}</h1>
            <p>{data.location}</p>
            <img src={data.avatar_url} alt={data.login}/>
          </div>
        );
      }
    ```
    - In the JSON tree, `data` is the root element, and then there are numerous child elements, so we refer to them using parent-child dot notation, e.g., `data.name`.
- Handling Loading States
  - When we make an HTTP request to an API, there are three possible States:
    - Pending AKA Loading
    - Success
    - Failed
  - In the following code snippet, we handle each of the possible component states:
    - We use the `useState` hook to create state variables for loading and errors (failure)
    - We reference the loading and error states in the `useEffect` function.
    ```
    function App({login}) {
      const [data, setData] = useState(null);
      const [loading, setLoading] = useState(false);
      const [error, setError] = useState(null);

      useEffect ( () => {
        if (!login) return;
        setLoading(true);
        fetch(`https://api.github.com/users/${login}`)
        .then((response) => response.json())
        .then(setData)
        .then(() => setLoading(false))
        .catch(setError); //a state updater with no values returns the current state
      }, []);

      if (loading) {
        return <h1>Loading...</h1>
      };

      if (error) {
        return <pre>{JSON.stringify(error, null, 2)}</pre>
      };

      if (!data) return null;

      return (
          <div>
            <h1>{data.name}</h1>
            <p>{data.location}</p>
            <img src={data.avatar_url} alt={data.login}/>
          </div>
        );

    }
    ```

### Section 6 React testing
- Using Create React App as a Testing Platform
  - When Create React App was installed, Jest, which is a test runner was also installed.
  - Test files should end with `.test.js` -- for example, `app.test.js`
  - In the "Terminal" app, if you execute the command `npm test`, all files designated `.test.js` will be run
  - File Structure:
    - Functions will be written in the file `functions.js`
    - Test will be written in the file `functions.test.js`
  - More information on React testing can be found at <https://create-react-app.dev/docs/running-tests>
  - More information on Jest and its syntax can be found at <https://jestjs.io/docs/using-matchers>
- Testing Small Files with Jest
  - Basic Jest testing syntax:
    - `test('name of the test', () => { /* Jest syntax here */});`
  - Example
    - `functions.js`:
      ```
      export function timesTwo(a) {
        return a * 2;
      }
      ```
    - `functions.test.js`:
      ```
      import { timesTwo } from "./functions";

      test("Multiplies by two", () => {
        expect(timesTwo(4)).toBe(8);
      });
      ```
      - `expect()` and `toBe()` are functions from the Jest library
      - `timesTwo()` is a function that resides in the `functions.js` file
      - ___Test Driven Development (TDD)___ methodology writes the tests first, and secondly, uses the feedback from the test result feedback to develop and modify the source code.
- Introducing React Testing library
  - Testing Library is a useful testing library that we can use with React or outside of React. It renders elements so that we can test the output to make sure that it matches our expectations.
  - More information on Testing Library can be found at: <https://testing-library.com/docs/react-testing-library/intro/>
  - Example:
    - `App.js`:
      ```
      import React from "react";
      import './App.css';

      // https://api.github.com/users/tjamcas

      function App() {

        return (
          <h1>Hello React Testing Library</h1>
        );

      }

      export default App;
      ```
    - `App.test.js`:
      ```
      import { render } from "@testing-library/react";
      import React from "react";
      import App from "./App";

      test("renders an h1", () => {
          const { getByText } = render(<App />);
          const h1 = getByText(/Hello React Testing Library/);
          expect(h1).toHaveTextContent("Hello React Testing Library");
      });
      ```
      - `const { getByText } = render(<App />);` is Testing Library syntax
      - `const h1 = getByText(/Hello React Testing Library/);` is a Testing Library query
      - The other code is Jest syntax
