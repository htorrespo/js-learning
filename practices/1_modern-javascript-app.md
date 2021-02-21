# The Anatomy of a Modern JavaScript Application

There’s no doubt that the JavaScript ecosystem changes fast. Not only are
new tools and frameworks introduced and developed at a rapid rate, the
language itself has undergone big changes with the introduction of ES2015
(aka ES6). Understandably, many articles have been written complaining
about how difficult it is to learn modern JavaScript development these days.


## Who Should Read This?
This is for all front-end developers who wish to improve their JavaScript 
skills. 

You’ll need to be familiar with HTML and CSS and have a reasonable level 
of understanding of JavaScript in order to follow the discussion.

## JavaScript ES2015+

In 2015, the sixth version ofECMAScript— the specification that defines the
JavaScript language — was released under the name ofES2015(still often
referred to as ES6). This new version included substantial additions to the
language, making it easier and more feasible to build ambitious web applications.
But improvements don’t stop with ES2015; each year, a new version is released.

### Declaring variables

JavaScript now has two additional ways to declare variables:let and const. 
let is the successor to var. Although var is still available, let limits the
scope of variables to the block (rather than the function) they’re declared 
within, which reduces the room for error:

``` javascript
// ES5
for(var i=1;i<5;i++) {
  console.log(i);
}
// <-- logs the numbers 1 to 4
console.log(i);
// <-- 5 (variable i still exists outside the loop)

// ES2015
for(let j=1;j<5;j++){
  console.log(j);
}
console.log(j);
// <-- 'Uncaught ReferenceError: j is not defined'
```
Using const allows you to define variables that cannot be rebound to new 
values. For primitive values such as strings and numbers, this results in
something similar to a constant, as you cannot change the value once it has 
been declared:

```javascript
const name = 'Bill';
name = 'Steve';
// <-- 'Uncaught TypeError: Assignment to constant variable.'

// Gotcha
const person = { name: 'Bill' };
person.name = 'Steve';
// person.name is now Steve.
// As we're not changing the object that person is bound to, JavaScript doesn't
// complain.

```
### Arrow functions

Arrow functions provide a cleaner syntax for declaring anonymous functions
lambdas), dropping the function keyword and the return keyword when the
body function only has one expression. This can allow you to write functional
style code in a nicer way:

```javascript
// ES5
var add = function(a, b) {
  returna+b;
}
// ES2015
const add = (a, b) => a + b;
```

The other important feature of arrow functions is that they inherit the value of
this from the context in which they are defined:

```javascript
function Person() {
  this.age=0;
  // ES5
  setInterval(function(){
    this.age++; // |this| refers to the global object
  }, 1000);

  // ES2015
  setInterval(() => {
    this.age++; // |this| properly refers to the person object
  },1000);
}
var p = new Person();
```
### Improved Class syntax

If you’re a fan of object-oriented programming, you might like theaddition of
classes to the language on top of the existent mechanism based on prototypes.
While it’s mostly just syntactic sugar, it provides a cleaner syntax for developers
trying to emulate classical object-orientation with prototypes.

```javascript
class Person {
  constructor(name) {
    this.name=name;
  }
    
  greet() {
    console.log(`Hello, my name is${this.name}`);
  }
}
```
### Promises / Async functions

The asynchronous nature of JavaScript has long represented a challenge; any
non-trivial application ran the risk of falling into a callback hell when dealing with
things like Ajax requests.

Fortunately, ES2015 added native support for promises. Promises represent
values that don’t exist at the moment of the computation but that may be
available later, making the management of asynchronous function calls more
manageable without getting into deeply nested callbacks. 

ES2017 introduced async functions(sometimes referred to as async/await) that
make improvements in this area, allowing you to treat asynchronous code as if it
were synchronous:

```javascript
async function doAsyncOp(){
  var val = await asynchronousOperation();
  console.log(val);
  return val;
};
```

### Code linting

Linters are tools that parse your code and compare it against a set of rules,
checking for syntax errors, formatting, and good practices. Although the use of a
linter is recommended to everyone, it’s especially useful if you’re getting started.
When configured correctly for your code editor/IDE, you can get instant
feedback to ensure you don’t get stuck with syntax errors as you’re learning new
language features.

You can check out ESLint, which is one of the most popular and supports ES2015+.

## Modular Code

Modern web applications can have thousands (even hundred of thousands) of
lines of code. Working at that size becomes almost impossible without a
mechanism to organize everything in smaller components, writing specialized
and isolated pieces of code that can be reused as necessary in a controlled way.
This is the job of modules.

### CommonJS modules

A handful of module formats have emerged over the years, the most popular of
which is CommonJS. It’s the default module format in Node.js, and can be used in
client-side code with the help of module bundlers, which we’ll talk about shortly.

It makes use of a module object to export functionality from a JavaScript file and
a require() function to import that functionality where you need it.

```javascript
// lib/math.js
function sum(x, y) {
  return x + y;
}

const pi = 3.141593

module.exports = {
  sum: sum,
  pi: pi
};

// app.js
const math = require("lib/math");
console.log("2π = " + math.sum(math.pi, math.pi));
```

### ES2015 modules

ES2015 introduces a way to define and consume components right into the
language, which was previously possible only with third-party libraries. You can
have separate files with the functionality you want, and export just certain parts
to make them available to your application.

Here’s an example:

```javascript
// lib/math.js
export function sum(x,y){
  return x+y;
}

export const pi=3.141593;
```

Here we have a module thatexportsa function and a variable. We can include
that file in another one and use those exported functions:

```javascript
// app.js
import * as math from "lib/math";

console.log("2π = " + math.sum(math.pi,math.pi));
```

Or we can also be specific and import only what we need:

```javascript
// otherApp.js

import{sum,pi}from"lib/math";

console.log("2π = "+sum(pi,pi));
```

## Package Management

Other languages have long had their own package repositories and managers to
make it easier to find and install third-party libraries and components. Node.js
comes with its own package manager and repository, npm. Although there are
other package managers available, npm has become the de facto JavaScript 
package manager and is said to be the largest package registry in the world.

In the npm repository you can find third-party modules that you can easily
download and use in your projects with a single npm install <package> 
command. The packages are downloaded into a local node_modules directory,
which contains all the packages and their dependencies.

The packages that you download can be registered as dependencies of your
project in a package.json file, along with information about your project or
module (which can itself be published as a package on npm).

You can define separate dependencies for both development and production.
While the production dependencies are needed for the package to work, the
development dependencies are only necessary for the developers of the
package.


Example package.json file

{
  "name":"demo",
  "version":"1.0.0",
  "description":"Demo package.json",
  "main":"main.js","dependencies": {
    "mkdirp":"^0.5.1",
    "underscore":"^1.8.3"
  },
  "devDependencies":{},
  "scripts":{
    "test":"echo \"Error: no test specified\" && exit 1"
  },
  "author":"Sitepoint",
  "license":"ISC"
}

## Build Tools

The code that we write when developing modern JavaScript web applications
almost never is the same code that will go to production. We write code in a
modern version of JavaScript that may not be supported by the browser, we
make heavy use of third-party packages that are in a node_modules folder along
with their own dependencies, we can have processes like static analysis tools or
minifiers, etc. Build tooling exists to help transform all this into something that
can be deployed efficiently and that’s understood by most web browsers.

### Module bundling

When writing clean, reusable code with ES2015/CommonJS modules, we need
some way to load these modules (at least until browsers support ES2015 module
loading natively). Including a bunch of script tags in your HTML isn’t really a
viable option, as it would quickly become unwieldy for any serious application,
and all those separate HTTP requests would hurt performance.

We can include all the modules where we need them using the import
statement from ES2015 (orrequire, for CommonJS) and use a module bundler 
to combine everything together into one or more files (bundles). It’s this bundled
file that we’re going to upload to our server and include in our HTML. It will
include all your imported modules and their necessary dependencies.

There are currently a couple of popular options for this, the most popular ones 
being Webpack, Browserify and Rollup.js. You can choose one or another
depending on your needs.

If you want to learn more about module bundling and how it fits into the 
bigger picture of app development, I recommend reading Understanding JavaScript Modules: Bundling & Transpiling (https://www.sitepoint.com/javascript-modules-bundling-transpiling/)

### Transpilation

While support for modern JavaScript is pretty good among newer browsers, your
target audience may include legacy browsers and devices with partial or nosupport.

In order to make our modern JavaScript work, we need to translate the code we
write to its equivalent in an earlier version (usually ES5). The standard tool for
this task is Babel— a compiler that translates your code into compatible code for
most browsers. In this way, you don’t have to wait for vendors to implement
everything; you can just use all the modern JS features.

There are a couple of features that need more than a syntax translation. Babel
includes a Polyfill that emulates some of the machinery required for some
complex features such as promises.

### Build systems & task runners

Module bundling and transpilation are just two of the build processes that we
may need in our projects. Others include code minification (to reduce file sizes),
tools for analysis, and perhaps tasks that don’t have anything to do with
JavaScript, like image optimization or CSS/HTML pre-processing.

The management of tasks can become a laborious thing to do, and we need a
way to handle it in an automated way, being able to execute everything with
simpler commands. The two most popular tools for this are Grunt.js and Gulp.js,
which provide a way to organize your tasks into groups in an ordered way.

For example, you can have a command like gulp build which may run a code
linter, the transpilation process with Babel, and module bundling with Browserify.
Instead of having to remember three commands and their associated arguments
in order, we just execute one that will handle the whole process automatically.

Wherever you find yourself manually organizing processing steps for your
project, think if it can be automatized with a task runner.

Further reading: An Introduction to Gulp.js (https://www.sitepoint.com/introduction-gulp-js/).

## Application Architecture

Web applications have different requirements from websites. For example, while
page reloads may be acceptable for a blog, that’s certainly not the case for an
application like Google Docs. Your application should behave as closely as
possible to a desktop one. Otherwise, the usability will be compromised.
Old-style web applications were usually done by sending multiple pages from a
web server, and when a lot of dynamism was needed, content was loaded via
Ajax by replacing chunks of HTML according to user actions. Although it was a
big step forward to a more dynamic web, it certainly had its complications.
Sending HTML fragments or even whole pages on each user action represented
a waste of resources — especially of time, from the user’s perspective. The
usability still didn’t match the responsiveness of desktop applications.

Looking to improve things, we created two new methods to build web
applications — from the way we present them to the user, to the way we
communicate between the client and the server. Although the amount of
JavaScript required for an application also increased drastically, the 
result is now applications that behave very closely to native ones, without 
page reloading or extensive waiting periods each time we click a button.

### Single Page Applications (SPAs)

The most common, high-level architecture for web applications is called
SPA, which stands for Single Page Application. SPAs are big blobs of JavaScript that
contain everything the application needs to work properly. The UI is rendered
entirely client-side, so no reloading is required. The only thing that changes is the
data inside the application, which is usually handled with a remote API via
Ajax or another asynchronous method of communication.

One downside to this approach is that the application takes longer to load for the
first time. Once it has been loaded, however, transitions between views (pages)
are generally a lot quicker, since it’s only pure data being sent between client and
server.

### Universal / Isomorphic Applications

Although SPAs provide a great user experience, depending on your needs, they
might not be the optimal solution — especially if you need quicker initial
response times or optimal indexing by search engines.

There’s a fairly recent approach to solving these problems, called Isomorphic (or 
Universal) JavaScript applications. In this type of architecture, most of the code
can be executed both on the server and the client. You can choose what you
want to render on the server for a faster initial page load, and after that, the
client takes over the rendering while the user is interacting with the app.
Because pages are initially rendered on the server, search engines can index
them properly.

## Deployment

With modern JavaScript applications, the code you write is not the same as the
code that you deploy for production: you only deploy the result of your build
process. The workflow to accomplish this can vary depending on the size of your
project, the number of developers working on it, and sometimes the tools/libraries 
you’re using.

For example, if you’re working alone on a simple project, each time you’re ready
for deployment you can just run the build process and upload the resulting files
to a web server. Keep in mind that you only need to upload the resulting files
from the build process (transpilation, module bundling, minification, etc.), which
can be just one .js file containing your entire application and dependencies.

You can have a directory structure like this:

```termminal
├── dist
│   ├── app.js
│   └── index.html
├── node_modules
├── src
│   ├── lib
│   │   ├── login.js
│   │   └── user.js
│   ├── app.js
│   └── index.html
├── gulpfile.js
├── package.json
└── README
```

You thus have all of your application files in a src directory, written in ES2015+,
importing packages installed with npm and your own modules from a lib 
directory.

Then you can run Gulp, which will execute the instructions from a gulpfile.js
to build your project — bundling all modules into one file (including the ones
installed with npm), transpiling ES2015+ to ES5, minifying the resulted file, etc.
Then you can configure it to output the result in a convenient dist directory.

If you have files that don’t need any processing, you can just copythem from
src to the dist directory. You can configure a task for that in your build system.

Now you can just upload the files from the dist directory to a web server,
without having to worry about the rest of the files, which are only useful for
development.

### Team development

If you’re working with other developers, it’s likely you’re also using a shared code
repository, like GitHub, to store the project. In this case, you can run the build
process right before making commits and store the result with the other files in
the Git repository, to later be downloaded onto a production server.

However, storing built files in the repository is prone to errors if several
developers are working together, and you might want to keep everything clean
from build artifacts. Fortunately, there’s a better way to deal with that problem:
you can put a service like Jenkins, Travis CI, CircleCI, etc. in the middle of the
process, so it can automatically build your project after each commit is pushed to
the repository. Developers only have to worry about pushing code changes
without building the project first each time. The repository is also kept clean of
automatically generated files, and at the end, you still have the built files
available for deployment.

