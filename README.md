flask App + Docker--------------------------

template
  ->register.html
  ->thank-you.html


//requirements.txt
Flask==2.2.5



//Dockerfile

FROM python:3.9-slim


WORKDIR /app


COPY . /app


RUN pip install --no-cache-dir -r requirements.txt


EXPOSE 5000


CMD ["python", "app.py"]



//app.py

from flask import Flask, render_template, request

app = Flask(name)

@app.route('/')
def home():
    return render_template('register.html')

@app.route('/register', methods=['POST'])
def register():
    name = request.form['name']
    email = request.form['email']
    return f"User Registered Successfully!<br>Name: {name}<br>Email: {email}"

if name == 'main':
    app.run(host='0.0.0.0', port=5000)
    
 command
 docker build -t flask-registration .
 docker run -p 5000:5000 flask-registration
 



github----------------------------------------------------------------------------------

first perform docker containerizartion then   open a repo in github
 
 
 //github
 git init
 git remote add origin <url of repo    >
 git add .
 git commit -m "any name   "
 git push -u origin main/master



repo---->Actions---->workflow---->copy this code


name: Flask CI/CD         

on: [push]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v3
    - name: Setup Python
      uses: actions/setup-python@v4
      with:
        python-version: '3.10'
    - name: Install dependencies
      run: pip install -r requirements.txt
    - name: Run tests
      run: python -m unittest discover


Selenium------------------------------------------------------------------------------------------------------------


requirements

for node.js installation  ------> https://nodejs.org

set environment variable

check npm version ----> npm --version
for node -------> node --version


 ///index.html (selenium)

<!-- index.html -->
<!DOCTYPE html>
<html>
<head>
    <title>Simple Form</title>
</head>
<body>
    <h2>Enter your name</h2>
    <input type="text" id="nameInput" />
    <button onclick="greet()">Submit</button>
    <p id="output"></p>

    <script>
        function greet() {
            const name = document.getElementById("nameInput").value;
            document.getElementById("output").innerText = "Hello, " + name + "!";
        }
    </script>
</body>
</html>



//test.js

// test.js
require('chromedriver');

const { Builder, By, until } = require('selenium-webdriver');
const path = require('path');

(async function runTest() {
    let driver = await new Builder().forBrowser('chrome').build();

    try {
        const filePath = path.resolve(__dirname, 'index.html');
        await driver.get(filePath);

        // Find input, enter name
        const input = await driver.findElement(By.id('nameInput'));
        await input.sendKeys('Alice');

        // Click the button
        const button = await driver.findElement(By.tagName('button'));
        await button.click();

        // Wait for output and assert
        const output = await driver.wait(until.elementLocated(By.id('output')), 5000);
        const text = await output.getText();

        if (text === 'Hello, Alice!') {
            console.log('✅ Test passed!');
        } else {
            console.log('❌ Test failed! Output:', text);
        }
    } finally {
        await driver.quit();
    }
})();



commands

npm init -y
npm install selenium-webdriver
npm install chromedriver
node test.js




Jenkins-----------------------------------------------------------------------------------------

//dockerfile 

from node:18
workdir /usr/src/app
copy package*.json ./
run npm install 
copy . .
expose 3000
cmd ["node", "server.js"]



//html file

<!DOCTYPE html>
<html>
<head>
  <title>Docker Content Management</title>
</head>
<body>
  <h1>Hello from Dockerized App!</h1>
  <p>This is static content served inside a Docker container.</p>
</body>
</html>


//package.json

{
  "name": "content-manager-app",
  "version": "1.0.0",
  "main": "server.js",
  "dependencies": {
    "express": "^4.18.2"
  }
}


//server.js

const express = require('express');
const app = express();
const PORT = 3000;

app.use(express.static('content'));

app.listen(PORT, () => {
  console.log(Server running at http://localhost:${PORT});
});


for installing jenkins --------> https://www.jenkins.io/download/thank-you-downloading-windows-installer-stable/
open jenkin on local host 8080

step 1: Manage jenkins---->plugins---> avalaible plugins ------> type git server & pipeline download------> 
manage jenkins----->environment variable------> paste docker path C:\Program Files\Docker\Docker\resources\bin;%PATH%------> 

go to git hub create personal access token----> in developers setting  ----> classic token add permition/scope ---> repo & workflow --> save the token 

step 2: manage jenkins ----->system configure ----> github add credentials ----> kind(secret text) ---> copy personal token value in the secret ---add id cicd----->test connection

step 3:create item ---->select freestyle ----->select source code =git---->ennter repo link---->chenge master to main ----->build steps --->docker build -t jenkins-app
docker run -p 8000:8000 -d jenkins-app ----> save

step 4: build now check the console logs



------------------------------------------------------------------------------------------------------------------------------
////html for register.html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Event Registration</title>
</head>
<body>
    <h1>Register for the Event</h1>
    <form action="/register" method="post">
        <label>Name: <input type="text" name="name" required></label><br><br>
        <label>Email: <input type="email" name="email" required></label><br><br>
        <input type="submit" value="Register">
    </form>
</body>
</html>
------------------------------------------------------------------------------------------------------------------------------
///html for thank-you.html
<!DOCTYPE html>
<html>
<head>
     <meta charset="UTF-8">
    <title>Thank You</title>
</head>
<body>
    <h1>Thank You for Registering!</h1>
    <h2>Registered Participants:</h2>
    <ul>
        {% for reg in registrations %}
            <li>{{ reg.name }} ({{ reg.email }})</li>
        {% endfor %}
    </ul>
</body>
</html>
--------------------------------------------------------------------------------------------------------------------------------
