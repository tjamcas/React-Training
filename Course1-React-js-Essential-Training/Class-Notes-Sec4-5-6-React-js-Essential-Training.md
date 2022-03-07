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
          - It is convention to name the state updater function `setState-name` -- in this case state name is 'emotion' and the updater is, then, `setEmotion`
      - ___Note:___ For a refesher on the use of the JavaScript `=>` arrow function, see <https://www.w3schools.com/js/js_arrow_function.asp>. The arrow `=>`function is shorthand syntax to declare a JavaScript function.
- Working with useEffect
  - From W3Schools at <https://www.w3schools.com/react/react_useeffect.asp>:
    - The `useEffect` Hook allows you to perform side effects in your components, like: consle messaging, animations, fetching data, directly updating the DOM, and timers.
    - useEffect accepts two arguments. The second argument is optional:
      - `useEffect(<function>, <dependency>)`
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
    - The following example is basic use of `useEffect` to og cominent state to the console log
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
    - The following snippet revises the example above by adding dependency arrays to two `useEffect` statements in order to track two separate state variables (`emmotion` and `secondary`) in the console log
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
