# Course 2: React.js: Building an Interface
## Class Notes / Section 4

### Section 3: Mutating Data
- useEffect and useCallback Hooks
  - The following code simulates fetching data from an external source. 
  - In the past examples, `./src/data.json` has resided in the `src` folder, but our data will more likely come fro an external API or will the results of a SQL query.
  - To simulate data received from an external source, we will move `data.jsn` to the `/public` folder and use `fetch` function to retrieve it
    - Note 1: files in the public folder will be included in an application build
    - Note 2: in our source code we can reference files that reside in the `/public` folder as if they resided in the same `/src` folder as our source code -- i.e., we will reference `/public/data.json` as `./data.json`
  - Side Effects: Data fetching, setting up a subscription, and manually changing the DOM in React components are all examples of side effects. In React class components, the render method itself shouldn’t cause side effects. It would be too early — we typically want to perform our effects after React has updated the DOM. from <https://reactjs.org/docs/hooks-effect.html>
  - Here is our revised `App.js` code:
    ```
    
    ```
    - Note 1: we need to import `useState`, `useEffect`, and `useCallback` from the React library
    - Note 2: We are no longer going to import the `data.json` file, and so we remove the line:    
      `import appointmentList from "./data.json"`
    - Note 3: We create a state variable named  `appointmentList` and initialize it as an empty array:   
      `let [appointmentList, setAppointmentList] = useState([]);`
    - Note 4: We retrieve our data by creating a function, `fetchData()`, that uses the `useCallback` hook. After we have retrieved the data, `useCallback` will monitor any changes that happen to that data.
      ```
      const fetchData = useCallback(
        () => {
          fetch('./data.json')
            .then(response => response.json())
            .then(data => {
              setAppointmentList(data)
            })
        }, []
      )
      ```
      - The `fetch` is a promise that says our data will be retrieved the server (in this case, from our file directory). 
      - The first `.then` says that we will receive a `response` from the server and we'll use an arrow function to take that response from the server and convert it from text to JSON.
      - The second `.then` is another promise in which we create a temporary variable, `data`, where all the data is received and we pass it into the function `setAppointmentList`
      - The empty array `[]` is where we could specify variables that we want to track
