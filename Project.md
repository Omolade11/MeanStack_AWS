# WEB STACK IMPLEMENTATION (MEAN STACK) IN AWS
In this project, we will implement a web solution based on the MEAN stack in AWS Cloud. MEAN Web stack consists of the following components:

1. MongoDB: A document-based, No-SQL database used to store application data in a form of documents, (Document database) – Stores and allows retrieval of data.
2. ExpressJS:(Back-end application framework) – Makes requests to Database for Reads and Writes.
3. Angular (Front-end application framework) – Handles Client and Server Requests.
4. Node.js: A JavaScript runtime environment. It is used to run JavaScript on a machine rather than in a browser – Accepts requests and displays results to end user.

Project Prequisite: AWS EC2 instance.

## Steps Involved
1. Install NodeJs
2. Install MongoDB
3. Install Express and set up routes to the server
4. Access the routes with AngularJS

## Implementation
Since we are using the Ubuntu 20.04 AMI of AWS EC2 instance to host our application. On our terminal, we will change directory to the folder we have our .pem file. In this case, it is in the Downloads folder

```
cd ~/Downloads
```

Now, we will change permissions for the private key file (.pem), otherwise we will get the error “Bad permission”

```
sudo chmod 0400 <private-key-name>.pem
```

We will then connect to the instance by running 

```
ssh -i <private-key-name>. pem ubuntu@<Public-IP-address>
```

## Install NodeJS
Node.js is a JavaScript runtime built on Chrome’s V8 JavaScript engine. Node.js is used in this tutorial to set up the Express routes and AngularJS controllers.
Update Ubuntu
```
sudo apt update
```
Now, we will upgrade ubuntu by running
```
sudo apt upgrade

```
Next, we will install Node.js version 14.x
We will run the following command as a user with sudo privileges to download and execute the NodeSource installation script:

```
curl -sL https://deb.nodesource.com/setup_14.x | sudo -E bash -
```
Afterward, we will Install NodeJS and npm
```
sudo apt install nodejs
```
Next, we will verify that the Node.js and npm were successfully installed by printing their versions:
```
node --version
```
```
npm --version
```
## Install MongoDB
MongoDB stores data in flexible, JSON-like documents. Fields in a database can vary from document to document and data structure can be changed over time. For our example application, we are adding book records to MongoDB that contain book name, isbn number, author, and number of pages.

```
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6
echo "deb [ arch=amd64 ] https://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list
```
### Install MongoDB and NPM
```
sudo apt install -y mongodb
```
We will start The server by running
```
sudo service mongodb start
```
Verify that the service is up and running
```
sudo systemctl status mongodb
```
After running the command, our result should be similar to the image below:
![](https://github.com/Omolade11/MeanStack_AWS/blob/main/Images/Screenshot%202022-12-20%20at%2014.08.00.png)
### Install body-parser package
We need ‘body-parser’ package to help us process JSON files passed in requests to the server.
```
sudo npm install body-parser
```
We will create a folder named ‘Books’
```
mkdir Books && cd Books
```
In the Books directory, Initialize npm project
```
npm init
```
Like the image below, click 'enter' key or type in the value you prefer
![](https://github.com/Omolade11/MeanStack_AWS/blob/main/Images/Screenshot%202022-12-20%20at%2014.45.32.png)
Next, we will add a file to it named server.js
```
vi server.js
```
Copy and paste the web server code below into the server.js file.
```
var express = require('express');
var bodyParser = require('body-parser');
var app = express();
app.use(express.static(__dirname + '/public'));
app.use(bodyParser.json());
require('./apps/routes')(app);
app.set('port', 3300);
app.listen(app.get('port'), function() {
    console.log('Server up: http://localhost:' + app.get('port'));
});
```

## Install Express and set up routes to the server
Express is a minimal and flexible Node.js web application framework that provides features for web and mobile applications. We will use Express to pass book information to and from our MongoDB database.
We also will use Mongoose package which provides a straightforward, schema-based solution to model your application data. We will use Mongoose to establish a schema for the database to store data of our book register.
```
sudo npm install express mongoose
```
In ‘Books’ folder, create a folder named apps
```
mkdir apps && cd apps
```
Create a file named routes.js
```
vi routes.js
```
Copy and paste the code below into routes.js
```
var Book = require('./models/book');
module.exports = function(app) {
  app.get('/book', function(req, res) {
    Book.find({}, function(err, result) {
      if ( err ) throw err;
      res.json(result);
    });
  }); 
  app.post('/book', function(req, res) {
    var book = new Book( {
      name:req.body.name,
      isbn:req.body.isbn,
      author:req.body.author,
      pages:req.body.pages
    });
    book.save(function(err, result) {
      if ( err ) throw err;
      res.json( {
        message:"Successfully added book",
        book:result
      });
    });
  });
  app.delete("/book/:isbn", function(req, res) {
    Book.findOneAndRemove(req.query, function(err, result) {
      if ( err ) throw err;
      res.json( {
        message: "Successfully deleted the book",
        book: result
      });
    });
  });
  var path = require('path');
  app.get('*', function(req, res) {
    res.sendfile(path.join(__dirname + '/public', 'index.html'));
  });
};
```
In the ‘apps’ folder, create a folder named models
```
mkdir models && cd models
```
Create a file named book.js
```
vi book.js
```
Copy and paste the code below into ‘book.js’
```
var mongoose = require('mongoose');
var dbHost = 'mongodb://localhost:27017/test';
mongoose.connect(dbHost);
mongoose.connection;
mongoose.set('debug', true);
var bookSchema = mongoose.Schema( {
  name: String,
  isbn: {type: String, index: true},
  author: String,
  pages: Number
});
var Book = mongoose.model('Book', bookSchema);
module.exports = mongoose.model('Book', bookSchema);
```

## Access the routes with AngularJS
AngularJS provides a web framework for creating dynamic views in your web applications. In this tutorial, we use AngularJS to connect our web page with Express and perform actions on our book register.
Change the directory back to ‘Books’
```
cd ../..
```
Create a folder named public
```
mkdir public && cd public
```
Add a file named script.js
```
vi script.js
```
Copy and paste the Code below (controller configuration defined) into the script.js file.
```
var app = angular.module('myApp', []);
app.controller('myCtrl', function($scope, $http) {
  $http( {
    method: 'GET',
    url: '/book'
  }).then(function successCallback(response) {
    $scope.books = response.data;
  }, function errorCallback(response) {
    console.log('Error: ' + response);
  });
  $scope.del_book = function(book) {
    $http( {
      method: 'DELETE',
      url: '/book/:isbn',
      params: {'isbn': book.isbn}
    }).then(function successCallback(response) {
      console.log(response);
    }, function errorCallback(response) {
      console.log('Error: ' + response);
    });
  };
  $scope.add_book = function() {
    var body = '{ "name": "' + $scope.Name + 
    '", "isbn": "' + $scope.Isbn +
    '", "author": "' + $scope.Author + 
    '", "pages": "' + $scope.Pages + '" }';
    $http({
      method: 'POST',
      url: '/book',
      data: body
    }).then(function successCallback(response) {
      console.log(response);
    }, function errorCallback(response) {
      console.log('Error: ' + response);
    });
  };
});
```
In the public folder, create a file named index.html;
```
vi index.html 
```
Copy and paste the code below into index.html file.
```
<!doctype html>
<html ng-app="myApp" ng-controller="myCtrl">
  <head>
    <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.6.4/angular.min.js"></script>
    <script src="script.js"></script>
  </head>
  <body>
    <div>
      <table>
        <tr>
          <td>Name:</td>
          <td><input type="text" ng-model="Name"></td>
        </tr>
        <tr>
          <td>Isbn:</td>
          <td><input type="text" ng-model="Isbn"></td>
        </tr>
        <tr>
          <td>Author:</td>
          <td><input type="text" ng-model="Author"></td>
        </tr>
        <tr>
          <td>Pages:</td>
          <td><input type="number" ng-model="Pages"></td>
        </tr>
      </table>
      <button ng-click="add_book()">Add</button>
    </div>
    <hr>
    <div>
      <table>
        <tr>
          <th>Name</th>
          <th>Isbn</th>
          <th>Author</th>
          <th>Pages</th>
 
        </tr>
        <tr ng-repeat="book in books">
          <td>{{book.name}}</td>
          <td>{{book.isbn}}</td>
          <td>{{book.author}}</td>
          <td>{{book.pages}}</td>
 
          <td><input type="button" value="Delete" data-ng-click="del_book(book)"></td>
        </tr>
      </table>
    </div>
  </body>
</html>
```
Change the directory back up to Books
```
cd ..
```
Start the server by running this command:
```
node server.js
```
It should give you value like the image below
![](https://github.com/Omolade11/MeanStack_AWS/blob/main/Images/Screenshot%202022-12-20%20at%2015.01.25.png)
The server is now up and running, we can connect it via port 3300. 
We can view it with our public ip address and port 3300 as the port like the image below.
![](https://github.com/Omolade11/MeanStack_AWS/blob/main/Images/Screenshot%202022-12-20%20at%2015.08.31.png)
We can launch a separate Putty or SSH console to test what the curl command returns locally.
```
curl -s http://localhost:3300
```

It shall return an HTML page, it is hardly readable in the CLI, but we can also try and access it from the Internet.
For this – we need to open TCP port 3300 in your AWS Web Console for your EC2 Instance like in the image below.
![](https://github.com/Omolade11/MeanStack_AWS/blob/main/Images/Screenshot%202022-12-20%20at%2015.06.31.png)
 





