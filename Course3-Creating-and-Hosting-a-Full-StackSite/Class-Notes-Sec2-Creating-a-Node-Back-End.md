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
