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

## ES5 Classes

## ES6 Classes

## Comparison

### Performance

### Features

## Conclusion
