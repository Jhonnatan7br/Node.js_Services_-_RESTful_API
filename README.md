$ rm server.mjs "Deleting files"
$ New-Item index.js "Item_name.js , Item_name.mjs"
$ mkdir name_dir "Create a directory"
$ cd directory_to_go "Go to directory"
$ cd .. "Return to higher directory"

"
To create a custom NPM shell command, add the following lines to the scripts object in your package.json file:
"
"scripts": {
    "static": "serve -p 5050 static",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
"
Use the npm run command, followed by the script name, to execute these commands in the terminal. For instance:
"
$ npm run static



#Mocking a Web Service (1) = creating a mock or fake version of a service or component.
const API = "http://localhost:3000/"
"
Let's create a file called server.mjs at the top level of our project (next to the static folder
"
'use strict';
import { createServer } from "node:http";
server.listen(3000);
console.log("Server listening on port http://localhost:3000/")
"""
Since our hosted web app is served on http://localhost:5050 and our service is hosted on http://localhost:3000, requests from our web app to our service are considered cross-domain requests
"""

#Mocking GET Routes
$ mkdir mock-srv
$ cd mock-srv
"install the Fastify framework"
$ npm add fastify fastify-cli
"generate a Fastify project scaffold by running the following command within the mock-srv"
npx fastify generate . --esm
"install the project dependencies"
$ npm install
"launch the Fastify server to see if everything is working"
$ npm start
"""
We should see the following in the console:
> mock-srv@1.0.0 start
> fastify start -l info app.js
"""
<Ctrl> + C "To quit the active shell process an Abort Signal"
"Install the fastify-cors plugin:"
$ npm install @fastify/cors
"create our route folders"
$ cd routes
$ mkdir confectionery
$ mkdir electronics
$ cd ..
"""
Fastify works by dividing the service up into plugins. A plugin is a module that exports a function.
The Fastify instance can be used to register a GET route by calling fastify.get. 
"""