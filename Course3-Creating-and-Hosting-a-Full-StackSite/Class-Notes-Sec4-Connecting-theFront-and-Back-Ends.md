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
- __Video 2: Adding React Hooks__
  - In this section we will add functionality so that when we go to a specific article page, we query the server and display all comments about the article we navigated to.
  - This requires us to edit the `/my-blog/src/pages/ArticlePage.js` and add state to the component using the React `useState` and `useEffect` hooks.
  - Here is the modified code for `ArticlePage.js`:
    ```
    
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
        setArticleInfo({ upvotes: 3 });
    });   
    ```
      - Note 3a: The first argument of `useEffect` is an anonymous function to launch the side effects code. The second argument of `useEffect` is an array of values that useEffect should watch, and if one of them changes, useEffect should be called again.
