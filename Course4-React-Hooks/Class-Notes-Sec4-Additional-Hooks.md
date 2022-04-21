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
