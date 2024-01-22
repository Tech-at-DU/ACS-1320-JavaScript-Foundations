<!-- .slide: data-background="./Images/header.svg" data-background-repeat="none" data-background-size="40% 40%" data-background-position="center 10%" class="header" -->
# ACS 1320 - Lesson 3 - OOP

<!-- Put a link to the slides so that students can find them -->

<!-- ➡️ [**Slides**](https://docs.google.com/presentation/d/1nheTZgY5fXrCbiw9hst0XGkr_fpnlAq44y7o5GrYvS4/edit?usp=sharing) -->

<!-- > -->

## Overview 🌏

Class Objects and OOP. Use Object Oriented programming techniques to make your code modular and organized. 

You've written lots of code so far you've probably incurred some [technical debt](https://en.wikipedia.org/wiki/Technical_debt). It's time to pay this off by refactoring. 

<!-- > -->

## Learning Objectives 🥸

- Use Refactoring to improve code quality
- Build systems with Objects 
- Define classes 
- Use dependency injection

<!-- > -->

## Refactoring Code 🍜

The goal of [refactoring code](https://en.wikipedia.org/wiki/Code_refactoring) in short is to improve your existing code base and put it into a shape that will accept future updates. 

<!-- > -->

Refactoring is not about adding new features. Instead, you want to have the **same functionality** with an **improved codebase** underneath it.

🏆

<!-- > -->

## Creating classes ⚒️

<!-- > -->

In JavaScript a Class is really a function and a function is really an object. All functions are just objects. Seriously try this: 

```JS
function FunkyTown() {
  console.log("Won't you take me to")
}

FunkyTown.band = 'Lipps Inc'

console.log(FunkyTown.band) // Lipps Inc
console.log(FunkyTown())    // "Won't you take me to"
```

<!-- > -->

A function is also a class! Yep, it sure is. Try this: 

```JS
const myTown = new FunkyTown()
```

<!-- > -->

Wait we need to add some properties to our object. Let's make a dog! 🐶

```JS 
function Dog(name) {
  this.name = name
}

const myDog = new Dog('Spot')
console.log(myDog.name) // Spot
```

<!-- > -->

We use the `new` keyword when creating instances. 

This creates a new object and assigns it to `this`. 

```js
// It's like this is happening in the background
new Dog() // this = {}
```

See how name got assigned to `this` in the Dog function? 

The Dog function is the constructor for our class.

<!-- > -->

How do we add a method? 

```JS 
function Dog(name) {
  this.name = name
}

Dog.prototype.bark = function() {
  console.log(`${this.name} says: gimme a biscuit!`)
}

const myDog = new Dog('Spot')
console.log(myDog.name) // Spot
myDog.bark() // Spot says: gimme a biscuit!
```

Methods are added to the prototype property! 

<!-- > -->

**Q:** I heard you could use the class keyword? 🤔

**A:** Yes you can it's a new syntax. It's better, you should use it. 💪

<!-- > -->

**Q:** Is it different? 🤔

**A:** Nope, it's the same thing. We call it "syntactical sugar." 🍰 It's a nicer flavor of the same old thing. 

<!-- > -->

How to make a Dog class 🐕 with the class keyword:

```JS 
class Dog {
  constructor(name) {
    this.name = name
  }

  bark() {
    console.log(`${this.name} says: gimme a biscuit!`)
  }
}

const myDog = new Dog('Spot')
console.log(myDog.name) // Spot
myDog.bark() // Spot says: gimme a biscuit!
```

<!-- > -->

NOTE! We used the `constructor()` function to initialize the class. 

Notice that all of the initial property values (`name`) are passed into the function here and assigned to `this`. **You must do this to store a value on the newly created class instance.**

<!-- > -->

### What about inheritence? 👩‍👧‍👦

<!-- > -->

You can do that with JS. SpaceDogs can shoot lasers from their eyes. 

```JS 
class SpaceDog extends Dog {
  constructor(name, planet) {
    super(name)
    this.planet = planet
  }

  shootLaser() {
    console.log(`${this.name} shoots lasers!`)
  }
}

const spaceDog = new SapceDog('Rocko', 'Mars')
console.log(spaceDog.name) // Rocko
spaceDog.bark() // Rocko says: gimme a biscuit!
spaceDog.shootLaser() // Rocko shoots lasers!
spaceDog.planet // Mars
```

<!-- > -->

We use the extends keyword to create a subclass of a prarent or super class. 

<!-- > -->

NOTE! You must call `super()` in the constructor. 🦹‍♀️ 

Note! Calling `super()` you must pass any properties required by consructor of the super class. This is how `name` is set in our super class `Dog`. 🦸‍♂️

<!-- > -->

## Modules 📦

<!-- > -->

Be sure to declare your script as a module: 

Edit index.html:

```HTML
<script src="main.js" type="module"></script>
```

Add type="module"

<!-- > -->

Use import and export to share things between modules. 

Export your Dog from Dog.js:

```JS
//Dog.js
class Dog() { ... }

export default Dog
```

Import your Dog into main.js:

```JS
//main.js
import Dog from './Dog.js'
```

<!-- > -->

### Creating Classes for Breakout

<!-- > -->

The engineering team has decided to **OOP**ify the whole game. 

<!-- > -->

**You** are in charge of the **refactor**. 

<!-- > -->

You need to make this game **Object Oriented**.

<!-- > -->

You need to make a class for each of the game objects.

- **Brick**
- **Ball**
- **Paddle**
- **Score**
- **Lives**

<!-- > -->

Objects give you an **abstract way to think about and visualize your code**. 

<!-- > -->

You'll be making a **Class for each** of these. 

Define properties in each class with the values that the object needs to do it's job. 

<!-- > -->

### Sprite class

<!-- > -->

A Sprite is a game object. Think about it like a rectangle on the screen. The game is built with Sprites. 

Everything on the screen has an x and y position and most things have a width and height and a color. 

- x
- y
- width
- height
- color

<!-- > -->

```JS
class Sprite {
  constructor(x = 0, y = 0, width = 100, height = 100, color = '#f00') {
    this.x = x
    this.y = y
    this.width = width
    this.height = height
    this.color = color
  }
}

export default Sprite
```

<!-- > -->

A Sprite class should have a `render()` method. This method needs to take in the ctx as a parameter. 

```JS
class Sprite {
  ...

  render(ctx) {
    ctx.beginPath();
    ctx.rect(this.x, this.y, this.width, this.height);
    ctx.fillStyle = this.color;
    ctx.fill();
    ctx.closePath();
  }
}
```

<!-- > -->

**Q:** Why pass the `ctx` as a parameter it's a global variable? 

**A:** We want to avoid global variables! These open our code up to problems. 

It also means the code only works when this mysterious value `ctx` happens to be defined in the global scope. If you were to use this class in another project error messages would start asking you why the mysterious `ctx` is not defined.

<!-- > -->

Passing `ctx` as a parameter is safe and reliable. The variable is scoped to this method. Any programmer can see where it is defined and decide how they will provide it. 

<!-- > -->

### Brick Class 🧱

<!-- > -->

The Brick class can extend the Sprite class. Brick only adds a single new property. A brick can also initialize itself to fixed expected values. See the width, height, and color. 

```JS
class Brick extends Sprite {
  constructor(x, y, width = 75, height = 20, color = '#0095DD') {
    super(x, y, width, height, color) // pass arguments to Sprite!
    this.status = true // adds a new property
  }
}
```

<!-- > -->

A brick is just a Sprite with an extra property called `status` and some standard values for `width`, `height`, and `color`.

<!-- > -->

Use the brick class! The bricks are stored in an array and initialized like this in the original code: 

```JS
var bricks = [];
for (var c = 0; c < brickColumnCount; c++) {
  bricks[c] = [];
  for (var r = 0; r < brickRowCount; r++) {
    bricks[c][r] = { x: 0, y: 0, status: 1 };
  }
}
```

<!-- > -->

The brick object is here: 

```JS
bricks[c][r] = { x: 0, y: 0, status: 1 };
```

The Object here has the properties: x, y, status.

```JS
{ x: 0, y: 0, status: 1 }
```

<!-- > -->

We can now replace this with: 

```JS
bricks[c][r] = new Brick(0, 0)
```

Here the two parameters are the x and y.

<!-- > -->

Let's be clear! 

```JS
{ x: 0, y: 0, status: 1 }

// and 

new Brick(0, 0)
```

Make an object with the same properties. 

<!-- > -->

### Ball class ⚽️

<!-- > -->

For example, the Ball class might look like this: 

```JavaScript
class Ball extends Sprite{
  constructor(x = 0, y = 0, radius = 10, color = "#0095DD") {
    super(x, y, 0, 0, color)
    this.radius = radius;
    this.dx = 2
    this.dy = -2
  }

  move() {
    this.x += this.dx
    this.y += this.dy
  }

  render(ctx) { // Overrides the existing render method!
    ctx.beginPath();
    ctx.arc(this.x, this.y, this.radius, 0, Math.PI * 2);
    ctx.fillStyle = this.color;
    ctx.fill();
    ctx.closePath();
  }
}
```

<!-- > -->

Here `Ball` Class defines instances which will have four properties. Two of the properties, `radius`, and `color` are assigned when the Ball is initialized. `color` has a default value. 

- `color`: the color the ball will render as
- `radius`: the size of the ball measured as it's radius
- `x`: the position of the ball on the x-axis of a canvas
- `y`: the position of the ball on the y-axis of a canvas

<!-- > -->

### Making an instance

<!-- > -->

Make instance of a class like this: 

```JS 
const ball = new Ball(200, 200, 10)

console.log( ball.x ) // 200
console.log( ball.y ) // 200
console.log( ball.radius ) // 10
```

<!-- > -->

### What about drawing the ball?

<!-- > -->

Objects like: Ball, Brick, and Paddle own all of the properties they need to render themselves on canvas. 

It makes sense they should own their render method. 

<!-- > -->

These objects should have a `render()` method.

```JS 
class Ball {
  ...
  render(ctx) {
    ctx.beginPath();
    ctx.arc(this.x, this.y, this.radius, 0, Math.PI * 2);
    ctx.fillStyle = this.color;
    ctx.fill();
    ctx.closePath();
  }
}
```

**Important**: The render method should take the canvas context as a parameter. 

<!-- > -->

### What about this global references?

<!-- > -->

The canvas context `ctx` is a global variable in the original code. To reference a global variable in a class is bad practice. Imagine you needed to change the name of that variable, you'd have to make lots changes. What if you wanted to use this class in another project, that project would have to define a variable with the same name. 

<!-- > -->

A better way to handle this is to pass the *dependancy* into the class as a parameter. 

```JS 
...
const ctx = canvas.getContext('2d')
...
class Ball {
  ...
  render(ctx) { // pass ctx here! 
    ctx.beginPath();
    ctx.arc(this.x, this.y, this.radius, 0, Math.PI * 2);
    ctx.fillStyle = this.color;
    ctx.fill();
    ctx.closePath();
  }
}
```

<!-- > -->

### Dependency Injection 💉

<!-- > -->

Call this: *dependency injection* 

```JS 
...
const ctx = canvas.getContext('2d')

class Ball {
  ...
  render(ctx) {
    ctx.beginPath();
    ctx.arc(this.x, this.y, this.radius, 0, Math.PI * 2);
    ctx.fillStyle = this.color;
    ctx.fill();
    ctx.closePath();
  }
}

const b = new Ball(200, 200, 10)
b.render(ctx) // pass the cts as an argument! 
```

<!-- > -->

**Calling methods and passing dependencies.**

```JS 
...
const ctx = canvas.getContext('2d')
const ball = new Ball(...)
...

function draw() {
  ball.move()
  ball.render(ctx) // pass the dependency!
  ...
}
```

<!-- > -->

### Lab

Start working on [Assignment 3](../Assignments/Assignment-3-OOP.md)

<!-- > -->

### Classes

Read up on classes in the JS docs: 

- [Review Classes in JS](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes) 

## After Class 

- [Assignment 3 OOP](../Assignments/Assignment-3-OOP.md)

<!-- > -->

## Additional Resources

1. Video Playlist walking through the entire assignment: https://www.youtube.com/playlist?list=PLoN_ejT35AEiSYr-OhYV-C6uWZgPLBMZM

<!-- > -->

<!-- ## Minute-by-Minute

| **Elapsed** | **Time** | **Activity** |
| ----------- | --------- | ----------- |
| 0:05 | 0:05 | Admin |
| 0:05 | 0:10 | [Overview](#overview) |
| 0:05 | 0:15 | [Learning Objectives](#learning-objectives) |
| 0:15 | 0:05 | [Refactoring](#refactoring-code) |
| 0:30 | 0:15 | [OOP and JS](#oop) |
| 0:45 | 0:15 | [Defining Classes](#js-classes) |
| 0:55 | 0:10 | [BREAK](#break) |
| 1:55 | 0:60 | [Lab - OOP](#lab) |
| 2:10 | 0:15 | [Post Lab Q & A](#after-lab) |
| 2:35 | 0:15 | [Dependency Injection](#dependency-injection) |
| 2:45 | 0:10 | Review Homework | -->
