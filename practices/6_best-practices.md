# Best Practices for Using Modern JavaScript Syntax

Modern JavaScript is evolving quickly to meet the changing needs of new
frameworks and environments. Understanding how to take advantage of
those changes can save you time, improve your skill set, and mark the
difference between good code and great code.

Knowing what modern JavaScript is trying to do can help you decide when to use
the new syntax to your best advantage, and when it still makes sense to use
traditional techniques.

## Something Solid to Cling To

I don’t know anybody who isn’t confused at the state of JavaScript these days,
whether you’re new to JavaScript, or you’ve been coding with it for a while. So
many new frameworks, so many changes to the language, and so many contexts
to consider. It’s a wonder that anybody gets any work done, with all of the new
things that we have to learn every month.

I believe that the secret to success with any programming language, no matter
how complex the application, is getting back to the basics. If you want to
understand Rails, start by working on your Ruby skills, and if you want to use
immutables and unidirectional data flow in isomorphic React with webpack (or
whatever the cool nerds are doing these days) start by knowing your core
JavaScript.

Understanding how the language itself works is much more practical than
familiarizing yourself with the latest frameworks and environments. Those
change faster than the weather. And with JavaScript, we have a long history of
thoughtful information online about how JavaScript was created and how to use
it effectively.

The problem is that some of the new techniques that have come around with the
latest versions of JavaScript make some of the old rules obsolete. But not all of
them! Sometimes a new syntax may replace a clunkier one to accomplish the
same task. Other times the new approach may seem like a simpler drop-in
replacement for the way we used to do things, but there are subtle differences,
and it’s important to be aware of what those are.

## A Spoonful of Syntactic Sugar

A lot of the changes in JavaScript in recent years have been described as
syntactic sugar for existing syntax. In many cases, the syntactic sugar can help
the medicine go down for Java programmers learning how to work with
JavaScript, or for the rest of us we just want a cleaner, simpler way to accomplish
something we already knew how to do. Other changes seem to introduce
magical new capabilities.

But if you try to use modern syntax to recreate a familiar old technique, or stick it
in without understanding how it actually behaves, you run the risk of:

- having to debug code that worked perfectly before
- introducing subtle mistakes that may catch you at runtime
- creating code that fails silently when you least expect it.

In fact, several of the changes that appear to be drop-in replacements for
existing techniques actually behave differently from the code that they
supposedly replace. In many cases, it can make more sense to use the original,
older style to accomplish what you’re trying to do. Recognizing when that’s
happening, and knowing how to make the choice, is critical to writing effective
modern JavaScript.

## When Your const Isn’t Consistent

Modern JavaScript introduced two new keywords, _let_ and _const_ , which
effectively replace the need for _var_ when declaring variables in most cases. But
they don’t behave exactly the same way that _var_ does.

In traditional JavaScript, it was always a clean coding practice to declare your
variables with the _var_ keyword before using them. Failure to do that meant that
the variables you declared could be accessed in the global scope by any scripts
that happened to run in the same context. And because traditional JavaScript
was frequently run on webpages where multiple scripts might be loaded
simultaneously, that meant that it was possible for variables declared in one
script to leak into another.

The cleanest drop-in replacement for _var_ in modern JavaScript is _let_ . But _let_
has a few idiosyncrasies that distinguish it from _var_ . Variable declarations with
_var_ were always hoisted to the top of their containing scope by default,
regardless of where they were placed inside of that scope. That meant that even
a deeply nested variable could be considered declared and available right from
the beginning of its containing scope. The same is not true of _let_ or _const_ .

```javascript
console.log(usingVar); // undefined
var usingVar = "defined";
console.log(usingVar); // "defined"

console.log(usingLet); // error
let usingLet = "defined"; // never gets executed
console.log(usingLet); // never gets executed
```

When you declare a variable using _let_ or _const_ , the scope for that variable is
limited to the local block where it’s declared. A block in JavaScript is
distinguished by a set of curly braces _{}_ , such as the body of a function or the
executable code within a loop.

This is a great convenience for block-scoped uses of variables such as iterators
and loops. Previously, variables declared within loops would be available to the
containing scope, leading to potential confusion when multiple counters might
use the same variable name. However _let_ can catch you by surprise if you
expect your variable declared somewhere inside of one block of your script to be
available elsewhere.

```javascript
for (var count = 0; count < 5; count++) {
  console.log(count);
} // outputs the numbers 0 - 4 to the console

console.log(count); // 5

for (let otherCount = 0; otherCount < 5; otherCount++) {
  console.log(otherCount);
} // outputs the numbers 0 - 4 to the console

console.log(otherCount); // error, otherCount is undefined
```

The other alternative declaration is _const_ , which is supposed to represent a
constant. But it’s not completely constant.

A _const_ can’t be declared without a value, unlike a _var_ or _let_ variable.

```javascript
var x; // valid
let y; //valid
const z; // error
```

A _const_ will also throw an error if you try to set it to a new value after it’s been
declared:

```javascript
const z = 3; // valid
z = 4; // error
```

But if you expect your _const_ to be immutable in all cases, you may be in for a
surprise when an object or an array declared as a _const_ lets you alter its
content.

```javascript
const z = []; // valid
z.push(1); // valid, and z is now [1]
z = [2] // error
```
pagina 66
For this reason, I remain skeptical when people recommend using _const_
constantly in place of _var_ for all variable declarations, even when you have
every intention of never altering them after they’ve been declared.

While it’s a good practice to treat your variables as immutable, JavaScript won’t
enforce that for the contents of a reference variables like arrays and objects
declared with _const_ without some help from external scripts. So the _const_
keyword may make casual readers and newcomers to JavaScript expect more
protection than it actually provides.

I’m inclined to use _const_ for simple number or string variables I want to initialize
and never alter, or for named functions and classes that I expect to define once
and then leave closed for modification. Otherwise, I use _let_ for most variable
declarations — especially those I want to be bounded by the scope in which they
were defined. I haven’t found the need to use _var_ lately, but if I wanted a
declaration to break scope and get hoisted to the top of my script, that’s how I
would do it.

## Limiting the Scope of the Function

Traditional functions, defined using the _function_ keyword, can be called to
execute a series of statements defined within a block on any parameters passed
in, and optionally return a value:

```javascript
function doSomething(param) {
  return(`Did it: ${param}`);
}
console.log(doSomething("Hello")); // "Did it: Hello"
```

They can also be used with the _new_ keyword to construct objects with
prototypal inheritance, and that definition can be placed anywhere in the scope
where they might be called:

```javascript
function Animal(name) {
  this.name = name;
}

let cat = new Animal("Fluffy");
console.log(`My cat's name is ${cat.name}.`); // "My cat's name is Fluffy."
```

Functions used in either of these ways can be defined before or after they’re
called. It doesn’t matter to JavaScript.

```javascript
console.log(doSomething("Hello")); // "Did it: Hello"

let cat = new Animal("Fluffy");
console.log(`My cat's name is ${cat.name}.`); `My cat's name is ${cat.name}.`// "My cat's name is 

function doSomething(param) {
  return(`Did it: ${param}`);
}

function Animal(name) {
  this.name = name;
}
```

traditional function also creates its own context, defining a value for this that
exists only within the scope of the statement body. Any statements or subfunctions
defined within it are executing, and optionally allowing us to bind a
value for this when calling the function.

That’s a lot for the keyword to do, and it’s usually more than a programmer needs
in any one place. So modern JavaScript split out the behavior of the traditional
function into arrow functions and classes.


### Getting to Class on Time

One part of the traditional function has been taken over by the class keyword.
This allows programmers to choose whether they would prefer to follow a more
functional programming paradigm with callable arrow functions, or use a more
object-oriented approach with classes to substitute for the prototypal
inheritance of traditional functions.

Classes in JavaScript look and act a lot like simple classes in other object
oriented languages, and may be an easy stepping stone for Java and C++
developers looking to expand into JavaScript as JavaScript expands out to the
server.

One difference between functions and classes when doing object-oriented
programming in JavaScript is that classes in JavaScript require forward
declaration, the way they do in C++ (although not in Java). That is, a _class_ needs
to be declared in the script before it is instantiated with a _new_ keyword.
Prototypal inheritance using the _function_ keyword works in JavaScript even if
it’s defined later in the script, since a _function_ declaration is automatically
hoisted to the top, unlike a _class_.

```javascript
// Using a function to declare and instantiate an object (hoisted)
let aProto = new Proto("Myra");
aProto.greet(); // "Hi Myra"

function Proto(name) {
  this.name = name;
  this.greet = function() {
    console.log(`Hi ${this.name}`);
  };
};

// Using a class to declare and instantiate an object (not hoisted)
class Classy {
  constructor(name) {
    this.name = name;
  }
  greet() {
    console.log(`Hi ${this.name}`);
  }
};

let aClassy = new Classy("Sonja");
aClassy.greet(); // "Hi Sonja"
```

### Pointed Differences with Arrow Functions

The other aspect of traditional functions can now be accessed using arrow
functions, a new syntax that allows you to write a callable function more
concisely, to fit more neatly inside a callback. In fact, the simplest syntax for an
arrow function is a single line that leaves off the curly braces entirely, and
automatically returns the result of the statement executed:

```javascript
const traditional = function(data) {
  return (`${data} from a traditional function`);
}

const arrow = data => `${data} from an arrow function`;

console.log(traditional("Hi")); // "Hi from a traditional function"
console.log(arrow("Hi")); // "Hi from an arrow function"
```

Arrow functions encapsulate several qualities that can make calling them more
convenient, and leave out other behavior that isn’t as useful when calling a
function. They are not drop-in replacements for the more versatile traditional
_function_ keyword.

For example, an arrow function inherits both _this_ and _arguments_ from the
contexts in which it’s called. That’s great for situations like event handling or
_setTimeout_ when a programmer frequently wants the behavior being called to
apply to the context in which is it was requested. Traditional functions have
forced programmers to write convoluted code that binds a function to an
existing this by using _.bind(this)_. None of that is necessary with arrow
functions.

```javascript
class GreeterTraditional {
  constructor() {
    this.name = "Joe";
  }
  greet() {
    setTimeout(function () {
      console.log(`Hello ${this.name}`);
    }, 1000); // inner function has its own this with no name
  }
}

let greeterTraditional = new GreeterTraditional();
greeterTraditional.greet(); // "Hello "

class GreeterBound {
  constructor() {
    this.name = "Steven";
  }
  greet() {
    setTimeout(function () {
      console.log(`Hello ${this.name}`);
    }.bind(this), 1000); // passing this from the outside context
  }
}

let greeterBound = new GreeterBound(); // "Hello Steven"
greeterBound.greet();

class GreeterArrow {
  constructor() {
    this.name = "Ravi";
  }
  greet() {
    setTimeout(() => {
      console.log(`Hello ${this.name}`);
    }, 1000); // arrow function inherits this by default
  }
}

let greeterArrow = new GreeterArrow();
greeterArrow.greet(); // "Hello Ravi"
```

## Understand What You’re Getting

It’s not all just syntactic sugar. A lot of the new changes in JavaScript have been
introduced because new functionality was needed. But that doesn’t mean that
the old reasons for JavaScript’s traditional syntax have gone away. Often it
makes sense to continue using the traditional JavaScript syntax, and sometimes
using the new syntax can make your code much faster to write and easier to
understand.

Check out those online tutorials you’re following. If the writer is using _var_ to
initialize all of the variables, ignoring classes in favor of prototypal inheritance, or relying on _function_ statements in callbacks, you can expect the rest of the
syntax to be based on older, traditional JavaScript. And that’s fine. There’s still a
lot that we can learn and apply today from the traditional ways the JavaScript
has always been taught and used. But if you see _let_ and _const_ in the
intializations, arrow functions in callbacks, and classes as the basis for object oriented patterns, you’ll probably also see other modern JavaScript code in the
examples.

The best practice in modern JavaScript is paying attention to what the language
is actually doing. Depending on what you’re used to, it may not always be
obvious. But think about what the code you’re writing is trying to accomplish,
where you’re going to need to deploy it, and who will be modifying it next. Then
decide for yourself what the best approach would be.
