# Clean code

Creating a method also means writing an API — whether it’s for yourself,
another developer on your team, or other developers using your project.
Depending on the size, complexity, and purpose of your function, you have to
think of default settings and the API of your input/output.

Default function parameters and property shorthands are two handy features of
ES6 that can help you write your API.

## ES6 Default Parameters

Let’s freshen up our knowledge quickly and take a look at the syntax again.
Default parameters allow us to initialize functions with default values. A 
default is used when an argument is either omitted or undefined— meaning null
is avalid value. A default parameter can be anything from a number to another
function.

```javascript
// Basic syntax
function multiply (a, b = 2) {
  return a * b; 
}

multiply(5); // 10

// Default parameters are also available to later default parameters
function foo (num = 1, multi = multiply (num)) {
  return [num, multi];
}

foo(); // [1, 2]
foo(6); // [6, 12]
```

### A real-world example

Let’s take a basic function and demonstrate how default parameters can speed
up your development and make the code better organized.

Our example method is called createElement(). It takes a few configuration
arguments, and returns an HTML element. The API looks like this:

```javascript
// We want a <p> element, with some text content and two classes attached.
// Returns <p class="very-special-text super-big">Such unique text</p>
createElement ('p', {
  content: 'Such unique text',
  classNames: ['very-special-text','super-big']
});

// To make this method even more useful, it should always return a default
// element when any argument is left out or none are passed at all.
createElement();// <div class="module-text default">Very default</div>
```
