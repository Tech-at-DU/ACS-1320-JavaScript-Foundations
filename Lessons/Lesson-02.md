<!-- .slide: data-background="./Images/header.svg" data-background-repeat="none" data-background-size="40% 40%" data-background-position="center 10%" class="header" -->
# ACS 1320 - Lesson 2 - JavaScript Professional Best Practice

<!-- Put a link to the slides so that students can find them -->

➡️ [**Slides**](https://docs.google.com/presentation/d/1ex_YOuuPXkCClYfjfQmmxK5Ur9X8cJzFfmDDNCQqBNM/edit?usp=sharing)

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
  - e.g. `const` > `let` > `var`
  - Deconstruction `const { a } = Obj`
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

<!-- > -->

1️⃣ **Setup npm**

```
npm init -y
```


2️⃣ **Install ESLint** 

```
npm install eslint -g
```

3️⃣ **Setup a config file**

```
npx eslint --init
```

Use the answers below as you follow the setup process. 

<!-- > -->

**Choose these options:**

- How would you like to use ESLint? To check syntax, find problems, and enforce code style
- What type of modules does your project use? None of these
- Which framework does your project use? None of these
- Does your project use TypeScript? No
- Where does your project run? Browser
- How would you like to define a style for your project? Use a popular style guide
- Which style guide do you want to follow? Airbnb: https://github.com/airbnb/javascript 
- What format do you want your config file to be in? JavaScript
- Would you like to install them now with npm? Yes

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

<!-- .slide: data-background="#087CB8" -->
## [**10m**] BREAK 🦜

<!-- > -->

### JS - Const, Let and Var


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

<!-- > -->

### JS - Functions and Hoisting

<!-- > -->

JavaScript is processed in two steps. In the first step, the JavaScript engine examines code and processes it. In the second step, the code is executed. 

<!-- > -->

When JS code is run one of the first steps is Hoisting. Hoisting effectively moves some elements to the top of their scope. Things that are affected by hoisting are:

- variables declared with `var`
- `function` declarations

<!-- > -->

### JS - Template Strings 

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

✚ is a common source of problems in JS

<!-- > -->

## Wrap up

List linter suggestions on the board. Split into groups of 4 to discuss these. Each group decides why these suggestions were included in the style guide. 

- What changes did the linter ask for? 
- Why do you think these changes? 
- Were linter errors that could not be solved?

<!-- > -->

## Homework

- [Assignment 2 ESLint.md](Assignments/Assignment-2-EsLint.md)

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
