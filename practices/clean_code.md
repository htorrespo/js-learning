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
The implementation of this won’t have much logic, but can become quite large
due to it’s default coverage.

```javascript
// Without default parameters it looks quite bloated and unnecessary large.
function createElement (tag, config) {
  tag = tag || 'div';
  config = config || {};

  const element = document.createElement (tag);
  const content = config.content || 'Very default';
  const text = document.createTextNode(content);
  let classNames = config.classNames;

  if (classNames === undefined) {
    classNames = ['module-text', 'default'];
  }

  element.classList.add (...classNames);
  element.appendChild (text);

  returnelement;
}
```

So far, so good. What’s happening here? We’re doing the following:

1. setting default values for both our parameters tag and config, in case
they aren’t passed (note that some linters don’t like parameter reassigning,
https://eslint.org/docs/rules/no-param-reassign).
2. creating constants with the actual content (and default values)
3. checking if classNames is defined, and assigning a default array if not.
4. creating and modifying the element before we return it.

Now let’s take this function and optimize it to be cleaner, faster to write, and so
that it’s more obvious what its purpose is:

```javascript
// Default all the things
function createElement (tag = 'div', {
  content = 'Very default', 
  classNames = ['module-text','special']
} = {}) {
  const element = document.createElement (tag);
  const text = document.createTextNode (content);
  element.classList.add(...classNames);
  element.appendChild (text);

  return element;
}
```

We didn’t touch the function’s logic, but removed all default handling from the
function body. The function signature now contains all defaults.

Let me further explain one part, which might be slightly confusing:

```javascript
// What exactly happens here?
function createElement ({
  content = 'Very default',
  classNames = ['module-text','special']
} = {}) {
// function body
}
```

We not only declare a default object parameter, but also default object
properties. This makes it more obvious what the default configuration is
supposed to look like, rather than only declaring a default object (e.g. 
config = {} ) and later setting default properties. It might take some 
additional time to get used to it, but in the end it improves your workflow.

Of course, we could still argue with larger configurations that it might create
more overhead and it’d be simpler to just keep the default handling inside of the
function body.

## ES6 Property Shorthands

If a method accepts large configuration objects as an argument, your code can
become quite large. It’s common to prepare some variables and add them to said
object. Property shorthands are _syntactic sugar_ (is designed to make things easier 
to read or to express) to make this step shorter and more readable:
