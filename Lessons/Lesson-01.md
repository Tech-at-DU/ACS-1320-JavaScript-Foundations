<!-- .slide: data-background="./Images/header.svg" data-background-repeat="none" data-background-size="40% 40%" data-background-position="center 10%" class="header" -->
# ACS 1320 - Lesson 1 - JavaScript Basics

<!-- Put a link to the slides so that students can find them -->

<!-- ➡️ [**Slides**](../Slides/Lesson-01.html#/) -->

<!-- https://docs.google.com/presentation/d/11uxo6bwj0HB4B7qKp5SNLfmL4LgZLB0BxDh6UB7dPfs/edit?usp=sharing -->

<!-- > -->

## Class Learning Objectives

The goal of this class is to improve and develop your JavaScript skill, knowledge, and to further develop your skills in software development. 

<!-- > -->

By the end of the class you should be able to:

- Use functions and variables
- Write for loops in JavaScript
- Write if statements in JavaScript
- Use Canvas to draw bitmap graphics
- Describe program flow

<!-- > -->

### Your Goals ⭐️

These are the two goals that will bring you success. 

- Building Portfolio Projects 🖼
- Master Learning Objectives 🧠

<!-- > -->

Portfolio projects 🖼 are what get you noticed 👁. Your portfolio is often the reason people get invited to job interviews 🤝.

<!-- > -->

### What are Learning Objectives? 

Learning objectives are the concepts and ideas that you need to know to claim mastery of a subject. 🏫

Learning Objectives are often skills that are related to success at job interviews and on the job. 😎

<!-- > -->

When you understand a learning objective you will be able to explain it and put it into practice. 

<!-- > -->

**There will be learning objectives for each class.** You should test your knowledge by explaining the concepts to someone else. 🤼‍♀️ 

<!-- > -->

**If you are having trouble understanding a learning objective you need to take action.** 

1. Discuss the topic with another student
1. Talk with a TA 
1. Bring questions to class
1. Talk to an instructor during lab or office hours. 

<!-- > -->

## Projects 

The goal is to produce something that shows your skills. 

- **Breakout** - This is a JavaScript implementation of the arcade game Breakout 
- **React Fundamentals Tutorial** - This tutorial will get you started with React
- **Custom/Contractor Project with React** - This will be a website built with React and is open to your ideas and input.

<!-- > -->

## Start Breakout tutorial

The Breakout tutorial is a great JS practice project. It makes use of many of the core concepts found in every programming language. 

<!-- > -->

- **Variables** 
- **Functions** 
- **Flow control** 
	- **loops** 
	- **if else** 
- **Arrays** 
- **Objects**

<!-- > -->

After completing the tutorial you will improve the code by applying modern techniques and best practices. This will include:

- Using **ES6** JS ideas and syntax
- **Linting** to professional standards 
- Using build systems and **bundling**

[Assignment 1 - Break Out](../Assignments/Assignment-1-Break-Out.md)

<!-- > -->

### Break Out 🧱

<!-- > -->

- What is Breakout?
- How does the tutorial game work? 
	- Draws with canvas
	- Use cartesian coordinates? 

<!-- > -->

[Breakout](https://en.wikipedia.org/wiki/Breakout_(video_game)) was invented by Atari in 1976! 

![atari-breakout](Images/atari-breakout.png)

It looked like this.

<!-- > -->

## Discussion

Answer these questions to check your understanding what is in the Break Out tutorial. 

<!-- > -->

### Defining Variables with JS
- Q: How are variables declared? 
- Q: What are the variables used in Break Out?
- Q: What value types are used in the Break Out tutorial? 

<!-- > -->

### Canvas 

Canvas is a JS API that allows you to draw bitmapped graphics. Use it to draw pictures. It's good for games. 

Canvas is a tag: 

```html
<canvas id="myCanvas"></canvas>
```

<!-- > -->

Think of canvas as the `<img>` tag with the ability to draw on it. 

Like the `<img>` tag you should give `<canvas>` width and height. 

```html
<canvas id="myCanvas" width="480" height="320"></canvas>
```

<!-- > -->

Your game will look like this:

![Breakout-0](Images/Breakout-0.png)

<!-- > -->

You will draw your game on Canvas with JS. 

Canvas allows you to draw shapes into a rectangular area.

<!-- > -->

Canvas is mapped in cartesian coordinates. 

![Breakout-1](Images/Breakout-1.png)

<!-- > -->

Canvas is mapped in cartesian coordinates. 

![Breakout-2](Images/Breakout-2.png)

<!-- > -->

You'll draw the ball from the center:

![Breakout-3](Images/Breakout-3.png)

<!-- > -->

You'll draw the rectangles from the upper left corner:

![Breakout-4](Images/Breakout-4.png)

<!-- > -->

To draw on canvas you need to get a canvas context.

```js
const canvas = document.getElementById("myCanvas");
const ctx = canvas.getContext("2d");
```

`ctx` is the canvas context. You'll call on methods of this object to draw on the canvas. 

- **Q:** What's a method?
- **Q:** What's an Object?

<!-- > -->

Draw on your canvas using the canvas API. The tutorial uses a few of the APIs methods.

<!-- > -->

Drawing with canvas generally follows these steps: 

1. begin a new path
1. draw a shape with one of the drawing methods
1. set the fill style 
1. set the stroke style 
1. fill and stroke your path

Here are a few examples...

<!-- > -->

**Draw a rectangle**

```js 
ctx.beginPath() // begins a new path
ctx.rect(x, y, width, height) // draws a rectangular path
ctx.fillStyle = "#0095DD" // sets the fill color
ctx.closePath() // closes the path
ctx.fill() // fills the current path
```

- **Q:** How big is the rectangle? 
- **Q:** Where is it in the canvas?

<!-- > -->

**Draw a circle**

```js
ctx.beginPath()
ctx.arc(x, y, radius, startAngle, endAngle)
ctx.fillStyle = "#0095DD"
ctx.fill()
ctx.closePath()
```

- **Q:** How big is the circle? 
- **Q:** Where is the circle?
- **Q:** What color is the circle? 

<!-- > -->

**Clear Rectangle**

```js
ctx.clearRect(x, y, width, height)
```

- **Q:** What area of the screen is being cleared? 

<!-- > -->

### Defining Functions in JS
- Q: How are functions defined in JS? 
- Q: What functions are defined in the Break Out tutorial? 
- Q: What do these functions do? 

<!-- > -->

### if else in JS
- Q: How is an if else structure written in JS?
- Q: What is this logical operator `||`?
- Q: Where is `if` and `if else` used in the tutorial? 

<!-- > -->

### Arrays in JS? 
- Q: What is an Array? 
- Q: Where are Arrays used in the tutorial? 
- Q: What is stored in the array in the tutorial? 

<!-- > -->

### Objects in JS
- Q: What is a Object in JS? 
- Q: Where are Objects used in the tutorial? 
- Q: What do thes objects represent? 
- Q: What are the properties of these objects? 

<!-- > -->

## Lab 60 mins

Start working on assignment 1 [here](../Assignments/Assignment-1-Break-Out.md).

This tutorial is 10 pages. At the end of every page is the completed source code. If you run into any problems you can check your code against the source.

<!-- > -->

While you work look for the things on this list:

- **Variables**
- **Functions** 
- **Flow control**
	- **loops** 
	- **if else** 
- **Arrays** 
- **Objects**

<!-- > -->

Today in class your job is to start on the tutorial and identify the things on this list

The code in the tutorial is very naive. You will be updating the code to modern standards in the next assignment.

**Come back to class at: ...**

<!-- > -->

## Review Breakout

- Show progress
- Review Objectives
- Questions 

<!-- > -->

## Homework: Breakout Tutorial 

Follow the instructions [here](../Assignments/Assignment-1-Break-Out.md)

<!-- > -->

## Additional Resources

1. [Types and Values ](https://eloquentjavascript.net/01_values.html)
1. [Variables](https://eloquentjavascript.net/02_program_structure.html#h_lnOC+GBEtu)
1. [Defining functions](https://eloquentjavascript.net/03_functions.html)
1. [Scope](https://eloquentjavascript.net/03_functions.html#h_XqQR5FlX+8)
1. [Arrays and Objects](https://eloquentjavascript.net/04_data.html)
1. [If else](https://eloquentjavascript.net/02_program_structure.html#h_wpz5oi2dy7)
1. [For loops](https://eloquentjavascript.net/02_program_structure.html#h_oupMC+5FKN)
1. [Canvas](https://eloquentjavascript.net/17_canvas.html)

<!-- > -->

<!-- ## Minute-by-Minute

| **Elapsed** | **Time** | **Activity** |
| ----------- | --------- | ------------ |
| 0:05 | 0:05 | [Class Learning Objectives](#class-learning-objectives) |
| 0:05 | 0:10 | [Projects](#projects) |
| 0:50 | 0:15 | [Start Break out](#start-break-out-tutorial) |
| 1:40 | 0:50 | [Lab: Break Out Tutorial](#lab) |
| 1:50 | 0:10 | [Break](#break) |
| 2:20 | 0:30 | Review progress on Tutorial |
| 2:40 | 0:10 | Review Homework |
| 2:45 | 0:05 | Review objectives | -->