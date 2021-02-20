# JavaScript’s New Private Class Fields, and How to Use Them

ES6 introduced classes to JavaScript, but they’re too simplistic for complex
applications. Class fields (also referred to as class properties) aim to deliver
simpler constructors with private and static members. The proposal is
currently at TC39 stage 3: candidate and could appear in ES2019 (ES10).

A quick recap of ES6 classes is useful before we examine class fields in more
detail.

## ES6 Class Basics

JavaScript’s prototypal inheritance model can appear confusing to developers
with an understanding of the classical inheritance used in languages such as
C++, C#, Java and PHP. JavaScript classes are primarily syntactical sugar, but
they offer more familiar object-oriented programming concepts.

A class is a template which defines how objects of that type behave. The
following Animal class defines generic animals (classes are normally denoted
with an initial capital to distinguish them from objects and other types):

pagina 88

```javascript
class Animal {
  constructor(name = 'anonymous', legs = 4, noise = 'nothing') {
  
    this.type = 'animal';
    this.name = name;
    this.legs = legs;
    this.noise = noise;
  }
  
  speak() {
    console.log(`${this.name} says "${this.noise}"`);
  }
  
  walk() {
    console.log(`${this.name} walks on ${this.legs} legs`);
  }
}
```

Class declarations execute in strict mode; there’s no need to add '_use strict_' .

A _constructor_ method is run when an object of this type is created, and it
typically defines initial properties. _speak()_ and _walk()_ are methods which add
further functionality.

An object can now be created from this class with the _new_ keyword:


```javascript
const rex = new Animal('Rex', 4, 'woof');
rex.speak(); // Rex says "woof"
rex.noise = 'growl';
rex.speak(); // Rex says "growl"
```

## Getters and Setters

Setters are special methods used to define values only. Similarly, Getters are
special methods used to return a value only. For example:

```javascript
class Animal {

  constructor(name = 'anonymous', legs = 4, noise = 'nothing') {
    this.type = 'animal';
    this.name = name;
    this.legs = legs;
    this.noise = noise;

  }

  speak() {
    console.log(`${this.name} says "${this.noise}"`);
  }

  walk() {
    console.log(`${this.name} walks on ${this.legs} legs`);
  }

  // setter
  set eats(food) {
    this.food = food;
  }

  // getter
  get dinner() {
    return `${this.name} eats ${this.food || 'nothing'} for dinner.`;
  }
}

const rex = new Animal('Rex', 4, 'woof');
rex.eats = 'anything';
console.log( rex.dinner ); // Rex eats anything for dinner.
```

## Child or Sub-Classes

It’s often practical to use one class as the base for another. If we’re mostly
creating dog objects, _Animal_ is too generic, and we must specify the same 4-leg
and “woof” noise defaults every time.

A Dog class can inherit all the properties and methods from the Animal class
using _extends_ . Dog-specific properties and methods can be added or removed
as necessary:

```javascript
class Dog extends Animal {

  constructor(name) {
  // call the Animal constructor
    super(name, 4, 'woof');
    this.type = 'dog';
  }
  
  'woof'// override Animal.speak
  speak(to) {
    super.speak();
      if (to) console.log(`to ${to}`);
  }
}

`to ${to}`
```

_super_ refers to the parent class and is usually called in the _constructor_ . In this
example, the Dog _speak()_ method overrides the one defined in _Animal_ .

Object instances of Dog can now be created:

```javascript
const rex = new Dog('Rex');
rex.speak('everyone'); // Rex says "woof" to everyone

rex.eats = 'anything';
console.log( rex.dinner ); // Rex eats anything for dinner.
```

## Static Methods and Properties

Defining a method with the static keyword allows it to be called on a class
without creating an object instance. JavaScript doesn’t support static properties
in the same way as other languages, but it is possible to add properties to the
class definition (a class is a JavaScript object in itself!).

The Dog class can be adapted to retain a count of how many dog objects have
been created:

```javascript
class Dog extends Animal {

  constructor(name) {
  
    // call the Animal constructor
    super(name, 4, 'woof');
    this.type = 'dog';

    // update count of Dog objects
    Dog.count++;
  }

  // override Animal.speak
  speak(to) {
    super.speak();
      if (to) console.log(`to ${to}`);
  }

  // return number of dog objects
  static get COUNT() {
    return Dog.count;
  }
}

// static property (added after class is defined)
Dog.count = 0;
```

The class’s static _COUNT_ getter returns the number of dogs created:

```javascript
console.log(`Dogs defined: ${Dog.COUNT}`); // Dogs defined: 0
const don = new Dog('Don');

console.log(`Dogs defined: ${Dog.COUNT}`); // Dogs defined: 1
const kim = new Dog('Kim');

console.log(`Dogs defined: ${Dog.COUNT}`); // Dogs defined: 2
```

For more information, refer to Object-oriented JavaScript: A Deep Dive into ES6
Classes. (https://www.sitepoint.com/object-oriented-javascript-deep-dive-es6-classes/)


## ESnext Class Fields

The class fields proposal allows properties to be initialized at the top of a class:

```javascript
class MyClass {
  a = 1;
  b = 2;
  c = 3;
}
```
This is equivalent to:

```javascript
class MyClass {
  constructor() {
    this.a = 1;
    this.b = 2;
    this.c = 3;
  }
}
```

Initializers are executed before any constructor runs (presuming a constructor is
still necessary).

pagina 93

### Static Class Fields

Class fields permit _static_ properties to be declared within the _class_ . For
example:


```javascript
class MyClass {
  x = 1;
  y = 2;
  static z = 3;
}

console.log( MyClass.z ); // 3
```

The inelegant ES6 equivalent:

```javascript
class MyClass {
  constructor() {
    this.x = 1;
    this.y = 2;
  }
}

MyClass.z = 3;
console.log( MyClass.z ); // 3
```

### Private Class Fields

All properties in ES6 classes are public by default and can be examined or
modified outside the class. In the _Animal_ example above, there’s nothing to
prevent the _food_ property being changed without calling the _eats_ setter:

```javascript
class Animal {
  constructor(name = 'anonymous', legs = 4, noise = 'nothing') {
    this.type = 'animal';
    this.name = name;
    this.legs = legs;
    this.noise = noise;
  }
  
  set eats(food) {
    this.food = food;
  }
  
  get dinner() {
    return `${this.name} eats ${this.food || 'nothing'} for dinner.`;
  }
}

const rex = new Animal('Rex', 4, 'woof');
rex.eats = 'anything'; // standard setter
rex.food = 'tofu'; // bypass the eats setter altogether
console.log( rex.dinner ); // Rex eats tofu for dinner.
```

Other languages permit private properties to be declared. That’s not possible
in ES6, although developers can work around it using an underscore convention
( `_propertyName` ), closures, symbols, or WeakMaps
(https://curiosity-driven.org/private-properties-in-javascript).

pagina 95

In ESnext, private class fields are defined using a hash # prefix:



## Immediate Benefit: Cleaner React Code!

## Using Class Fields Today

## Class Fields: an Improvement?
