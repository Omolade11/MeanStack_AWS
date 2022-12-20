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
Next, we will add certificates by running
```
sudo apt -y install curl dirmngr apt-transport-https lsb-release ca-certificates
curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -
```
Afterward, we will Install NodeJS
```
sudo apt install -y nodejs
```
## Install MongoDB

