# Course 4: React Hooks
## Class Notes / Section 4

### Section 3: Creating Custom Hooks
- __Video 1: Reusing Form Logic with Custom Hooks__
  - We can abstract repeated logic in our code to a custom hook.
  - In the previous example, we have very similar code that is repeated in the returned JSX text and color input element. If we had a form with even more input fields, we would see a similar repititin. Therefore, we should abstract the repeated, similar code into a customized React hook.
  - Here is a new file,`src/useInput.js`, that contains the customized hook/function:
    ```
    import {useState} from "react";

    export function useInput(initialValue) {
        const [value, setValue] = useState(initialValue);
        return [
            {value, onChange: (e) => setValue(e.target.value)},
            () => setValue(initialValue)
        ];
    }
    ```
    - Note 1: we are only creating a single customized hook, but if we had multiple customized hooks, then we might have wanted to store them all in a single file named `src/hooks.js`
    - Note 2: our customized hook is named `useInput` and it takes in a single parameter containing the initial value of the input element/form field
      ```
      export function useInput(initialValue) {
        // logic
      }
      ```
    - Note 3: We employ `useState` to create the state variable and its "setter":   
      `const [value, setValue] = useState(initialValue);`
    - Note 4: The hook returns an array containing: 1. an object with a `value` and `onChange` logic, and 2. a function to re-set to the initial value:
      ```
      return [
          {value, onChange: (e) => setValue(e.target.value)},
          () => setValue(initialValue)
      ];
      ```
  - Here is the refactored `src/index.js` code using a customized hook:
    ```
    import React from 'react';
    import ReactDOM from 'react-dom';
    import './index.css';
    import { useInput } from './useInput';

    function App() {
      const [soundProps, resetSound] = useInput("");
      const [colorProps, resetColor] = useInput("#000000");

      const submit = (e) => {
        e.preventDefault();
        alert(`${soundProps.value} sounds like ${colorProps.value}`);
        resetSound();
        resetColor();
      };

      return (
        <form onSubmit={submit}>
          <input {...soundProps} type = "text" placeholder="Sound..." />
          <input {...colorProps} type = "color" />
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
    - Note 1: instead of using `useState`, we will use the customized hook, `useInput`, to get additional properties for our input fields and to get a function to reset the elements to their initial values:
      ```
      const [soundProps, resetSound] = useInput("");
      const [colorProps, resetColor] = useInput("#000000");
      ```
    - Note 2: the `value` and `onChange` properties for the text (i.e. "sound") and color input form fields have been abstracted away and into the `useInput` hook. Also, we use the `{...}` spread operator to add the `value` and `onchange` properties from the array that is returned by `useInput`:
      ```
      <input {...soundProps} type = "text" placeholder="Sound..." />
      <input {...colorProps} type = "color" />
      ```
    - Note 3: the `submit()` function has to be refactored as well:
      ```
      const submit = (e) => {
        e.preventDefault();
        alert(`${soundProps.value} sounds like ${colorProps.value}`);
        resetSound();
        resetColor();
      };
      ```
- __Video 2: Placing Data in Context__
