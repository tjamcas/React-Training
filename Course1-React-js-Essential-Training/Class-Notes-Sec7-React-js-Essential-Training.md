# Course 1: React.js Essential Training
## Class Notes / Section 7

### Section 7: React Router

- Installing React Router
  - To install React Router: `npm install react-router@next react-router-dom@next
  - We also need to install React History: `npm install history`
  - React creates single page applications.
    -  Instead of creating different files for different pages, React creates a single page with JavaScript loading information to change the UI.
    -  ___Routing___ is used to navigate from page to page. React Router is the tool used to perform this navigation.
  -  We create a file `pages.js`that has functions containing the different views of our application.
  -  `pages.js`:
    ```
    import React from "react";

    export function Home(){
        return(
            <div>
                <h1>[Company Website]</h1>
            </div>
        );
    }

    export function About(){
        return(
            <div>
                <h1>[About]</h1>
            </div>
        );
    }

    export function Events(){
        return(
            <div>
                <h1>[Events]</h1>
            </div>
        );
    }

    export function Contact(){
        return(
            <div>
                <h1>[Contact]</h1>
            </div>
        );
    }
    ```
- Configuring the router
  - The React Router will reside in the `index.js` file so that we can pass Router information to the nested components
  - In `index.js`, we wrap our `<App>` component in the `<Router>` to give the App access to all the properties of the Router - like location and history.
  - `index.js`:
    ```
    import React from "react";
    import ReactDOM from "react-dom";
    import "./index.css";
    import App from "./App";
    import { BrowserRouter as Router } from "react-router-dom";

    ReactDOM.render(
      <Router>
        <App />
      </Router>, 
      document.getElementById("root")
    );
    ```
  - Next, in `App.js`, we're going to import `home`, `about`, `events` and `contact` from our `pages.js` file in order to make the App responsible for rendering these.
    - Inside the `App` function, we create a `<Routes> ... </Routes>` component that wraps around a `<Route> ... </Route>` for each of the URL paths that we want to display as part of our UI
    - So, when the user types in the url path `https://.../appName/about`, the `App` function will return the `<About />` element, because we have defined a route, `<Route path = "/about" element = {<About />} />`
      ```
      import React from "react";
      import './App.css';
      import { Routes, Route } from "react-router-dom";
      import { Home, About, Events, Contact } from "./pages";
      
      function App() {
        return (
          <div>
            <Routes>
              <Route path = "/" element = {<Home />} />
              <Route path = "/about" element = {<About />} />
              <Route path = "/events" element = {<Events />} />
              <Route path = "/contact" element = {<Contact />} />
            </Routes>
          </div>
        ); 
      }
      
      export default App;
      ```
