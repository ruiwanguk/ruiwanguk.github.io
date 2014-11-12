---
layout:     post
title:      Constructor in JavaScript
date:       2014-11-04 20:27:19
summary:    Constructor in JavaScript can be a source of confusion for beginners, this confusion is inherited from its original language design.
categories: JavaScript
---

Constructor in JavaScript is often a source of confusion for people new to the language. Particularly if coming from a classical language (such as Java or C#), it can feel a bit alien at first. This post is aimed at clarifying the mechanical details behind the constructor, also serves as a self reminder on the inner workings of JavaScript. 

**Friendly warning**: Please make sure you have some basic understanding of JavaScript before reading further. 

###Inheritance

In classical languages (such as Java), inheritance provides two key benefits. First, it is the foundation for code reuse. Some people would argue that [Polymorphism](http://en.wikipedia.org/wiki/Polymorphism_(computer_science)) should be the correct term, but it is really just a clever way of making minor changes to code while retaining most of the business logic. Second, it is important for static type language to avoid casting between similar types. 

JavaScript on the other hand is a loosely typed programming language. This means that it does not need to cast. Therefore for JavaScript, the object's own inheritance hierarchy is not important, what matters is what it does.

JavaScript's inheritance is prototypal by nature. Unlike the classical languages, it does not have the concept of '_Class_'. Instead, inheritance is archived directly via objects. _Object A_ can inherit _Object B_'s methods and variables only if _Object B_ is a 'prototype' of the object. The root of this object inheritances is always Object.prototype.

For example, you can create an object using the JavaScript object literal:

	// create an empty object
	var myObj = {}

The object created in the above code seems to be empty, but actually it has a hidden reference to _Object.prototype_. Now, there are two key points worth highlighting here:

First, an object's prototype is always hidden: this seems so obvious, but it is so easy to get this concept wrong. What I mean by hidden? Once a object's prototype is assigned, it can not be accessed by code directly, and also it can not be re-assigned.

	// This will return nothing
	console.log(myObj.prototype)

Second, do not confuse _Object.prototype_ with an object's prototype. _Object.prototype_ is _Object_'s 'prototype' property, whereas object's prototype is the object created and inherited from. The reason why this is done this way will be become clear in the next section.

###Constructor

As mentioned by Douglas Crockford in [JavaScript:The Good Parts](http://shop.oreilly.com/product/9780596517748.do), JavaScript is conflicted about its prototypal nature. It was trying to emulate classical languages on constructing objects using the '_new_' keyword. For example:

	// a simple constructor
	var MyConstructor = function () {
		this.name = "Rui";
	}

	// create a new object
	var myObj = new MyConstructor();
	
	// prints 'Rui'
	console.log(myObj.name);

What happened was that a new object will be created if the _myConstructor_ function is invoked with the '_new_' prefix. '_this_' in the constructor function will be bound to the newly created object, and all the properties will be added to the new object directly. The new object will be returned as the result of the constructor call. 

More importantly, the prototype of the newly created object (_myObj_) is the value of the function's 'prototype' property. So if we change the code slightly:

	// a simple constructor
	var MyConstructor = function () {
		this.name = "Rui";
	}

	MyConstructor.prototype = {surname : "Wang"};

	// create a new object
	var myObj = new MyConstructor();
	
	// prints 'Wang'
	console.log(myObj.surname);

In the code above, we have assigned a new object to _MyConstructor.prototype_ with property 'surname'. _myObj_ inherits this property, so the value of _MyConstructor.prototype_ becomes the prototype of _myObj_. 

All constructor functions are normal functions in JavaScript. When a function object is created, the code that produces the function object runs something like this:

	this.prototype = {constructor : this}

So by default, invoking '_new_' on a constructor is going to produce an object that its prototype has a 'constructor' property, this property points to the original constructor function object. 

###Alternative

Instead of calling the constructor, one can adopt the coding pattern suggested by Douglas Crockford:

	if (typeof Object.create !== 'function') {
		Object.create = function (o) {
			var F = function() {};
			F.prototype = o;
			return new F();
		};
	}

	var newObj = Object.create(oldObj);

###Other resources
[Constructors Considered Mildly Confusing](http://zeekat.nl/articles/constructors-considered-mildly-confusing.html) provides nice diagrams that illustrate the points above.