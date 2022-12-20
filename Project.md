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
Since we are using the Ubuntu AMI of AWS EC2 instance to host our application. On our terminal, we will change directory to the folder we have our .pem file. In this case, it is in the Downloads folder

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
![]()
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
![]()
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





