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

To run the migration, use the following command.
sequelize

db:migrate

If you need to undo the last migration, this command will undo the last migration that was applied to the database.
sequelize

db:migrate:undo

Use the psql shell to verify that your database and table was created:
psql
\l
\c userapp_development
\dt
