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
