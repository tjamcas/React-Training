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
  - In apps where there are many nested components, and in which some or all of the components share data, it is difficult to share data by passing `props` between neted components. Instead we can use `createContext` to put shared data in context that is accessible by all child components.
  - Here is the full code to use `createContext` to put an array of data into context in the file `src/index.js`:
    ```
    import React, { createContext } from 'react';
    import ReactDOM from 'react-dom';
    import './index.css';
    import App from './App';

    export const TreesContext = createContext();

    const trees = [
      {id: "1", type: "Maple"},
      {id: "2", type: "Oak"},
      {id: "3", type: "Birch"},
      {id: "4", type: "Alder"},
      {id: "5", type: "Red Pine"},
    ]


    ReactDOM.render(
      <TreesContext.Provider value={{ trees }}>
        <App />
      </TreesContext.Provider>,
      document.getElementById("root")
    );
    ```
    - Note 1: We import `createContext` and use it to create a context "container":   
      `export const TreesContext = createContext();`
    - Note 2: We create the context values, in a variable named `trees`, which is an array of objects:
      ```
      const trees = [
        {id: "1", type: "Maple"},
        {id: "2", type: "Oak"},
        {id: "3", type: "Birch"},
        {id: "4", type: "Alder"},
        {id: "5", type: "Red Pine"},
      ]
      ```
    - Note 3: We wrap the `<App>` component in the context provider, `TreesContext.Provider`, and we pass the data that we want accessible to the `<App>` component and all of its children components through the `value` property. In this case, we want to pass the `trees` array variable:
      ```
      ReactDOM.render(
        <TreesContext.Provider value={{ trees }}>
          <App />
        </TreesContext.Provider>,
        document.getElementById("root")
      );
      ```
  - Here is the full code for the component file, `src/app.js`:
    ```
    import './App.css';
    import { TreesContext } from './';
    import { useContext } from 'react';

    function App() {
      const { trees } = useContext(TreesContext);
      return (
        <div>
          <h1>Trees in the Forest</h1>
          {trees.map((tree)=> (
            <li key={tree.id}>{tree.type}</li>
            ))}
        </div>
      );
    }

    export default App;
    ```
    - Note 4: We import the `TreesContext`container variable from the `src/index.js` file:    
      `import { TreesContext } from './';`
    - Note 5: We import the `useContext` React hook:    
      `import { useContext } from 'react';`
    - Note 6: `useContext` returns an object from which we can deconstruct the `trees` array:   
      `const { trees } = useContext(TreesContext);`
    - Note 7: we can iterate through the `trees` array to create list items:
      ```
      <h1>Trees in the Forest</h1>
      {trees.map((tree)=> (
        <li key={tree.id}>{tree.type}</li>
        ))}
      ```
