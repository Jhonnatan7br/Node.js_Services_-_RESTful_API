# Node.js Services, RESTful APIs & Cli tools

# Setting and Installing node.js dependencies (Cli tools and Linux, MAcOS and Windows interoperability)

> - $ choco install fnm "Admin access required"
> - $ fnm install --lts " Install latest version"
> - $ fnm --version
> - $ node -v
* fnm documentation: https://github.com/Schniz/fnm  

# Quick File Server
> - $ npm init
> - $ npm install serve "Instlling base modules of node.js"
> - $ npm mkdir static "Directory for static"
> - $ npx serve -p 5050 static "start the file server on port 5050 and serve the contents of the static folder"

## Commands

> - $ rm server.mjs "Deleting files"
> - $ New-Item index.js "Item_name.js , Item_name.mjs"
> - $ mkdir name_dir "Create a directory"
> - $ cd directory_to_go "Go to directory"
> - $ cd .. "Return to higher directory"

## Creating npm commands

To create a custom NPM shell command, add the following lines to the scripts object in your package.json file:

'''
"scripts": {
    "static": "serve -p 5050 static",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
'''

Use the npm run command, followed by the script name, to execute these commands in the terminal. For instance:
> - $ npm run static


# Mocking service
## Mocking a Web Service (1) = creating a mock or fake version of a service or component.
const API = "http://localhost:3000/"
Let's create a file called server.mjs at the top level of our project (next to the static folder

"""
'use strict';
import { createServer } from "node:http";
server.listen(3000);
console.log("Server listening on port http://localhost:3000/")
"""

Since our hosted web app is served on http://localhost:5050 and our service is hosted on http://localhost:3000, requests from our web app to our service are considered cross-domain requests

## Mocking GET Routes

* fastify-cli framework documentation: https://github.com/fastify/fastify-cli#generate

> - $ mkdir mock-srv
> - $ cd mock-srv
Install the Fastify framework
> - $ npm add fastify fastify-cli
Generate a Fastify project scaffold by running the following command within the mock-srv
> - $npx fastify generate . --esm
Install the project dependencies
> - $ npm install
Launch the Fastify server to see if everything is working
> - $ npm start

We should see the following in the console:
> - mock-srv@1.0.0 start
> - fastify start -l info app.js
> - Ctrl + C "To quit the active shell process an Abort Signal"

Install the fastify-cors plugin:
> - $ npm install @fastify/cors
Create our route folders

> - $ cd routes
> - $ mkdir confectionery
> - $ mkdir electronics
> - $ cd ..

Fastify works by dividing the service up into plugins. A plugin is a module that exports a function.
The Fastify instance can be used to register a GET route by calling fastify.get. 
Now let's create the mock-srv/routes/electronics/index.js
The folder name will determine the top-level path of the route.

It's important to note that the route handler function also passes request and reply objects. These are conceptually the same, but functionally different to the req and res objects passed to the Node core http.createServer request listener function, because they have their own (higher level) APIs or context. See https://www.fastify.io/docs/v3.9.x/Request/ and https://www.fastify.io/docs/v3.9.x/Reply/ for full information on their APIs.

Run in two different consoles on each determined directory to have the GET route 
> - $ cd .. && npm run static
> - $ npm run dev

## Creating POST
Certain function or functions has to be upgraded to handle both GET and POST scenarios. A POST scenario occurs when the form is submitted. 
The browser native fetch function can also perform POST requests, which are configured via a second options argument.

It is highly recommended that production Node.js services are stateless. That is, they don't store their own state, but retrieve it from an upstream service or database. 

We will need to create an ID for each new entry. Since we have two routes and we don't want duplicate logic (even in mocking web services, the Don't Repeat Yourself principle applies) we can create a small data utility library plugin that both routes can use.

> - $ mock-srv> cd plugins
> - $ New-Item utils.js

The calculateID(idPrefix,data) method. The function starts by extracting unique IDs from the data array. Next, the code retrieves the last ID from the sorted array. Finally, the function constructs a new ID string by combining the idPrefix and the calculated next value.
The fastify-plugin module that is used to de-encapsulate a plugin.
The fastify.decorateRequest method is used to decorate the request object that is passed to route handler functions with a method we name mockDataInsert.

"""fastify.mockDataInsert(request, opts.prefix.slice(1), data);"""

Update POST routes to handle 

In a production scenario, failing to sanitize and validate incoming POST data and then sending that same POST data back in the response can lead to vulnerabilities.