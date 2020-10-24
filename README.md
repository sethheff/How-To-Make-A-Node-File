# How-To-Make-A-Node-File
Install Node
We will use Homebrew to install Node via the command line.

1. update Homebrew with the latest version of Node
brew update

2. install Node on your machine (this may take a few minutes)
brew install node

3. verify that Node was installed by checking the version that is on your machine
node -v


Setting up a Node Project
1. Create a new folder for your first node project.
mkdir my-first-node-proj

Open the folder in your favorite text editor.

2. Initialize Node inside the project folder.
(check the command line prompt to make sure you're actually inside the project folder)
npm init

You will be prompted to enter values for a number of fields to set up the node project. You can just press enter to accept the default value, or enter specific values if you'd like.

To skip this step in the future (and accept all default values at once), type npm init -y.

Take a look at the package.json file that was just created. This is where the values we just set up via npm init are stored. This is like your settings file. You can edit these values by changing them in this file (make sure to save).

3. Make your entry point.

Unless you specified a different file name in setup (check the main value in package.json), Node will look for a file called index.js as the entry point for running your project. This file holds the code to be executed - this is the heart of your program. Create this file now.
touch index.js

Let's write some code in here and run it to see Node in action! Write the following line to your index.js:
console.log("Hello world!")

4. Run your program!

To run a file in node via the command line, type node [file name here].
node index.js
Congratulations, you've created and run your first Node program! Let's learn more about how Node is used in the wild (and eventually, in the web-development context).


HOW TO CREATE AN EXPRESS APP 

1. CD into your directory and run.. npm init -y to create a json file inside our document.

then from your local terminal, run 

1-5. Add a .js file

2. npm i express to install express

once installed, it will add a 'dependencies' object in your json file. It tells you what your express version is. It also adds a package-lock.json file which contains the files needed for express.

Your node_modules file is too big to upload all the time. Because of this we want to add a new file called .gitignore

3. add .gitignore file and add (in plain text) the name of the files you dont want to send to git .

4. add node_modules to .gitignore

5. To add a hello world home route write, add to your js file: 

'''
app.get('/', (req,res)=>{
    res.send('Hello, World!')
    
})

// to get your file to listen to a particular port 

app.listen(8000, () =>{
    console.log("You're listening to the smooth sounds of port 8000.") 
})
'''

app.get(,) is how you connect to the domain you want (ie localhost:8000), after working on a local server in private

the '/' portion of the app.get file is called a URL route

example: https://www.reddit.com (= base url) /search (= url pattern) ?q=cute%20puppies (= url string)

6. Run nodemon in your local terminal to check express connection and/or go to browser and look up localhost:8000


--------VIEWS---------
We're going to create a personal website with the following pages:
```
Home
About
Blog
Objectives:
```
Practice stubbing out routes
Learn how to set up views
Prerequisites:

Know how to write a GET route in Node/Express
Part 1: ROUTES

We're going to stub out the routes for the home, about, and blog pages!

1. Set up your app Create a new Node/Express project.

2. Create the following routes:

Make sure the message displays on the page when you navigate to the appropriate URL.
```
Route
HTTP Verb
URL Pattern
Message
Home
GET
/
"This is the Home Page!"
About
GET
/about
"Some stuff about me will go here."
Blog
GET
/blog
"A directory of all my blog posts will go here."
Part 2: Views
```

Writing text to a web page using ``` res.send()``` gives us something to look at, but isn't very pretty. Instead of sending plain text, let's start serving HTML files. Since each page will display different HTML, we'll have several HTML files, or views.

1. Create a views folder inside your project directory. Inside this folder, create three HTML files:
```
index.html (this is the standard filename for the view associated with the base URL)
about.html
blog-directory.html
```
2. Put some basic HTML in these files so you can test them.

3. In your routes, replace the res.send(<message>) with res.sendFile(<absolute file path>) (docs)

```res.sendFile()``` takes an absolute path, so we can't just give it ```./views/index.html.``` How do you get the absolute file path for each of these files? Don't know/don't care? You're in luck! __dirname is a Node keyword that gives us the absolute path of the current directory (docs), so we can just tack that on in front of the relative path

Your home route should look like this:
```
app.get('/', (req, res) {
  res.sendFile(__dirname+'/views/index.html');
});
```
Test it out to see if your HTML shows up!

Make sure to do this for all three routes. Now check to see that your html is coming through on the browser.


---- TEMPLATES ----

Pre-reqs:

Express Personal Website from views lesson

Template Engines

The downside to this method is that we are only sending HTML files, but what if we want to customize what's on the page? On the front-end, we could manipulate the DOM with Javascript, that's certainly an option! But what if we want to display data that we pull from a database? Template engines allow us to inject values into the HTML, and even script logic into the HTML. This will be extremely useful for building in CRUD functionality and full stack apps in general. docs
EJS: Embedded Javascript

There are several javascript template engines for express, one of the most popular of which, is EJS, available via npm. Let's update our express personal website views with EJS.

Install EJS

Add EJS to your personal website project using npm:
```npm i ejs```

Set the view engine to EJS

Above your routes, add ```app.set(name, value) (docs)``` where the name is view engine and the value is ejs.
```
app.set('view engine', 'ejs');
```
This tells express that we'll be using ejs as our view engine.
Adapt your routes to ejs

1. Rename the .html files to .ejs files.
2. Replace your res.sendFile(<absolute path>) statements with res.render(<file name>) statements.
3. Ejs assumes a lot about the path to the template files, so as long as they are nested in a views folder and have .ejs extensions, you can simply pass the filename (no extension, though it wont break it if you include it) into res.render().
Your home route should look like this:
app.get('/', (req, res)=>{
  res.render('index.ejs');
});
    
Note: you can even leave off the .ejs because express knows to look for ejs files.

app.get('/', (req, res)=>{
  res.render('index');
});

The Cool Part: Templating with Variables

Templating with variables means we can pass in an object to res.render() and access the values stored in it as variables inside the ejs template.
This is best demonstrated with an example. Create an object with at least one key-value pair and pass that object in as the second argument to the render function in one of your routes:

index.js

app.get('/', function(req, res) {
  res.render('index', {name: "Sterling Archer", age: 35});
});

We now have access to a name variable inside our index.ejs file! We can access this variable by embedding it into the html using this notation: <%= embedded js goes here %>.

For example: index.ejs
```
<!DOCTYPE html>
<html>
  <head>
    <title>Home Page</title>
  </head>
  <body>
    <h1>Hello, <%= name %>!</h1>
  </body>
</html>
```
The any JavaScript can be embedded using the <% %> tags. The addition of the = sign on the opening tag means that a value will be printed to the screen.
```
<!DOCTYPE html>
<html>
  <head>
    <title>Home Page</title>
  </head>
  <body>
    <h1>Hello, <%= name %>!</h1>
    <% let dogAge = age*7 %>
    <h2>You are <%= dogAge %> in dog years.</h2>
  </body>
</html>
```
<% %> without the = will not print out the expression, but it will execute it. This comes in handy for if statements and loops.
This doesn't only apply to primitive variables. We can even include variable declarations and iterators using ejs.
```
<!DOCTYPE html>
<html>
  <head>
    <title>Home Page</title>
  </head>
  <body>
    <h1>Hello, <%= name %>!</h1>
    <% let dogAge = age*7 %>
    <h2>You are <%= dogAge %> in dog years.</h2>
    <% let status %>
    <%if (dogAge<100) {%>
      <% status = 'young' %>
    <%} else {%>
      <% status = 'old' %>
    <% } %>
    <h3>This means you are <%=status%>!</h3>
  </body>
</html>
```

Notice that ejs requires ejs tags (<% %>, also called alligators) around each line of the javascript.

Partials

Partials can be used to modularize views and reduce repetition. A common pattern is to move the header and footer of a page into separate views, or partials, then render them on each page.

Create the partials example

In the main directory of your project, create a partials folder that includes a header.ejs file.
```
partials/header.ejs
  <header>
    <img src="http://placekitten.com/500/500">
  </header>
Include your partial
index.ejs
<!DOCTYPE html>
<html>
  <head>
    <title>Home Page</title>
  </head>
  <body>

    <%- include('../partials/header.ejs') %>

    <h1>Hello, <%= name %>!</h1>
.
.
.

```

------ LAYOUTS ---------
Express gives us a lot of flexibility out of the box (configuration over convention). While this is a good thing, it can become a problem when we don't take the time to organize our project.

Objectives

Utilize layouts and controllers in an Express app

Prereqs:
ability to create a basic node/express app with basic routes and views
Set Up a new Express App

Before we do anything else, let's set up a new basic Express app called love-it-or-leave-it.
         
1. Create a new project

2. Initialize Node

3. Install Dependencies

express
ejs

4. Set up Express
index.js file
require express
create an instance of express
tell the app which port to listen to

5. Set up EJS
set view engine to ejs
create a views folder
EJS Layouts

Adding partials can dry up the code a bit, but EJS Layouts can take this modularity even farther and make a big diffence with large applications.
EJS layouts is a node package that allows us to create a boilerplate (aka a layout) that we can inject content into based on which route is reached. Layouts normally include header and footer content that you want to display on every page (navbar, sitemap, logo, etc.).

Install EJS Layouts

Step 1: Install EJS layouts
Install ```express-ejs-layouts via npm
npm i express-ejs-layouts```

Step 2: Set up EJS layouts
Require the module and add it to the app.
index.js
```
var express = require('express');
var app = express();
var ejsLayouts = require('express-ejs-layouts');

app.set('view engine', 'ejs');
app.use(ejsLayouts);

app.listen(3000)
```
What is app.use()? This is an express function that indicates middleware. Middleware functions intercepts the request object when it comes in from the client, but before it hits any route. We'll see more examples of middleware later.
How are you supposed to know that ejs layouts requires middleware? The docs.
Step 3: Create a Layout
In the root of the views folder, add a layout called layout.ejs. It must be called layout.ejs, as mandated by express-ejs-layouts.
layout.ejs
```
<!DOCTYPE html>
<html>
<head>
  <title>Love It or Leave It</title>
</head>
<body>
  <%- body %>
</body>
</html>
```
This layout will be used by all pages, and the content will be filled in where the <%- body %> tag is placed. <%- body %> is a special tag used by express-ejs-layouts that cannot be renamed.

Step 4: Use the Layout
In the views folder, create a home.ejs file:

home.ejs

<h1>This is the home page!</h1>
Now create a home route in index.js below the middleware:
app.get('/', (req, res) => {
  res.render('home');
});

Ejs will assume that home means home.ejs. Now starte nodemon and check that your home page renders as desired.

Step 5: Set up a few more views/routes
index.js
```
app.get('/animals', (req, res) => {
  res.render('animals', {animals: ['sand crab', 'corny joke dog']})
});
animals.ejs
<h1>Favorite Animals</h1>
<ul>
  <% animals.forEach((animal) => { %>
    <li><%= animal %></li>
  <% }) %>
</ul>
```   
Visit localhost:3000/animals to make sure that all is well.


Add a simple navigation list to the top of the layout page so there's a link to every page from every page:
layout.ejs
```
<!DOCTYPE html>
<html>
<head>
  <title>Love It or Leave It</title>
</head>
<body>
  <ul>
    <li><a href='/foods'>Favorite Foods</a></li>
    <li><a href='/animals'>Favorite Animals</a></li>
    <li><a href='/movies'>Worst Movies</a></li>
    <li><a href='/products'>Worst Products</a></li>
  </ul>
  <%- body %>
</body>
</html>
```
Controllers & Express Router

Controllers become important organizational tools when you start making apps with several views, so let's organize the routs/views we have into two sections: love-it and leave-it.

1. Change your routes to have the following url patterns:
```
/loveit/food
/loveit/animals
/leaveit/movies
/leaveit/products
```
Now check that these new url patterns render the expected html, and fix your nav bar to have the correct links.

1. Inside the views folder, create a love-it folder and move your foods.ejs and animals.ejs files into it.
We have been placing all routes into index.js when creating a Node/Express app, but this can get cumbersome when dealing with many routes. The solution is to group related routes and separate these groups into separate files. These files will go into a controllers folder.

2. Create a controllers folder inside the root directory that will contain all routes except for the home route.

3. Inside the controllers folder, create a file called loveit.js, and copy your two loveit routes into this file.
with the following routes:
```
app.get('/lovit/foods', (req, res) => {
  res.render('foods', {foods: ['coconut', 'avocado']});
});
```
```
app.get('/loveit/animals', (req, res) => {
  res.render('animals', {animals: ['sand crab', 'corny joke dog']})
});
```

But wait! app doesn't exist in this file! Express has a Router() function that will help us wrap these routes into a module that we'll export back into our main server file.

5. Add these wrapper lines of code to loveit.js, and replace app with router.
```
const express = require('express');
const router = express.Router();
```
```
router.get('/loveit/foods', (req, res) => {
  res.render('foods', {foods: ['coconut', 'avocado']});
});
```
```
router.get('/loveit/animals', (req, res) => {
  res.render('animals', {animals: ['sand crab', 'corny joke dog']})
});
```
```
module.exports = router;
```
6. Now back in index.js, we just need to add some middleware to get these routes working again!
index.js
```
app.use('/loveit', require('./controllers/loveit'));
```
This middelware says "Dear Express, if you get a request for a url pattern that starts with /loveit, please to to the loveit controller file to find the relevant routes." SO, by the time express is looking in the right controller file, it already has processed the /loveit part of the url pattern, thus, we can now remove that part from the routes in controllers/loveit.js:
```
const express = require('express');
const router = express.Router();
```
```
router.get('/foods', (req, res) => {
  res.render('foods', {foods: ['coconut', 'avocado']});
});
```
```
router.get('/animals', (req, res) => {
  res.render('animals', {animals: ['sand crab', 'corny joke dog']})
});
```
```
module.exports = router;
```
Note that we defined the routes relative to the definition in app.use. In other words, take note that our URL patterns in loveit.js don't inclued '/loveit', because that is taken care of by the middleware.
Check that these routes are working by visiting http://localhost/loveit/foods and http://localhost/loveit/animals.
Finally, it is standard to organize your views the same way you organize your routes. This means we should create a subdirectory inside of views called loveit and move our animals.ejs and food.ejs files into it.
We also need ot update our res.render() lines accordingly:
const express = require('express');
const router = express.Router();

```
router.get('/foods', (req, res) => {
  res.render('loveit/foods', {foods: ['coconut', 'avocado']});
});
```
```
router.get('/animals', (req, res) => {
  res.render('loveit/animals', {animals: ['sand crab', 'corny joke dog']})
});
```
```
module.exports = router;
```

ENV 

Next we want to make sure we have config variables. This is Heroku's version of a .env file or environment variables. Any variable names that are found in your local .env file should have a corresponding variable on Heroku.

npm i env

GITIGNORE 

Add a .gitignore file and add (in plain text)``` node_modules and .env``` so that these massive files dont get pushed back to github


SET UP SEQUELIZE 


Setup part 1 - getting the sequelize-cli tool (you only have to do this once)

We need install a generator so we can use sequelize. Much like our other terminal apps, we will only install this once.
npm install -g sequelize-cli

Setup part 2 - starting a new node project

Let's build our first app using Sequelize! First we need to create a node app and include our dependencies. All in terminal:
Create a new folder and add an index.js and .gitignore and initialize the repository


```
mkdir userapp

cd userapp

npm init -y

touch index.js

echo "node_modules" >> .gitignore
``` 

Add/save dependencies (sequelize needs pg for Postgres)

```
npm install pg sequelize
```
Initialize a sequelize project
```
sequelize init
```
For your historical reference...

WARNING (2017) Edited (2018): At one point, sequelize-cli, sequelize, and pg modules were not playing nicely with each other. Luckily, this issue (for version Sequelize 4) has been resolved and we can resume using the current versions of both. In the future, be mindful that many modules you use are maintained by individual third parties and issues like this may come up!

If you used to use Sequelize 3, keep in mind that Sequelize 4 has breaking changes! If you need to upgrade your app, refer to these docs, which guide you in the update process.

Setup part 3 - config.json, models and migrations:

In the text editor we should now see a bunch of new folders. We now have config, migrations and models. This was created for us when we ran sequelize init.
Let's start in the config folder and open up the config.json file. This file contains information about the database we are using as well as how to connect.
We have three settings, one for development (what we will use now), test (for testing our code), and production (when we deploy our app on AWS/Heroku).
Let's change the config.json so it looks like this.
```
config/config.json
{
  "development": {
    "database": "userapp_development",
    "host": "127.0.0.1",
    "dialect": "postgres"
  },
  "test": {
    "database": "userapp_test",
    "host": "127.0.0.1",
    "dialect": "postgres"
  },
  "production": {
    "database": "userapp_production",
    "host": "127.0.0.1",
    "dialect": "postgres"
  }
}
```
If the dialects defaults to mySql, change them to postgres
change the database names

If you have a username and password for your Postgres server, you must include those as well

When we deploy to Heroku, they will provide us a long url that contains password and login that will be secure when deployed. More on this later.
Once this is complete, let's move to the models folder.

Create a database inside of Postgres

The sequelize CLI has an equivalent command to createdb. You can use either, they do the same thing!

sequelize db:create userapp_development

Create a model and a matching migration

In order to create a model, we start with sequelize model:create and then specify the name of the model using the --name flag. Make sure your models are always singular (table name in plural, model name in singular). See the Table Name Inference section of [these docs](https://sequelize.org/master/manual/model-basics.html#:~:text=Models are the essence of,(and their data types)for more. After passing in the --name flag followed by the name of your model, you can then add an --attributes flag and pass in data about your model. Generating the model also generates a corresponding migration. You only need to do this once for your model.

```
sequelize model:create --name user --attributes firstName:string,lastName:string,age:integer,email:string
```

Make sure you do not have any spaces between each of the attributes and their data types. Convention matters!
This will generate the following model:
```
models/user.js
'use strict';
const {
  Model
} = require('sequelize');
module.exports = (sequelize, DataTypes) => {
  class user extends Model {
    /**
     * Helper method for defining associations.
     * This method is not a part of Sequelize lifecycle.
     * The `models/index` file will call this method automatically.
     */
    static associate(models) {
      // define association here
    }
  };
  user.init({
    firstName: DataTypes.STRING,
    lastName: DataTypes.STRING,
    age: DataTypes.INTEGER,
    email: DataTypes.STRING
  }, {
    sequelize,
    modelName: 'user',
  });
  return user;
};
```

If you want to make changes to your model after generating it - all you have to do is make a change in this file and save it before running the migrate command.
And a corresponding migration:
```
migrations/*-create-user.js
'use strict';
module.exports = {
  up: async (queryInterface, Sequelize) => {
    await queryInterface.createTable('users', {
      id: {
        allowNull: false,
        autoIncrement: true,
        primaryKey: true,
        type: Sequelize.INTEGER
      },
      firstName: {
        type: Sequelize.STRING
      },
      lastName: {
        type: Sequelize.STRING
      },
      age: {
        type: Sequelize.INTEGER
      },
      email: {
        type: Sequelize.STRING
      },
      createdAt: {
        allowNull: false,
        type: Sequelize.DATE
      },
      updatedAt: {
        allowNull: false,
        type: Sequelize.DATE
      }
    });
  },
  down: async (queryInterface, Sequelize) => {
    await queryInterface.dropTable('users');
  }
};
```
What is this "associate" thing in my model?

In this function, we specify any relations/associations (one to one, one to many or many to many) between our models (hasMany or belongsTo). We'll discuss this more, but always remember, the association goes in the model and the foreign keys go in the migration.

Running the migration

To run the migration, use the following command in your VSCODE terminal:
```
sequelize db:migrate
```

If you need to undo the last migration, this command will undo the last migration that was applied to the database.

```
sequelize db:migrate:undo
```

Use the psql shell to verify that your database and table was created:

```
psql
\l
\c userapp_development
\dt
```
