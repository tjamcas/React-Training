# Course 3: React: Creating and Hosting a Full Stack Site
## Class Notes / Section 3

### Section 3: Setting Up MongoDB
- __Video 2: Installing MongoDB__
  - /1. From a terminal window, run the command to install Homebrew that is given at <brew.sh>:   
    `/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"`
  - /2. From the same terminal window, use homebrew to install MongoDB:
    - Instructions for using Hommebrew to install the latest version of MongoDB for a Mac can be found at:   
      <https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-os-x/>
      - `brew tap mongodb/brew`
      - `brew install mongodb-community@5.0`
  - /3. To create a default directory with the appropriate permissions for MongoDB files:
    - The following instructor directions did __NOT__ work
      - `sudo mkdir -p /data/db`
      - `sudo chown -R `id -un` /data/db`
    - The following instructions from <https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-os-x/> worked
      - To run MongoDB (i.e. the mongod process) as a macOS service, run:   
        `brew services start mongodb-community@5.0`
      - To verify that MongoDB is running, perform (if you started MongoDB as a macOS service):   
        `brew services list`
      - To begin using MongoDB, connect mongosh to the running instance. From a new terminal, issue the following:    
        `mongosh`
      - To stop a mongod running as a macOS service, use the following command as needed:   
        `brew services stop mongodb-community@5.0`
    - If you're having a database (`/data/db`) not found issue, reference this video: <https://www.youtube.com/watch?v=AIGabiOixzc>
  - /4. To create a new database:   
      `use my-blog`
  - /5. To create a new article:
    - In MongoDB, each database is composed of a few or many _collections_. These collections can contain any number of JSON objects called _documents_
    - In the example below, we create a collection called 'articles' -- so, `articles` is a collection (i.e., array) of documents (i.e., JSON objects)
      ```
      db.articles.insert([{
      name: 'learn-react',
      upvotes: 0,
      comments: [],
      }, {
      name: 'learn-node',
      upvotes: 0,
      comments: [],
      }, {
      name: 'my-thoughts-on-resumes',
      upvotes: 0,
      comments: [],
      }])
      ```
    - __The `db.articles.insert()` command is deprecated -- use `db.articles.insertMany()` (or `insertOne` or `bulkWrite`) instead__
  - /6. To see what is in our articles collection:    
    `db.articles.find({})`
    - To find specific articles insert your criteria between the curly braces, for example:   
      `db.articles.find({name: 'learn-react'})`
    - Or to find a single article:    
       `db.articles.findOne({name: 'learn-react'})`
- __Video 3: Adding MongoDB to Express__
  - To install MongoDB package into our Node Express server project, go into your server directory (in this tutorial, that would be 'my-blog-backend`):    
    `npm install --save mongodb`
  - We can now use this installed MongoDB driver to connect to and manipulate our database. As a result, we no longer need and can delete the 'temporary database' `articlesInfo` object variable that is in `/my-blog-backend/src/server.js`. Instead, we will manipulate the `articles` database. This database will also be persistent and we won't lose our edits when we start and stop the server.
  - We need to import the MongoDB tools and driver we installed into the project:   
    `import { MongoClient } from "mongodb";`
  - We need to create a new `GET` route for when a user requests a specific article -- we will:
    - /1. use the route parameters to get the specific name of the article:    
      `app.get('/api/articles/:name', (req, res) => { ... }`
    - /2. We will connect to our database with the `MongoClient.connect` command which also returns a client object that we can use to query the database
      ```
      const mongoClient = new MongoClient('mongodb://localhost/27017');
      mongoClient.connect(async (error, client) => {
      ...
      }
      ```
    - /3. To be able to query the database we issue the command:    
      `const db = client.db('my-blog');`
    - /4. To query the database for the article that the user requested, we write this command:   
      `const articleInfo = await db.collection(`articles`).findOne({ name: articleName })`
    - /5. To return the article that was requested to the user:   
      `res.status(200).json(articleInfo);`
    - /6. Remember to always close the connection to the database:    
      `client.close();`
    - Helpful link: <https://1sherlynn.medium.com/how-to-handle-errors-in-asynchronous-javascript-code-when-working-with-callbacks-dcd32bca4b6b>. Discussion of error-first callback, which is a common pattern used in Javascript, especially in NodeJS. Also see this explanation: <https://stackoverflow.com/questions/31375728/node-js-callback-function-error-parameter-explanation>.
    - Helpful link: <https://mongodb.github.io/node-mongodb-native/3.0/tutorials/connect/> for tutorial on MongoDb version 3.0 of the Node.js driver documentation. See also <https://www.mongodb.com/docs/drivers/node/current/> for version 4.5 of MongoDB Node.js driver.
  - Here is the full, working code for `/my-blog-backend/src/server.js`:
    ```
    import express from "express";
    import bodyParser from "body-parser";
    import { MongoClient } from "mongodb";

    const app = express();

    const articlesInfo = {
        'learn-react': {
            upvotes: 0,
            comments: []
        },
        'learn-node': {
            upvotes: 0,
            comments: []
        },
        'my-thoughts-on-resumes': {
            upvotes: 0,
            comments: []
        },
    }

    app.use(bodyParser.json());

    app.get('/api/articles/:name', (req, res) => {
        try {
            const articleName = req.params.name;
            const mongoClient = new MongoClient('mongodb://localhost/27017');
            mongoClient.connect(async (error, client) => {
                const db = client.db('my-blog');
                const articleInfo = await db.collection(`articles`).findOne({ name: articleName });
                res.status(200).json(articleInfo);
                client.close();
            })
        } 
        catch (error) {
            res.status(500).json({message: 'Error connecting to database', error});
        }    
    })

    app.post('/api/articles/:name/upvote', (req, res) => {
        const articleName = req.params.name;
        articlesInfo[articleName].upvotes += 1;
        res.status(200).send(`${articleName} now has ${articlesInfo[articleName].upvotes} votes!`);
    });

    app.post('/api/articles/:name/add-comment', (req, res) => {
        const { username, text } = req.body;
        const articleName = req.params.name;
        articlesInfo[articleName].comments.push({username,text});
        res.status(200).send(articlesInfo[articleName]);
    });

    app.listen(8000, () => console.log('Listening on port 8000'));
    ```
- __Video 3: Rewriting the upvote endpoint__
  - In a similar way to the get article endpoint above, we need to refactor the upvote endpoint to access and manipulate the persistent MongoDB database that we have created.
    - We will use the URL parameter to receive the article name that the user wants to upvote.
    - We will use the URL parameter to select the database record for the article that the user specifies, and then increase the upvote property by 1.
    - Finally, we will send the user a status and the database record for the article.
  - The upvote POST route, looks similar to the GET article route pattern with the following additional `updateOne` query to increase the number of upvotes:
    ```
    await db.collection('articles').updateOne( {name: articleName}, {
        '$set': {
            upvotes: articleInfo.upvotes + 1,
        },
    } )
    ```
  - After updating the article upvote count, then we send a repsonse to the user with a `200` status along with the full, modified article:
    ```
    const updatedArticleInfo = await db.collection(`articles`).findOne({ name: articleName });
    res.status(200).json(updatedArticleInfo);
    ```
- The updated upvote route pattern in the `/my-blog-backend/src/server.js` file is as follows:
  ```
  app.post('/api/articles/:name/upvote', (req, res) => {
      try {
          const articleName = req.params.name;
          const mongoClient = new MongoClient('mongodb://localhost/27017');
          mongoClient.connect(async (error, client) => {
              const db = client.db('my-blog');
              const articleInfo = await db.collection(`articles`).findOne({ name: articleName });
              await db.collection('articles').updateOne( {name: articleName}, {
                  '$set': {
                      upvotes: articleInfo.upvotes + 1,
                  },
              } )
              const updatedArticleInfo = await db.collection(`articles`).findOne({ name: articleName });
              res.status(200).json(updatedArticleInfo);
              client.close();
          });
      } 
      catch (error) {
          res.status(500).json({message: 'Error connecting to database', error});
      }    
  });
  ```
