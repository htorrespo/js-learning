# JavaScript Object Creation: Patterns and Best Practices

In this chapter, I’m going to take you on a tour of the various styles of
JavaScript object creation, and how each builds on the others in incremental
steps.

JavaScript has a multitude of styles for creating objects, and newcomers and
veterans alike can feel overwhelmed by the choices and unsure which they
should use. But despite the variety and how different the syntax for each may
look, they’re more similar than you probably realize.

## Object Literals

The first stop on our tour is the absolute simplest method of JavaScript object
creation — the object literal. JavaScript touts that objects can be created “ex
nilo”, out of nothing — no class, no template, no prototype — just poof!, an object
with methods and data:

pag 56

```javascript
var o = {
  x: 42,
  y: 3.14,
  f: function() {},
  g: function() {}
};
```

But there’s a drawback. If we need to create the same type of object in other
places, then we’ll end up copy-pasting the object’s methods, data, and
initialization. We need a way to create not just the one object, but a family of
objects.

## Factory Functions

The next stop on our JavaScript object creation tour is the factory function. This
is the absolute simplest way to create a family of objects that share the same
structure, interface, and implementation. Rather than creating an object literal
directly, instead we return an object literal from a function. This way, if we need
to create the same type of object multiple times or in multiple places, we only
need to invoke a function:

```javascript
function thing() {
  return {
    x: 42,
    y: 3.14,
    f: function() {},
    g: function() {}
  };
}

var o = thing();
```

But there’s a drawback. This approach of JavaScript object creation can cause
memory bloat, because each object contains its own unique copy of each
function. Ideally, we want every object to share just one copy of its functions.


## Prototype Chains

JavaScript gives us a built-in mechanism to share data across objects, called the
prototype chain. When we access a property on an object, it can fulfill that
request by delegating to some other object. We can use that and change our
factory function so that each object it creates contains only the data unique to
that particular object, and delegate all other property requests to a single, shared
object:

```javascript
var thingPrototype = {
  f: function() {},
  g: function() {}
};

function thing() {
  var o = Object.create(thingPrototype);
  
  o.x = 42;
  o.y = 3.14;
  return o;
}

var o = thing();
```

In fact, this is such a common pattern that the language has built-in support for
it. We don’t need to create our own shared object (the prototype object). Instead,
a prototype object is created for us automatically alongside every function, and
we can put our shared data there:

```javascript
thing.prototype.f = function() {};
thing.prototype.g = function() {};

function thing() {
   var o = Object.create(thing.prototype);
   
   o.x = 42;
   o.y = 3.14;
   return o;
}

var o = thing();
```

But there’s a drawback. This is going to result in some repetition. The first and
last lines of the thing function are going to be repeated almost verbatim in
every such delegating-to-prototype factory function.

## ES5 Classes

We can isolate the repetitive lines by moving them into their own function. This
function would create an object that delegates to some other arbitrary function’s
prototype, then invoke that function with the newly created object as an
argument, and finally return the object:

```javascript
function create(fn) {
  var o = Object.create(fn.prototype);
  
  fn.call(o);
  return o;
}

// ...
Thing.prototype.f = function() {};
Thing.prototype.g = function() {};

function Thing() {
  this.x = 42;
  this.y = 3.14;
}
var o = create(Thing);
```

In fact, this too is such a common pattern that the language has some built-in
support for it. The _create_ function we defined is actually a rudimentary version
of the _new_ keyword, and we can drop-in replace _create_ with _new_ :

```javascript
Thing.prototype.f = function() {};
Thing.prototype.g = function() {};

function Thing() {
  this.x = 42;
  this.y = 3.14;
}

var o = new Thing();
```

We’ve now arrived at what we commonly call “ES5 classes”. They’re object
creation functions that delegate shared data to a prototype object and rely on
the _new_ keyword to handle repetitive logic.

But there’s a drawback. It’s verbose and ugly, and implementing inheritance is
even more verbose and ugly.

## ES6 Classes

A relatively recent addition to JavaScript is ES6 classes, which offer a
significantly cleaner syntax for doing the same thing:

```javascript
class Thing {
  constructor() {
  this.x = 42;
  this.y = 3.14;
  }

  f() {}
  g() {}
}

const o = new Thing();
```

## Comparison

Over the years, we JavaScripters have had an on-and-off relationship with the
prototype chain, and today the two most common styles you’re likely to
encounter are the class syntax, which relies heavily on the prototype chain, and
the factory function syntax, which typically doesn’t rely on the prototype chain at
all. The two styles differ — but only slightly — in performance and features.


### Performance

JavaScript engines are so heavily optimized today that it’s nearly impossible to
look at our code and reason about what will be faster. Measurement is crucial.
Yet sometimes even measurement can fail us. Typically, an updated JavaScript
engine is released every six weeks, sometimes with significant changes in
performance, and any measurements we had previously taken, and any
decisions we made based on those measurements, go right out the window. So,
my rule of thumb has been to favor the most official and most widely used
syntax, under the presumption that it will receive the most scrutiny and be the
most performant most of the time. Right now, that’s the class syntax, and as I
write this, the class syntax is roughly 3x faster than a factory function returning a
literal.


### Features

What few feature differences there were between classes and factory functions
evaporated with ES6. Today, both factory functions and classes can enforce truly
private data—factory functions with closures and classes with weak maps. Both
can achieve multiple-inheritance factory functions by mixing other properties
into their own object, and classes also by mixing other properties into their
prototype, or with class factories, or with proxies. Both factory functions and
classes can return any arbitrary object if need be. And both offer a simple syntax.

## Conclusion

All things considered, my preference for JavaScript object creation is to use the
class syntax. It’s standard, it’s simple and clean, it’s fast, and it provides every
feature that once upon a time only factories could deliver.
