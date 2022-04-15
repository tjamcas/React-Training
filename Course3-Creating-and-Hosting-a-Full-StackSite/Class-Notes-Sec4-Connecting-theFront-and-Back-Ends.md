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
- __Video 5: Displaying Comments__
  - In this video, we want to take an article's comments (with their usernames), which have been stored in our MongoDB database, and and display them in the requested `ArticlePage` component.
  - This will require us to:
    - Create a new component `/my-blog/src/components/CommentsList.js` which will be the child component
    - Modify the `/my-blog/src/pages/ArticlePage.js` component which will call the `CommentsList` component and pass a `comments` property.
    - Recall that the `my-blog` database has a collection called `articles`. Each article in the `articles` collection has an object with a`name` string, `upvotes` integer, and a `comments` array. The comments array consists of zero or more objects with each object having a `username` and `text` value.
  - Here is the new `CommentsList` component:
    ```
    import React from "react";

    const CommentsList = ({ comments }) => (
        <>
        <h3>Comments:</h3>
        {comments.map((comment, key) => (
            <div className = "comments" key = {key}>
                <h4>{comment.username}</h4>
                <p>{comment.text}</p>
            </div>
        ))}
        </>
    );

    export default CommentsList;
    ```
    - Note 1: As a reminder, in JavaScript, parentheses are used instead of curly brackets after an arrow function to return an object.
    - Note 2: the `CommentsList` component/function receives the `comments` parameter from the parent component - in this case the parent is `ArticlePage`.
    - Note 3: the `comment` property value is extracted from the MongoDb database and saved to a variable in the parent `ArticlePage`. `comment` is an array of objects, with each object containing a `username` and `text` value.
      - `comment.username` and `comment.text` refer to those key-value pairs
  - Here is the modified `ArticlePage` component:
    ```
    import React, { useState, useEffect } from "react";
    import { useParams } from "react-router-dom";
    import ArticlesList from "../components/ArticlesList";
    import CommentsList from "../components/CommentsList";
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
                console.log(body);
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
                <CommentsList comments={articleInfo.comments} />
                <h3>Other Articles:</h3>
                <ArticlesList articles={otherArticles} />
            </>
        );
    }

    export default ArticlePage;
    ```
    - Note 1: We import the `CommentsList` child component:   
      `import CommentsList from "../components/CommentsList";`
    - Note 2: the state variable, ` articleInfo`, was created with the `useState` hook and will receive the article upvotes and comments from the database based on the name of the article requested:    
      `const [articleInfo, setArticleInfo] = useState({ upvotes: 0, comments: [] });`
- __Video 6: Adding an Upvote Button__
  - In this video we add a component for an upvote button. From the `ArticlePage` component, a user can click the button, and the upvote is tallied and stored in the database by the server.
  - In the new component, we will have a function that makes a post request using the fetch function to access the article information in the MongoDB database for the article that is being upvoted.
  - Here is the new `/my-blog/src/components/UpvotesSection.js` component file:
    ```
    import React from "react";

    const UpvotesSection = ({ articleName, upvotes, setArticleInfo }) => {
        const upvoteArticle = async () => {
            const result = await fetch(`/api/articles/${articleName}/upvote`, {
                method: 'post',
            });
            const body = await result.json();
            setArticleInfo(body);
        };

        return (
            <div id="upvotes-section">
                <button onClick={() => upvoteArticle()}>Add Upvote</button>
                <p>This article post has been up voted {upvotes} times</p>
            </div>
        );
    }

    export default UpvotesSection;
    ```
    - Note 1: in the component function we return JSX containing paragraph text and a button with an onclick function that kicks off the server fetch and increment code:
      ```
      return (
          <div id="upvotes-section">
              <button onClick={() => upvoteArticle()}>Add Upvote</button>
              <p>This article post has been up voted {articleInfo.upvotes} times</p>
          </div>
      );
      ```
    - Note 2: Separately in the component code, we define an async function, `upvoteArticle`, with the fetching logic:
      ```
      const upvoteArticle = async () => {
          const result = await fetch(`/api/articles/${articleName}/upvote`, {
              method: 'post',
          });
          const body = await result.json();
          setArticleInfo(body);
      };
      ```
      - Note 2a: Recall that `/my-blog-backend/src/server.js` contains a route pattern that looks responds to a user request, increments an article's upvotes, and returns the updated article information:   
        `app.post('/api/articles/:name/upvote', (req, res) => { ///... increment upvote count logic ... });`
    - Note 3: Notice that the component function references the `articleName` and `upvotes` variables as well as the `setArticleInfo` function. None of these are defined in the component, so this is a tip-off that they must be passed to the `UpvotesSection` child component as properties:   
      `const UpvotesSection = ({ articleName, upvotes, setArticleInfo }) => { ... };`
  - Next, we need to add the new `UpvotesSection` child component to the `ArticlePage` parent component.
  - Here is the updated `/my-blog/src/pages/ArticlePage.js` component file:
    ```
    import React, { useState, useEffect } from "react";
    import { useParams } from "react-router-dom";
    import ArticlesList from "../components/ArticlesList";
    import CommentsList from "../components/CommentsList";
    import UpvotesSection from "../components/UpvotesSection";
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
                console.log(body);
                setArticleInfo(body);
            }
            fetchData();
        }, [name]);

        if (!article) return <NotFoundPage />

        const otherArticles = articleContent.filter(article => article.name !== name);

        return(
            <>
                <h1>{article.title}</h1>
                <UpvotesSection articleName={name} upvotes={articleInfo.upvotes} setArticleInfo={setArticleInfo} />
                {article.content.map((paragraph,key) => (
                    <p key={key}>{paragraph}</p>
                ))}
                <CommentsList comments={articleInfo.comments} />
                <h3>Other Articles:</h3>
                <ArticlesList articles={otherArticles} />
            </>
        );
    }

    export default ArticlePage;
    ```
    - Note 1: we import the `UpvotesSection` component:   
      `import UpvotesSection from "../components/UpvotesSection";`
    - Note 2: we add the `UpvotesSection` component beneath the article title and pass the idenitifed properties from above:    
      ```
      <h1>{article.title}</h1>
      <UpvotesSection articleName={name} upvotes={articleInfo.upvotes} setArticleInfo={setArticleInfo} />
      ```
- __Video 7-9 Adding an Add Comment Form__
  - In these videos, we create a new child component file, '/my-blog/src/component/AddCommentForm.js', for an add comments form. This component is a child (i.e., will reside in) the `ArticlePage` component.
  - The `AddCommentForm` returns JSX code with input fields for user name and comment text with a button that triggers logic to access the MongoDB database through the web server and append the user input to the a specific article's information.
  - Here is the new '/my-blog/src/component/AddCommentForm.js' component file:
    ```
    import React, { useState } from "react";

    const AddCommentForm = ({ articleName, setArticleInfo }) => {
        const [username, setUsername] = useState('');
        const [commentText, setCommentText] = useState('');

        const addComment = async () => {
            const result = await fetch(`/api/articles/${articleName}/add-comment`,{
                method: 'post',
                body: JSON.stringify({username, text: commentText}),
                headers: {
                    'Content-Type': 'application/json',
                }
            });
            const body = await result.json();
            setArticleInfo(body);
            setUsername('');
            setCommentText('');
        }

        return (
            <div id = "add-comment-form">
                <h3>Add a Comment</h3>
                <label>
                    Name:
                    <input type="text" value={username} onChange={(event)=>setUsername(event.target.value)} />
                </label>
                <label>
                    Comment:
                    <textarea rows = "4" columns = "50" value={commentText} onChange={(event)=>setCommentText(event.target.value)} />
                </label>
                <button onClick={()=>addComment()}>Add Comment</button>
            </div>
        );
    };

    export default AddCommentForm;
    ```
    - Note 1: Recall that `/my-blog-backend/server.js` has a route pattern for adding a user name and their comment text to an article's database record:    
      `app.post('/api/articles/:name/add-comment', (req, res) => { //... Logic to accept and append user name and comment text to an article ...}
    - Note 2: Also recall that the `my-blog` database contains a collection named `articles` that looks like this:
      ```
      my-blog> db.articles.find({})
      [
        {
          _id: ObjectId("62543d9f9aba7919f60ada83"),
          name: 'learn-react',
          upvotes: 5,
          comments: [
            { username: 'Yo', text: 'Yeah!' },
            { username: 'Me', text: 'Like it!' }
          ]
        },
        {
          _id: ObjectId("62543d9f9aba7919f60ada84"),
          name: 'learn-node',
          upvotes: 11,
          comments: []
        },
        {
          _id: ObjectId("62543d9f9aba7919f60ada85"),
          name: 'my-thoughts-on-resumes',
          upvotes: 2,
          comments: []
        }
      ]
      ```
    - Note 3: We return JSX to lay out the user interface form that has a heading, two labeled areas to add text for name and comments, and a submit button. The submit button calls the `addComment` function when there is an `onclick` event:
      ```
      return (
          <div id = "add-comment-form">
              <h3>Add a Comment</h3>
              <label>
                  Name:
                  <input type="text" />
              </label>
              <label>
                  Comment:
                  <textarea rows = "4" columns = "50" />
              </label>
              <button onClick={()=>addComment()}>Add Comment</button>
          </div>
      );
      ```
    - Note 4: We import the `useState` hook and create two state variables: `username` and `commentText`:
      ```
      const [username, setUsername] = useState('');
      const [commentText, setCommentText] = useState('');
      ```
    - Note 5: We link the state variables to their input fields by adding the value properties coupled with an `onChange` event to the `<input>` and `<textarea>` elements:
      ```
      <label>
          Name:
          <input type="text" value={username} onChange={(event)=>setUsername(event.target.value)} />
      </label>
      <label>
          Comment:
          <textarea rows = "4" columns = "50" value={commentText} onChange={(event)=>setCommentText(event.target.value)} />
      </label>
      ```
    - Note 6: We create an `addComment` async function that is triggered when the user clicks on the "add Comment" button and that fetches the current article information with a POST request to the server MongoDB database and appends the username and comment text:
      ```
      const addComment = async () => {
          const result = await fetch(`/api/articles/${articleName}/add-comment`,{
              method: 'post',
              body: JSON.stringify({username, text: commentText}),
              headers: {
                  'Content-Type': 'application/json',
              }
          });
          const body = await result.json();
          setArticleInfo(body);
          setUsername('');
          setCommentText('');
      }
      ```
      - Note 6a: The variable `result` holds the response from the server from a `fetch` request. In the lengthy `fetch` request we have to attach an options object that 1. specifies the POST method (recall that the default is GET), 2. attaches the body, and 3. specifies in the header that the data is being sent in JSON format:
        ```
        const result = await fetch(`/api/articles/${articleName}/add-comment`,{
            method: 'post',
            body: JSON.stringify({username, text: commentText}),
            headers: {
                'Content-Type': 'application/json',
            }
        });
        ```
      - Note 6b: `result` contains the server response which is the updated article information with the added username and comment text. We will store the result in JSON format into the `body` variable, and then use the state variable setter, `setArticleInfo`, to update `articleInfo` variable with the updated information from the MongoDB - including the just-added comment:
        ```
        const body = await result.json();
        setArticleInfo(body);
        ```
      - Note 6c: We clear the form fields use the state variable setters:   
        ```
        setUsername('');
        setCommentText('');
        ```
      - Note 6d: Lest we forget, the `addComment` function is triggered by an `onClick` event in the JSX code for the "add COmment" button:   
        `<button onClick={()=>addComment()}>Add Comment</button>`
    - Note 7: Finally, we add the variable (`articleName`) and function (`setArticleInfo`) that are referenced in, but not defined in, the `AddCommentForm` component as passed properties:   
      `const AddCommentForm = ({ articleName, setArticleInfo }) => { ...}`    
  - Here is the updated `/my-blog/src/pages/ArticlePage.js` component file:
    ```
    import React, { useState, useEffect } from "react";
    import { useParams } from "react-router-dom";
    import ArticlesList from "../components/ArticlesList";
    import CommentsList from "../components/CommentsList";
    import UpvotesSection from "../components/UpvotesSection";
    import AddCommentForm from "../components/AddCommentForm";
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
                console.log(body);
                setArticleInfo(body);
            }
            fetchData();
        }, [name]);

        if (!article) return <NotFoundPage />

        const otherArticles = articleContent.filter(article => article.name !== name);

        return(
            <>
                <h1>{article.title}</h1>
                <UpvotesSection articleName={name} upvotes={articleInfo.upvotes} setArticleInfo={setArticleInfo} />
                {article.content.map((paragraph,key) => (
                    <p key={key}>{paragraph}</p>
                ))}
                <CommentsList comments={articleInfo.comments} />
                <AddCommentForm articleName={name} setArticleInfo={setArticleInfo} />
                <h3>Other Articles:</h3>
                <ArticlesList articles={otherArticles} />
            </>
        );
    }

    export default ArticlePage;
    ```
    - Note 1: More specifically, we import (`import AddCommentForm from "../components/AddCommentForm";`) and then add the `AddCommentForm` child component to the parent `ArticlePage` component just below the article content and right above the "Other Articles" header:    
      '<AddCommentForm articleName={name} setArticleInfo={setArticleInfo} />'
