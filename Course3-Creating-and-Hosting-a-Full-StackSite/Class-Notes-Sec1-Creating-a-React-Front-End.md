# Course 3: React: Creating a React Front End
## Class Notes / Section 1

### Section 1: Setting-Up-a-React-Project
#### Note:
The Section 1 videos reference an older version of React for setting up the front end webpages. As a result the `Router` and `Route` syntax and structure is out of date, and while the instructor code will compile, it will not display the web pages. The appropriate front end code should look like the following, corrected files after completing Section 1, Video 5 "Using React Router Links" ...
- `/my-blog/src/index.js`:
  ```
  import React from 'react';
  import ReactDOM from 'react-dom';
  import './index.css';
  import App from './App';
  import { BrowserRouter as Router } from "react-router-dom";

  ReactDOM.render(
    <Router>
      <App />
    </Router>,
    document.getElementById('root')
  );
  ```
- `/my-blog/src/App.js`:
  ```
  import React from 'react';
  import { Routes, Route } from "react-router-dom";
  import HomePage from './pages/HomePage';
  import AboutPage from './pages/AboutPage';
  import ArticlesList from './pages/ArticlesList';
  import ArticlePage from './pages/ArticlePage';
  import NavBar from "./NavBar";
  import './App.css';

  function App() {

    return (
      <div className="App">
        <NavBar />
        <div id="page-body">
          <Routes>
            <Route path="/" element={<HomePage />} exact />
            <Route path="/about" element={<AboutPage />} />
            <Route path="/articles-list" element={<ArticlesList />} />
            <Route path="/article" element={<ArticlePage />} />
          </Routes>
        </div>
      </div>
    ); 
  }

  export default App;
  ```
- `/my-blog/src/NavBar.js`:
  ```
  import { React } from "react";
  import { Link } from "react-router-dom";

  const NavBar = () => (
      <nav>
          <ul>
              <li>
                  <Link to="/">Home</Link>
              </li>
              <li>
                  <Link to="/about">About</Link>
              </li>
              <li>
                  <Link to="/articles-list">Articles</Link>
              </li>
          </ul>
      </nav>
  );

  export default NavBar;
  ```
- `/my-blog/pages/HomePage.js`:
  ```
  import { React } from "react";

  const HomePage = () => (
      <>
          <h1>Hello, welcome to my blog!</h1>
          <p>
              Welcome to my blog! Proin congue
              ligula id risus posuere, vel eleifend ex egestas. Sed in turpis leo.
              Aliquam malesuada in massa tincidunt egestas. Nam consectetur varius turpis,
              non porta arcu porttitor non. In tincidunt vulputate nulla quis egestas. Ut
              eleifend ut ipsum non fringilla. Praesent imperdiet nulla nec est luctus, at
              sodales purus euismod.
          </p>
          <p>
              Donec vel mauris lectus. Etiam nec lectus urna. Sed sodales ultrices dapibus.
              Nam blandit tristique risus, eget accumsan nisl interdum eu. Aenean ac accumsan
              nisi. Nunc vel pulvinar diam. Nam eleifend egestas viverra. Donec finibus lectus
              sed lorem ultricies, eget ornare leo luctus. Morbi vehicula, nulla eu tempor
              interdum, nibh elit congue tellus, ac vulputate urna lorem nec nisi. Morbi id
              consequat quam. Vivamus accumsan dui in facilisis aliquet.,
          </p>
          <p>
              Etiam nec lectus urna. Sed sodales ultrices dapibus.
              Nam blandit tristique risus, eget accumsan nisl interdum eu. Aenean ac accumsan
              nisi. Nunc vel pulvinar diam. Nam eleifend egestas viverra. Donec finibus lectus
              sed lorem ultricies, eget ornare leo luctus. Morbi vehicula, nulla eu tempor
              interdum, nibh elit congue tellus, ac vulputate urna lorem nec nisi. Morbi id
              consequat quam. Vivamus accumsan dui in facilisis aliquet.,
          </p>
      </>
  )

  export default HomePage
  ```
- `/my-blog/pages/AboutPage.js`:
  ```
  import { React } from "react";

  const AboutPage = () => (
      <>
          <h1>About me ...</h1>
          <p>
              Welcome to my blog! Proin congue
              ligula id risus posuere, vel eleifend ex egestas. Sed in turpis leo.
              Aliquam malesuada in massa tincidunt egestas. Nam consectetur varius turpis,
              non porta arcu porttitor non. In tincidunt vulputate nulla quis egestas. Ut
              eleifend ut ipsum non fringilla. Praesent imperdiet nulla nec est luctus, at
              sodales purus euismod.
          </p>
          <p>
              Donec vel mauris lectus. Etiam nec lectus urna. Sed sodales ultrices dapibus.
              Nam blandit tristique risus, eget accumsan nisl interdum eu. Aenean ac accumsan
              nisi. Nunc vel pulvinar diam. Nam eleifend egestas viverra. Donec finibus lectus
              sed lorem ultricies, eget ornare leo luctus. Morbi vehicula, nulla eu tempor
              interdum, nibh elit congue tellus, ac vulputate urna lorem nec nisi. Morbi id
              consequat quam. Vivamus accumsan dui in facilisis aliquet.,
          </p>
          <p>
              Etiam nec lectus urna. Sed sodales ultrices dapibus.
              Nam blandit tristique risus, eget accumsan nisl interdum eu. Aenean ac accumsan
              nisi. Nunc vel pulvinar diam. Nam eleifend egestas viverra. Donec finibus lectus
              sed lorem ultricies, eget ornare leo luctus. Morbi vehicula, nulla eu tempor
              interdum, nibh elit congue tellus, ac vulputate urna lorem nec nisi. Morbi id
              consequat quam. Vivamus accumsan dui in facilisis aliquet.,
          </p>
      </>
  )

  export default AboutPage
  ```
- `/my-blog/pages/ArticlesList.js`:
  ```
  import { React } from "react";

  const ArticlesList = () => (
      <>
          <h1>Articles</h1>
      </>
  )

  export default ArticlesList
  ```
- `/my-blog/pages/ArticlePage.js`:
  ```
  import { React } from "react";

  const ArticlePage = () => (
      <>
          <h1>This is an article ...</h1>
      </>
  )

  export default ArticlePage
  ```
- __URL Parameters with react-router__
  - In this section, we will modify the `ArticlePage` component
    - We want to structure our application so that each article has its own URL. The desired behavior is to display blog articles so that if we navigate to `/article/article-name`, it will display that specific article. For example, `/article/learn-react` should route the user to an article about learning React.
    - We can accomplish this with URL parameters.
    - In `/src/App.js`, we define a route to an article/page, and we append a ':' and a variable name to the route:    
      `<Route path="/article/:name" element={<ArticlePage />} />`
      - The URL parameter is `:name`
      - When react-router sees a URL parameter in the `Route path`, it passes a `prop` with the value of the parameter/variable to the component named in the route. In this case, the `name` prameter/variable and its string value get passed to the component `<ArticlePage />`.
    - In `/pages/ArticlePage.js`, the useParams hook returns an object of key/value pairs of the dynamic params from the current URL that were matched by the `Route path`. We will find `name` inside the params object, and can extract it and assign it to a local variable. For more info on the `useParams` hook, see <https://reactrouter.com/docs/en/v6/api#useparams> 
      - We use destructuring syntax to extract `name` from the params object:   
      `const { name } = useParams();`
      - We reference `name` in our JSX:
        `<h1>This is the {name} article ...</h1>`
  - Here is the modified `/src/App.js` component:
    ```
    
    ```
  -Here is the modified `/pages/ArticlePage.js` component:
    ```

    ```
