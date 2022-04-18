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
       `node_modules`
      - This will prevent Git from adding the Node modules, which are very large, to the Git repository. We can install these files from the hosting services server by issuing the command in the server CLI to `run npm install`.
    - /3. In the terminal window, type the command:
      `git add .`
      - This will identify all the files in `my-blog-backend` (except the node_modules directory) to be committed the Git repo
      - `git status` will reurn a scroll of all the files that have been flagged for commit
    - /4. To actually commit (not just flag/identify) files, type:    
      `git commit -m "First commit"` where `"First commit"` is the commit message text
    - /5. Go to your GitHub account and create a new repository -- name it `my-blog` -- to where we will push our local files.    
      Copy GitHub commands for "pushing an exisiting repository from the command line:    
      ```
      git remote add origin https://github.com/tjamcas/my-blog.git
      git branch -M main
      git push -u origin main
      ```
      - This will require a GitHub personal access token - go to this link for instructions on setting up a PAT: <https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token>
- __Video 3: Creating and SSHing into an AWS Server__
  - First, we need to create an Amazon Web Services EC2 virtual server:
    - Log into the AWS Management Console
    - Under "All Services" search and click on "EC2"
      - An Amazon EC2 instance is a virtual server in Amazon's Elastic Compute Cloud (EC2) for running applications on the Amazon Web Services (AWS) infrastructure.
    - On the resulting screen - you will see a "Resources" box -  click on "Launch instance"
    - On the resulting "Launch an instance" screen:
      - Create a name for your server - e.g., "My blog server"
      - Scroll down to the "Key pair (login)" box and click on "Create new key pair"
        - Input a "Key pair name" - e.g., "my-blog-key" -- select RSA type and .pem private key file format, and click on "create key pair"
      - In "Summary" box on the right side of the window, click on "Launch instance"
  - Second, we configure our local laptop to be able to SSH into the AWS EC2 server:
    - Open a terminal window and go to home directory:
      - Create a ".ssh" directory
        - Type "ls -al" to see if `.ssh` directory exists
        - If it doesn't exist, type `mkdir .ssh`
        - Move the .pem file to the `.ssh` directory:   
          `mv ~/Downloads/my-blog-key.pem ~/.ssh/my-blog-key.pem`   
          and type `ls -al .ssh` to confirm the file was moved
  - Third, 
    - Return to the AWS management console, go to "Instances", select "My Blog Server", and copy the "Public IPv4 DNS" address
    - Go to Terminal window and cange permission on the `.ssh/my-blog-server.pem` file:   
      `chmod 400 ~/.ssh/my-blog-server.pem`
    - From the terminal window log into the AWS server using the .pem file    
      `ssh -i ~/.ssh/my-blog-key.pem ec2-user@ec2-99-999-999-99.compute-1.amazonaws.com`    
      - where `ec2-99-999-999-99.compute-1.amazonaws.com` is the Public IPv4 DNS that you copied in the previous step
      - __NOTE:__ IF you stop and re-start the virtual server, then you will generate a NEW Public IPv4 DNS address
- __Video 4: Setting Up an AWS Instance__
  - /1. Install Git on our server instance:   
    In the terminal window where we are logged into the AWS server instance, type:    
    `sudo yum install git`
  - /2. Install NPM on the AWS server - refer to the AWS tutorial <https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/setting-up-node-on-ec2-instance.html>
    - /2a. Install node version manager (nvm):    
      `curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.34.0/install.sh | bash`
    - /2b. Activate nvm:    
      `. ~/.nvm/nvm.sh`
    - /2c. Install your version of node:    
      `nvm install 16.14.0`
    - /2d. Install the latest version of npm:   
      `npm install -g npm@latest`
  - /3. Install MongoDB on the AWS server - refer to the MongoDB tutorial <https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-amazon/>
    - /3a. Configure the package management system (yum), in the AWS terminal window:   
      `sudo nano /etc/yum.repos.d/mongodb-org-5.0.repo`   
      Edit file and copy in the following lines:    
      ```
      [mongodb-org-5.0]
      name=MongoDB Repository
      baseurl=https://repo.mongodb.org/yum/amazon/2/mongodb-org/5.0/x86_64/
      gpgcheck=1
      enabled=1
      gpgkey=https://www.mongodb.org/static/pgp/server-5.0.asc

      ```
    - /3b. Install the MongoDB packages:    
      `sudo yum install -y mongodb-org`
    - /3c. Start the MongoDB service:    
      `sudo service mongod start`
    - /3d. Open a MongoDB shell:    
      `mongo` (DEPRECATED) - instead use `mongosh`
    - /3e. Insert articles for the new database running on the AWS server.    
      In MongoDB shell, type:
      `use my-blog`   
      Followed by:    
      ```
      db.articles.insertMany([
          {
              name: 'learn-react',
              upvotes: 0,
              comments: [],
          },  {
              name: 'learn-node',
              upvotes: 0,
              comments: [],
          },  {
              name: 'my-thoughts-on-resumes',
              upvotes: 0,
              comments: [],
          },
      ])
      ```
- __Video 5: Running a Full Stack App on AWS__
  - /1. Clone the GitHub repo to AWS
    - /1a. Copy the link to your GitHub "my-blog" repository
    - /1b. In the AWS server terminal, type:   
      `git clone https://github.com/tjamcas/my-blog.git`
    - /1c. Install Node:
      Change to the `my-blog` directory:   
      `cd my-blog`
      `npm install`
  - /2. Install the npm package called forever to ensure that the server runs the "my-blog" app continuously:
    From the AWS terminal window, type:      
    `npm install -g forever`    
    Start the "my-blog" app with forever:   
    `forever start -c "npm start" .`    
    Confirm that the process is running by typing:    
    `forever list`
  - /3.  Since our app is running on port 8000 but we want to be able to access it at the default HTTP port which is 80, we're going to want to map port 80 on our AWS instance to port 8000 on our Node server:    
    `sudo iptables -t nat -A PREROUTING -p tcp --dport 80 -j REDIRECT --to-ports 8000`
  - /4. Go into AWS console, and ... 
    - 4a. select AWS EC2 instance for "my-blog" and in the Security tab, note the security group that it uses (e.g. "launch-wizard-1")
    - 4b. from sidebar, select "Network & Security > Security Groups", select the security group previously noted (e.g. "launch-wizard-1")
    - 4c. in bottom pane, select "Inbound rules" tab, and then click "Edit inbound rules" button
    - 4d. click on "Add rule" button, from "Type" dropdown select "HTTP", from "Source" dropdown select either "Anywhere" or "My IP"
    - 4e. From sidebar select "Instances", click on "My Blog Server", and copy "Public IPv4 DNS" address
  - /5 Paste the IP address into browser, and you should see a working "My Blog" webpage

### Conclusion: AWS Resource Teardown
#### Terminate the server
To avoid potential usage minutes and associated charges during trial period, terminate the server using the following steps:
1. In AWS console, click on "Instances" and select your server instance (in this case, "My Blog Server")
2. From "Instance State" dropdown, select "Terminate instance".   
___Note that the Amazon AWS free trial period lasts 12 months - Don't forget to cancel service___
