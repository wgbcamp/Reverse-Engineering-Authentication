# Reverse-Engineering-Authentication

Unit 14 Sequelize: Reverse Engineering Code

Server.js:

The server.js file pulls in the npm packages required for the application. For this program, express.js is included as the web application framework for communicating with the backend of the application. Additionally, passport is used for authenticating server requests and maintaining a user’s login session. 

Var PORT is used to connect to port 8080 of the user’s machine to connect to a local server once it is started up. Additionally, process.env.PORT is used to connect to the environment variable’s port as an alternative to the hardcoded 8080 port. This option allows your server to accept ports from the environment as an alternative. 

Var db is the variable that is storing our imported models folder, making it accessible by our server.

Var app = express(); is starting our express app, app.use(express.urlencoded({ extended: true })); is being used to return middleware that returns middleware that only parses urlencoded bodies and only looks at requests where the Content-Type header matches the type option, with the extended option set to true, the values are open to any type, instead of strings and arrays.
app.use(express.json()); is used for returning middleware that only parses JSON and looks at requests where the content-type header is matching the type option.
app.use(express.static(“public”)); is used to set the public folder in our application as the default folder for receiving files used by our frontend. In this example, it is making use of the javascript, css, and images in the assets folder. 

app.use(session)); is storing the login status of our user and app.use passport.initialize begins passport and session invokes the process of a persistent login session.

require("./routes/html-routes.js")(app); is importing our html routes to be used by our server for displaying content after a user clicks on a link.
require("./routes/api-routes.js")(app); is importing our api routes to be used by our server for sending and receiving content after a user causes the application to make a request.

db.sequelize.sync() syns all of our models to the database when we run the application. The next function that follows is app.listen, which passes in the PORT used by our server into a function that logs the fact that we are listening to a port for the developer.


Package.json 

This file holds metadata related to our project that allows the developer to see the name of the program, version, description, and which file will serve as the main entry point for the application. Later down the object we find the scripts that can be run in the terminal to start the application with the commands that they correlate to. For example, the start script in this application will run “node server.js,” which will start the application. The keywords option holds various strings that can assist other users in finding your package after performing an npm search command in terminal. The author serves as the individual that created the program while the license explains to the user what restrictions that users of this program are legally required to abide by. Finally, the dependencies reveal the middleware used in the program and the specified versions. Users can perform an npm install to download the middleware listed in the package.json. 

Routes

The routes folder holds our html routes and our api routes. The api-routes.js file will contain our post requests and get requests in our application. At the beginning, the application imports the models and passport configuration in order to function properly. Later, a function used for authenticating local user logins is exported to the application for use by the server file. Errors in authentication will be displayed to the user. The next app.post is used for signing up users by taking in user input that was entered into the relevant email and password fields and then posting that information to the user.js model, which will then be sent to the orm and into the database through a mysql connection query. Afterwards, a .then function is run to redirect the user to the login page so that they may log in with their newly created credentials. If any errors are detected, an error message will be displayed to the user on submission. Next, there is an app.get route used to sign out users and redirect the user back to the root directory. At the end, there is an app.get route used for allowing users who are signed in to view their email address and id, however, this is done at the end of an else statement, which runs after an if statement fails to see that a user is not logged in and sent back an empty object.

The html-routes.js file includes code that will direct the user to specific parts of the website after the user sends specific get requests to the address bar. At the start, the path middleware is required in order to allow the application to grab files from specific file paths. Next, middleware is imported into a variable from the config folder to check if a user is logged in or not. Finally, a function is exported to the rest of the application for use by the server. If a user sends a get request to “/”, they are redirected to the signup page if they aren’t logged in or they are sent to the members page. The “/login” get request redirects the user to the login page if they aren’t signed in, otherwise, they are sent to the members page. Users who have not signed in will be sent to the signup page if they send a get request to “/members.”


Public

The public folder holds all of the assets that the frontend of the application will use including the css styling and html and javascript files used for the login, members, and signup pages. The html files will be sent to the user depending on the type of request that is sent and whether or not the authentication middleware detects that the user is signed in or not. Javascript files titled under the same names as the html files are linked together at the end of the html files that the scripts can load and allow users to manipulate the html after it has loaded on the client-side. Css files are linked to all of the html files as well to provide 50px margin spacing at the top of the signup and login forms.

Models

The models folder holds our models that will be used for managing our database without interacting with mysql workbench directly. The index.js file uses strict ruling that does not allow undeclared variables. Then, fs is required for reading and writing to files and path is required for allowing files to be written and imported to locations in our file directory. Sequelize is required to allow us to manipulate our database by mapping the database entries to objects. It also allows us to synchronize or clear our database at the start of the application. The env variable processes whether or not the server will run in development mode or a mode exposed by the env file.
The config variable is importing the config.json file to retrieve the databases that can be used by the program based on the mode selected on line 7. If an env variable is available, it will be used, otherwise, a new sequelize constructor creates new information to be used in the database. 

The readdir function returns a pointer to a structure representing the directory entry at the current position specified by current __dirname, then files are returned that do not contain periods at the first position of their indices, do not contain the filename part of the file path being searched, and are also .js files. Next, a for loop is performed to import every file returned to be imported into the current directory. The names of the models found at the end of the function are stored in the db array to be used in a for loop that will return the files that are associated with the model names that match. Finally, the resulting models are initialized and exported to the program.

The user.js folder requires middleware for encrypting user passwords for enhanced security. Next, the user model is define and exports the following code to the program. The user’s email input from the client-side and sent to the backend must be a string, cannot be null, and must not match another email already present in the database. The email is checked for validation and must return true. Afterwards, the password inputted by the user must both be not null and be type string. Afterwards, the password inputted by the user is checked to see if it can be compared to the encrypted. The comparison will be returned, finally, a .addHook method is used to automatically hash a password before it is associated with a user.

Config

The config folder holds our databases including a demo database used for testing passport, a testing database to test the full application, and a production database that will be deployed to users. The passport.js file imports the passport package for authenticating requests and passport-local package for authentication of usernames and passwords. Additionally, the models directory is required for storing user credentials into the database. After passport.use is invoked, a user will be required to enter an email when logging in. If the email is found by the db.User.findOne function after receiving the email, password, and done parameters, then another function is called after a .then function, which checks to see if the email exists or not. If the check fails, the user is notified that the email is incorrect. Alternatively, if the next function, which is called by an else if statement finds no password associated with the email address, the user is notified of this error as well. 

As long as none of the functions succeed in finding errors, the user is returned. At the end of the file, the user’s credentials are serialized and deserialized for quicker transmission with callback functions that interact with other files in the application that require the user’s credentials. The file is then exported to the program. Finally, the middleware folder contains an isAuthenticated.js file, which exports a function that blocks users from accessing specific pages and redirects them to the root directory if they are not signed in. Otherwise, the signed-in user is free to proceed. 
