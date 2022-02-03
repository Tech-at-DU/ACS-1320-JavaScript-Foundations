<!-- .slide: data-background="./Images/header.svg" data-background-repeat="none" data-background-size="40% 40%" data-background-position="center 10%" class="header" -->
# ACS 1320 - Lesson 5 - Bundling JS

<!-- Put a link to the slides so that students can find them -->

<!-- ➡️ [**Slides**](/Syllabus-Template/Slides/Lesson1.html ':ignore') -->

<!-- > -->

# Overview

Bundling 🎁 is a common practice used across the JS ecosystem. While JS is not compiled to machine code like other languages, JS files are processed heavily in modern applications. 

<!-- > -->

## Why learn bundling

- 👩‍🍳 It's a professional best practice
- 👩‍💻 Expect this to be used in a software job
- 🙌 Make more reliable apps
- 🔌 Make compatible applications

<!-- > -->

## Learning Objectives

1. Describe bundling
1. Describe why bundling is important
1. Implement bundling system with webpack

<!-- > -->

# Bundling and compiling JS

<!-- > -->

Has anyone used bundling or JS compiler before? 

What did it do? 

<!-- > -->

## Bundling 🎁

<!-- > -->

Modern JS applications are not just written in code and used. At the professional level, they are compiled/bundled. 👩‍🍳 ➡ 🥧

<!-- > -->

JavaScript is a scripting language. This means it doesn't need to be compiled in a strict sense, processed and turned into machine code. 

<!-- > -->

While it doesn't need to be turned into machine code we can still do work to make our code more reliable and compatible with the widest array of browsers.

👨‍✈️ 👨‍🎨 👩‍🍳 🧑‍🌾 👩‍🚀

<!-- > -->

A bundler is a tool that converts the source JS that you wrote into code that will be used in your published application/web site. 

👩‍💻 ➡️ 🌏

<!-- > -->

What does bundling do? 🤔

<!-- > -->

Bundling processes your sources file and produces new files.

📦 📦 📦 ⚙️ 🎁

<!-- > -->

**What types of processing?** 🤔

<!-- > -->

**Minify** - 🗜

Reduces file size by removing unnecessary characters: 

```JS
function hello(str) {
	if (str === 'foo') {
		return 'bar'
	}
	return 'foo'
}
```

Becomes: 

```JS 
function hello(str){if(str==='foo'){return 'bar'}return'foo'}
```

<!-- > -->

**Uglify** - 🙀

Renames elements to obfuscate code and reduces file size

<!-- > -->

**Compile TypeScript** - 🦆 →  🦢

Converts .ts files into .js files

We can also convert JSX to JS. 

<!-- > -->

**Compatibility** - 🔩

Convert ES6 JS into JS that is compatible with older browsers.

<!-- > -->

- **bundling** - 🏀🏓🧱 → 🎁

Combining all .js files into a single bundle.js file

<!-- > -->

Try it out:

Minify: https://javascript-minifier.com

Uglify: https://skalman.github.io/UglifyJS-online/

<!-- > -->

Read more about these: https://blog.logrocket.com/uglify-vs-babel-minify-vs-terser-a-mini-battle-royale/

<!-- > -->

## Why call it bundling? 

<!-- > -->

Another effect of the bundler is that it combines multiple .js files into a single .js file. You could say you were bundling these files together. We often call this file `bundle.js` 🎁

<!-- > -->

This provides an advantage when loading projects by reducing the number of streams your page requires to load itself. 

Loading fewer files is faster. 💨

<!-- > -->

Each file is a separate stream. A browser has to provide overhead and manage each stream. More streams require more work, memory, and CPU. 

<!-- > -->

In the end, bundling is the process of processing and combining files into a single file, often called bundle.js. This bundle.js is your "compiled" application and the code you will publish to the world! 

🎁 ➡️ 🌏

<!-- > -->

## Libraries and npm 

<!-- > -->

Have you used npm before?

How did you use it? 

What did it do?

<!-- > -->

The bundling process will require npm. Let's take a look at npm. 

<!-- > -->

You may have used npm before but have you ever thought about what npm is? Here is how npm describes itself. 

<!-- > -->

> npm is the world’s largest software registry. Open-source developers from every continent use npm to share and borrow packages and many organizations use npm to manage private development as well.

<!-- > -->

npm consists of three distinct components:

- 🌏 website
- 💻 Command Line Interface (CLI)
- 📋 package registry

<!-- > -->

npm stores packages in a registry. Packages are programs (snippets of code).

<!-- > -->

npm is a code library, a web site and a command-line tool that allows you to "check out" packages for use in your projects. 

npm === 📚

<!-- > -->

# How to bundle code 🗜

<!-- > -->

You should all have npm installed. If not follow their guide here: https://docs.npmjs.com/about-npm-versions

<!-- > -->

Create a new npm project in your Break Out directory. 

`npm init -y`

<!-- > -->

Install webpack. This is a bundler. It's the tool that will do the processing and bundling. 

`npm install --save-dev webpack webpack-cli`

<!-- > -->

## Setup directory 

Setup some directories to manage your files. 

<!-- > -->

Make the following folders: 

- src (contains the code you write)
- dist (holds code you will distribute)

<!-- > -->

Arrange your files. You should already have some but may need to create them as well! 

- src/index.js (Code you wrote)
- dist/index.html (Code compiled by webpack)
- package.json
- webpack.config.js (can be manually created as an empty .js file)

<!-- > -->

In the end your directory will look like this:

```
|- package.json
|- webpack.config.js
|- /src
  |- index.js
|- /dist
  |- index.html
```

<!-- > -->

Edit index.html. The script tag should point to `bundle.js`. 

`<script src="bundle.js"></script>`

`bundle.js` doesn't exist yet...

<!-- > -->

**Notice we haven't created bundle.js.** This file will be created in the next step by webpack. This will be the compiled, minified, and uglified file built from your JS code and code you might have imported from other libs. 

<!-- > -->

## webpack.config.js

<!-- > -->

Webpack is an industry-standard tool. You will see this tool in the future, expect to work with it. 

The `webpack.config.js` file configures webpack, it is written in JavaScript. 

<!-- > -->

Copy the code below and paste it into your `webpack.config.js`. 

```JS
const path = require('path');

module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist'),
  },
};
```

<!-- > -->

Examine the config file discuss it with another student. What do you think it will tell webpack to do? 

<!-- > -->

- `entry` - start with `src/index.js`
- `output` 
 - `filename` - create a file named `bundle.js`
 - `path` - put `bundle.js` in the `dist/` folder 

<!-- > -->

You need to change the name here (`index.js`) to the name and path of the JS file that you link to in your inde.html! For example: 

```JS
...
entry: './src/index.js',
...
```

<!-- > -->

## Running Scripts

You can run scripts from the command line. You did this earlier when you installed the npm packages. You can also store scripts in your `package.json` and run them with your project. 

<!-- > -->

Open `packages.json` and find the scripts section. 

edit `package.json`

```
{
  ...
  "scripts": {
    "develop": "webpack --mode development --watch",
    "build": "webpack --mode production"
  },
  ...
}
 ```

<!-- > -->

Here you are adding two scripts

- `"develop": "webpack --mode development --watch"`
- `"build": "webpack --mode production"`

<!-- > -->

The first is for **dev**elopment mode. Use this one when you are working on your project as a developer. 

<!-- > -->

The second is the **build** script. Use this when you want to create the `bundle.js` and distribute your project to the world. 

<!-- > -->

### Run your scripts

Test your work. 

<!-- > -->

Bundle your project in development mode: 

`npm run develop`

<!-- > -->

Notice that since we aren't running a specific file but rather **running a script**, we use `npm run <scriptName>`!

Running this script should launch Webpack in development mode. It should compile your .js files and create dist.bundle.js. 

<!-- > -->

This mode uses the `--watch` flag. The watch flag monitors file changes you make and recompiles each time you save a change.

<!-- > -->

Try it make some changes, save. Notice the terminal shows output each time you save. Telling us that webpack is working recompiling `bundle.js`. 

<!-- > -->

Take a look at the distribution code in `bundle.js`. It is human readable, even if it isn't what you would have written. Note the file size of this file. For me it was 19k. 

This should contain all of the JS code from all of the JS files imported into your project along with some extra code to make it compatible with every browser imaginable.  

<!-- > -->

Try production mode. 

`npm run build`

<!-- > -->

This script doesn't use the `--watch` flag. It will only run once. 

`"build": "webpack --mode production"`

<!-- > -->

Running it once builds your code for production. This should be your last step before distributing your work to the world. 

<!-- > -->

Take a look at the distribution code in `bundle.js`. What happened? The code here has been minified and uglified. Check the file size. For me it was 5k.

The code here is not human reable. Take a look at it. 

<!-- .slide: data-background="#087CB8" -->
# BREAK

Take a 10-minute break

<!-- > -->

# Lab

Use the lab time to complete your project. 

- Solve any final errors
- Write a readme
- Publish your work to GitHub Pages
- Post a link to the Project tab in the Progress Tracker

<!-- > -->

# Wrap Up and review

- What is bundling? 
- Why use it? 
- Your thoughts? 
- Use cases? 
- What problems did you encounter? 
- How did you solve them?

<!-- > -->

## Additional Resources

- https://javascript-minifier.com
- https://skalman.github.io/UglifyJS-online/
- https://blog.logrocket.com/uglify-vs-babel-minify-vs-terser-a-mini-battle-royale/
- https://www.sitepoint.com/beginners-guide-webpack-module-bundling/

<!-- > -->

<!-- ## Minute-by-Minute

| **Elapsed** | **Time** | **Activity** |
| ----------- | -------- | ------------ |
| 0:05 | 0:05 | admin |
| 0:05 | 0:10 | [Overview: Bundling](#overview) |
| 0:05 | 0:15 | [Learning Objectives](#learning-objectives) |
| 0:10 | 0:25 | [Bundling and compiling JS](#bundling-and-compiling-js) |
| 0:30 | 0:55 | [How to bundle code](#how-to-bundle-code) |
| 1:05 | 0:10 | [BREAK](#break) |
| 2:05 | 1:00 | [Lab](#lab) |
| 2:35 | 0:30 | [Wrap Up and review](#wrap-up-and-review) |
| 2:45 | 0:10 | admin | -->