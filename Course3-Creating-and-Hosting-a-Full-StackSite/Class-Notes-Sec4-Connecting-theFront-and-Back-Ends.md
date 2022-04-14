# Course 3: React: Creating and Hosting a Full Stack Site
## Class Notes / Section 4

### Section 4: Connecting the Front and Back Ends
- __Video 1: The Fetch API__
  - In these exercises we are using the `fetch` API to make requests from our front end user interface.
  - `fetch` is an asynchronous function.
  - `fetch` syntax: `fetch(URL, {fetchOptions})`
    - The second argument is optional an optional object to specify details about the request we want to send
    - For example, we can specify that the  fetch use `{method: 'POST'}` - note that the defult method is 'GET'.
  - `fetch` is a built in function so it doesn't have to be imported.
  - However, it is not built int Internet Explorer, and to support IE you must install a polyfill (for a definition of polyfill, see <https://developer.mozilla.org/en-US/docs/Glossary/Polyfill>).
    - To install the `fetch` polyfill, 
      - /1. in a terminal, go to the front end directory and type:   
        `npm install --save whatwg-fetch`
      - /2. go to `index.js`, and add the following `import` statement on the first line:   
        `import 'whatwg-fetch';`
- __Video 2: Adding React Hooks / Video 3: Calling useEffect at the Right Time / Video 4: Adding Fetch to Pages__
  - In this section we will add functionality so that when we go to a specific article page, we query the server and display all comments about the article we navigated to.
  - This requires us to edit the `/my-blog/src/pages/ArticlePage.js` and add state to the component using the React `useState` and `useEffect` hooks.
  - Here is the modified code for `ArticlePage.js`:
    ```
    import React, { useState, useEffect } from "react";
    import { useParams } from "react-router-dom";
    import ArticlesList from "../components/ArticlesList";
    import articleContent from "./article-content";
    import NotFoundPage from "./NotFoundPage";

    const ArticlePage = ( ) => {
        const { name } = useParams();
        const article = articleContent.find(article => article.name === name);

        const [articleInfo, setArticleInfo] = useState({ upvotes: 0, comments: [] });

        useEffect(() => {
            const fetchData = async () => {
                const result = await fetch(`/api/articles/${name}`);
                const body = await result.json();
                setArticleInfo(body);
            }
            fetchData();
        }, [name]);

        if (!article) return <NotFoundPage />

        const otherArticles = articleContent.filter(article => article.name !== name);

        return(
            <>
                <h1>{article.title}</h1>
                <p>This article post has been up voted {articleInfo.upvotes} times</p>
                {article.content.map((paragraph,key) => (
                    <p key={key}>{paragraph}</p>
                ))}
                <h3>Other Articles:</h3>
                <ArticlesList articles={otherArticles} />
            </>
        );
    }

    export default ArticlePage;
    ```
  - Note 1: Import the React hooks for `useState` and `useEffect`:    
    `import React, { useState, useEffect } from "react";`
  - Note 2: Add a state variable for the `ArticlePage` component (NOTE: this has to be positioned inside the `ArticlePage` component function or else you will receive an error), with the initial state of `articleInfo` set to upvotes equal to 0, and comments as an empty array:   
    `const [articleInfo, setArticleInfo] = useState({ upvotes: 0, comments: [0] });`
  - Note 3: We use the `useEffect` function to make requests to the back end server to load our `ArticleInfo` state variable. The `useEffect` hook launches the side effects of our components, such as, fetching data and setting the state with the results. This requires a number of edits.
    - First, we create a paragraph element that displays the number of up votes:    
      `<p>This article post has been up voted {articleInfo.upvotes} times</p>`
    - Second, we call the `useEffect` function ___inside___ the `ArticlePage` component function:
    ```
    useEffect(() => {
        const fetchData = async () => {
            const result = await fetch(`http://localhost:8000/api/articles/${name}`);
            const body = await result.json();
            setArticleInfo(body);
        };
        fetchData();
    }, [name]);   
    ```
      - Note 3a: The first argument of `useEffect` is an anonymous function to launch the side effects code. The second argument of `useEffect` is an array of values that useEffect should watch, and if one of them changes, useEffect should be called again -- here we are watching the `name` variable which will change when a user selects a new article name to view.
      - Note 3b: the `useEffect` hook does not allow its anonymous callback function to be asynchronous and use the `async` key word, so we devise a workaround where we create the aysnchronous function `fetchData` inside the callback function
  - Note 4: We need to make a modification to the `package.json` on the `my-blog` client side and add a property called `proxy`. For the value of this `proxy`, put the address that our server is running at. Here, we're going to use `"proxy": "http://localhost/8000/",`. When we write the URL of requests to our server, for example when we use fetch, we can leave off the `http://localhost/8000` string that we wrote for the proxy property. In other words, instead of writing ``fetch(`http://localhost:8000/api/articles/${name}`)`` we can simply write ``fetch(`/api/articles/${name}`)`` and the request will be automatically proxied/prefixed to the address that we defined in the package dot json file. Here is a relevant snippet from `package.json`:
    ```
    {
      "name": "my-blog",
      "version": "0.1.0",
      "private": true,
      "proxy": "http://localhost:8000/",
      ...
    }
    ```
- __WARNING:__ be sure to use the `npm start` command has been issued in a termnal window from __BOTH__ the client directory, `my-blog`, and the server directory, `my-blog-backend`. __Failure to do so will result in errors!__ In my case I received a JSON error about an unexpected token: `VM270:1 Uncaught (in promise) SyntaxError: Unexpected token P in JSON at position 0`. I also received a proxy error: `Proxy error: Could not proxy request /api/articles/learn-node from localhost:3000 to http://localhost:8000/.`
  - From the `my-blog-backend` directory, run `npm start` and expect to see:
    ```
    Tims-MacBook-Pro:my-blog-backend tim$ npm start

    > my-blog-backend@1.0.0 start
    > npx nodemon --exec babel-node src/server.js

    [nodemon] 2.0.15
    [nodemon] to restart at any time, enter `rs`
    [nodemon] watching path(s): *.*
    [nodemon] watching extensions: js,mjs,json
    [nodemon] starting `babel-node src/server.js`
    Listening on port 8000
    ```
  - From the `my-blog` directory, run `npm start` and expect to see:
    ```
    Compiled successfully!

    You can now view my-blog in the browser.

      Local:            http://localhost:3000
      On Your Network:  http://10.0.0.14:3000

    Note that the development build is not optimized.
    To create a production build, use npm run build.

    asset static/js/bundle.js 1.73 MiB [emitted] (name: main) 1 related asset
    asset index.html 1.67 KiB [emitted]
    asset asset-manifest.json 190 bytes [emitted]
    cached modules 1.6 MiB (javascript) 28.3 KiB (runtime) [cached] 128 modules
    webpack 5.70.0 compiled successfully in 3330 ms
    
    ```
