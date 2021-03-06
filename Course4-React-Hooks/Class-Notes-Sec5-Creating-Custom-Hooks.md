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
- __Video 2: Placing Data in Context / Video 3: Retrieving Data with useContext__
  - In apps where there are many nested components, and in which some or all of the components share data, it is difficult to share data by passing `props` between nested components. Instead we can use `createContext` to put shared data in context that is accessible by all child components.
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
      - from the React Documentation at <https://reactjs.org/docs/hooks-reference.html#usecontext>, useContext accepts a context object (the value returned from React.createContext) and returns the current context value for that context. The current context value is determined by the `value` prop of the nearest `<MyContext.Provider>` above the calling component in the tree.
    - Note 7: we can iterate through the `trees` array to create list items:
      ```
      <h1>Trees in the Forest</h1>
      {trees.map((tree)=> (
        <li key={tree.id}>{tree.type}</li>
        ))}
      ```
- __Video 4: Creating a Custom Hook with Context__
  - We can make it even simpler for children components to access shared context by creting a custom hook.
  - The full code for `index.js` follows:
    ```
    import React, { createContext, useContext } from 'react';
    import ReactDOM from 'react-dom';
    import './index.css';
    import App from './App';

    const TreesContext = createContext();
    export const useTrees = () =>
      useContext(TreesContext);

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
    - Note 1: We import `useContext` into `index.js`.
    - Note 2: we will not export the context `TreesContext`, and instead we will create and export a custom hook named `useTrees`:
      ```
      const TreesContext = createContext();
      export const useTrees = () =>
        useContext(TreesContext);
      ```
  - The refactored code for `app.js` follows:
    ```
    import './App.css';
    import { useTrees } from './';`
 
    function App() {
      const { trees } = useTrees();
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
    - Note 3: we import the custom hook `useTrees`:   
      `import { useTrees } from './';`
    - Note 4: calling `useTrees()` is more concise and readable than calling `useContext(TreesContext)` as we did before refactoring:    
      `const { trees } = useTrees();`
- __Video 5: Data Fetching with a Fetch Hook__
  - In this video, we fetch data with a custom hook
  - The custom hook must handle three possible states when fetching data: 1. Loading data, 2. Fetched data successfully, 3. Error in fetching data
  - We create a the custom hook in the file `src/useFetch.js` - here is the entire file
    ```
    // This custom hook handles the three possible states when attempting to fetch data:
    // 1. Loading data, 2. Fetched data successfully, 3. Error in fetching data

    import { useState, useEffect} from "react";

    export function useFetch(uri) {
        const [loading, setLoading] = useState(true);
        const [data, setData] = useState();
        const [error, setError] = useState();

        useEffect(() => {
            if (!uri) return;
            fetch(uri)
            .then(data => data.json())
            .then((res) => setData(res))
            .then(()=>setLoading(false))
            .catch(setError);
        }, [uri]);
        return { loading, data, error };
    }
    ```
    - Note 1: we export the custom hook: `export function useFetch(uri) { // hook function logic }`
    - Note 2: within the `usefech` function, we create state variables for the three possible fetching states/conditions:
      ```
      const [loading, setLoading] = useState(true);
      const [data, setData] = useState();
      const [error, setError] = useState();
      ```
    - Note 3: We use the `useEffect` hook to fetch the data and handle changes in the state variables:
      ```
      useEffect(() => {
          if (!uri) return;
          fetch(uri)
          .then(data => data.json())
          .then((res) => setData(res))
          .then(()=>setLoading(false))
          .catch(setError);
      }, [uri]);
      ```
      - Note 3a: we could have use the abbreviated expression `.then(setData)`, rather than the expanded expression `.then((res) => setData(res))` used above
      - Note 3b: for the dependency array (i.e., the second argument in the `useEffect` hook), we specify the `uri` variable. We want the useEffect callback function to execute on the first rendering and when there is a change in the `uri` that is specified.
    - Note 4: the custom hook returns the three state variables:    
      `return { loading, data, error };`
- __Video 6: Building a Fetch Component__
  - In this demo, we create the `<App>` component in the `src/index.js` file. Here is full `index.js` file:
    ```
    import React from 'react';
    import ReactDOM from 'react-dom';
    import './index.css';
    import { useFetch } from './useFetch';

    function App({ login }) {
      const { loading, data, error } = useFetch(`https://api.github.com/users/${login}`);
      if (loading) return (<h1>Loading...</h1>);
      if (error) return (<pre>{JSON.stringify(error, null, 2)}</pre>);
      return (
        <div>
          <pre>{JSON.stringify(data, null, 2)}</pre>
        </div>
      );
    }

    ReactDOM.render(
      <App login="github-username" />,
      document.getElementById("root")
    );
    ```
    - Note 1: This code will return the github user record for `login="github-username"`, where you can substitute a user name for github-username
