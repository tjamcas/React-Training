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
- __Video 6: URL Parameters with react-router__
  - In this section, we will modify the `ArticlePage` component
    - We want to structure our application so that each article can be called by its own URL. The desired behavior is to display blog articles so that if we navigate to `/article/article-name`, it will display that specific article. For example, `/article/learn-react` should route the user to an article about learning React.
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
    - We also have placed all of our blog articles in a single `.js` file that is an array of articles in object form, and we want to use the objects in the array to create an article page.
    - Each article in the array is an object with three (3) key-value pairs   
      - 1. key: "name" / Value: String    
      - 2. Key: "title" / Value: String   
      - 3. Key: "content" / Value: an array of string, with each string representing a paragraph in the blog article
    - The blog articles reside in the file `/src/pages/article-content.js` and have the form:
      ```
      const articles = [
          {
              name: 'learn-react',
              title: 'The Fastest Way to Learn React',
              content: [
                  `Welcome! Today we're going to be talking about the fastest way to
                  learn React. We'll be discussing some topics such as proin congue
                  ligula ... Praesent imperdiet nulla nec est luctus, at 
                  sodales purus euismod.`,
                  `Donec vel mauris lectus. Etiam ...ac vulputate urna lorem nec nisi. Morbi id 
                  consequat quam. Vivamus accumsan dui in facilisis aliquet.`,
                  `Etiam nec lectus urna ... Vivamus accumsan dui in facilisis aliquet.`,
              ]
          },    {
              name: 'learn-node',
              title: 'How to Build a Node Server in 10 Minutes',
              content: [
                  `In this article, we're going to be talking looking at a very quick way
                  to set up a Node.js server. We'll be discussing some topics such as proin congue
                  ... sodales purus euismod.`,
                  `Donec vel mauris lectus. Etiam nec lectus urna. Sed sodales ultrices dapibus. 
                  ... consequat quam. Vivamus accumsan dui in facilisis aliquet.`,
                  `Etiam nec lectus urna. Sed sodales ultrices dapibus. 
                  Nam blandit tristique risus, eget accumsan nisl interdum eu. Aenean ac accumsan 
                  ... consequat quam. Vivamus accumsan dui in facilisis aliquet.`,
              ]
          },     {
              name: 'my-thoughts-on-resumes',
              title: 'My Thoughts on Resumes',
              content: [
                  `Today is the day I talk about something which scares most people: resumes.
                  In reality, I'm not sure why people have such a hard time with proin congue
                  ... eleifend ut ipsum non fringilla. Praesent imperdiet nulla nec est luctus, at 
                  sodales purus euismod.`,
                  `Donec vel mauris lectus. Etiam nec lectus urna. Sed sodales ultrices dapibus. 
                  ... consequat quam. Vivamus accumsan dui in facilisis aliquet.`,
                  `Etiam nec lectus urna. Sed sodales ultrices dapibus. 
                  ... consequat quam. Vivamus accumsan dui in facilisis aliquet.`,
              ]
          },  
      ];

      export default articles;
      ```
    - In `/src/pages/ArticlePage.js`, we use the `name` variable that we have extracted from the user requested URL, to find the correct article:   
      `const article = articleContent.find(article => article.name === name);`
    - Remember: the `article` object has three key-value pairs. One of them, `content` is an array of strings. Each string is a paragraph of the blog article. Therefore, `article.content` is an iterable that can use the `map` method.
    - Then, we display it's title and content to the appropriately named article page:
      ```
      return(
          <>
              <h1>{article.title}</h1>
              {article.content.map((paragraph,key) => (
                  <p key={key}>{paragraph}</p>
              ))}
          </>
      );
      ```
  - Here is the modified `/src/App.js` component:
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
              <Route path="/article/:name" element={<ArticlePage />} />
            </Routes>
          </div>
        </div>
      ); 
    }

    export default App;
    ```
  - Here is the modified `/pages/ArticlePage.js` component:
    ```
    import { React } from "react";
    import { useParams } from "react-router-dom";
    import articleContent from "./article-content";

    const ArticlePage = ( ) => {
        const { name } = useParams();
        const article = articleContent.find(article => article.name === name);
        if (!article) return <h1>Article does not exist!</h1>
        return(
            <>
                <h1>{article.title}</h1>
                {article.content.map((paragraph,key) => (
                    <p key={key}>{paragraph}</p>
                ))}
            </>
        );
    }

    export default ArticlePage
    ```
- __Video 7: Creating and Linking the Articles List__
  - In this video we edit the `ArticlesList` component to allow users to see a listing of all the blog articles and select one to open that blog entry.
  - Here is the revised `/src/pages/ArticlesList.js` component file:
    ```
    import { React } from "react";
    import { Link } from "react-router-dom";
    import articleContent from "./article-content";

    const ArticlesList = () => (
        <>
            <h1>Articles</h1>
            {articleContent.map((article, key) => (
                <Link className="article-list-item" key={key} to={`/article/${article.name}`}>
                    <h3>{article.title}</h3>
                    <p>{article.content[0].substring(0, 150)}...</p>
                </Link>))
            }
        </>
    );

    export default ArticlesList;
    ```
    - Note 1: there are two new imports:    
      ```
      import { Link } from "react-router-dom";
      import articleContent from "./article-content";
      ```
    - Note 2: recall that `articleContent` is an iterable array of article objects. We will map each article object into a JSX expression:    
      ```
      {articleContent.map((article, key) => (
          <Link className="article-list-item" key={key} to={`/article/${article.name}`}>
              <h3>{article.title}</h3>
              <p>{article.content[0].substring(0, 150)}...</p>
          </Link>))
      }
      ```
- __Video 7: Making the Articles List Modular__
  - We will refactor our code to make the articles list that we just created reusable.
    - This will require pulling out the articles list code into its own component
    - The existing `ArticlesList` component will be renamed to `ArticlesListPage`, and the new component will be named `ArticlesList`
    - We will have to edit all previous references to `ArticlesList` in `App.js`.
    - We will have to rename `ArticlesList.js` to `ArticlesListPage.js` and edit all previous references to `ArticlesList` in this file as well.
  - Here is the code for `/src/components/ArticlesList.js`:
    ```
    import React from "react";
    import { Link } from "react-router-dom";

    const ArticlesList = ({ articles }) => (
        <>
        {articles.map((article, key) => (
            <Link className="article-list-item" key={key} to={`/article/${article.name}`}>
                <h3>{article.title}</h3>
                <p>{article.content[0].substring(0, 150)}...</p>
            </Link>))
        }
        </>
    );

    export default ArticlesList;
    ```
    - Note 1: We copy the `{articleContent.map((article, key) => ...}` codeblock from `ArticlesListPage.js` and place it in our new component with some modification -- see next note.
    - Note 2: We add a prop named `articles` to the `ArticlesList` component rather than importing `article-content.js` -- this makes the component reusable and any parent component can call it with different arrays of articles.
  - Here is the code for `/src/pages/ArticlesListPage.js`:
    ```
    import { React } from "react";
    import articleContent from "./article-content";
    import ArticlesList from "../components/ArticlesList";

    const ArticlesListPage = () => (
        <>
            <h1>Articles</h1>
            <ArticlesList articles={articleContent} />
        </>
    );

    export default ArticlesListPage;
    ```
    - Note 1: We remove the `{articleContent.map((article, key) => ...}` codeblock and replace it with the new subcomponent `<ArticlesList />` subcomponent:    
      `<ArticlesList articles={articleContent} />`
      - Note we are passing an `articles` prop to the `<ArticlesList />` subcomponent
