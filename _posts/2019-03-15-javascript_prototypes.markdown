---
layout: post
title:      "Javascript Prototypes"
date:       2019-03-15 19:03:28 +0000
permalink:  javascript_prototypes
---


Vincent is a really handsome cat. Let's bring him into the Javascript world. There are many ways to create objects in Javascript. We'll instantiate him with Object Literal.

`let vincent = {name: 'Vincent'}`

Let's add more properties to describe him a little bit more.
We can add properties easily to the already created object with dot notation.

```
vincent.breed = 'Maine coon'
vincent.weight = '18 lbs'
```

Vincent is a little lonely so we're going to create a few friends using classes.
A class is like a blueprint for creating objects so we can create multiple objects that are similar to one another. We're also going to add a method 'meow' so they can talk to each other.

```
class Cat {
	constructor(name, breed, weight) {
	this.name = name;
	this.breed = breed;
	this.weight = weight;
	}

	meow() {
	console.log("Meow!")
	}

}

```


The class declaration is just syntactic sugar on top of the ES5 constructor function so it can be rewritten like this:

```
function Cat(name, breed, weight) {
	this.name = name;
	this.breed = breed;
	this.weight = weight;
};
```

And the meow method would look like this.

```
Cat.prototype.meow = function() {
	console.log("Meow!")
}
```

Using prototype is great because the method implementation will only occur once instead of every time new object is created. It will actually save a little bit of  memory and increase performance.

```
let velvet = new Cat('Velvet', 'Turkish Van', '12 lbs')
let viking = new Cat('Viking', 'Bombay', '13 lbs')
let vann = new Cat('Vann', 'Bombay', '14 lbs')
```

Here we created three'instance-objects' of a Cat. Let's check out properties of velvet.
```
Cat {name: "Velvet", breed: "Turkish Van", weight: "12 lbs"}
  breed: "Turkish Van"
  name: "Velvet"
  weight: "12 lbs"
  __proto__:
    constructor: class Cat
    meow: ƒ meow()
    __proto__: Object
```

Meow method is a property of prototype and they all share the same reference to the Cat prototype.
`velvet.__proto__ === Cat.prototype`
 returns true.
 
```
velvet.meow();
//Meow!
```
Velvet said meow to Vincent but he can't meow back because he was not created with the class Cat. 

```
vincent.meow();

VM236:1 Uncaught TypeError: cat.meow is not a function
    at <anonymous>:1:5
(anonymous) @ VM236:1
```

But there is still hope! Meow method is part of the Cat prototype so we can assign him with `setPrototypeOf`.

```
Object.setPrototypeOf(vincent, Cat.prototype);

vincent.meow();
//Meow!
```


Yay! Now he can talk with beautiful Velvet. Vincent wants to show off his ability to jump really high. We can add methods after the object was created.

```
vincent.jumpHigh = function() {
	return this.name + " can jump really high!";
}

console.log(vincent.jumpHigh());

Vincent can jump really high!
```

Velvet is really impressed with his skills and falls in love with him! They lived together happily ever after.




