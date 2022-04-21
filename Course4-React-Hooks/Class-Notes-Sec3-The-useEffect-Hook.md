# Course 4: React Hooks
## Class Notes / Section 3

### Section 3: The useEffect Hook
- __Video 4: Introducing useEffect__
  - Side Effect - formal definition: in computer science, an operation, function or expression is said to have a side effect if it modifies some state variable value(s) outside its local environment, that is to say has an observable effect besides returning a value (the primary effect) to the invoker of the operation. Example side effects include:
    - modifying a non-local variable, 
    - modifying a static local variable, 
    - modifying a mutable argument passed by reference, 
    - performing I/O or calling other functions with side-effects.    
In the presence of side effects, a program's behaviour may depend on history; that is, the order of evaluation matters. Understanding and debugging a function with side effects requires knowledge about the context and its possible histories. From Wikipedia "Side effect" article: <https://en.wikipedia.org/wiki/Side_effect_%28computer_science%29>.
  - In the following example we will code a component with a button that changes paragraph text in the component when the button is clicked. It will also have a side effect where the DOM title defined in the head section (outside of the component) also changes.
  - Here is the code with the `useEffect` function:
    ```
    import React, { useState, useEffect } from 'react';
    import ReactDOM from 'react-dom';
    import './index.css';

    /**
     * App is the parent component
     * Note 1A: App creates a property named totalStars that is equal to the JS expression, the integer 10
     */
    function App() {
      const [name, setName] = useState("James");
      useEffect(() => {
        document.title = `Celebrate ${name}`;
        });

      return (
        <section>
          <p>Congratulations {name}!</p>
          <button onClick={() => setName("Tim")}>Change Winner</button>
        </section>
      );
    }

    ReactDOM.render(
      <React.StrictMode>
        <App />
      </React.StrictMode>,
      document.getElementById("root")
    );
    ```
    - Note 1: we use `useState` to create a state variable, `name`, within the app component:   
      `const [name, setName] = useState("James");`
    - Note 2: we use `useEffect` to create a side effect that changes the document `<title>` element in the `<head>` section (outside of the `App` component) when the state variable, `name`, changes within the `App` component:    
      ```
      useEffect(() => {
        document.title = `Celebrate ${name}`;
        });
      ```
