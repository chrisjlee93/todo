## Getting Started
You should have the project open in intellij and the app running in a browser.
Put both windows side by side on your screen. You can use `[ctrl + option + left/right arrow]` if you're using Rectangle
As you read through this page, open each file and directory in intellij and follow along.
## File Structure
You'll find files highlighted in a few different colors depending on what theme you use in intelliJ. For the defualt intelliJ dark theme, test files and directories will be green, production will be blue or while, and ignored/generated directories will be orange. If these colors do not match what you see, try to figure out how to identify each category based on your theme. 
When you do a `bootRun`, the code actually being executed can be found in the `./build` directory
Look through each file in the root directory. It's okay if you don't know what every file is doing. `build.gradle` and `docker-compose.yml` are two of the important ones.
### `docker-compose.yml` 

- This where we configure external dependencies that our app needs. In this case, we just have 1 container configured - which runs an instance of postgreSQL that our app connects to at startup. You can start and stop the container in your terminal: 
- start: `docker-compose up -d` 
- stop: `docker-compose down` 

## Frontend  

### `./frontend`  
This is where your frontend (Typescript/React) code lives.  
Look through each file in the root here, the most important one for now is `package.json`. This is similar to `build.gradle`, but for the frontend. You'll find some configuration details, scripts, dependencies, and devDependencies.  
### `./package.json`  

#### scripts  
These are commands you can execute in terminal (when you're in the frontend directory) similar to gradle tasks.  
`yarn start` - this allows you to run just the frontend which can be accessed on port 3000

`yarn test` - this runs all tests in your frontend code  

#### dependencies
These are the external libraries that our frontend uses. `react`, `react-router-dom` and `@mui/material` are some of the important ones to note  
You can add other dependencies from the terminal using yarn: `yarn add <package name>`  

#### devDependencies
These are more dependencies that we need for our frontend, but they are only needed for development. These dependencies should not be referenced in production code as these dependencies will not be included in the built version of our application. You'll find things like `typescript`, `vite`, and several testing libraries here.  
You can add other devDependencies from the terminal using yarn: `yarn add -D <package name>`  

### `./index.html`
 This is the single html file in our single page web application. React is a javascript framework that enables building single page applications. That means that there is only one html file sent to the browser, and we use javascript to manipulate the html and css rendered on that page. This is that file.

### `./src/index.tsx`
This is where we connect React to that single html file. notice: document.getElementById(\'root\')  
If you go back to `./index.html`, you'll find:  
`<div id="root"></div>`  

React is injecting itself into your html file at this div. This is the start of your React app and is the top node of your hierarchy. You'll see this hierarchy represented by `tsx` tags like this `<App />`. All the other `.tsx` files in the repo are referenced as sub-components of this one.  
You can hold `cmd` and click on the `<App />` (or click on the text App and press `cmd + B`) to navigate to where the code for App is defined (`./src/App.tsx`)  
From here you'll find the hierarchy continues, and you can navigate into `<HomePage/>` and the other Route Elements which each correspond to a page you'll see in your browser.  
Notice the path in `<Route path=\'page\' />`  corresponds to the url in your browser.  

## Backend

### `build.gradle`
This is where we configure and define the backend of our application. Some things you'll find here
java dependencies
the version and name of the app
tasks - these tasks are essentially functions that can be executed from the terminal to do all kinds of things. You'll find a few tasks defined like `buildFrontend` `copyFrontend`. These tasks build our frontend code and copy it into the build directory of the backend code so that our backend can actually serve our frontend code also. This allows us to do a `bootRun` and access both the frontend and backend on port 8080 in your browser. This is known as a Monorepo. There are pros and cons to deploying an app as a mono-repo vs deploying your frontend and backend separately.

### `./src`
This is where your backend (java) code lives. You'll find `./src/main` and `./src/test` directories which nearly mirror one another. `test` is where we write tests against our production code. `main` is where we write our production code that you see when you run the app.

### `./src/main/resources/application.yaml`
This is the configuration file for our backend. You can configure all kinds of things in this file like the port the app runs on, the connection details for the database or other services, feature flags, logging settings, authentication details. This list goes on, but an important thing about this file is that we can override the values in this file using environment variables which means we can one value set in the file when running locally and when we deploy the app, we can have different values set in production.

### `./src/main/resources/db/migration`
This directory contains database migrations. These are a series of sql commands that are executed against your database at runtime. These migrations are managed by Flyway which is a dependency you'll see in build.gradle.
Flyway ensures that each migration is only run once.
Migrations allow us to incremementally modify the database schema in a way that can be shared across multiple environments (development, staging, production)

### `./src/main/java/.../[APP_NAME]Application.java`
This is the starting point for our java application. This is the first code that gets executed when running the application. It doesn't look like much is going on here, but notice the annotation `@SpringBootApplication`. Spring does all kinds of things behind the scenes for us like making the connection to the database, and doing a component scan which you'll learn more about later.

### `./src/test`
Look through the test files here. You should see green play buttons next to some of the test methods/classes. This allows you to run the tests in intellij.
You can also use the `./gradlew test` command in terminal to run all tests in the project through the terminal.
