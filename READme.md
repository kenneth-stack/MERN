## WEB STACK IMPLEMENTATION (MERN STACK) IN AWS

### INTRODUCTION:

### What is the MERN stack?
The 
***MERN stack***
 is a web development framework made up of the stack of MongoDB, Express, React.js, and Node.js. It is one of the several variants of the 
MEAN stack.

***MERN***  Stands For:

- M- MongoDB
- E- ExpressJS
- R-ReactJS
- N-Node.js

When you use the MERN stack, you work with React to implement the presentation layer, 
Express and Node.js to make up the middle or application layer, and MongoDB to create the database layer.

In this MERN stack tutorial, we will utilize these four technologies to develop a basic **To-Do  application** that is able to create a to-do list.

### Setting up the project

MERN lets us create full-stack solutions. So, to leverage its full potential, we will be creating a MERN stack project. For this project, we will create both a back end and a front end. The front end will be implemented with React and the back end will be implemented with MongoDB, Node.js, and Express. We will call the front end client and the back end server.


## Step 1 - BackEnd Configuration
__1.__ __Update ubuntu__
```
sudo apt update
```

__2.__ __Upgrade ubuntu__
```
sudo apt upgrade
```

__3.__ __location of Node.js__
```
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
```

__4.__ Install Node.js on the server
```
sudo apt-get install -y nodejs
```

__5.__ Verify the node installation with the command 
```
node -v
or 
npm -v
```

### Application Code Setup

__6.__ Create a new directory for your To-Do project
```
$ mkdir Todo
```

__7.__ Verify the **Todo** directory is created with ***ls*** commond
```
$ ls
```

**Tip**: In order to see some more useful information about files and directories, you can use following combination of keys *ls-lih-*. It will show you different properties and size in human readable format. You can learn more anout different useful keys for ls comman with *ls--help*

__8.__ Change your current directory to the newly created one
```
$ cd Todo
```

__9.__ Initialise the project 

```
npm init
```
___NB: A new file package.json will be created.___


## Step 2: Install ExpressJS

__1.__ To use express, install it using npm
```
$ npm install express
```

__2.__ Create a file ***index.js*** with the command bellow
```
 $ touch index.js
```

__3.__ Run ls to confirm that your index.js file is sucessfully created

__4.__ Install the dotenv module
```
$ npm install dotenv
```

__5.__ Open the index.js file with the command bellow
```
$ vim index.js
```

Type the code below into it and save.
```
const express = require('express');
require('dotenv').config();

const app = express();

const port = process.env.PORT || 5000;

app.use((req, res, next) => {
  res.header("Access-Control-Allow-Origin", "\*");
  res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
  next();
});

app.use((req, res, next) => {
  res.send('Welcome to Express');
});

app.listen(port, () => {
  console.log(`Server running on port ${port}`);
});
```
__NB: use `:w` to save in vim and use `:qa` to exit_


__5.__  Start the server to see if it works. 

```
$ node index.js
```

you should see`server running on port 5000` in your terminal

__6.__ Open up your browser and try to access your server's Public IP followed by port `5000`

```
http://localhost:5000
```

![express_webpage](/images/express_site.png)

## Step 2 - Creating the Routes

There are three actions that our To-Do application needs to be able to do:

- Create a task
- Display list of all tasks
- Delete a completed task

Each task will be associated with some particular endpoint and will use differnet standard HTTP request menthods: POST, GET, DELETE.

For each task, I created `routes` that will define various endpoints that the `To-do` app will depend on. 

__1.__ Create a folder `routes`.
```
$ mkdir routes 
```

__2.__ Change directory to `routes` folder.
```
$ cd routes 
```

__3.__ Create a file `api.js` and open the file then write the code below.
```
$ touch api.js
```
__4.__ Open the file with the comand below
```
$ vim api.js
```
__5.__ Copy below code in the file
```
const express = require('express');
const router = express.Router();

router.get('/todos', (req, res, next) => {

});

router.post('/todos', (req, res, next) => {
 
});

router.delete('/todos/:id', (req, res, next) => {
 
})

module.exports = router;
```

## Step 3 - Define the Models

This app makes use of MongoDB, we need to create a model and a schema. <br>A Model is at the heart of JavaScript based applicationa, and it's what makes it interactive. It is also use to defined database schema. <br> The schema is a blueprint of how the database will be constructed. 

To create a schema and a model, install Mongoose which is a Node package that makes working with MongoDB easier.

__1.__ Install `mongoose`

```
npm install mongoose
```

__2.__ Create a new folder in your root directory and name it `models`. Inside it create a file and name it `todo.js` with the following code in it:

```
$ mkdir models && cd models && touch todo.js
```

__3.__ Open the file created with `vim todo.js` then paste the code below in the file

Now, we need to update our routes in api.js to make use of the new model.
```
const mongoose = require('mongoose');
const Schema = mongoose.Schema;

// Create schema for todo
const TodoSchema = new Schema({
  action: {
    type: String,
    required: [true, 'The todo text field is required']
 }
})

// Create model for todo
const Todo = mongoose.model('todo', TodoSchema);

module.exports = Todo;
```

__4.__ Now, we need to update our routes in api.js to make use of the new model.

In the Routes directory, Open `api.js`

```
vim api.js
```

Paste this in `routes/api.js`:

```
conconst express = require('express');
const router = express.Router();
const Todo = require('../models/todo');

router.get('/todos', (req, res, next) => {


  // This will return all the data, exposing only the id and action field to the client
  Todo.find({}, 'action')
    .the(data => res.json(data))
    .catch(next);
});

router.post('/todos', (req, res, next) => {
  if (req.body.action) {
    Todo.create(req.body)
      .then(data => res.json(data))
      .catch(next);
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
                    
```

## Step 4 - Connecting to a Database

__1.__ [Sign Up](https://www.mongodb.com/products/try-free/platform/atlas-signup-from-mlab)  to mongoDB
   
  ![vim_api_edit](/images/Database1.png)

__2.__  Create a cluster, select AWS as the cloud provider and choose a region near you. 

![vim_api_edit](/images/Database1.png)

__3.__ Then your Cluster is created.

![mongo_](/images/database2.png)

__4.__  Create a user and give it admin access. Click on `database Access` on the left sidebar.

![mongos_admin](/images/database4.png)

__5.__ Click on `Browse collection`, to create a database.

![mongoDB_DB](/images/database5.png)

__5.__ Edit your IP Access List Entry to anywhere to access your database

![mongoDB_DB](/images/database6.png)

__6.__ Click `Connect` to connect your Database with a Driver. Select `mongoose` and copy the `Connection strings` 
  <br></br>
![mongoose_connect](/images/database7.png)



__7.__ Create a file in the `Todo` directory and name it `.env`

```
$ touch .env

```

__7.__ Open the file with vim editor

```bash
vi .env

# Paste your database connection string in it 

DB = 'mongodb+srv://<username>:<password>@<network-address>/<dbname>?retryWrites=true&w=majority'
```
Ensure to update `<username>` `<password>` `<network-address>` and  `<database>` according to your setup.


__8.__ Update `index.js` to reflect the use of `.env` so that Node.js can connect to the DB. 

```
vim index.js
```

Paste this:

```
const mongoose = require('mongoose');
const routes = require('./routes/api');
const path = require('path');
require('dotenv').config();

const app = express();

const port = process.env.PORT || 5000;

// Connect to the database
mongoose.connect(process.env.DB, { useNewUrlParser: true,useUnifiedTopology: true })
  .then(() => console.log(`Database connected successfully`))
   .catch((err) => console.log(err));

// Since mongoose's Promise is deprecated, we override it with Node's Promise
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

__8.__  Start Nodejs Server

```bash
node index.js
```

## Step 5 - Test Backend Code without Frontend using RESTful API

So far we have written backend part of out  **To-Do** application, and configured a database, but we do not have a frontend UI yet. We need a ReactJS code to achieve that. 

__1.__  You'll need to install [Postman](https://www.getpostman.com) or you can use VScode Extension called ThunderClient. 


Next, we open send an API request, and create a POST request to our API(make sure your server is running on your terminal)

__2.__  Create a POST method  and navigate to `http://publicIP:5000/api/todos.` and type this in the `body`

```
POST publicIP:5000/api/todos/
```

```
{
  "action": "Database is to be connected"
}
```
![post](/images/postman.png)
```
{
  "action": "Ensuring Database is updated"
}
```
![post](/images/postman2.png)

__3.__ To list all the POST request, we run the GET method

```
GET publicIP:5000/api/todos/
```
```
{
  "action": "Database is confirmed"
}
```
![post](/images/get_postman.png)

__4.__  To Delete a specific POST require, we run this on the DELETE method

```
Delete publicIP:5000/api/todos/<id>
```
![post](/images/delete_postman.png)

## STEP 6 - Create the FrontEnd

it is time to create an interface for the client to interact with the API. To start out with the frontend of the todo app, you will use the `create-react-app` command to scaffold your app.
In the same `todo` directory

__1.__  Run this command

```
npx create-react-app client
```

__2.__ Install concurrently 
Concurrently is used to run more than one command simultaneously from the same terminal window. 

```
npm install concurrently --save-dev
```

__3.__ Install nodemon 
Nodemon is used to run the server and monitor it as well. If there is any change in the server code, Nodemon will restart it automatically with the new changes.

```
npm install nodemon --save-dev
```

__4.__Configure nodemon in the package.json in the `Todo `directory

```
nano package.json
```

Paste this in the Scripts block:
```
{
  // ...
  "scripts": {
    "start": "node index.js",
    "start-watch": "nodemon index.js",
    "dev": "concurrently \"npm run start-watch\" \"cd client && npm start\""
  },
  // ...
}
```

__4.__ Configure Proxy in package.json located in the `Client` directory.

```
$ cd client
vi package.json
```

then add

```bash 
{
  
  "proxy": "http://localhost:5000"
}
```
![proxy](/images/proxy.png)

***NOTE:*** The whole purpose of adding the proxy configuration in number 3 above is to make it possible to access the application directly from the browser by simply calling the server url like http://localhost:5000 rather than always including the entire path like http://localhost:5000/api/todos.

__5.__ Run `npm run dev` and make sure you are in the todo directory and not in the client directory.

```
npm run dev
```

_Your app will be open and running on PublicIP:3000_


## Step 7 — Creating the React Components

For your todo app, there will be two state components and one stateless component.

__1.__ Inside your `src` folder create another folder called  `components` and inside it create three files `Input.js`, `ListTodo.js`, and `Todo.js`.

```
$ cd client && cd src && mkdir components && cd components

$ touch Input.js ListTodo.js Todo.js
```
__2.__ Open Input.js
```bash
vim Input.js
```

```
import React, { Component } from 'react';
import axios from 'axios';

class Input extends Component {
  state = {
    action: " "
  }

  addTodo = () => {
    const task = { action: this.state.action }

    if (task.action && task.action.length > 0) {
      axios
        .post('/api/todos', task)
        .then((res) => {
          if (res.data) {
            this.props.getTodos();
            this.setState({ action: "" })
          }
        })
        .catch((err) => console.log(err));
    } else {
      console.log('input field required');
    }
  }

  handleChange = (e) => {
    this.setState({
      action: e.target.value,
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

_To make use of axios, which is a Promise-based HTTP client for the browser and Node.js, you will need to navigate to your client directory from your terminal:_

```
$ cd client
```

Then run 

```
npm install axios
```

__3.__ Go back to the component directory

```bash 
$ cd src/components
```

__4.__ Open `ListTodo.js` with vim text editor 

```
nano ListTodo.js
```
```
import React, { Component } from 'react';
import axios from 'axios';

import Input from './Input';
import ListTodo from './ListTodo';

class Todo extends Component {
  state = {
    todos: []
  }

  componentDidMount() {
    this.getTodos();
  }

  getTodos = () => {
    axios.get('/api/todos')
      .then(res => {
        if (res.data) {
          this.setState({
            todos: res.data
          });
        }
      })
      .catch(err => console.log(err));
  }

  deleteTodo = (id) => {
    axios.delete(`/api/todos/${id}`)
      .then(res => {
        if (res.data) {
          this.getTodos();
        }
      })
      .catch(err => console.log(err));
  }

  render() {
    let { todos } = this.state;
    return (
      <div>
        <h1>My Todo(s)</h1>
        <Input getTodos={this.getTodos} />
        <ListTodo todos={todos} deleteTodo={this.deleteTodo} />
      </div>
    );
  }
}
export default Todo;
```


__5.__ Adjust the React code by Deleting the logo and adjust `App.js` to look like this:

```
$ cd ..
```

Open App.js with nano text editor

```
vim App.js
```
Paste this:

```bash

import React from 'react';
import Todo from './components/Todo';
import './App.css';

const App = () => {
  return (
    <div className="App">
      <Todo />
    </div>
  );
};

export default App;
```

__6.__ In the src directory, open the  `App.css`

```
vim App.css
```
Paste the following code into it
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
    width: 100%;
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

__7.__ In the src directory, open the  `index.css`

```
vim index.css
```

Paste this:

```
body {
  margin: 0;
  padding: 0;
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "Roboto", "Oxygen", "Ubuntu", "Cantarell", "Fira Sans", "Droid Sans", "Helvetica Neue", sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  box-sizing: border-box;
  background-color: #282c34;
  color: #787a80;
}

code {
  font-family: source-code-pro, Menlo, Monaco, Consolas, "Courier New", monospace;
}
```

__8.__  Go back to the `Todo` directory 

```
$ cd  ..    #Do it twice
```

__9.__  On the Todo directory run

```bash
npm run dev
```

__10.__ React build would compile sucessfully and both backend and front end will run concurrently through the ports we earlier opened

Open in the browser http//publicIP:3000

![todolist](/images/my%20_todo.png)

**Note** To make sure it is working, let us type a word on the insert then press add todo, it should reflect on the list


![todolist](/images/todolist_test.png)


We have successfully developed a simple To-Do application and deployed it using the MERN stack, which consists of MongoDB, Express.js, React.js, and Node.js. The frontend is built with React.js, providing a dynamic user interface that communicates seamlessly with the backend powered by Express.js. This setup allows for efficient handling of requests and responses. Additionally, you’ve implemented a MongoDB database to store tasks, ensuring data persistence and easy retrieval. This project showcases your ability to integrate multiple technologies to create a fully functional web application.