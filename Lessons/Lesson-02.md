<!-- .slide: data-background="./Images/header.svg" data-background-repeat="none" data-background-size="40% 40%" data-background-position="center 10%" class="header" -->
# ACS 1320 - Lesson 2 - JavaScript Professional Best Practice

<!-- Put a link to the slides so that students can find them -->

<!-- ➡️ [**Slides**](https://docs.google.com/presentation/d/1ex_YOuuPXkCClYfjfQmmxK5Ur9X8cJzFfmDDNCQqBNM/edit?usp=sharing) -->

<!-- > -->

## Class Learning Objectives 📋

- Describe linting
- Use ESLint
- Evaluate your work against professional standards
- Write JS to professional standards

<!-- > -->

## Overview 🔎

In this class, you will look at the JS you wrote in the tutorial to improve and upgrade it to modern and professional standards.

<!-- > -->

## Code Quality 🥇

Pair up and discuss code quality. Answer these questions:

- What is code quality?
- What does quality code look like?
- How do you write quality code?

<!-- > -->

- Consistent style ✅
  - Reads well to Everyone on your team 
  - Uses consistent syntax
- Uses best practices ⭐️
  - e.g. `const` > `let` > ~~`var`~~
  - Deconstruction `const { a } = Obj`
  - Template strings: ``Score: ${score}``
- DRY

<!-- > -->

### Why is this important? 🤔

<!-- > -->

You should be using the professional JS in your work. It is expected in a work environment. 

👨‍💻 👩‍💻 👩‍💼 🧑‍💼

<!-- > -->

Improves the quality of your work by reducing errors and making your code easier to understand. 

🙌

<!-- > -->

You will be using the Airbnb JS style guide. This guide was developed at Airbnb and is used by their engineering team. It defines the coding style expected from Airbnb engineers.

🔬 🧬 🧫

<!-- > -->

**Using this style guide is professional best practice, it will make your code better, and make you write higher quality code.**

🏅

<!-- > -->

## Linting 🔎

<!-- > -->

**Lint** was the name of a program that flagged suspicious non-portable constructs in C code that were likely to cause bugs. The term is now applied generically to tools that flag suspicious code and inconsistent style in any language.

<!-- > -->

### ESLint 🔎

**ESLint** is a tool that "lints" your JavaScript. It has many options and is widely used. 

You will set up ESLint with a Style guide used by professionals. 

<!-- > -->

**ESlint has two parts:**

1. 🔌 A plugin/extension for your code editor
1. 📦 A package with options specific to your project

<!-- > -->

**Install eslint in your project**

https://eslint.org/docs/user-guide/getting-started

### 0. **init npm**
Create a package.json:
```bash
npm init -y
```

### 1. **Install ESLint**
First, install ESLint if you haven't already:
```bash
npm install --save-dev eslint
```

### 2. **Install the Airbnb Style Guide**
Use `install-peerdeps` to install the Airbnb configuration along with its required peer dependencies:
```bash
npx install-peerdeps --dev eslint-config-airbnb
```

This command will install `eslint-config-airbnb` and all its required peer dependencies, such as `eslint-plugin-import`, `eslint-plugin-react`, and others.

### 3. **Manually Configure ESLint**
If `npx eslint --init` does not handle the Airbnb configuration automatically, you can add it manually to your ESLint configuration file.

1. Open or create the `.eslintrc` file in your project root.
2. Add the Airbnb style guide under the `extends` property. For example:
   ```json
   {
     "extends": ["airbnb"]
   }
   ```

### 4. **Lint Your Code**
Run ESLint to verify everything is working:
```bash
npx eslint .
```

### **Additional Tips**
- If you're using a specific framework like React, ensure you include the necessary plugins, which should already be installed with `eslint-config-airbnb`.
- If you prefer a specific configuration format (e.g., `.eslintrc.js` or `.eslintrc.yml`), adjust accordingly.

<!-- > -->

**Install ESLint in your code editor**

You'll need to install ESLint in your code editor. 

- 1️⃣ Go to Packages/Plugins/Extensions
- 2️⃣ search for ESLint and install. 

<!-- > -->

### Lint your code 🧼

<!-- > -->

**Move your code into a JS file** (If you haven't already)

Then link that to your project. In index.html use: 

`<script src="index.js"></script>`

<small>The linter will only lint files with the `.js` file extension. Move your code into it a separate JS file `index.js`. </small>

<!-- > -->

**Start Linting** 🦫

Take a look at `index.js`. There should be some red lines highlighting sections of your code. These are linting errors.

<!-- > -->

🔬

Move the cursor over these and you'll see notes from the linter telling you how your code doesn't meet the requirements of the style guide. 

<!-- > -->

🔨

Your job id to figure these out and solve the problems. Your deeper and more important goal job is to do your best to understand why professionals would ask for these changes. 

<!-- > -->

Take a look at the errors you see and answer these questions: 

- 💬 What changes is the linter asking for? 
- 🤔 Why these changes?

<!-- > -->

## JS Best Practice

You can read about the Airbnb style guide here: 

https://github.com/airbnb/javascript#types

Take a look at the style guide. Pair and discuss.

<!-- > -->

## Why are we using ESLint and a style guide?

Linting your is best prasctice. Adding the style guide is like having an industry professional looking over your shoulder offering helpful advice. 

**You should be asking yourself why the linter is is asking you to make changes.**

In a professional environment you would be required to use a linter!

<!-- > -->

## Discussion
Answer these questions to test your understanding.

### JS - Const, Let and Var
- Q: Why does the linter suggest using `const` and `let` and not `var`?
- Q: What is difference between `const`, `let` and `var`?

Bad 

```JS
var score = 0
```

Goode 

```JS
let score = 0
```

Why? `const` and `let` are block-scoped and are not hoisted. This makes your code more predictable.

`const` defines a value that can not be reassigned. Knowing a value will not be reassigned makes code more efficient. It also adds safety to your program. 

https://javascript.info/variables

<!-- > -->

### Hoisting
- Q: What is hoisting? (look it up!)
- Q: Does hoisting affect this tutorial?
- Q: Does hoisting affect `const`, `let`, and `var`?

<!-- > -->

JavaScript is processed in two steps. In the first step, the JavaScript engine examines code and processes it. In the second step, the code is executed. 

<!-- > -->

When JS code is run one of the first steps is Hoisting. Hoisting effectively moves some elements to the top of their scope. Things that are affected by hoisting are:

- variables declared with `var`
- `function` declarations

https://javascript.info/var

<!-- > -->

### Template Strings 
- Q: What is a template string? 
- Q: Were template strings used in the tutorial?

Best Practice use template literal over the + for concatenation. 

Bad!

```JS 
const name = 'joe'
console.log('hello' + name) // concatenate with +
```

Good

```JS
const name = 'joe'
console.log(`hello ${name}`) // concatenate with template literal
```

<!-- > -->

### What's wrong with `"" + ?`

The + is used for addition _and concatenation_. 

This makes it ambiguous sometimes and might confuse or generate errors.

```JS
const str = "Score:" + score + 10 // Score:1010
```

**✚ is a common source of problems in JS**

https://javascript.info/string

<!-- > -->

## Object Destructuring
- Q: What is Object Destructuring? (Look it up: https://javascript.info/destructuring-assignment)
- Q: Did the linter ask you to use this? 

Any time you have an object you use the dot to access one of its properties. All of these dots make code harder to read. Use destructuring to make code easier to read. 

```JS
const obj = {a: 11, b: 22}
const { a, b } = obj 
console.log(a, b) // 11, 22
```

Notice the properties were moved into valariables with the same names! This is easier to read than: 

```JS
const obj = {a: 11, b: 22}
console.log(obj.a, obj.b) // 11, 22
```

https://javascript.info/destructuring-assignment

<!-- > -->

## Challenge Problems

### Challenge 1

The tutorial works but isn't written well. Notice that the array that holds the list of brick objects. Look closely. Each of these objects stores the x and y position as 0 and 0. This doesn't make any sense!

Now look at the draw the `drawBricks` fucntion. This is called every "frame" as the canvas is updated. Here the x and y properties are set. They are constantly calculated and set to the same value! 

More effecient would be to set the initial values to the correct x and y positions when the array is created, and not update the values in `drawBricks`. 

### Challenge 2

Remember that `makeBo(x, y, width, height, color)` function? It might be useful in the Break Game! Implement it there! 

### Challenge 3 

The game is very predictable! Notice that the ball always starts at the same angle every time! You can make vary this by using a random number! Here are a couple things to keep in mind. 

- Use `Math.random()` to generate a random number between 0 and 1. You can multiply this by another number to scale it to a range you like. For example `Math.random() * 10` would give a random number between 0 and 10.
- `dx` and `dy` set the speed and direction of the ball. The default values are 2 and -2.
- `dx` can be any number between 2 and -2.
- `dy` should always be negative, since the ball needs to start moving up the screen!

**Advanced!** The speed is a vector. If the ball is moving 2 on the x and 2 on the y the total distance moved is 2.83. Remember Pythagoras! Look up hypotenuse. 

To calculate the distance the ball should move at any angle, you can: 
- Generate an angle in randians, something like: `angle = Math.random() * Math.PI * 2`
- Use sine and cosine to get the `dx` and `dy` values something like: `Math.sin(angle) * speed` and `Math.cos(angle) * speed` where speed is the speed of the ball. In the original code the ball speed is 2. 

## Homework

- [Assignment 2 ESLint.md](Assignments/Assignment-2-ESLint.md)

<!-- > -->

## Additional Resources

1. https://eslint.org/docs/user-guide/getting-started
1. https://www.youtube.com/watch?v=SydnKbGc7W8
1. [Assignment 2 ESLint.md](Assignments/Assignment-2-EsLint.md)

<!-- ## Minute-by-Minute

| **Elapsed** | **Time** | **Activity** |
| ----------- | --------- | ----------- |
| 0:05 | 0:05 | Roll |
| 0:10 | 0:05 | [Class Learning Objectives](#class-learning-objectives) (Lecture) |
| 0:20 | 0:10 | [Code Quality](#code-quality) (Discussion) |
| 0:40 | 0:20 | [ESLint - Setup](#eslint) (Lab) |
| 1:40 | 0:60 | [ESLint - Lab](#lab) (Lab) |
| 1:50 | 0:10 | Break |
| 2:00 | 0:10 | [JS Best Practice](#js-est-practice) |
| 2:40 | 0:40 | Code Review/Lab Time |
| 2:45 | 0:05 | [Homework](#homework) | -->

<!-- > -->
