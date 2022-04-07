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
      - ___Generic pattern to handle a user `GET` request:___ `app.get('endpointURL', (reg, res) => callbackFunction );
        - The first parameter within the callback function is the request object:
          - it is usually abbreviated as req, and contains details about the request that we received
        - The second parameter is the response object:
          - it is usually abbreviated as res, and is used to send a response back to whoever sent the request
      - Example: `app.get('/hello', (req, res) => res.send('Hello!'));`
    - Note 3: we start the server with `app.listen(8000, () => console.log('Listening on port 8000'));`
  - /7. Finally, we issue the command for Node.js to run the Express server:    
    `npx babel-node src/server.js`
    - To see if the server set up has worked, enter the endpoint into your browser: `http://localhost:8000/hello`
      - You should see the 'Hello!' message in the browser window
- __Video 3: Testing an Express server with Postman__
  - Postman can be used to preform more complicated testing operations on the backend server, and to develop the backend server first (before developing and integrating the front end code).
  - Postman link: <https://web.postman.co/home>
  - In Postman we will send `GET` and `POST` requests to the `localhost:8000` server
    - `GET` requests get information from the server, and usually the only information they carry is contained inside the requested URL
      - ___Generic route pattern to handle a user `GET` request:___ `app.get('endpointURL', (reg, res) => callbackFunction );
        - The first parameter within the callback function is the request object:
          - it is usually abbreviated as req, and contains details about the request that we received
        - The second parameter is the response object:
          - it is usually abbreviated as res, and is used to send a response back to whoever sent the request
      - Example: `app.get('/hello', (req, res) => res.send('Hello!'));`
    - `POST` requests modify something on the server, and carry additionaly information in the request body
      - ___Generic route pattern to handle a user `POST` request (the route looks just like a `GET` request:___ `app.post('endpointURL', (reg, res) => callbackFunction );
        - The first parameter within the callback function is the request object:
          - it is usually abbreviated as req, and contains details about the request that we received
        - The second parameter is the response object:
          - it is usually abbreviated as res, and is used to send a response back to whoever sent the request
      - Example: `app.post('/hello', (req, res) => res.send('Hello!'));`
  - Sending data in the `POST` body request
    - In the following example, we will add a `body` object to a "POST" request. The `body` object will contain a `name` property. We will create a route in the `server.js` file that can prse the `body` object and use the value of the `name` property in the formulation of the server's response.
    - The `POST` request body must be configured in Postman to "raw" with a JSON object, for example:
      ```
      {
          "name": "Tim"
      }
      ```
    - We install the "body parser" NPM module that allows the server to extract the JSON data that is sent along with the endpoint request by using the follow terminal command:   
      `npm install --save body-parser`
    - `/src/server.js` is modified:
      ```
      import express from 'express';
      import bodyParser from 'body-parser';

      const app = express();

      app.use(bodyParser.json());

      app.get('/hello', (req, res) => res.send('Hello!'));
      app.post('/hello', (req, res) => res.send(`Hello ${req.body.name}!`));

      app.listen(8000, () => console.log('Listening on port 8000'));
      ```
      - Note 1: We need to add an `import` statement for body parser
      - Note 2: The following line adds a `body` property to the request and gives `app` the ability to use body-parser to interpret the JSON object contained in the `body` property/parameter:    
        `app.use(bodyParser.json());`
      - Note 3: To access the `name` property in our response, we code the following JavaScript literal:
        `app.post('/hello', (req, res) => res.send(`Hello ${req.body.name}!`));`
      - Note 4: after making these changes in the `server.js` code, we need to stop and re-start the Express server (`npx babel-node src/server.js')
    - With these changes, the server will respond to the endpoint, '/hello', with "Hello Tim!"
- __Video 4: Route parameters in Express__
  - We can also extract information from the user specified URL endpoint. This information is carried in a "route parameter", AKA "url parameter', that is part of the user-specified URL endpoint string.
  - Here is an example with a modified `server.js` that uses a route parameter within a GET request route:
    ```
    import express from "express";
    import bodyParser from "body-parser";

    const app = express();

    app.use(bodyParser.json());

    app.get('/hello', (req, res) => res.send('Hello'));
    app.get('/hello/:name', (req, res) => res.send(`Hello ${req.params.name}!`));
    app.post('/hello', (req, res) => res.send(`Hello ${req.body.name}!`));

    app.listen(8000, () => console.log('Listening on port 8000'));
    ```
    - Note 1: We add a new route with the `:name` route parameter in the following line:    
      `app.get('/hello/:name', (req, res) => res.send(`Hello ${req.params.name}!`));`
    - Note 2: we could have added the following route for a POST request and have gotten the same results:
      `app.post('/hello/:name', (req, res) => res.send(`Hello ${req.params.name}!`));`
- __Video 5: Upvoting articles__
  - In the following example, we will upvote blog-articles and keep track of the number of up votes each article receives.
    - Up votes will be registered at a URL endpoint of the form:    
      `http://localhost:8000/api/articles/:name/upvote`
      - Note that we are using a route parameter in our endpoint to specify the name of the blog article that we want to up vote
      - The use of `.../api/...` in the endpoint will become apparent/be explained in a later video
    - Normally, we would track the up votes in a persistent database but for this example we store vote tallies using a temporary variable that will reset when the server is re-started
      - The variable is a JSON object that contains objects for each article and its corresponding number of votes:
        ```
        const articlesInfo = {
            'learn-react': {
                upvotes: 0,
            },
            'learn-node': {
                upvotes: 0,
            },
            'my-thoughts-on-resumes': {
                upvotes: 0,
            },
        }
        ```
    - We create a new POST endpoint route with a callback funtion that increments an article's `upvotes` value. We access the correct article using a route parameter, `:name`:
      ```
      app.post('/api/articles/:name/upvote', (req, res) => {
          const articleName = req.params.name;
          articlesInfo[articleName].upvotes += 1;
          res.status(200).send(`${articleName} now has ${articlesInfo[articleName].upvotes} votes`);
      });
      ```
  - Here is the entire code in the modified `server.js` file:
    ```
    import express from "express";
    import bodyParser from "body-parser";

    const app = express();

    const articlesInfo = {
        'learn-react': {
            upvotes: 0,
        },
        'learn-node': {
            upvotes: 0,
        },
        'my-thoughts-on-resumes': {
            upvotes: 0,
        },
    }

    app.use(bodyParser.json());

    app.post('/api/articles/:name/upvote', (req, res) => {
        const articleName = req.params.name;
        articlesInfo[articleName].upvotes += 1;
        res.status(200).send(`${articleName} now has ${articlesInfo[articleName].upvotes} votes`);
    });

    app.listen(8000, () => console.log('Listening on port 8000'));
    ```
- __Video 6: Automatically updating with nodemon__
  - We can use the npm package, `nodemon`, to detect when a change has been made to `/src/server.js`, and automatically restart the server. This will save us from manually stopping (`<CTL>-C`) and restarting the server (`npx babel-node src/server.js`) in a terminal shell.
  - To install `nodemon`:   
    `npm install --save-dev nodemon`
  - To activate `nodemon` with `server.js`:
    `npx nodemon --exec babel-node src/server.js`
    - This command instructs `nodemon` to rerun the command after `--exec` -- i.e., `babel-node src/server.js` -- whenever there is a change in `server.js`
- We can specify a shortcut to our lengthy `npx nodemon --exec babel-node src/server.js` command in `package.json`:
  ```
  "scripts": {
    "start": "npx nodemon --exec babel-node src/server.js"
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  ```
- To start the server, we issue the terminal shell command:   
  `npm start`
