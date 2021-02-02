
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


