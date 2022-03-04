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
