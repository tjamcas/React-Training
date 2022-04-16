# Course 3: React: Creating and Hosting a Full Stack Site
## Class Notes / Addendum

### Starting the my-blog application - DEVELOPMENT with separate client and backend servers
While in __DEVELOPMENT__,
1. In a terminal window, go to `/my-blog` directory and start the client side: `npm start`
2. In a separate terminal window, from the default directory, start the MongoDB service: `brew services start mongodb-community@5.0`. You may want to rename your terminal window to `Mongo` for ease of use.
3. In the `Mongo` terminal window, open the MongoDB shell: `mongosh`.
4. In a separate terminal window, go to `/my-blog-backend` directory and start the web server: `npm start`

### Shutting down the my-blog application - DEVELOPMENT with separate client and backend servers
While in __DEVELOPMENT__,
1. In the `node my-blog` terminal window (assuming that we are using VisualCode's terminals and window names), and press`<ctl>-C`
2. In the `node my-blog-backend` terminal window (assuming that we are using VisualCode's terminals and window names), and press`<ctl>-C`
3. In the `Mongo` terminal window (assuming that we used VisualCode functionality to rename the terminal window), 
  a. press`<ctl>-C`
  b. type `brew services stop mongodb-community@5.0`

### Starting the my-blog application - PRODUCTION environment after build
While in __PRODUCTION__,
1. In a separate terminal window, from the default directory, start the MongoDB service: `brew services start mongodb-community@5.0`. You may want to rename your terminal window to `Mongo` for ease of use.
2. In the `Mongo` terminal window, open the MongoDB shell: `mongosh`.
3. In a separate terminal window, go to `/my-blog-backend` directory and start the web server: `npm start`

### Shutting down the my-blog application - PRODUCTION environment after build
While in __PRODUCTION__,
1. In the `node my-blog-backend` terminal window (assuming that we are using VisualCode's terminals and window names), and press`<ctl>-C`
2. In the `Mongo` terminal window (assuming that we used VisualCode functionality to rename the terminal window), 
  a. press`<ctl>-C`
  b. type `brew services stop mongodb-community@5.0`
