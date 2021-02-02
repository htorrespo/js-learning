# JS-Learning
Personal learning activities for JavaScript Languages

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

#### Declaring variables

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

