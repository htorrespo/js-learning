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


## Limiting the Scope of the Function

### Getting to Class on Time

### Pointed Differences with Arrow Functions

## Understand What You’re Getting

