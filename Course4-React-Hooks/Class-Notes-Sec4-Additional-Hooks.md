# Course 4: React Hooks
## Class Notes / Section 4

### Section 3: Additional Hooks
- __Video 1: Handling Complex State With useReducer__
  - The `useReducer` hook can also be used to manage state variables
  - Syntax:   
    `const [state, setState] = useReducer(reducerFunction, initValue);`
    - more specifically, as an example:   
      `const [number, setNumber] = useReducer((number, newNumber) => number + newNumber, 0);`    
    - Note: the first parameter in the reducer function is the state variable, `number`.
    - Note: the second parameter is the reducer function and this function is executed when we call the set `setState` funtion - in this case, `setNumber`. It returns a single value which is stored in the state variable - in this case `number`.
    - Recall: a reducer function takes in multiple values and returns a single value -- see article: <https://css-tricks.com/getting-to-know-the-usereducer-react-hook/>
  - Here is an example of the `useReducer` function in action -- it increments by one the value in the `<h1>` element everytime the element is clicked:
    ```
    import React, { useReducer } from 'react';
    import ReactDOM from 'react-dom';
    import './index.css';

    function App() {
      const [number, setNumber] = useReducer(
          (number, newNumber) => number + newNumber, 
          0
        );
      return (
        <h1 onClick={() => setNumber(1)}>{number}</h1>
      );
    }

    ReactDOM.render(
      <React.StrictMode>
        <App />
      </React.StrictMode>,
      document.getElementById("root")
    );
    ```
    - Note: in this line of code, `<h1 onClick={() => setNumber(1)}>{number}</h1>`, the argument to setNumber (i.e., the number "1") gets mapped to the variable `newNumber` in the reducer function.
- __Video 2: Refactoring useState to useReducer__
  - Recall the `index.js` file to create a dynamic checkbox using `useState`:
    ```
    import React, { useState } from 'react';
    import ReactDOM from 'react-dom';
    import './index.css';

    function App() {
      const [checked, setChecked] = useState(false);

      return (
        <>
          <input 
            type="checkbox"
            value={checked}
            onChange= {() => setChecked((checked)=> !checked)}
          />
        {checked ? "checked" : "not checked"}
        </>
      );
    }

    ReactDOM.render(
      <React.StrictMode>
        <App />
      </React.StrictMode>,
      document.getElementById("root")
    );
    ```
  - We can refactor this code with the `useReducer` hook:
    ```
    import React, { useReducer } from 'react';
    import ReactDOM from 'react-dom';
    import './index.css';

    function App() {
      const [checked, toggle] = useReducer(
        (checked)=> !checked,
        false
      );

      return (
        <>
          <input 
            type="checkbox"
            value={checked}
            onChange= {toggle}
          />
        {checked ? "checked" : "not checked"}
        </>
      );
    }

    ReactDOM.render(
      <React.StrictMode>
        <App />
      </React.StrictMode>,
      document.getElementById("root")
    );
    ```
    - Note 1: `checked` is still the state variable.
    - Note 2: instead of naming the state setter function to `setChecked`, it is more accurately named `toggle`
    - Note 3: for the reducer function, we use the same logic as before: `(checked)=> !checked)`
- __Video 3: Dispatching Actions With useReducer__
  - We can use `useReducer` to execute a list of actions.
  - We can re-write the syntax pattern as follows:    
    ```
    const [state, dispatch] = useReducer(   
      reducer,    
      initialState    
    );
    ```
  - A "reducer" function takes in a `state`, and an `action`, and returns a new `state` - here is the pattern for a reducer function:   
    ```
    function reducer(state, action) {
      // reducer logic goes here
    }
    ```
  - The full code for `index.js` is:
    ```
    import React, { useReducer } from 'react';
    import ReactDOM from 'react-dom';
    import './index.css';

    function App() {
      const initialState = {
        message: "hi"
      }

      function reducer(state, action) {
        switch (action.type) {
          case "yell":
            return {
              message: `HEY! I JUST SAID ${state.message}`
            };
          case "whisper":
            return {
              message: `excuse me. I just said ${state.message}`
            };
        }
      }

      const [state, dispatch] = useReducer(
        reducer,
        initialState
      );

      return (
        <>
          <p>Message: {state.message}</p>
          <button onClick={() => dispatch({type:"yell"})}>YELL</button>
          <button onClick={() => dispatch({type:"whisper"})}>whisper</button>
        </>
      );
    }

    ReactDOM.render(
      <React.StrictMode>
        <App />
      </React.StrictMode>,
      document.getElementById("root")
    );
    ```
    - Note 1: if we had not defined the `reducer` function outside of the `useReducer` statement, and instead embedded the `reducer` code directly in the `useReducer` statement, the code logic would be written as follows:
      ```
      const [state, dispatch] = useReducer(
        (state, action) => {
          switch (action.type) {
            case "yell":
              return {
                message: `HEY! I JUST SAID ${state.message}`
              };
            case "whisper":
              return {
                message: `excuse me. I just said ${state.message}`
              };
          }
        },
        initialState
      );
      ```
- __Video 4: Managing Form Inputs with useRef__
  - useRef accesses a component and determines its value -- this can be extremely useful, particularly with forms.
  - In the following example, we want the user to type a sound into a text input box, and select a color from a color input box that corresponds.
  - In React, a `ref` is an object that stores values about a DOM element/node for the lifetime of a component.
  - The useRef hook creates a `ref`
  - The following declarations creates two `ref`'s:
    ```
    const sound = useRef();
    const color = useRef();
    ```
  - We add the `ref` into each of the input elements:
    ```
    <input ref = {sound} type = "text" placeholder="Sound..." />
    <input ref = {color} type = "color" />
    ```
  - The logic for handling the submission of the form is placed in a `submit` function that accesses the value of each `ref`:
    ```
    const submit = (e) => {
      e.preventDefault();
      const soundVal = sound.current.value;
      const colorVal = color.current.value;
      alert(`${soundVal} sounds like ${colorVal}`);
    };
    ```
    - Note that the line, `e.preventDefault();` prevents the default behavior of re-loading the page, which allows us to access the value in the `ref`'s we created.
    - Note also that `.current.value` is an attribute of the `ref` we created
  - We create an `onSubmit` event handler for when the form's "ADD" button is clicked to submit the page:
    ```
    <form onSubmit={submit}>
      ...
    </form>
    ```
  - Here is the full code:
    ```
    import React, { useRef } from 'react';
    import ReactDOM from 'react-dom';
    import './index.css';

    function App() {
      const sound = useRef();
      const color = useRef();

      const submit = (e) => {
        e.preventDefault();
        const soundVal = sound.current.value;
        const colorVal = color.current.value;
        alert(`${soundVal} sounds like ${colorVal}`);
        sound.current.value = "";
        color.current.value = "";
      };

      return (
        <form onSubmit={submit}>
          <input ref = {sound} type = "text" placeholder="Sound..." />
          <input ref = {color} type = "color" />
          <button>ADD</button>
        </form>
      );
    }

    ReactDOM.render(
      <React.StrictMode>
        <App />
      </React.StrictMode>,
      document.getElementById("root")
    );
    ```
  - With the ability to access a `ref`, instead of just printing an alert to the screen, we could do something more useful like writing the value to a record in a database.
