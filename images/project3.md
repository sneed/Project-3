## Project 3 Documentation

### STEP 1 – BACKEND CONFIGURATION
Update ubuntu

`sudo apt update`

![Update Ubuntu](../images/update-packages.png)

Upgrade ubuntu

`sudo apt upgrade`

![Upgrade Ubuntu](../images/upgrade-ubuntu.png)

Lets get the location of Node.js software from Ubuntu repositories.

`curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -`

![Node.js from Ubuntu Repositories](../images/nodejs-ubunturepos.png)

**Install Node.js on the server**

Install Node.js with the command below

`sudo apt-get install -y nodejs`

![Install node.js](../images/install-nodejs.png)

Note: The command above installs both <mark>nodejs</mark> and <mark>npm</mark>. [NPM](https://www.npmjs.com) is a package manager for Node like apt for Ubuntu, it is used to install Node modules & packages and to manage dependency conflicts.

Verify the node installation with the command below

`node -v`

![Verify node installation](../images/verify-nodeversion.png)

Verify the node installation with the command below

`npm -v`

![Verify npm installation](images/verify-npmversion.png)

Application Code Setup

Create a new directory for your To-Do project:

`mkdir Todo`

![Create new directory ](../images/newdir-Todoproj.png)

Run the command below to verify that the Todo directory is created with ls command

`ls`

![List directory](../images/ls-dir.png)

TIP: In order to see some more useful information about files and directories, you can use following combination of keys ls -lih – it will show you different properties and size in human readable format. You can learn more about different useful keys for ls command with ls --help.

Now change your current directory to the newly created one:

`cd Todo`

![Change current directory](../images/changedir-Todo.png)

Next, you will use the command npm init to initialise your project, so that a new file named package.json will be created. This file will normally contain information about your application and the dependencies that it needs to run. Follow the prompts after running the command. You can press Enter several times to accept default values, then accept to write out the package.json file by typing yes.

`npm init`

![Initialise project](../images/npm-init.png)

Run the command ls to confirm that you have package.json file created.

`ls`

![Confirm package.json file created](../images/package-json.png)

Next, we will Install <mark>ExpressJs</mark> and create the <mark>Routes</mark> directory.


### INSTALL EXPRESSJS

**Install ExpressJS**

Remember that Express is a framework for Node.js, therefore a lot of things developers would have programmed is already taken care of out of the box. Therefore it simplifies development, and abstracts a lot of low level details. For example, Express helps to define routes of your application based on HTTP methods and URLs.

To use express, install it using npm:

`npm install express`

![Express intsall](../images/npm-install.png)

Now create a file index.js with the command below

`touch index.js`

![Create index.js file](../images/create-indexjs.png)

Run ls to confirm that your index.js file is successfully created

`ls`

![Confirm index.js file](../images/confirm-indexjs.png)

Install the dotenv module

`npm install dotenv`

![Dotenv module install](../images/npminstall-dotenv.png)

Open the index.js file with the command below

`nano index.js`

![Open file with command](../images/nano-indexjs.png)

Type the code below into it and save. Do not get overwhelmed by the code you see. For now, simply paste the code into the file.

const express = require('express');
require('dotenv').config();

const app = express();

const port = process.env.PORT || 5000;

app.use((req, res, next) => {
res.header("Access-Control-Allow-Origin", "\*");
res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
next();
});

![Content of index.js](../images/open-indexjs.png)

app.use((req, res, next) => {
res.send('Welcome to Express');
});

app.listen(port, () => {
console.log(`Server running on port ${port}`)
});
Notice that we have specified to use port 5000 in the code. This will be required later when we go on the browser.

Use :w to save in vim and use :qa to exit vim

Now it is time to start our server to see if it works. Open your terminal in the same directory as your index.js file and type:

`node index.js`

![Server running on port 5000](../images/server-port5000.png)

If every thing goes well, you should see Server running on port 5000 in your terminal.

Now we need to open this port in EC2 Security Groups. Refer to Project 1 Step 1 – Installing the Nginx Web Server. There we created an inbound rule to open TCP port 80, you need to do the same for port 5000, like this:

Open up your browser and try to access your server’s Public IP or Public DNS name followed by port 5000:

`http://<PublicIP-or-PublicDNS>:5000`

![Server public DNS ](../images/server-publicDNS.png)

Quick reminder how to get your server’s Public IP and public DNS name:
1. You can find it in your AWS web console in EC2 details
2. Run:

`curl -s http://169.254.169.254/latest/meta-data/public-ipv4` for Public IP address 

or 

`curl -s http://169.254.169.254/latest/meta-data/public-hostname` for Public DNS name.

Welcome to Express

Routes

There are three actions that our To-Do application needs to be able to do:

1. Create a new task
2. Display list of all tasks
3. Delete a completed task
4. Each task will be associated with some particular endpoint and will use different standard [HTTP request methods](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods): POST, GET, DELETE.

For each task, we need to create [routes](https://expressjs.com/en/guide/routing.html) that will define various endpoints that the <mark>To-do</mark> app will depend on. So let us create a folder <mark>routes</mark>

`mkdir routes`

![make directory routes](../images/makedir-routes.png)

Tip: You can open multiple shells in Putty or Linux/Mac to connect to the same EC2

Change directory to routes folder.

`cd routes`

![Change directory to routes](../images/changedir-routes.png)
Now, create a file api.js with the command below

`touch api.js`

![Create file api.js](../images/create-apijs.png)

Open the file with the command below

`nano api.js`

![Open file api.js](../images/open-apijs.png)

Copy below code in the file. (Do not be overwhelmed with the code)

const express = require ('express');
const router = express.Router();

router.get('/todos', (req, res, next) => {

});

router.post('/todos', (req, res, next) => {

});

router.delete('/todos/:id', (req, res, next) => {

})

module.exports = router;

![wite into file api.js](../images/write-apijs.png)

Moving forward let create <mark>Models</mark> directory.


### MODELS

Now comes the interesting part, since the app is going to make use of [Mongodb](https://www.mongodb.com) which is a NoSQL database, we need to create a model.

A model is at the heart of JavaScript based applications, and it is what makes it interactive.

We will also use models to define the database schema . This is important so that we will be able to define the fields stored in each Mongodb document. (Seems like a lot of information, but not to worry, everything will become clear to you over time. I promise!!!)

In essence, the Schema is a blueprint of how the database will be constructed, including other data fields that may not be required to be stored in the database. These are known as virtual properties

To create a Schema and a model, install [mongoose](https://mongoosejs.com) which is a Node.js package that makes working with mongodb easier.

Change directory back Todo folder with <mark>cd ..</mark> and install Mongoose

`npm install mongoose`

![Mongoose Installation](../images/install-mongoose.png)

Create a new folder  models :

`mkdir models`

![Create models folder](../images/mkdir-models.png)

Change directory into the newly created ‘models’ folder with

`cd models`

![Change directory into models folder](../images/cd-models.png)

Inside the models folder, create a file and name it todo.js

`touch todo.js`

![Create todo.js file](../images/touch-todojs.png)

Tip: All three commands above can be defined in one line to be executed consequently with help of && operator, like this:

`mkdir models && cd models && touch todo.js`



Open the file created with <mark>nano todo.js</mark> then paste the code below in the file:

![Open todo.js and past below code](../images/nano-todojs.png)

const mongoose = require('mongoose');
const Schema = mongoose.Schema;

//create schema for todo
const TodoSchema = new Schema({
action: {
type: String,
required: [true, 'The todo text field is required']
}
})

//create model for todo
const Todo = mongoose.model('todo', TodoSchema);

module.exports = Todo;
Now we need to update our routes from the file <mark>api.js</mark> in ‘routes’ directory to make use of the new model.

In Routes directory, open api.js with <mark>nano api.js</mark>, delete the code inside with <mark>:%d</mark> command and paste there code below into it then save and exit

![Open file api.js and delete existing code](../images/write-apijs.png)
![Paste code below into api.js](../images/rewrite-apijs.png)

const express = require ('express');
const router = express.Router();
const Todo = require('../models/todo');

router.get('/todos', (req, res, next) => {

//this will return all the data, exposing only the id and action field to the client
Todo.find({}, 'action')
.then(data => res.json(data))
.catch(next)
});

router.post('/todos', (req, res, next) => {
if(req.body.action){
Todo.create(req.body)
.then(data => res.json(data))
.catch(next)
}else {
res.json({
error: "The input field is empty"
})
}
});

router.delete('/todos/:id', (req, res, next) => {
Todo.findOneAndDelete({"_id": req.params.id})
.then(data => res.json(data))
.catch(next)
})

module.exports = router;

The next piece of our application will be the <mark>MongoDB Database</mark>


### MONGODB DATABASE

**MongoDB Database**

We need a database where we will store our data. For this we will make use of mLab. mLab provides MongoDB database as a service solution [DBaaS](https://en.wikipedia.org/wiki/Cloud_database), so to make life easy, you will need to sign up for a shared clusters free account, which is ideal for our use case. [Sign up here](https://www.mongodb.com/atlas-signup-from-mlab). Follow the sign up process, select AWS as the cloud provider, and choose a region near you.

Complete a get started checklist as shown on the image below

![MONGO DB SET UP](../images/mongodb-setup.png)

Allow access to the MongoDB database from anywhere (Not secure, but it is ideal for testing)

IMPORTANT NOTE
In the image below, make sure you change the time of deleting the entry from 6 Hours to 1 Week


![Access list](../images/ip-accesslist.png)

Create a MongoDB database and collection inside mLab

![alt text](../)

![alt text](../)

In the <mark>index.js</mark> file, we specified <mark>process.env</mark> to access environment variables, but we have not yet created this file. So we need to do that now.

Create a file in your <mark>Todo</mark> directory and name it <mark>.env.</mark>

`touch .env`

![Create file .env](../images/create-envintodo.png)


`nano .env`

![Open file .env](../images/open-env.png)


Add the connection string to access the database in it, just as below:

`DB = 'mongodb+srv://<username>:<password>@<network-address>/<dbname>?retryWrites=true&w=majority'
Ensure to update <username>, <password>, <network-address> and <database> according to your setup`

![Add connection string into file .env](../images/connection-string.png)

Here is how to get your connection string

![How to get connection string](../images/get-connectionstring.png)

Now we need to update the <mark>index.js</mark> to reflect the use of <mark>.env</mark> so that Node.js can connect to the database.

Simply delete existing content in the file, and update it with the entire code below.

To do that using nano, follow below steps

Open the file with nano index.js

The entire content should be deleted

Now, paste the entire code below in the file.

![Paste code to connect to DB](../images/code-connect2db.png)

```
const app = express();

const port = process.env.PORT || 5000;

//connect to the database
mongoose.connect(process.env.DB, { useNewUrlParser: true, useUnifiedTopology: true })
.then(() => console.log(`Database connected successfully`))
.catch(err => console.log(err));

//since mongoose promise is depreciated, we overide it with node's promise
mongoose.Promise = global.Promise;

app.use((req, res, next) => {
res.header("Access-Control-Allow-Origin", "\*");
res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
next();
});

app.use(bodyParser.json());

app.use('/api', routes);

app.use((err, req, res, next) => {
console.log(err);
next();
});

app.listen(port, () => {
console.log(`Server running on port ${port}`)
});
```

Using environment variables to store information is considered more secure and best practice to separate configuration and secret data from the application, instead of writing connection strings directly inside the <mark>index.js</mark> application file.

Start your server using the command:

`node index.js`

![Start server](../images/db-consuccess.png)

You shall see a message **‘Database connected successfully’**, if so – we have our backend configured. Now we are going to test it.

**Testing Backend Code without Frontend using RESTful API**

So far we have written backend part of our <mark>To-Do</mark> application, and configured a database, but we do not have a frontend UI yet. We need ReactJS code to achieve that. But during development, we will need a way to test our code using RESTfulL API. Therefore, we will need to make use of some API development client to test our code.

In this project, we will use [Postman](https://www.postman.com) to test our API.
Click [Install Postman](https://www.postman.com/downloads/) to download and install postman on your machine.

Click [HERE](https://www.youtube.com/watch?v=FjgYtQK_zLE) to learn how perform [CRUD operartions](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete) on Postman

You should test all the API endpoints and make sure they are working. For the endpoints that require body, you should send JSON back with the necessary fields since it’s what we setup in our code.

Now open your Postman, create a POST request to the API 

<mark>http://<PublicIP-or-PublicDNS>:5000/api/todos.</mark> 


This request sends a new task to our To-Do list so the application could store it in the database.

Note: make sure your set header key <mark>Content-Type</mark> as <mark>application/json</mark>

Check the image below:

![POST request to API](../images/post-postman.png)


Create a GET request to your API on <mark> http://<PublicIP-or-PublicDNS>:5000/api/todos.</mark> This request retrieves all existing records from out To-do application (backend requests these records from the database and sends it us back as a response to GET request).

![GET request to API](../images/get-apireq.png)

**Optional task:** Try to figure out how to send a DELETE request to delete a task from out To-Do list.

**Hint:** To delete a task – you need to send its ID as a part of DELETE request.

![Delete request to API](../images/delete-request.png)

By now you have tested backend part of our To-Do application and have made sure that it supports all three operations we wanted:

1. Display a list of tasks – HTTP GET request
2. Add a new task to the list – HTTP POST request
3. Delete an existing task from the list – HTTP DELETE request
4. We have successfully created our Backend, now let go create the Frontend.


### STEP 2 – FRONTEND CREATION

Step 2 – Frontend creation

Since we are done with the functionality we want from our backend and API, it is time to create a user interface for a Web client (browser) to interact with the application via API. To start out with the frontend of the To-do app, we will use the <mark>create-react-app</mark> command to scaffold our app.

In the same root directory as your backend code, which is the Todo directory, run:

`npx create-react-app client`

![Create react app](../images/create-reactapp.png)

This will create a new folder in your <mark>Todo</mark> directory called <mark>client</mark>, where you will add all the react code.

**Running a React App**

Before testing the react app, there are some dependencies that need to be installed.

1. Install [concurrently](https://www.npmjs.com/package/concurrently). It is used to run more than one command simultaneously from the same terminal window.

`npm install concurrently --save-dev`

![Install Concurrently](../images/install-concurrently.png)

2. Install [nodemon](https://www.npmjs.com/package/nodemon). It is used to run and monitor the server. If there is any change in the server code, nodemon will restart it automatically and load the new changes.

`npm install nodemon --save-dev`

![Install nodemon](../images/install-nodemon.png)

3. In <mark>Todo</mark> folder open the <mark>package.json</mark> file. Change the highlighted part of the below screenshot and replace with the code below.

"scripts": {
"start": "node index.js",
"start-watch": "nodemon index.js",
"dev": "concurrently \"npm run start-watch\" \"cd client && npm start\""
},

![Edit package.jason file](../images/nano-packagejason.png)

Configure Proxy in <mark>package.json</mark>

1. Change directory to ‘client’

`cd client`

![Change directory to client](../images/cd-client.png)

2. Open the package.json file

`nano package.json`

![Open package.jason file](../images/clientnano-packagejason.png)

3. Add the key value pair in the package.json file <mark>"proxy": "http://localhost:5000".</mark>

The whole purpose of adding the proxy configuration in number 3 above is to make it possible to access the application directly from the browser by simply calling the server url like <mark>http://localhost:5000</mark> rather than always including the entire path like <mark>http://localhost:5000/api/todos</mark>

Now, ensure you are inside the <mark>Todo</mark> directory, and simply do:

`npm run dev`

![Open and run app](../images/npm-rundev.png)

Your app should open and start running on <mark>localhost:3000</mark>

Important note: In order to be able to access the application from the Internet you have to open TCP port 3000 on EC2 by adding a new Security Group rule. You already know how to do it.

Creating your React Components

One of the advantages of react is that it makes use of components, which are reusable and also makes code modular. For our Todo app, there will be two stateful components and one stateless component.
From your Todo directory run

`cd client`

![Change to client directory](../images/cd-client1.png)

move to the src directory

`cd src`

![Change to src directory](../images/cd-src.png)

Inside your <mark>src</mark> folder create another folder called <mark>components</mark>

`mkdir components`

![Make components directory](../)

Move into the components directory with

`cd components`

![Change to components directory](../images/mkdir-components.png)

Inside ‘components’ directory create three files <mark>Input.js, ListTodo.js</mark> and <mark>Todo.js.</mark>

`touch Input.js ListTodo.js Todo.js`

![Make three files ](../images/touch3file-componentsdir.png)

Open <mark>Input.js</mark> file

`nano Input.js`

![Open Input.js](../images/nano-inputjs.png)

Copy and paste the following

``` 
import React, { Component } from 'react';
import axios from 'axios';

class Input extends Component {

state = {
action: ""
}

addTodo = () => {
const task = {action: this.state.action}

    if(task.action && task.action.length > 0){
      axios.post('/api/todos', task)
        .then(res => {
          if(res.data){
            this.props.getTodos();
            this.setState({action: ""})
          }
        })
        .catch(err => console.log(err))
    }else {
      console.log('input field required')
    }

}

handleChange = (e) => {
this.setState({
action: e.target.value
})
}

render() {
let { action } = this.state;
return (
<div>
<input type="text" onChange={this.handleChange} value={action} />
<button onClick={this.addTodo}>add todo</button>
</div>
)
}
}

export default Input
```

To make use of [Axios](https://github.com/axios/axios), which is a Promise based HTTP client for the browser and node.js, you need to cd into your client from your terminal and run yarn add axios or npm install axios.

Move to the src folder

`cd ..`

![Move to src folder](../images/moveto-src.png)

Move to clients folder

`cd ..`

![Move to clients folder](../images/moveto-client.png)

Install Axios

`npm install axios`

![Install Axios](../images/npm-installaxios.png)

Sip a coffee, click on the next button and let finish this up.

### FRONTEND CREATION (CONTINUED)

Go to ‘components’ directory

`cd src/components`

![Go to Components directory](../images/cd-srccomponents.png)

After that open your ListTodo.js

`nano ListTodo.js`

![Open ListTodo.js](../images/nano-ListTodojs.png)

![Paste code into ListTodo.js](../images/write-ListTodojs.png)

in the <mark>ListTodo.js</mark> copy and paste the following code

```
import React from 'react';

const ListTodo = ({ todos, deleteTodo }) => {

return (
<ul>
{
todos &&
todos.length > 0 ?
(
todos.map(todo => {
return (
<li key={todo._id} onClick={() => deleteTodo(todo._id)}>{todo.action}</li>
)
})
)
:
(
<li>No todo(s) left</li>
)
}
</ul>
)
}

export default ListTodo
```

Then in your <mark>Todo.js</mark> file you write the following code

![Open Todo.js and write code](../images/nano-componentsTodojs.png)

```
import React, {Component} from 'react';
import axios from 'axios';

import Input from './Input';
import ListTodo from './ListTodo';

class Todo extends Component {

state = {
todos: []
}

componentDidMount(){
this.getTodos();
}

getTodos = () => {
axios.get('/api/todos')
.then(res => {
if(res.data){
this.setState({
todos: res.data
})
}
})
.catch(err => console.log(err))
}

deleteTodo = (id) => {

    axios.delete(`/api/todos/${id}`)
      .then(res => {
        if(res.data){
          this.getTodos()
        }
      })
      .catch(err => console.log(err))

}

render() {
let { todos } = this.state;

    return(
      <div>
        <h1>My Todo(s)</h1>
        <Input getTodos={this.getTodos}/>
        <ListTodo todos={todos} deleteTodo={this.deleteTodo}/>
      </div>
    )

}
}

export default Todo;
```

We need to make little adjustment to our react code. Delete the logo and adjust our <mark>App.js</mark> to look like this.

Move to the src folder

`cd ..`

Make sure that you are in the src folder and run

`nano App.js`

![Open App.js and write code](../images/nano-appjs.png)

Copy and paste the code below into it

```
import React from 'react';

import Todo from './components/Todo';
import './App.css';

const App = () => {
return (
<div className="App">
<Todo />
</div>
);
}

export default App;
```

After pasting, exit the editor.

In the src directory open the <mark>App.css</mark>

`nano App.css`

![Open App.css and write code](../images/nano-Appcss.png)

Then paste the following code into <mark>App.css</mark>:

```
.App {
text-align: center;
font-size: calc(10px + 2vmin);
width: 60%;
margin-left: auto;
margin-right: auto;
}

input {
height: 40px;
width: 50%;
border: none;
border-bottom: 2px #101113 solid;
background: none;
font-size: 1.5rem;
color: #787a80;
}

input:focus {
outline: none;
}

button {
width: 25%;
height: 45px;
border: none;
margin-left: 10px;
font-size: 25px;
background: #101113;
border-radius: 5px;
color: #787a80;
cursor: pointer;
}

button:focus {
outline: none;
}

ul {
list-style: none;
text-align: left;
padding: 15px;
background: #171a1f;
border-radius: 5px;
}

li {
padding: 15px;
font-size: 1.5rem;
margin-bottom: 15px;
background: #282c34;
border-radius: 5px;
overflow-wrap: break-word;
cursor: pointer;
}

@media only screen and (min-width: 300px) {
.App {
width: 80%;
}

input {
width: 100%
}

button {
width: 100%;
margin-top: 15px;
margin-left: 0;
}
}

@media only screen and (min-width: 640px) {
.App {
width: 60%;
}

input {
width: 50%;
}

button {
width: 30%;
margin-left: 10px;
margin-top: 0;
}
}
```

Exit

In the <mark>src</mark> directory open the <mark>index.css</mark>

`nano index.css`

![Open index css](../images/nano-indexcss.png)

Copy and paste the code below:

```
body {
margin: 0;
padding: 0;
font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "Roboto", "Oxygen",
"Ubuntu", "Cantarell", "Fira Sans", "Droid Sans", "Helvetica Neue",
sans-serif;
-webkit-font-smoothing: antialiased;
-moz-osx-font-smoothing: grayscale;
box-sizing: border-box;
background-color: #282c34;
color: #787a80;
}

code {
font-family: source-code-pro, Menlo, Monaco, Consolas, "Courier New",
monospace;
}
```
Go to the <mark>Todo</mark> directory

`cd ../..`

When you are in the Todo directory run:

`npm run dev`

![Server running](../images/compilation-serverrunning.png)
![Todo app](../images/Todo-app.png)

Assuming no errors when saving all these files, our To-Do app should be ready and fully functional with the functionality discussed earlier: creating a task, deleting a task and viewing all your tasks.

Congratulations
In this Project #3 you have created a simple To-Do and deployed it to MERN stack. You wrote a frontend application using React.js that communicates with a backend application written using Expressjs. You also created a Mongodb backend for storing tasks in a database.

Credits:

1. [This guide was inspired by Digital Ocean](https://www.digitalocean.com/community/tutorials/getting-started-with-the-mern-stack)
2. [Educative.io](https://www.educative.io/answers/what-is-mern-stack)
