---
title: Hopping on the ES6 train
date:   2016-04-05 14:00:00
description: Understanding and applying some basic features from the new ECMAScript 2015 (ES6)
---

Remember our [last post](http://saviolucena.com/2016/understanding-javascript-event-emmiter/) where we reasoned about JavaScript events by building our own tiny EventEmitter library? So, today we'll re-factor that same code to make use of some new features introduced by ECMAScript 2015 (ES6).

> [ECMAScript](https://en.wikipedia.org/wiki/ECMAScript) is a scripting-language specification standardized by an organization called Ecma International. JavaScript is one of the implementations of this language.

Up to the day of this post the latest version of Node.js (v5.10.0) ships with **V8 (4.6.85.31)** engine which allows us to use [this](https://nodejs.org/en/docs/es6/#which-features-ship-with-node-js-by-default-no-runtime-flag-required) set of features by default, without any additional runtime flags required. Today we'll take a brief look at three of those features:

- Classes
- Arrow Functions
- Const

> ***Runtime Flags*** - flags passed to a Node.js script call to make features not yet considered stable by V8 available. All ES6 features are split into three groups: **shipping** (stable), **staged** and **in progress**. Staged features can be enabled with the ***--es_staging or --harmony***, whereas in progress features must be enabled individually setting their respective harmony flag (--harmony_modules).

# Classes
Classes are being introduced in ECMAScript 6 and are just a syntactical sugar over the well known and existing prototype-based inheritance, meaning that the syntax used by **classes** is not introducing a **new** inheritance model to the language, it's just another way to implement the same model but with a more conventional syntax approach to create objects and deal with inheritance.

Classes are implemented in ES6 by the use of the ***class*** keyword followed by the name of the class.

``` javascript
class Emitter {
  ...
}
```

Everything inside of the curly brackets {} is the body of a class, and that's where you define class members, such as methods or constructors.

## Hoisting
One important thing to mention when working with classes is that as opposed to functions declarations, class declarations are not [hoisted](https://developer.mozilla.org/en-US/docs/Glossary/Hoisting) so you need to declare your class and then access it, otherwise it will throw ReferenceError.

## Constructors
The constructor method is a special method for creating and initializing an object created with a class. There can only be one special method with the name "constructor" in a class. A **SyntaxError** will be thrown if the class contains more than one occurrence of a constructor method.

> A constructor can use the ***super*** keyword to call the constructor of a parent class.

``` javascript
class Emitter {
  constructor() {
    this._events = {};
  }
}
```

At this point it's interesting for learning purposes to compare our previous implementation ES5, with our current one.

``` javascript
function Emitter (){
  this._events = {};
}
```

Both codes are implementing the same JavaScript prototype-based inheritance, at this point, you have a **class** called Emitter and a **function constructor** called Emitter which will instantiate the same object if called with the **new** keyword, and append a property _events to this object.

## Prototype Methods
Prototype methods are defined within the **class** body like so:

``` javascript
class Emitter {
  constructor() {
    this._events = {};
  }

  on(type, listener){
      this._events[type] = this._events[type] || [];
      this._events[type].push(listener);
  }

  emit(type){
      if(!this._events[type]) return;
      this._events[type].forEach(function (listener){
        listener();
      });
  }
}
```

These methods defined above will compose the prototype chain of newly created objects just like our previous implementation.

``` javascript
function Emitter (){
    this._events = {}
}

Emitter.prototype.on = function (type, listener){
    this._events[type] = this._events[type] || [];
    this._events[type].push(listener);
}

Emitter.prototype.emit = function (type){
    if (!this._events[type]) return;
    this._events[type].forEach(function (listener){
        listener();
    });
}
```

At this point we've fully re-factored our previous implementation of the Emitter class to work with the new **class** feature provided by ES6.

Although we didn't have a use for it in our code, another important feature regarding **Classes** is the sub classing ability using the ***extends*** keyword, with which you can create a class as a child of another class.

``` javascript
class Cat {
  constructor(name) {
    this.name = name;
  }

  speak() {
    console.log(this.name + ' makes a noise.');
  }
}

class Lion extends Cat {
  speak() {
    super.speak();
    console.log(this.name + ' roars.');
  }
}
```

> Check out more information on classes [here](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes)

# Arrow Functions
Arrow functions have a shorter syntax compared to the ordinary [function expression](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/function), lexically binds the **this** value and are always **anonymous**.

# Syntax
Arrow functions can be written in a few [different](http://wiki.ecmascript.org/doku.php?id=harmony:arrow_function_syntax) manners, but the most basic syntax is implemented as follow:

``` javascript
(param1, param2, …, paramN) => { statements }
```

Making use of arrow functions you can write short yet meaningful code specially in some functional patterns.

``` javascript
var a = ['Hydrogen', 'Helium'];
var a2 = a.map(function(s){ return s.length });
var a3 = a.map( s => s.length );
```

Arrow functions also introduce a new functionality; **Lexical** ***this***. Up until arrow functions, every new function defined its own ***this*** value depending on several conditions (a new object in case of a constructor, undefined in strict mode function calls, the context object if the function is called as an "object method", etc.), but now they capture the ***this*** value of the enclosing context, making it more suitable for object-oriented style of programming.

Since for our implementation we are only interested in the shorter syntax of arrow functions, we are not going to dig deeper into all of its features and use cases, but I invite you all to go check more information in [this](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Arrow_functions) link.

``` javascript
class Emitter {
  constructor() {
    this._events = {};
  }

  on(type, listener){
      this._events[type] = this._events[type] || [];
      this._events[type].push(listener);
  }

  emit(type){
      if(!this._events[type]) return;
      this._events[type].forEach(listener => { listener(); });
  }
}
```

# Const
The ***const*** declaration is another simple yet useful feature that can make our code more reliable, and semantically correct. It creates a read-only reference to a value. It does **not** mean the value it holds is immutable, just that the variable identifier cannot be reassigned.

A simple example to exemplify the ***const*** declaration can be taken from the [MDN website](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Statements/const):

``` javascript
// define MY_FAV as a constant and give it the value 7
const MY_FAV = 7;
// will throw TypeError
MY_FAV = 20;

// const also works on objects
const MY_OBJECT = {'key': 'value'};
// will throw TypeError
MY_OBJECT = {'OTHER_KEY': 'value'};
// However, object keys are not protected,
// so the following statement is executed without problems
MY_OBJECT.key = 'otherValue';
```

Trying out our newly implemented ***class*** we can make use of this ***const*** declaration:

``` javascript
class Emitter {
  constructor() {
    this._events = {};
  }

  on(type, listener){
      this._events[type] = this._events[type] || [];
      this._events[type].push(listener);
  }

  emit(type){
      if(!this._events[type]) return;
      this._events[type].forEach(listener => { listener(); });
  }
}

const MyEmitter = new Emitter();

MyEmitter.on('some_event', () => {
    console.log('some_event happened');
});

MyEmitter.emit('some_event');
```

# ES6 Support

All JavaScript engines are still working to support, not only those three features that we've seen today, but all the features proposed and specified by ECMAScript 2015 (ES6), so it's important that you check which features are already implemented before you get on with it. You can take a look at [this](https://kangax.github.io/compat-table/es6/) link to follow the implementation of the features by various engines.

## Third-Party compilers

Many features that are not yet implemented within JavaScript engines can be utilized by making use of a third party compiler. A fairly famous one is [Babel.js](https://babeljs.io/), go check it out.