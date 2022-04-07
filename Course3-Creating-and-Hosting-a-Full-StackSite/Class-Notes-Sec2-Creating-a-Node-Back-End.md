# Course 3: React: Creating and Hosting a Full Stack Site
## Class Notes / Section 2

### Section 2: Setting-Up-a-React-Projectideo 1
- __Video 1: Why Node.js__
  - Node.js allows the backend to be coded in JavaScript rather than PHP or some other language
  - We are using the Express NPM package to code the server
- __Video 2: Setting up an Express Server__
  - /1. Make sure to be in the server code directory: `cd ../my-blog-backend`
  - /2. Initialize folder to be an NPM package: `npm init -y`
  - /3. Install Express into project: `npm install --save express`
  - /4. Create `/my-blog-backend/src/server.js` subdirectory and file
  - /5. Node.js does not support JavaScript ES6 syntax, but there are changes we can manually make to allow use of ES6:
    - 5a. Install Babel transpiler which interprets ES6:    
      `npm install --save-dev @babel/core @babel/node @babel/preset-env`
    - 5b. Create and configure `/my-blog-backend/.babelrc` file where we tell Babel how we want it to transform the ES6 code that we write into common JS code that Node.js can execute:
      ```
      {
          "presets": ["@babel/preset-env"]
      }
      ```
  - /6. Create and edit the file `/my-blog-backend/src/server.js` - this will be the netry point for our server. Here is a simple file that responds to the user entered URL endpoint `/hello`:
    ```
    import express from 'express';

    const app = express();

    app.get('/hello', (req, res) => res.send('Hello!'));

    app.listen(8000, () => console.log('Listening on port 8000'));
    ```
    - Note 1: we create our backend app with the following line:    
      'const app = express();`
    - Note 2:  with the `app` object we can define different endpoints for our app, and how the app responds when an endpoint is requested.
      - Generic pattern: `app.get('endpointURL', (reg, res) => callbackFunction );
        - The first parameter within the callback function is the request object:
          - it is usually abbreviated as req, and contains details about the request that we received
        - The second parameter is the response object:
          - it is usually abbreviated as res, and is used to send a response back to whoever sent the request
      - Example: `app.get('/hello', (req, res) => res.send('Hello!'));`
    - Note 3: we start the server with `app.listen(8000, () => console.log('Listening on port 8000'));`
