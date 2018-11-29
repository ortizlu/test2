# Prototypal Inheritance in Javascript

## Learning Objectives

- Demonstrate the use case that explains prototypal inheritance
- Demonstrate what kind of flexibility prototypal inheritance gives programmers.

## Overview

1. Review of OOP
2. What is a Prototype?
3. The Prototype Chain

## Schedule

| Time   | Section                 |
| ------ | ----------------------- |
| 3 min  | Review OOP              |
| 5 min  | What is a Prototype     |
| 4 min  | The Prototype Chain     |
| 3 min  | Summary                 |
| 10 min | Get scrutinized by Zakk |
| 5 min  | Fail again              |
| 20 min | Debrief                 |

## Intro

### 1. Review of OOP in JS (3 minutes / 0:03)

So as you've been previously learning, object oriented programming is used to write cleaner and more organized code.

We organize objects based on their characteristics into a `class` (A blueprint per se). That class then has general properties and methods that can apply to multiple objects, and has a constructor function that defines the instances of our class. Instances are simply regular objects created by the class.

In addition, there's also a form of inheritance that is achieved through classes passing its properties and methods to subclasses. For example, in our previous car example, if you wanted to make a class that narrows properties of a specific type of car even more, you could make a `Honda` class that inherits its properties and methods from the `Car` class

```js
class Car {
  constructor(model, color) {
    this.model = model
    this.color = color
  }

  park() {
    console.log('parking.....')
  }
}

class Honda extends Car {
  constructor(model, color) {
    //because of super, this.model = model/this.color = color not required anymore because already declared in the Car class
    super(model, color)
    //this can also now be accessed because of super() to add a new property called make that will be given to all Honda instances
    this.make = 'Honda'
  }
  drive() {
    console.log('vroom vroom')
  }
}

let accord = new Honda('Accord', 'gray')

console.log(accord) //Honda {model: 'Accord', color: 'blue', make: 'Honda'}

console.log(accord.drive()) //vroom vroom
console.log(accord.park()) //parking.....
```

### 2. What is a Prototype? (5 minutes / 0:08)

So in this short class I'll give one example of prototypal inheritance, known as Prototype delegation. But before that, let's look at some terms

#### Vocabulary:

**prototype** - An object that all JS objects have access to. Objects use their prototype to inherit properties and methods from it.

**Prototype Chain** - the chain that is used in objects to look for a property inside themselves, their prototype (which is a reference to another object), and so on until finally reaching the `Object` object,and finally null.

#### We Do: Adding a property using the Prototype Property

Let's edit our previous car example to now use only objects and not classes.

```js
let honda = {
  make: 'honda',
  sound: 'vroom',

  drive() {
    console.log(`the ${this.make} says ${this.sound}`)
  }
}
```

so that's our honda object (similar to our honda subclass), now let's make the accord object

```js
let accord = Object.assign(Object.create(honda), {
  model: 'Accord',
  color: 'blue',
  fast: true //not really
})
```

So what did we do here with accord? We first used `Object.create` to create a new object and set the prototype to be Honda, then we used `Object.assign` to take that new object, along with the new values that we made with accord, and create a new object that way.

let's inspect the newly created object

```js
console.log(accord) //{ model: 'Accord', color: 'blue', fast: true }
```

So we see that `accord` has the properties `model`, `color`, and `make`

We can also check if a property belongs to the object by using the method `hasOwnProperty`

```js
//does accord have its own property named color?
accord.hasOwnProperty('color') //true
```

Now let's try using some of our cool new properties inherited from honda.

```js
accord.sound //vroom
```

just to make sure, let's check our `accord` object again to see if it has `isReliable`

```js
//does accord have its own property named sound?
console.log(accord.hasOwnProperty('sound')) //false
```

False? But Why? Because the property belongs to `Honda` and not accord. But we're able to access it using the `prototype` property

#### The Prototype Chain (4 minutes / 0:12)

(Check back to the definition of Prototype)

So how do we check out this famous `prototype` property? We can do it in two ways. The first (not recommended) is the `__proto__` method but its the most common since its the same name given to a prototype property in most browsers, including google Chrome. The second, more recommended way, is the getPrototypeOf() which is more descriptive. Let's try using `getPrototypeof()`first in Node and then let's head over to Chrome to take a better look.

```js
console.log(Object.getPrototypeOf(accord)) //{ make: 'honda', sound: 'vroom', drive: [Function: drive]}
```

(hmm... that's weird, it looks oddly familiar... Let's use chrome to take a closer look)

Check in Google Chrome for this part, and not Node

```js
accord
{model: "Accord", color: "blue", fast: true}
  color: "blue"
  fast: true
  model: "Accord"
  __proto__:
    drive: ƒ drive()
    make: "honda"
    sound: "vroom"
    __proto__:
      constructor: ƒ Object()
      hasOwnProperty: ƒ hasOwnProperty()
      isPrototypeOf: ƒ isPrototypeOf()
      propertyIsEnumerable: ƒ propertyIsEnumerable()
      toLocaleString: ƒ toLocaleString()
      toString: ƒ toString()
      valueOf: ƒ valueOf()
      __defineGetter__: ƒ __defineGetter__()
      __defineSetter__: ƒ __defineSetter__()
      __lookupGetter__: ƒ __lookupGetter__()
      __lookupSetter__: ƒ __lookupSetter__()
      get __proto__: ƒ __proto__()
      set __proto__: ƒ __proto__()
```

(Check back to the definition of the prototype chain)

So our example highlights exactly what the prototype chain is. An object checks itself for a property, and if it doesn't find it, checks its `__proto__` or prototype to see if it has it, if not it checks its parent prototype until it finds or does not find what its looking for.

## Summary (3 minute / 0:15)

- Demonstrate the use case that explains prototypal inheritance
- Demonstrate what kind of flexibility prototypal inheritance gives programmers.

So to conclude, this is only one form of prototypal inheritance. Other forms include functional inheritance (or factory functions), and concatenative inheritance (also known as mixins) which also use Object.assign.

And lastly, prototypal inheritance shows how modular JavaScript can be. Look how easier our new example was in terms of using other objects' properties in our own to make our accord object? If we wrap this in a function (also called a factory function) we can start pumping out objects like cake!

## Homework: [Prototype Party](https://git.generalassemb.ly/ga-wdi-exercises/)

#### Prototypal Inheritance

- [Inheritance and the Prototype Chain](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Inheritance_and_the_prototype_chain)
- [ES6 Classes and Javascript Prototypes](https://reinteractive.com/posts/235-es6-classes-and-javascript-prototypes)
- [Master the Javascript Interview: What's the Difference Between Class & Prototypical Inheritance](https://medium.com/javascript-scene/master-the-javascript-interview-what-s-the-difference-between-class-prototypal-inheritance-e4cd0a7562e9#.uzl8ohf8c)

```

```
