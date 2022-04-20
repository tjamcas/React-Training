# Course 4: React HOOKS
## Class Notes / Section 2

### Section 2: The useState Hook
- __Video 4: Working with Component Trees__
  - Rather than create a separate `index.js` file as our application entry point and a separate `App.js` file that functions as our parent component to the application, we will merge the two files and define an `App` component function in `index.js`. We do this so that we can see the logic for the component tree more readily in one file. In common practice, you would maintain two separate files for `index.js` and `app.js`.
  - Here is basic JavaScript/JSX logic to display stars in the file `/src/index.js` - note the comments in the code:
  ```
  import React from 'react';
  import ReactDOM from 'react-dom';
  import './index.css';
  import { FaStar } from "react-icons/fa";

  /**
   * createArray is a helper function that takes an integer input, named length, and creates 
   * an array of length
  */

  const createArray = (length) => [
    ...Array(length)
  ];

  // Star is the child component to StarRating and simply returns a single star icon
  function Star() {
    return <FaStar />
  }

  /**
   * StarRating is the child component to App
   * StarRating is the parent to the Star component
   * StarRating receives the property totalStars from App
   */
  function StarRating({totalStars}) {
    return createArray(totalStars).map((n, i) => (
      <Star key={i} />
    ));
  }

  /**
   * App is the parent component
   * Note 1A: App creates a property named totalStars that is equal to the JS expression, the integer 10
   */
  function App() {
    return <StarRating totalStars = {10} />
  }

  ReactDOM.render(
    <React.StrictMode>
      <App />
    </React.StrictMode>,
    document.getElementById("root")
  );
  ```
  - We modify the code in the following snippet such that:    
    - the `App` parent component function does not have to specify a `totalStars` property, and
    - the `StarRating` child component creates a default value of five for the `totalStars = 5` property that it will automatically use of its parent component does not pass the property.
      ```
      /**
       * StarRating is the child component to App
       * StarRating is the parent to the Star component
       * StarRating specifies a default value of five for the totalStars property
       */
      function StarRating({totalStars = 5}) {
        return createArray(totalStars).map((n, i) => (
          <Star key={i} />
        ));
      }

      /**
       * App is the parent component
       * Note 1A: App does NOT pass the totalStars property
       */
      function App() {
        return <StarRating />
      }
      
      // Five (5) stars are displayed in the App
      ```
  - Similarly, we can modify the child `Star` component so that a default color of the stars is specified - note that the `selected` property is not passed by the parent `StarRating` component:
    ```
    // Star is the child component to StarRating and simply returns a single star icon
    function Star({ selected = false }) {
      return <FaStar color={selected ? "red" : "gray"} />
    }

    /**
     * StarRating is the child component to App
     * StarRating is the parent to the Star component
     * StarRating receives the property totalStars from App or it declares a default value for totalStars
     */
    function StarRating({totalStars = 5}) {
      return createArray(totalStars).map((n, i) => (
        <Star key={i} />
      ));
    }
    ```
    
