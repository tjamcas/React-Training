# Course 1: React.js Essential Training
## Class Notes / Section 4 - 6

### Section 4: Conditional Rendering
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
    - the `?` format used in the refactored code above is the ___JavaScript conditional (ternary) operator___ and more information can be found at <https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Conditional_Operator>
