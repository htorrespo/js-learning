# Design Patterns

In this chapter, we’ll dig into the best way to implement a singleton in
JavaScript, looking at how this has evolved with the rise of ES6.

Among languages used in widespread production, JavaScript is by far the most
quickly evolving, looking less like its earliest iterations and more like Python, with
every new spec put forth by ECMA International. While the changes have their
fair share of detractors, the new JavaScript does succeed in making code easier
to read and reason about, easier to write in a way that adheres to software
engineering best practices (particularly the concepts of modularity and SOLID
principles), and easier to assemble into canonical software design patterns.

## Explaining ES6

ES6 (aka ES2015) was the first major update to the language since ES5 was
standardized in 2009. Almost all modern browsers support ES6
(https://kangax.github.io/compat-table/es6/). However, if you
need to accommodate older browsers, ES6 code can easily be transpiled into
ES5 using a tool such as Babel. ES6 gives JavaScript a ton of new features,
including a superior syntax for classes
(https://www.sitepoint.com/object-oriented-javascript-deep-dive-es6-classes/), 
and new keywords for variable
declarations. You can learn more about it by perusing SitePoint articles on the
subject.

### What Is a Singleton

In case you’re unfamiliar with the singleton pattern
(https://en.wikipedia.org/wiki/Singleton_pattern), it is, at its core, a design
pattern that restricts the instantiation of a class to one object. Usually, the goal is
to manage global application state. Some examples I’ve seen or written myself
include using a singleton as the source of config settings for a web app, on the
client side for anything initiated with an API key (you usually don’t want to risk
sending multiple analytics tracking calls, for example), and to store data in
memory in a client-side web application (e.g. stores in Flux).

A singleton should be immutable by the consuming code, and there should be no
danger of instantiating more than one of them.

There are scenarios when singletons might be bad, and arguments
that they are, in fact, always bad. For that discussion, you can check
out this helpful article on the subject
(https://www.sitepoint.com/whats-so-bad-about-the-singleton/).

## The Old Way of Creating a Singleton in JavaScript

The old way of writing a singleton in JavaScript involves leveraging closures and
immediately invoked function expressions 
(https://www.sitepoint.com/demystifying-javascript-closures-callbacks-iifes/). 
Here’s how we might write a (very
simple) store for a hypothetical Flux implementation the old way:

```javascript
var UserStore = (function(){
  var _data = [];

  function add(item){
    _data.push(item);
  }

  function get(id){
    return _data.find((d) => {
      return d.id === id;
    });
  }

  return {
    add: add,
    get: get
  };
}());
```
