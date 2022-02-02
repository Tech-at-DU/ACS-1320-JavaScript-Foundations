<!-- .slide: data-background="./Images/header.svg" data-background-repeat="none" data-background-size="40% 40%" data-background-position="center 10%" class="header" -->
# ACS 1320 - Lesson 5 - JS OOP Inheritence

<!-- Put a link to the slides so that students can find them -->

<!-- ➡️ [**Slides**](/Syllabus-Template/Slides/Lesson1.html ':ignore') -->

<!-- > -->

# Overview

This class will be an OOP workshop to review the concepts from the previous class and expand your understanding of OOP to include inheritence.

<!-- > -->

## Why learn Classes and Inheritance?

This is a big topic which reaches deeply into important areas of computer science. Expect to see OOP and inheritance in the libraries you might work with, in interview questions, and on the job as a software engineer. 

<!-- > -->

## Learning Objectives

1. Create base and super classes 
1. Use inheritance with `super` and `extends`
1. Create classes that inherit from a superclass

<!-- > -->

# Inheritance 

Inheritance is when you get something from your ancestors. By definition it's the act of passing property from an ancestor to a descendant. Properties can be: 

- genes - 💇‍♀️ 💇🏻‍♀️
- money - 💰 💎
- property - 🏡 🚗

<!-- > -->

In software a class can inherit properties and methods: 

- variables/properties - `'Foo', 999.999`
- methods/functions - `function draw() {}`

<!-- > -->

Who do _you_ inherit from? 

- Your parents - 👨‍👩‍👦‍👦
- Your grandparents - 👵🏽

<!-- > -->

In software who do you inherit from?

- Your parent/superclass -
  - `Rectangle` inherits from `Polygon` ▬ ⬢
  - `Pokemon` inherits from `GameObject` inherits from `Sprite` 

<!-- > -->

## Inheritence with JS

Any class can inherit from another class. You can also think of classes that inherit from another class as **extensions** of the other class. So you could say one class extends another class.

<!-- > -->

```js
// Sprite defines two properties and one method
class Sprite {
  constructor(x, y, width, height, color) {
    this.x = 0
    this.y = 0
    this.width = width
    this.height = height
    this.color = color
  }

  render(ctx) { ... }
}
```

<!-- > -->

```JS
// Ball has all of the properties and methods 
// that Sprite has and adds one new property
class Ball extends Sprite {
  constructor(x, y, radius = 10, color) {
    super(x, y, radius * 2, radius * 2, color) // Calling super initializes the super class!
    this.radius = radius
  }
}

// Making an instance 
const ball = new Ball(0, 0) 
// { x: 0, y: 0, radius: 10, render: function(){ ... } }
```

<!-- > -->

Calling `super()` in a subclass is like calling the constructor function in your **superclass**. You must supply any arguments that are required. 

**You must call `super()`!** It's how the properties in the super class get initialized!

<!-- > -->

If a class takes parameters in its constructor it **must pass these to super**. 

```js
class Sprite {
  constructor(x, y) {
    this.x = 0
    this.y = 0
  }

  render(ctx) { ... }
}

class Ball extends Sprite {
  constructor(x, y, color, radius) {
    super(x, y) // Must pass parameters to super!
    this.color = color
    this.radius = radius
  }
}
const ball = new Ball(10, 20, 'red', 30) 
// { x: 10, y: 20, color:'red', radius: 30 }
```

<!-- > -->

You must pass parameters to super. Notice the constructor takes these parameters, **calling super is like calling the constructor of the superclass.**

<!-- > -->

![Super and interitence](images/Inheritence-and-super.png)

<!-- > -->

# Inheritance Challenges

<!-- > -->

**Challenge 1**: many of the classes you created for Break Out share some of the same properties. Moving these to a super class would keep them in one locaiton that could be shared, edited, and make it easier to reason about. This would make your code DRY. 

<!-- > -->

All of these classes: 

- `Ball` ⚽️
- `Paddle` 🏓
- `Brick` 🧱
- `Lives` 🖖
- `Score` 🔟

<!-- > -->

Share these properties: 

- `x`
- `y`
- `width`
- `height`
- `color`

<!-- > -->

Sprites represent an object on the screen. They can all use these methods:  

- `move(x, y)`
- `render(ctx)`

<!-- > -->

Your goal with this challenge is to define a base class, let's call it Sprite. 

Here is some starter code: 

```js
class Sprite {
  constructor(x, y, width, height, color) {
    this.x = 0
    this.y = 0
    this.width = width
    this.height = height
    this.color = color
  }
}
```

<!-- > -->

Sprites can draw a rectangle. Give Sprite a render method: 

```js
class Sprite {
  constructor(x, y, width, height, color) {
    ...
  }

  render(ctx) {
    ctx.beginPath()
    ctx.rect(this.x, this.y, this.width, this.height)
    ctx.fillStyle = this.color
    ctx.fill()
  }
}
```

<!-- > -->

A Brick is a rectangle so it uses all of the properties of Sprite. But a Brick adds a new property: status. Create a brick by extending Sprite: 

```JS
class Brick extends Sprite {
  constructor(x, y, width = 75, height = 20, color = 'fuchsia') {
    super(x, y, width, height, color)
    this.status = 1;
  }
}
```

<!-- > -->

What properties does the Paddle class have beyond the properties of Sprite? 

```JS
class Paddle extends Sprite {
  // What properties would Paddle have? 
  constructor(x, y, width = 75, height = 20, color = 'orange', ?) {
    // initialize Sprite
    super(x, y, width, height)
    // Define the new properties here?
  }
}
```

<!-- > -->

All of these classes should extend the `Sprite` class. 

- `Ball`
- `Paddle`
- `Brick`
- `Lives`
- `Score`

<!-- > -->

**Challenge 2**: Some of the classes draw themselves with the same code.

- `Paddle`
- `Brick`

Both of these classes draw as Rectangles willed with a color. You can give the `Sprite` class a `render(ctx)` method. 

<!-- > -->

Doing this will allow any class that extends `Sprite` to use `render()` this method. 

<!-- > -->

Classes that draw themselves differently:

- `Ball`
- `Lives`
- `Score`

Will define their own `render()` which will override the render method defined in Sprite.

<!-- > -->

Override a method by defining that method in the child class. For example: 

```JS
class Ball extends Sprite {
  constructor(...) { ... }

  render(ctx) {
    ctx.beginPath()
    ctx.arc(this.x, this.y, this.radius, 0, Math.PI * 2)
    ctx.fillStyle = this.color
    ctx.fill()
  }
}
```

<!-- > -->

![Inheritence Methods](images/inheritence-methods.png)

<!-- > -->

In the case of Ball you are overriding the render method that exists in Sprite.

When a sub class defines a method with the same name as a method in it's super class that method is used instead of the method in the Super class. 

<!-- > -->

![override method](images/override-method.png)

<!-- > -->

<!-- .slide: data-background="#087CB8" -->
## BREAK

Take a 10-minute break and think about things in the world that share properties. 

<!-- > -->

## Wrap Up

- Review Inheritence

<!-- > -->

## After Class 

- [Assignment 4 Inheritence](../Assignments/Assignment-4-Inheritance.md)

<!-- > -->

## Additional Resources

1. https://javascript.info/classes


<!-- > -->

<!-- ## Minute-by-Minute

| **Elapsed** | **Time** | **Activity** |
| ----------- | --------- | ------------ |
| 0:05 | 0:05 | admin |
| 0:05 | 0:10 | [Overview](#overview) |
| 0:05 | 0:15 | [Why learn Classes and inheritance](#why-learn-classes-and-inheritance) |
| 0:20 | 0:05 | [Learning Objectives](#learning-objectives) |
| 0:50 | 0:30 | [Inheritence](#inheritence) |
| 1:00 | 0:10 | [BREAK](#break) |
| 2:25 | 1:30 | Lab |
| 2:45 | 0:20 | [Wrap up](#wrap-up) | -->
