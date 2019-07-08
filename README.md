# Testing with Jest

> Author(s): Brian Crites ([@brrcrites](https://github.com/brrcrites))

[Jest](https://jestjs.io) is one of the most popular JavaScript testing frameworks which aims to be simple to use while still covering a wide number of testing types and working with a large number of frameworks. It works with JavaScript like systems such as Babel and TypeScript as well as a large number of UI frameworks such as React, Angular, and Vue.

## Getting Started with Node

[Node](https://nodejs.org/en/) is a JavaScript runtime which allows for the execution of JavaScript code directly rather than needing to use a browser to execute it. This ability has taken javasript from being a front-end only language to being something that can be used full-stack. Jest has been designed to run on top of node to allow for local (and non-local) testing of your javscript code, which means in order to use jest we'll also need to use node and one of its package managers.

There are two competing package managers for node, the [Node Package Manager (npm)](https://www.npmjs.com) and [yarn](https://yarnpkg.com/en/). For this course we will be using npm because it automatically comes with node and is a bit more standard and likely to be used in walkthroughs. Start by installing node from either a [pre-built binary](https://nodejs.org/en/download/) or [through a package manager](https://nodejs.org/en/download/package-manager/) such as brew or apt.

## Initialize your Project

Once you have node and npm installed (you can verify they are both installed by runnin `npm --version` and `node --version` from the command line) you can setup your repository as a node project. Npm has an excellent built in system for setting up a new project and creating all the boilerplate files necessary. Run the following command and you'll answer a few questions to setup your system.

```
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

## JavaScript for Testing

Now that we have node setup we can execute JavaScript code directly without the need for the browser. Lets create a basic JavaScript function which we could execute with node in a file named `introduction.js`.

```js
function introduction(name) {
    return "hello my name is " + name;
}

module.exports = introduction;
```

If we created a file named `main.js` which just called `console.log(introduction('hannah'))` and then executed that with node by running `node main.js` we would see the string "hello my name is hannah" printed out to the terminal. However, this would be a poor form of testing because it doesn't validate if the output value is correct or not (along with some other shortcomings). The validation is where


This is beneficial because it means we can write, execute, and test the code from our system without having to develop HTML code that calls the JavaScript functions and then click those UI elements and hand validate that the functions work correctly. This type of bespoke testing is easy to forget to do, doesn't scale to larger projects, and can lead to regressions if we don't test everything every time we make a change to the code.

## Installing Jest

Jest is made up of two pieces wrapped into a single installation: (1) the framework of functions which are used for testing and (2) a CLI tool for running the tests (and performing other functions). In order to register that our code requires Jest to perform testing we need to list it as a dependency in the package.json file. Luckly npm has a built in system for installing a dependency and letting the package.json file know that it needs to be registered as a new dependency.

```
npm install --save-dev jest
```

The `npm install` portion tells npm to install the new package while the `--save-dev` portion tells it to add Jest as a developer dependency in the package.json file. Look at the package.json file and you should see jest and a version number listed under "devDependencies". This tells the system that Jest is required when running in a developer environemnt, and anyone who runs `npm install` in the directory where the package.json file is located will install all the dependencies into a new `node_modules/` directory.

This will tell other developers who want to use our code as well as automated systems what dependencies are required to run our code, but this doesn't give us easy access to the Jest CLI. In order to get that we will install Jest globally to our system, which will register the Jest CLI as a command in our terminal and allow us to run it more easily.

```
npm install --global jest
```

Now you should be able to type `jest --version` and see a versio number output. If you don't see a version output and instead get an error like "command not found" try closing all your command line programs and opening a new one.

## Writing Basic Jest Tests

With most testing frameworks groups of tests are collected into sets and typically all tests in a set are testing the same library or module, and are typically grouped together in reporting to make it easier to identify where an error occured. In Jest, all tests in the same file are automatically put into a set and reported together, and typically a set covers all the code in a single file (which will also be a single module if you are following good coding practices). Because of this, it is idiomatic to name your test files `<filename-under-test>.test.js` replacing `<filename-under-test>` with the name of the file you will be testing. Jest looks for files named this way to run, so as long as they are named `*.test.js` Jest should be able to automatically identify and run them. Create the below tests for the code we wrote previously.

```js
const introduction = require("./introduction.js")

test("introduce hannah", function() {
   expect(introduction("hannah")).toBe("hello my name is hannah"); 
});
```

Note that here we are checking that the string returned by the function and our string literal are equal. There are lots of different types of matching functions that you can use depending on your type and what you need check it against. You should refence [the official documentation on Jest matches](https://jestjs.io/docs/en/using-matchers) when you are trying to check something new to make sure you are checking it as efficiently and directly as possible. 

Now we can execute our tests and see if they have found any errors. Simply run `jest` from the command line and it should begin the execution showing you something like this.

![npm test output showing all tests have passed](https://raw.githubusercontent.com/cs100/template-lab-js-unit-testing/master/images/testing-results.png?token=ABZFRYFZ4ZYRH6APKHGBNOS5FPEAK)

However, because npm tries to cover everything you would need to build, test, and deploy node applications we can also perform our testing using npm. Run the command `npm test` from the command line and you should see the same tests run as  you did when you executed `jest`. This happens because during the setup process for our node application we wrote "jest" as the test command, which means when you run `npm test` then npm is actually executing `jest` itself. Using npm to abstract away the built, test, and deployment portions of your application allows you to use any libraries or frameworks you like and have a consistent method for performing those functions. This convention not only makes it easier for you and other devleopers to download and build, test, deploy others code but is also widely used in automated systems to integrate with your code.

## Setting up Continuous Integration (CI)

Continuous integration is the process of re-executing a suite of tests whenever certain events occur, typically when new commits are added to the codebase or a new PR is opened against a specific branch.

