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
