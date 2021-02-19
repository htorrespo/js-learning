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


## Static Methods and Properties

## ESnext Class Fields

### Static Class Fields

### Private Class Fields

## Immediate Benefit: Cleaner React Code!

## Using Class Fields Today

## Class Fields: an Improvement?