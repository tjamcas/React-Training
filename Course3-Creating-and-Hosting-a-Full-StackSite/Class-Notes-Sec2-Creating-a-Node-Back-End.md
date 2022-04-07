# Course 3: React: Creating and Hosting a Full Stack Site
## Class Notes / Section 2

### Section 2: Setting-Up-a-React-Projectideo 1
- __Video 1: Why Node.js__
  - Node.js allows the backend to be coded in JavaScript rather than PHP or some other language
  - We are using the Express NPM package to code the server
- __Video 2: Setting up an Express Server__
  - 1. Make sure to be in the server code directory: `cd ../my-blog-backend`
  - 2. Initialize folder to be an NPM package: `npm init -y`
  - 3. Install Express into project: `npm install --save express`
  - 4. Create `/my-blog-backend/src/server.js` subdirectory and file
  - 5. Node.js does not support JavaScript ES6 syntax, but there are changes we can manually make to allow use of ES6:
    - 5a. Install Babel transpiler which interprets ES6:    
      `npm install --save-dev @babel/core @babel/node @babel/preset-env`
    - 5b. Create and configure `/my-blog-backend/.babelrc` file where we tell Babel how we want it to transform the ES6 code that we write into common JS code that Node.js can execute:
      ```
      {
          "presets": ["@babel/preset-env"]
      }
      ```
