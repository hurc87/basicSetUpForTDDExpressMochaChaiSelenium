Basic Setup for TDD on an express app using mocha, chai and selenium:

- Go to directory and run npm init

- Install the packages needed :
    npm install --save-dev chromedriver selenium-webdriver mocha chai chai-as-promised

- Install express :
    npm install --save express

- Create an app.js file, then fill with basic set up :
    const express = require ('express');
    const app = express();

    app.get('/', (req, res) => {
      res.send();
    });

app.listen(3003, () => console.log('Hello world is listening to port 3003!!'));

- In the package.json file add the following in scripts :
    "start": "node app.js",
    "test:e2e": "mocha --timeout 20000 e2e/tests.js"
    Also change anywhere that shows index.js to app.js

- Create an e2e folder

- In the e2e tolder create a file called tests.js

- The top of the folder would need to have the following at the top of the page :
    require('chromedriver');
    const webdriver = require('selenium-webdriver');
    const { By, until } = webdriver;

    const chai = require('chai');
    const chaiAsPromised = require('chai-as-promised');

    chai.use(chaiAsPromised);
    const driver = new webdriver.Builder().forBrowser('chrome').build();
    const expect = chai.expect;

- For the basic 'Hello world' test, the following needs to be written below:
    describe('Viewing the webpage', done => {
      before(done => {
        driver.get('http://localhost:3003').then(function(res) {
          driver
          .then(() => {
            driver.wait(until.elementLocated(By.id('message'))).then(() => {
              done();
            });
          });
        });
      });

      it('can read the welcome message', () => {
        console.log('Ready to read welcome message');
        return expect(driver.findElement(By.id('message')).getAttribute('innerHTML'))
        .to.eventually.contain("Hello World");
      });
    });

- To run the server type the following into the command line:
    npm start

- To run the tests type the following in the command line :
    npm run test:e2e

- Running the test, the test will currently run but will time out and fail as it cannot see the Hello World message or find the 'message' id element.

- To pass the test insert an element with the id 'message' and pass the string "Hello World" :
    `<div id="message">Hello World</div>`

- If the tests are not running and syntax is correct, check version of Chrome, as the webdriver requires the latest update of Chrome to run properly. 


********************************************************************************

Changes to code

- At the top of the app.js file the following has been added :
    app.use(express.static(__dirname));

- This means that the app can point to a seperate html file using :
    res.sendFile('index.html');
