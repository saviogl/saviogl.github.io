---
title: Understanding JavaScript Events
date:   2016-03-23 21:00:00
description: Build your own event emitter library to better understand how JavaScript events work
---

I'm going to start off by saying that JavaScript events are somewhat **'fake'** in the sense that there is no native event object in JavaScript nor it has a native eventing concept. What happens is that we can **'fake'** it by creating our own way of telling the code that something has happened and providing it a way to react to those actions, and we are going to do so by building a conceptual event emitter library using the techniques that the **[Node.js Event Emitter](https://github.com/nodejs/node/blob/master/lib/events.js)** uses.

> The Node.js Event Emitter is a built-in library and can be required (***var EEmitter = require('events')***) without any further dependencies. It's heavily used by Node.js, and many built-in libraries inherited from it.

# Building our own event emitter library
So, we are going to start building our tiny library by declaring a function constructor and assigning an empty **_events** object:

``` javascript
function Emitter (){
	this._events = {};
}
// this._events[type] = [listener1, listener2]
```

This **_events** object will hold all the event **listeners** for a certain **type** of event and will be the core object which our library will manipulate to mimic the eventing concept.

Next, we'll implement a basic and functional version of the infamous **.on** method, which will receive a **type** and a **listener** as function arguments.

``` javascript
Emitter.prototype.on = function (type, listener){
	this._events[type] = this._events[type] || [];
	this._events[type].push(listener);
}
```

This function will be responsible for registering **listeners** (Event Listener) for a certain **type** of event, which will be called when that event occurs.

> **Event Listener**: The code that responds to an event. In JavaScript's case the listener will be a function

Now that we are able to register listeners to respond to certain **types** of events, we have got to define a function to **emit** these events, and therefore notify all the registered listeners that something happened.

``` javascript
Emitter.prototype.emit = function (type){
	if (!this._events[type]) return;
	this._events[type].forEach(function (listener){
		listener();
	});
}
```

As we can see in the implementation above, the process of notifying the listeners of a certain **type** of event is nothing more than just looping through all the registered listeners for that **type** of event and call for its execution.

Let's stop a little bit and bundle up all the code that we've implemented so far:

``` javascript
function Emitter (){
	this._events = {};
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

If you haven't noticed yet, with those few lines of codes you've just built a functional event emitter library. Let's try it out:

``` javascript
function Emitter (){
	this._events = {};
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

var MyEmitter = new Emitter();

MyEmitter.on('hello world', function (){
	console.log('hello world');
});

MyEmitter.emit('hello world');
```

If you run that code on the **console tab** of you browser's inspector, or as a Node.js script you will see the console.log outputted to the screen, meaning that when the **'hello world'** event was emitted you were notified and your **listener** responded to it.

Of course this a very basic and raw implementation, it's not at all robust and **definitely not production ready!!!**, but it works and more importantly it lets you easily understand what is underneath one of the most used JavaScript event emitter library and get the gist on how events work in JavaScript, being nothing more than objects and keys full of array of functions being manipulated to mimic that **'event'** behavior.

# Peeking at the Node.js EventEmitter library

If we a take a look at the **[Node.js Event Emitter](https://github.com/nodejs/node/blob/master/lib/events.js)** implementation at GitHub, although it's much more complete and has tons of others features, we can find common pieces of codes used to implement the **'event'** behavior.

### constructor
![EventEmitter Init](/assets/images/init.png)

### .on
![EventEmitter on](/assets/images/on.png)

### .emit
![EventEmitter emit](/assets/images/emit.png)

As we can see the core concept remains the same, it's just a much better, and improved implementation :). 