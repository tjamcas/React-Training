# Course 4: React Hooks
## Class Notes / Section 3

### Section 3: The useEffect Hook
- __Video 1: Introducing useEffect__
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
- __Video 2: Working with the Dependency Array__
  - We can add a dependency array as a second argument to the `useEffect` function. The dependency array contains the names of the variables that we want to monitor for a change in value that will trigger the `useEffect` callback function. In other words, if the variable(s) listed in the dependency array change, then we want to fire off the side effect.
  - If you specify an empty array as your second argument, then the side effect fires off once on first render of the component where `useEffect` resides.
  - If you do not specify a second argument with your dependency array, then the side effect will fire off any time their is a change in the component where the `useEffect` function resides.
  - In the following code we created two state variables: `name` and `admin`. We coded two `useEffect` hooks. One `useEffect` fires off a side effect - a message to the console - when the `name` state variable changes. The second `useEffect` fires off a side effect - a different message to the console - when the `admin` state variable changes.
  - Here is the code in `/src/index.js`:
    ```
    );
      useEffect(() => {
        console.log(`The user is ${admin ? "admin" : "not admin"}.`);
       });

      return (
        <section>
          <p>Congratulations {name}!</p>
          <button onClick={() => setName("Tim")}>Change Winner</button>
          <p>{admin ? "logged in" : "not logged in"}</p>
          <button onClick={() => setAdmin(true)}>Log in</button>
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
