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
> - $ mkdir -p name_dir/name_dir "Create a directory inside directory"
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

In two different terminals run each command, with current working directory set to the mock-srv folder.
$ npm run static 
$ npm run dev

Next we'll navigate to http://localhost:5050 and select Electronics from the category selector.
In a production scenario, proper validation should be implemented to ensure data integrity and security.

# Going Real Time Production (Web Sockets)
![image](https://github.com/user-attachments/assets/a2aba9e0-36fc-4048-85b3-ee290a1a83b3)


## Enhancing an HTTP Server with WebSockets 

WebSockets allow for two-way communication between browsers and servers. Similar to how HTTP protocol is built on top of the TCP protocol, the WebSocket protocol is built on top of the HTTP protocol.

A new function called realtimeOrders has been added to the frontend code. It is invoked within the event listener function attached to the input event of the category selector and within the submit event of the add form. 

When a category is selected (or when a new item is added to a category), a WebSocket connection to ws://localhost:3000/orders/{category} is established with the selected category. The WebSocket connection (socket) listens for real-time messages sent from the server.

This will not work until we have upgraded our mock service with server-side WebSocket functionality. To support these frontend changes, we need to support a  ws://localhost:3000/orders/{category} route. 

> - $ npm install @fastify/websocket 

Organize routes index.mjs and static app.js and index.html to respond to the new implemented websocket
The fastify-websocket plugin enhances the fastify.get method. If an options object is passed as a second argument (instead of a route handler) which has the websocket property set to true

## Enhancing an HTTP Server with WebSockets

"Modify the static/index.html and static/app.js to enhance with the ability of handle bidirectional solicitudes with websockets"

Upgrade our mock service with server-side WebSocket functionality, To create WebSocket routes (which are just HTTP routes which upgrade to realtime WebSocket connections), we need to install the fastify-websocket plugin.

> - $ npm install @fastify/websocket

"Modiify the mock-srv/app.mjs to mock-srv/app.mjs"

> - import websocket from "@fastify/websocket";
  // Register Websocket
  fastify.register(websocket, {});

"Create a mock-srv/routes/orders folder with an index.mjs file and sketch out our route."

> - $ cd mock-srv
> - $ mkdir -p routes/orders

"-p: This flag tells the command to create any necessary parent directories if they don't already exist. "

"Let's also create an index.mjs file in mock-srv/routes/orders folder with websocket: true argument that indicates that this route will handle WebSocket connections."

> { websocket: true },

"We can check whether this has worked by starting our server, serving the static files with the serve command. To start the server, we run the following in the mock-srv folder:"

> - $ npm run dev

"In a separate terminal, we serve the static files by running the following command from the root of the project folder:"

> - $ serve -p 5050 static

" This does not fully demonstrate real-time capabilities. If we are mocking real-time functionality, we want to see constant updates. To achieve this, we need to mock a data stream of order updates for all items. update the top of mock-srv/plugins/data-utils.mjs adding the fastify-plugin "

> - import fp from "fastify-plugin";
> - import {promisify} from "node:util"

" Here we have added a promisified setTimeout (timeout) which we will use later. Next to the same mock-srv/plugins/data-utils.mjs file add an orders simulator with an async generator function"

> async function* realtimeOrdersSimulator() {...}

"An async function produces a promise. A generator function produces an iterable. "

"Add another function to mock-srv/plugins/data-utils.mjs. This time we will add a synchronous generator function:"

function* currentOrders(category) {...}

" Modify the bottom of mock-srv/plugins/data-utils.mjs  to add decorators

> - export default fp(async function (fastify, opts) {
> -   fastify.decorate("currentOrders", currentOrders);
> -   fastify.decorate("realtimeOrders", realtimeOrdersSimulator);})

"Using decorators in this way, you're essentially registering these functions as named services within the Fastify application. This allows you to access and use these functions later in your code using their decorated names."

"Update mock-srv/routes/orders/index.mjs to complete the implementation of the /orders/{category} route using the newly available fastify.currentOrders and fastify.realtimeOrders methods."

"Now if we start our server (npm run dev in the mock-srv folder) and serve the static assets (npm run static in the project root), then navigate to the http://localhost:5050 and select any category we should see the orders of all items frequently updating."

# Bidirectional Real-Time Communication
(IMAGE)

"It would be good to support real-time server mocking of more optimal client-side behavior."

"Let's update the client-side realtimeOrders function (along with the socket variable just above it) in static/app.js to the following:"

> const realtimeOrders = (category) => {...}

"Update the mock-srv/routes/orders/index.mjs"

>      monitorMessages(socket);
>      sendCurrentOrders(request.params.category, socket);
>      for await (const order of fastify.realtimeOrders()) {...}

"We have added two functions: monitorMessages and sendCurrentOrders. The sendCurrentOrders function just factors out the for of loop that was in the body of the route handler function because the same logic is now used in the route handler function and in the monitorMessages function."

## Modifying and Receiving Server-Side State in Real-Time

"Similarly, we can implement an approach to updating server-side state, which could later integrate with a database if the service was to be moved towards a production use-case."

"We are going to add a POST route for the  /orders/{ID} endpoint. This will accept POST requests that increment the total for a given product. The WebSocket connection will no longer have simulated orders sent across it, only the current order totals and then any new order totals resulting from POST requests to the /orders/{ID} endpoint."

"Add the new POST route in mock-srv/routes/orders/index.mjs"
>  fastify.post("/:id", async (request) => {
>    const { id } = request.params;
>    fastify.addOrder(id, request.body.amount);
>    return { ok: true };
>  });
>}

"Alter the top of mock-srv/plugins/data-utils.mjs"
> import { PassThrough } from "node:stream";

" The orders and catToPrefix objects are the same, but we've gotten rid of the promisify function and the timeout function it was used to create. Now we are loading the PassThrough constructor from the streams module. We won't be needing the timeout function any more and we are going to use a PassThrough stream to route order updates through. "

" Node.js streams represent continuous data and have quite a vast API. There are readable streams, writable streams, and hybrid streams that are both readable and writable (duplex, transform, and passthrough streams). One very useful characteristic of readable streams is they are async iterables "

"Let's remove the realtimeOrdersSimulator async generator function and replace it"

> // Simulate real-time orders
> async function* realtimeOrdersSimulator() {
>   for await (const { id, total } of orderStream) {
>     yield JSON.stringify({ id, total });
>   }
> }

" Add function addOrder to validation of integer number and orders [id].total += amount to mock-srv/plugins/data-utils.mjs"

> // Add order to stream and update total
function addOrder(id, amount) {...}
> if (Number.isInteger(amount) === false) {...}
> orders[id].total += amount;

" In the next step, we will decorate the fastify instance with this function. This is the function we are calling from our new POST route that we created earlier. If the id isn't in our orders object, an error is thrown with the status property set to 404. Since this throw will occur inside the async function handler of the POST /:id route"

"It's worth pointing out that it is far better to do type checking of POST body data using Fastify schemas, and it is only for the sake of brevity and focus that we have not done so here. It is left as an exercise to remove this particular check from the addOrder function and instead use a route schema."

"All being well, the amount is added to the total for the given id. Then the id and new total are placed into an object written to the orderStream.
The final part of mock-srv/plugins/data-utils.mjs should have the fastify.decorate("addOrder", addOrder); on Pluggin export"

>   fastify.decorate("addOrder", addOrder);

"Verify the contents of mock-srv/plugins/data-utils.mjs"

"Let's start our server and web app by running npm run dev in the mock-srv folder and npm run static in the project root. If we now run the following command in a third terminal window, we can execute a POST request to add Vacuum Cleaner orders:"

> - $ node -e "http.request('http://localhost:3000/orders/A1', { method: \ 'POST', headers: {'content-type': 'application/json'}}, (res) => \ res.pipe(process.stdout)).end(JSON.stringify({amount: 10}))"

Or you can use curl to make the request from the terminal:

> - $ curl -X POST -H "Content-Type: application/json" -d '{"amount": 10}' http://localhost:3000/orders/A1

"This makes a POST request to http://localhost:3000/orders/A1 with a JSON payload of {"amount": 10}. This command should output {"ok":true} (which is the HTTP response body) to the terminal and exit."

# Creating a Command Line Tool

"We are going to build-out a simple command line tool that makes a POST request to the /orders/{ID} endpoint, first make a new folder in a project root"

> - $ mkdir my-cli

Our CLI tool is going to be created as an independent package, so let's change directories into the my-cli folder and initialize a Node.js package:

> - cd my-cli
> - npm init -y "The npm init -y command will automatically generate a package"

"The my-cli/package.json file can have a bin field added to it. This can then be used by the npm tool to create an executable file (whether it's a symlink with executable permissions on unix-based systems or a cmd file on Windows) that points to the path specified by the bin field"

> {"bin": "./bin/cmd.js",}

"Let's create my-cli/bin/cmd.js with the following contents:"

> #!/usr/bin/env node

"The path to env is typical across unix and unix-like systems. Passing the node as an argument to /usr/bin/env results in the full path to wherever node is installed being output. This full path to the node binary then becomes what the OS uses to execute the text. On Windows systems, npm wraps the file referenced from the bin field of my-cli/package.json in a .cmd file."

"Now let's append the following to the my-cli/bin/cmd.js file:"

> import got from 'got'

"Because we have opted into ESM modules using the type field of the my-cli/package.json, we are using the ESM syntax to import the got library."

> Now let's add a third line to our my-cli/bin/cmd.js file:

const API = "http://localhost:3000";

"This line sets up an API constant which references the default address of our local web service. This could theoretically be adapted in the future to a production URL."

"Now we will create a usage function by adding the following to my-cli/bin/cmd.js file:"

> const usage = (msg = 'Back office for My App') => {
>   console.log(`\n${msg}\n`);
>   console.log("Usage: cmd <ID> <AMOUNT>");
> };

"As we can see from the usage function, we're planning for our CLI to accept a product ID and an amount, as positional arguments. We can access the arguments that a Node.js process is executed with, using the argv property of the global process object. "

Add argv to my-cli/bin/cmd.js
// Get the arguments from the command line
const argv = process.argv.slice(2);

"Now for the actual meat of what we want our CLI to do: make a POST request to the /orders/{ID} route of our mock service. Let's finish up the my-cli/bin/cmd.js "

> // Update the order with the given ID
> try {
>   // Use GOT to make a POST request to the API
>   await got.post(`${API}/orders/${argID}`, {
>     json: { amount },
>   });

"Using a try-catch block, we pass a string to got.post that points to our mock service's /orders/{ID} endpoint, where {ID} is the first positional argument supplied to the command line tool (assigned to the id constant). "

"To complete the step of creating a node module executable file, we have to set the file permissions so the file is an executable. This can be done using the `chmod` command in the following way:"

> - $ chmod u+x bin/cmd.js


Let's run the following command from the my-cli folder:

> - $ npm link

"npm link is a two step process. The first step is to create a symlink from the application and the second is to register our application in the root project directory. Here we enter:"

> - $ npm link my-cli

"This command is like npm install<module> , where npm sets up the relevant command name to be used from any directory in the terminal. It links from the my-cli node_modules folder to a given packages project folder. This means any changes we make to my-cli/bin/cmd.js in the future will be immediately reflected the next time we run the my-cli command."

"Let's try this out. First we need to start the mock service we built in prior chapters and serve the web app."

> - $ serve -p 5050 static

In another terminal, with the mock-srv folder as the current working directory, let's execute the following command:

> - $ npm run static

In another terminal, with the mock-srv folder as the current working directory, let's execute the following command:

> - $ npm run dev

In a third terminal window, we can run the following:

> - $ my-cli

In the same terminal window let's run:

> - $ my-cli A2 35

If we observe the web app as we execute this command, we should see the order count for the Leaf Blower product jump from 7 to 42.


Jhonnatan B <jhonnatan7br@gmail.com>
Wed, Sep 11, 3:17 PM
to me

## Parsing Command-Line Flags

Command-line flags provide a versatile and efficient way to interact with programs, allowing users to customize behavior, provide input, and control output.

Examples of common flags:

-h or --help: Displays help or usage information.
-v or --version: Displays the program's version.
-f or --file: Specifies a file path as input.
-o or --output: Specifies an output file.
-d or --debug: Enables debug mode.
By understanding and effectively using command-line flags, you can enhance the usability, flexibility, and power of your programs.

Let's install commander with the following command, executed with the my-cli as the current working directory:

> - $ npm install commander

At the top of my-cli/bin/cmd.js we can import commander like so:

import {Command} from "commander";

We are instructing commander to create a new Command program. Commander is well maintained and prevalent in the industry. It provides a set of stable API’s that allows for easy creation of command line applications. To create a new program we apply a new Command instance to the program.

First we will create our program by entering the following:

> // Create a new Program
> program
>   .name("my-cli") // Set the name of the program
>   .description("Back office for My App") // Set the description
>   .version("1.0.0"); // Set the version
> // Parse the arguments from process.argv
> program.parse();

Now when we go to interact with my-cli application in the terminal, by entering:
> - $ my-cli --help "To output the arguments of the command"

To see the version that we build

> - $ my-cli -V | --version

All we have done here is drastically reduce the log output for the usage function. We are going to support two command line arguments for ID and AMOUNT. Arguments are values that can be entered in the console without the use of command line flags. Next we take the amount validation logic, along with our try-catch block, and scope them together.


Next we take the amount validation logic, along with our try-catch block, and scope them together.

Create our update command for the program

> // Create a command for adding a new order
> program
>   // Set the command name
>   .command("update")

Now we can begin to test our application in our terminal.

> - $ my-cli update A2 12

Now let's see this in action. First, we need to start the web service we built in prior chapters and also serve the web app. In one terminal from the project route let's run the following command:

> - $ npm run static

In another terminal, with the mock-srv folder as the current working directory, let's execute the following command:

> - $ npm run dev

Let's also navigate to http://localhost:5050 and select the Electronics category. The current order count for the two products in this category should be 3 and 7, respectively.

In a third terminal window, we can run the following:

> - $ my-cli

In the same terminal window let's run:

> - $ my-cli update A1 5

You should now see these changes reflected within our web application.

Let's try another command:

> - $ my-cli update A2 15

## Implementing Sub-Commands

So far we have implemented a single command to our CLI program. This only takes in two required fields <ID> and <AMOUNT> but we can do a lot more with commander. By doing so, we are going to be gradually increasing the complexity of our application.  We will be adding more “commands” with their own bespoke arguments and  options or --flags to our program, thus defining more complicated commands and their associated functionality.

First, we create a new module my-cli/src/utils.js to contain our “business logic”, or behaviors we want to have scoped to be their own functions.

By scoping the business logic to its own functions, you can isolate and manage the behaviors independently, making each behavior easier to understand, test, and modify. We will then import these into our main my-cli/bin/cmd.js to use with our program.

First, let's move the update() into the my-cli/src/utils.js to make it our first function for our module.

First we will be adding a couple of ancillary methods that we will be using continuously throughout our codebase. These are log(message) and its counterpart error(message) methods, which are simple abstractions over the console.

> // Log the usage of the command to the console export const log = (msg) => {
>   console.log(`\n${msg}\n`);
> };
> // Log the error to the console
> export const error = (msg) => {
>   console.error(`\n${msg}\n`);
> };

The next function we will implement is the add() method, which adds a new product to our web server.

> // Add a new order
> export async function add(...args) {...
> }

First, we are passing through all the arguments ...args that would be provided to the function. This allows us to destructure and extract the values that we need, to allow us to provide the additional functionality that we require.

Now let's write our corresponding command for this newly created functionality.

> // Create a command for listing categories by IDs
> program
>   // Set the command name
>   .command("add")
>   // Set the command description
>   .description("Add Product by ID to a Category")
>   // Set the category to be required
>   .argument("<CATEGORY>", "Product Category")

This allows us to send through strings to our backend to provide a long-form product description. Should a long-form description be supplied, we would include that in the got.post(`${API}/${category}`) request to our web server API. We can test this by entering:

> - $ my-cli add electronics A3 Laptop 599 "Best mid-priced laptop money can buy"

Now let's write the listCategories() function:

> // List the categories
> export function listCategories() {...
> }

The last method we will be creating is its sibling function listCategoryItems(category). The purpose of this function is to list the Items within each category when the category is supplied to the method. It makes a GET request to our mock-srv pointing at the relevant category endpoint, then displays the output to the terminal.

> // List the IDs for the given category
> export async function listCategoryItems(category) {
>   log(`Listing IDs for category ${category}`);
>   try {
>   }
}

Finally, we hook these two methods to our program.

> // Create a command for listing categories
> program
>   // Set the command name
>   .command("list")
>   // Set the command description
>   .description("List categories")
>  });

Here as you can see, we have increased the complexity of our program somewhat. With our progam.command(“list”) we have an optional [CATEGORY] command line argument.

For both of our flags, we are also setting our short-hand expressions `-a`, for its long-handed declaration `--all`. The idea is that we can either provide the name of the individual category to be listed or we can list all the categories from our application by commanding the output via a flag. Lastly, we are defining our logic within the program.action() method.

> // Set the action to be executed when the command is run
> .action(async (args, opts) => {
>   if (args && opts.all)
>     throw new Error("Cannot specify both category and 'all'");
>   if (opts.all || args === "all") {
>     listCategories();
>   }
> });

Each action method accepts three command line arguments, an options object that has the key all passed in as a boolean, and the last argument is the Command object itself (for the vast majority of purposes we will not be needing this parameter).

Now we should be able to view all our commands through the initial program.help() screen.

> - $ my-cli --help

Now let’s test our new list command:

> - $ my-cli list --all

This should be the same result as if we had entered:

> - $ my-cli list -a || my-cli list all

Next we will test our ability to list the items in a single category:

> - $ my-cli list electronics

Let's try out the new and improved my-cli tool. In one terminal from the project route, let's run the following command:

> - $ npm run static

In another terminal, with the mock-srv folder as the current working directory, let's execute the following command:

> - $ npm run dev

Let's also navigate to http://localhost:5050 and select the Electronics category.

Now let's execute the following in another terminal window:

> - $ my-cli --help

# Implementing a Terminal User Interface (Customizing)

To take our CLI tool to the next level, we are going to introduce an interactive mode. This interactive mode will provide the same functionality as the web app. It will have the ability to select a category, display the products in that category and then fill out a form to add new products to the category.

Let's install the modules we will be using. Ensuring our current working directory is the my-cli folder, let's execute the following command:

> - $ npm install chalk enquirer

This command will install the three specified dependencies and add them to the my-cli/package.json file.

## Styling the Output Messages

To begin, let’s create a new file my-cli/src/colors.js to contain only the colors that we will be using. As we do, we will explain the chalk API and how we can construct reusable color schemes that can be applied to decorate our terminal messages.

Since we are styling for the terminal, we will need to consider special ANSI-colors, which will be applied to our strings. This can be very difficult to concisely add to our code, but we can leverage libraries that make this string formatting experience a lot easier for developers. To this end, we only need to import chalk into our new colors.js file to get started

Is highly recommend taking a close look at chalk.js README on their github repository for additional support as we implement chalk and its API. This additional support will allow you to better decorate the terminal to your liking.

URL : https://github.com/chalk/chalk#readme

We have provided a set of both text colors and background colors on the RGB color spectrum. It also allows for additional text decorators like bold, underline and decorators to be further applied on top of each instance of chalk.

import chalk from "chalk";
> // Background colors
> export const bgBlue = chalk.bgRgb(52, 158, 219);
> export const bgCyan = chalk.bgRgb(26, 188, 156);
> // Text colors
> export const txBlue = chalk.rgb(52, 152, 219);
> export const txCyan = chalk.rgb(26, 188, 156);

The benefit of having our colors scoped to variables that we can export is that it allows us to make even the smallest changes to the values from a single place, instead of the values being reiterated all over our codebase.

We can then use these variables in the following way:

> console.log(txYellow('Hello, I’m a Yellow string'))

This will then decorate the string in a nice yellow. We can console.log this and see it in the terminal by running node on the file itself:

> - $ node src/colors.js

We will be creating our displays again in a separate module; my-cli/src/displays.js.

Here we will be importing some of the colors and background colors that we have created in our my-cli/src/colors.js to be used in our displays. We will also be taking the log() and error() that we created within our my-cli/src/utils.js.

> import {
>   bgCyan,
>   bgPurple,
>   bgRed,
>   txYellow,
> } from "./colors.js";

> // Export the output display functions

> // Log the usage of the command to the console
> export const log = (msg) => console.log(`\n${msg}\n`);

Here we will be importing some of the colors and background colors that we have created in our my-cli/src/colors.js to be used in our displays. We will also be taking the log() and error() that we created within our my-cli/src/utils.js.

Here are the following display methods that we will be consuming when we begin to decorate the messages that we are outputting to the terminal from `commands`:

> // Get the current timestamp
> const timestamp = () => new Date().toLocaleString();
> export const displayTimestamp = () => bgPurple(timestamp());

Run this command in the terminal to see the new improved output:

> - $ my-cli list A1 30

Now, we can expand this out to each one of our methods. The next update will be to the add():

> // Add a new order
> export async function add(...args) {...
> }

Execute the following command :

> - $ my-cli add electronics A3 Server 999 A super powerful server to run all your node applications on

## Adding Interactivity

Let's start this exercise by creating a new module for our prompts my-cli/src/prompts.js. Here we will begin to create and define the prompts for our interactive app.

Let’s import the enquirer module which allows us to easily use the pre-built prompts that come with the Enquirer library.

> import Enquirer from "enquirer";
> // Import the Enquirer prompt types
> const { prompt } = Enquirer;

The general idea for composing these prompts is to create a series of questions for each type of prompt. From there, we can pass the questions to `prompt()` that we import from `Enquirer`.

We also need to import our business logic from the my-cli/src/utils.js which will then act upon the prompt responses. These are received asynchronously by awaiting the response from the prompt:

> const { category } = await prompt(categoryQuestions)

We can test our methods by invoking them and then calling node to execute the file.

> // Import the Enquirer prompt types
> const { prompt } = Enquirer;
> const categoryQuestions = [
>   {...
>   },
> ];
> export const promptListIds = async () => {...
> };

Now call node to execute the file.

> - $ node src/prompts.js

With our prompts all setup, we can begin to construct out the main terminal prompt that will, in effect, enclose the predefined functionality together. Then, we can call our prompt behavior easily when we connect it to our program declared within our my-cli/bin/cmd.js.

With the command variable now defined from either the functional argument or from a user derived input, we pass it through a series of 'switches'. switch cases only operate when the case is truthy. Using this behavior we can create a controller that helps to specify the behavior according to the command that has been provided. We decorate the interactiveApp() with our already stylised log() methods.

> / Run the Interactive program
> await interactiveApp();

> - $ node src/prompts.js

With our interactiveApp() now meeting our requirements, we can make the final amendments to my-cli/bin/cmd.js in order to integrate our interactive session with our program.

Update code for the program.command() that we have already defined. Note the removal of .action() and how it made .arguments() optional. Also notice how we have added a `.option("-i, --interactive", "Run ‘this’ Command in interactive mode")`:

We can test our application by running the following commands to see if we have the expected behavior:

Execute the application in both the interactive mode and as a CLI tool.
> - $ my-cli -i
> - $ my-cli --interactive

Independently enter each command’s prompt behavior to provide inputs, while also keeping the existing CLI functionality.
> - $ my-cli add --interactive
> - $ my-cli list --interactive
> - $ my-cli list -i -a
> - $ my-cli update --interactive
