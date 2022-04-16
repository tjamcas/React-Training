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
- __Video 2: Pushing Code to GitHub__
  - In this video, we will move th `my-blog` app's local files to a hosting service server using GitHub.
  - This will be a multi-step process:
    - /1. Create an empty GitHub repository
    - /2. Push the `my-blog-backend` folder to our GitHub repository
    - /3. Clone the `my-blog-backend` GitHub repository onto the hosting service server
  - More specifically,
    - /1. Initialize the `my-blog-backend` folder as a GitHub repository:
       From a terminal window, change directories to `/my-blog-backend`    
       Issue the command: `git init`
    - /2. Create new file `my-blog-backend/.gitignore` with the following single line for content:   
       `node-modules`
      - This will prevent Git from adding the Node modules, which are very large, to the Git repository. We can install these files from the hosting services server by issuing the command in the server CLI to `run npm install`.
    - /3. In the terminal window, type the command:
      `git add .`
      - This will identify all the files in `my-blog-backend` (except the node-modules) to be committed the Git repo
      - `git status` will reurn a scroll of all the files that have been flagged for commit
    - /4. To actually commit (not just flag/identify) files, type:    
      `git commit -m "First commit"` where `"First commit"` is the commit message text
    - /5. Go to your GitHub account and create a new repository to where we will push our local files
