# Testing with Jest

> Author(s): Brian Crites ([@brrcrites](https://github.com/brrcrites))

[Jest](https://jestjs.io) is one of the most popular javascript testing frameworks which aims to be simple to use while still covering a wide number of testing types and working with a large number of frameworks. It works with javascript like systems such as Babel and TypeScript as well as a large number of UI frameworks such as React, Angular, and Vue.

## Getting Started with Node

[Node](https://nodejs.org/en/) is a javascript runtime which allows for the execution of javascript code directly rather than needing to use a browser to execute it. This ability has taken javasript from being a front-end only language to being something that can be used full-stack. Jest has been designed to run on top of node to allow for local (and non-local) testing of your javscript code, which means in order to use jest we'll also need to use node and one of its package managers.

There are two competing package managers for node, the [Node Package Manager (npm)](https://www.npmjs.com) and [yarn](https://yarnpkg.com/en/). For this course we will be using npm because it automatically comes with node and is a bit more standard and likely to be used in walkthroughs. Start by installing node from either a [pre-built binary](https://nodejs.org/en/download/) or [through a package manager](https://nodejs.org/en/download/package-manager/) such as brew or apt.

## Initialize your Project

Once you have node and npm installed (you can verify they are both installed by runnin `npm --version` and `node --version` from the command line) you can setup your repository as a node project. Npm has an excellent built in system for setting up a new project and creating all the boilerplate files necessary. Run the following command and you'll answer a few questions to setup your system.

```
npm init
```

* package name: (name-of-repo)
* version: (1.0.0)
* description:
* entry point: (index.js)
* test command:
* git repository: (url-of-repo)
* keywords: 
* author:
* license (ISC):

It will then print out the `package.json` file which will be generated and ask you to confirm.
