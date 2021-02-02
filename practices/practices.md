
## Who Should Read This?
This is for all front-end developers who wish to improve their JavaScript 
skills. 
You’ll need to be familiar with HTML and CSS and have a reasonable level 
of understanding of JavaScript in order to follow the discussion.

### JavaScript ES2015+

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
``


