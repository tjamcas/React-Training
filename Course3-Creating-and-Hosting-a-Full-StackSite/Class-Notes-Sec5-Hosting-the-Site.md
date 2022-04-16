# Course 3: React: Creating and Hosting a Full Stack Site
## Class Notes / Section 5

### Section 5: Hosting the Site
- __Video 1: Preparing the App for Release__
  - We need to prepare our App for release so people can access it - we will publish the App using Amazon Web Services (AWS).
  - Final clean-up:
    - /1. Rename the title that shows up in the browser tab - go to `/my-blog/public/index.html` and edit the following line:   
      `<title>Tim's Blog - Class Project</title>`
    - /2. Go to `/my-blog/public/manifest.json` and edit the following lines:
      ```
      {
        "short_name": "Tim's Blog",
        "name": "Tim's Blog - Creating and Hosting a Full Stack Site - Class Project",
        //...
      }
      ```
    - The `my-blog` app is ready to build
  - Build:
    - /1. Create an optimized production build - to create the `/build` folder with the compiled React code, in a terminal window:   
      `cd my-blog`    
      `npm run build`
      - You should see a message similar to the following:
        ```
        The project was built assuming it is hosted at /.
        You can control this with the homepage field in your package.json.

        The build folder is ready to be deployed.
        You may serve it with a static server:

          npm install -g serve
          serve -s build

        Find out more about deployment here:

          https://cra.link/deployment
        ```
    - /2. Enable the back end server (`my-blog-backend`) to serve the client-side application (like the article information and the application components) to users:     
      - /a. Copy and paste the `/my-blog/build` folder into the `/my-blog-backend/src` sub-directory using finder or your IDE (e.g., VisualStudio) functionality
      - /b. Edit `/my-blog-backend/server.js`,    
        add line (right below line `const app = express();`):    
        `app.use(express.static(path.join(__dirname, '/build')));`    
        and also remember to import `path`:   
        `import path from "path";`
        finally, towards the bottom of the file, right below the last API route path, add a line for a "catch-all` GET route:   
        ```
        app.get('*', (req, res) => {
            res.sendFile(path.join(__dirname + '/build/index.html'));
        });
        ```
        - This logic handles requests that aren't caught by any of our other API routes, and passes them to our app
- Test your application is still working:
  - /1. In a separate terminal window, from the default directory, start the MongoDB service: `brew services start mongodb-community@5.0`. You may want to rename your terminal window to `Mongo` for ease of use.
  - /2. In the `Mongo` terminal window, open the MongoDB shell: `mongosh`.
  - /3. In a separate terminal window, go to `/my-blog-backend` directory and start the web server: `npm start`
