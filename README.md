# Testing with Jest

> Author(s): Brian Crites ([@brrcrites](https://github.com/brrcrites))

[Jest](https://jestjs.io) is one of the most popular JavaScript testing frameworks which aims to be simple to use while still covering a wide number of testing types and working with a large number of frameworks. It works with JavaScript like systems such as Babel and TypeScript as well as a large number of UI frameworks such as React, Angular, and Vue.

> Note: for this lab we will have notes reminding your to commit and push your code to GitHub to get you in the habit of committing and pushing often and to illustrate good times in your development to commit your code.

## Getting Started with Node

[Node](https://nodejs.org/en/) is a JavaScript runtime which allows for the execution of JavaScript code directly rather than needing to use a browser to execute it. This ability has taken javasript from being a front-end only language to being something that can be used full-stack. Jest has been designed to run on top of node to allow for local (and non-local) testing of your javscript code, which means in order to use jest we'll also need to use node and one of its package managers.

There are two competing package managers for node, the [Node Package Manager (npm)](https://www.npmjs.com) and [yarn](https://yarnpkg.com/en/). For this course we will be using npm because it automatically comes with node and is a bit more standard and likely to be used in walkthroughs. Start by installing node from either a [pre-built binary](https://nodejs.org/en/download/) or [through a package manager](https://nodejs.org/en/download/package-manager/) such as brew or apt.

## Initialize your Project

Once you have node and npm installed (you can verify they are both installed by runnin `npm --version` and `node --version` from the command line) you can setup your repository as a node project. Npm has an excellent built in system for setting up a new project and creating all the boilerplate files necessary. Run the following command and you'll answer a few questions to setup your system.

```bash
npm init
```

Below are the questions you will be asked. For most of them you can leave them as the default values except the "test command:" question, which you should respond with "jest" since that is the command we'll use to run the tests.

* package name: (name-of-repo)
* version: (1.0.0)
* description:
* entry point: (index.js)
* test command:
* git repository: (url-of-repo)
* keywords: 
* author:
* license (ISC):

It will then print out the `package.json` file which will be generated and ask you to confirm. Read through the package file and double check that the test command is correct before confirming.

> Note: commit and push the work you have done so far to GitHub, primarily your `package.json` file.

## JavaScript for Testing

Now that we have node setup we can execute JavaScript code directly without the need for the browser. Lets create a basic JavaScript function which we could execute with node in a file named `introduction.js`.

```js
function introduction(name) {
    return "hello my name is " + name;
}

module.exports = introduction;
```

This is beneficial because it means we can write, execute, and test the code from our system without having to develop HTML code that calls the JavaScript functions and then click those UI elements and hand validate that the functions work correctly. This type of bespoke testing is easy to forget to do, doesn't scale to larger projects, and can lead to regressions if we don't test everything every time we make a change to the code.

If we created a file named `main.js` which just called `console.log(introduction('hannah'))` and then executed that with node by running `node main.js` we would see the string "hello my name is hannah" printed out to the terminal. However, this would be a poor form of testing because it doesn't validate if the output value is correct or not (along with some other shortcomings). The validation is where our unit testing framework will come in, but its usually easiest to validate types which are (or we can extract from) basic types. There are ways to gracefully handle validating more complex systems which we will discuss at the end of this lab for the Jest unit testing framework, but these options do not exist across all unit testing frameworks and when they do exist can be done very differently, so its alway bes to consult your documentation.

> Note: commit and push the work you have done so far to GitHub.

## Installing Jest

Jest is made up of two pieces wrapped into a single installation: (1) the framework of functions which are used for testing and (2) a CLI tool for running the tests (and performing other functions). In order to register that our code requires Jest to perform testing we need to list it as a dependency in the package.json file. Luckly npm has a built in system for installing a dependency and letting the package.json file know that it needs to be registered as a new dependency.

```bash
npm install --save-dev jest
```

The `npm install` portion tells npm to install the new package while the `--save-dev` portion tells it to add Jest as a developer dependency in the package.json file. Look at the package.json file and you should see jest and a version number listed under "devDependencies". This tells the system that Jest is required when running in a developer environemnt, and anyone who runs `npm install` in the directory where the package.json file is located will install all the dependencies into a new `node_modules/` directory.

This will tell other developers who want to use our code as well as automated systems what dependencies are required to run our code, but this doesn't give us easy access to the Jest CLI. In order to get that we will install Jest globally to our system, which will register the Jest CLI as a command in our terminal and allow us to run it more easily.

```bash
npm install --global jest
```

Now you should be able to type `jest --version` and see a versio number output. If you don't see a version output and instead get an error like "command not found" try closing all your command line programs and opening a new one.

> Note: commit and push the work you have done so far to GitHub, including your updated `package.json` and `package-lock.json` files.

## Writing a Basic Jest Test

With most testing frameworks groups of tests are collected into sets and typically all tests in a set are testing the same library or module, and are typically grouped together in reporting to make it easier to identify where an error occured. In Jest, all tests in the same file are automatically put into a set and reported together, and typically a set covers all the code in a single file (which will also be a single module if you are following good coding practices). Because of this, it is idiomatic to name your test files `<filename-under-test>.test.js` replacing `<filename-under-test>` with the name of the file you will be testing. Jest looks for files named this way to run, so as long as they are named `*.test.js` Jest should be able to automatically identify and run them. Create the below tests for the code we wrote previously.

```js
const introduction = require("./introduction.js")

test("introduce hannah", function() {
   expect(introduction("hannah")).toBe("hello my name is hannah"); 
});
```

> Note: here we are checking that the string returned by the function and our string literal are equal. There are lots of different types of matching functions that you can use depending on your type and what you need check it against. You should refence [the official documentation on Jest matches](https://jestjs.io/docs/en/using-matchers) when you are trying to check something new to make sure you are checking it as efficiently and directly as possible. 

Now we can execute our tests and see if they have found any errors. Simply run `jest` from the command line and it should begin the execution showing you something like this.

![npm test output showing all tests have passed](https://raw.githubusercontent.com/cs100/template-lab-js-unit-testing/master/images/testing-success.png)

However, because npm tries to cover everything you would need to build, test, and deploy node applications we can also perform our testing using npm. Run the command `npm test` from the command line and you should see the same tests run as  you did when you executed `jest`. This happens because during the setup process for our node application we wrote "jest" as the test command, which means when you run `npm test` then npm is actually executing `jest` itself. Using npm to abstract away the built, test, and deployment portions of your application allows you to use any libraries or frameworks you like and have a consistent method for performing those functions. This convention not only makes it easier for you and other devleopers to download and build, test, deploy others code but is also widely used in automated systems to integrate with your code.

> Note: commit and push the work you have done so far to GitHub.

## Failing a Jest Test

Now that we've written a basic Jest test and seen our function pass it, lets write a test for an edge case. This test will check the output of the function when an empty string is passed as input.

```js
test('introduce nobody', function() {
    expect(introduction("")).toBe("hello my name is");
});
```

Once you've added this test to the `introduction.test.js` file re-run the test suite using `npm` and you should see output like the following

![npm test output showing that one test has failed](https://raw.githubusercontent.com/cs100/template-lab-js-unit-testing/master/images/testing-failure.png)

As you can see from the printed "Expected" vs. "Recieved" values that are printed by Jest this is because the function adds a space at the end before appending the name, but that space wasn't present in the expected value. Rather than simply change the expected value lets change the funcitonality to not include the space when there isn't a name (seems like a reasonable thing to do with this edge case). Modify your `introduction` function to be the following.

```js
function introduction(name) {
    if(name.length > 0) {
        return "hello my name is " + name;
    }
    else {
        return "hello my name is";
    }
}
```

Now re-run your tests and you should see that they both are passing.

> Note: commit and push the work you have done so far to GitHub.

## Setting up Continuous Integration (CI)

Continuous integration is the process of re-executing a suite of tests, typically using an automated system, whenever certain events occur. Often it is setup to run when new commits are added to the codebase or a new PR is opened against a specific branch. We are going to utilize [Travis CI](https://travis-ci.com) for our continuous integrtion. Travis CI integrates with GitHub's checks system allowing it to display its status directly on a GitHub PR and can be used to gate PRs from beging merged until Travis has completed successfully. Travis, and other CI services, provide their (very valuable) service for free to public GitHub repoitories but we will be using Travis specifically because you also get free private CI as part of the GitHub Student Pack.

Start by going to their website and signing in using your GitHub credentials. From there you can set which repositories are monitored for continuous integreation in the settings, however even if you have all repositores enabled only those with a special `.travis.yml` file will actually be run by Travis CI. Create the following file named `.travis.yml` in the root directory of your project, and make sure Travis CI is monitoring the repo you are using for this lab.

```yaml
language: node_js
node_js:
  - "6.9"
```

Save this file then commit and push it to your master branch. This is the file that Travis CI looks for to know what to do for each repository, and it is highly customizable to allow for many different types of processes to be automated. The file we have written here simply tells Travis CI that our project is a node project and it requires node version 6.9 or newer, and from there it will perform its default behavior which is to run `npm install` followed by `npm test` and will report if there are any issues (this is one of those cases where following convention makes your life easier).

Go to your Tavis CI dashboard and you should see a green (or yellow but soon to be green) checkmark next to the name of your repository. Click on that and you'll be able to view the logs of the test, which show everything that is output when it performs the testing and the first place you should look if there is an error to find out what it is. Continuous integration isn't a replacement for running your test suite before you commit your code to the repository, but it acts as a nice double check that all tests pass before merging code into master and can be used as a basis to start other automated processes such as automated deployments.

# Intermediate and Advanced Jest Usage

* Specialized matchers for various data types which can vastly reduce the complexity of the validation when used correctly [Jest Docs](https://jestjs.io/docs/en/using-matchers)
* Built-in ability to use mocks to capture the vast majority of calls, returns, initializations, allocations, etc. to systems that haven't been developed or are easier to test externally [Jest Docs](https://jestjs.io/docs/en/mock-functions)
* The `.each()` function allows a single test run a number of different inputs against a templated testing function without having to copy-paste that test [Zetcode Tutorial](http://zetcode.com/javascript/jest/)
* Setup and teardown code using `beforeAll()`, `beforeEach()`, etc. to execute some code before any/each tests is run, good when complex initializations are needed for a set of tests [Jest Docs](https://jestjs.io/docs/en/setup-teardown)
* The `describe()` function makes it easy to group similar tests together making the jest testing output easier to read and understand and to manage scopes [Jest Docs](https://jestjs.io/docs/en/setup-teardown#scoping)
* Test asynchronous code with specialized methods for callbacks, promises, and async/await methods [Jest Docs](https://jestjs.io/docs/en/asynchronous.html)

