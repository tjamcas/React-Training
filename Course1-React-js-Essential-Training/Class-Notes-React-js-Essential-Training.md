# Course 1: React.js Essential Training
## Class Notes

### Section 1: what is React

- To add React Developer Tools extension to Chrome:
    - <https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi?hl=en>
- To add React Developer Tools extension to FIrefox:
    - <https://addons.mozilla.org/en-US/firefox/addon/react-devtools/>
- React learning (docs and tutorials) resource:
    - <https://reactjs.org/>
- Shortcut to open React Developer Tools in Chrome:
    - Mac: `Command+Option+J`
- Installing Node and NPM on your Mac:
    - Node.js® is a JavaScript runtime built on Chrome's V8 JavaScript engine.
    - node is a framework that can run JavaScript code on your machine while npm is a package manager. Using npm we can install and remove javascript packages also known as node modules.
    - go to <https://nodejs.org/en/> for latest version download


### Section 2: Intro to React Elements
- To create and run a package on your computer, run the following in Terminal:
    - `npx create-react-app appName`
    - `npx` is the package runner
    - `create-react-app` is the tool to create and build the project
- Once the package is built, you can:
    - `cd appname` to go to the directory where you created the package named `appName`
    - `npm start` to run the package
        - this will open `localhost:3000` in the Chrome browser
        - this is just a local web server in your browser that runs the package files
- Alternatively, you can run the package in `React-CodeSandbox`:
    - enter `react.new` in the Chrome browser
    - Your browser will go to the site: <https://codesandbox.io/s/new?utm_source=dotnew> where you can experiment with new code
- `create-react-app` builds the following file structure:
    - `appName`
        - `package.json`
            -  `"dependencies"` provides details of React version, testing libraries, and `react-scripts`
            -  `react-scripts` sets up the tooling
        -  `appName/src`
            -  contains all the code files that we write for an app
        -  `appName/public`
            -  contains assets like an index.html and logos files that will be used to build the app
