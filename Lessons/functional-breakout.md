# Breakout Functional

This tutorial is an introduction to to functional programming and general JavaScript. The goal of the tutorial is to create the game breakout using functional programming ideas. 

## What is functional programming? 

Check out this article on functional programming with JS. 

https://www.toptal.com/javascript/functional-programming-javascript

> Functional Programming is a paradigm of building computer programs using expressions and functions without mutating state and data.

## Key concepts

**Use pure functions.** A pure function takes some input and returns some output and causes no side effects. 

In the original Break Out tutorial many of the functions are impure. Take a look at the function below. 

```JS
// impure function
let x = 120
let dx = 2

function moveBall() {
	x += dx
}

moveBall()
```

In this function the state of the ball is stored outside the function. These are the variables `x` and `dx`. Notice that these are global, the function `moveBall()` function sets the value of `x`, this is called a side effect. 

Functional programming seeks to avoid side effects by using pure functions. 

Take look at the example below. This example converts the first example to a functional programming style. 

```JS
// pure function
let ballState = { x: 120, dx: 2 }

function moveBall(state) {
	const newState = { ...state }
	newState.x =+= newState.dx
	return newState
}

ballState = moveBall(ballState)
```

The example above declares state as an object with properties `x`, and `dx`. The `moveBall()` function takes state as an argument and returns new state. There are no side effects! 

**Do not mutate data.** The example above copies state and returns new state. Rather than updating existing variables. 

## How would you take the ideas here to refactor Breakout? 

Consider the example above. Imagine the state of the game stored in an object. This would include all of the values needed by the game to operate. 

```JS
let state = {
	x: 240,
	y: 160,
	dx: 2,
	dy: -2, 
	...
}
```

Imagine each of the operations that the game performs is defined in a function. This would be things like moveBall, drawBricks, checkForCollision etc. 

Pass state to each of these functions and each function returns state. If the function modifies state it has to copy state and return the copy! 

Why make a copy when you could just modify the existing object? The function would not be considered "pure" since it would be causing a side effect by modifying a value outside the function. 

## Getting started 

Here is an outline you can follow to create a functional version of the Break Out tutorial. 

### Define some constants

Constants will be variables that live in the global scope. This seems to break the functional paradigm, but I think it is okay becuase these values will not change and therefore are not side effects.

These are all of the fixed values used by the game. 

```js
const ballRadius = 10
const paddleHeight = 10
const paddleWidth = 75
const brickRowCount = 3
const brickColumnCount = 5
const brickWidth = 75
const brickHeight = 20
const brickPadding = 10
const brickOffsetTop = 30
const brickOffsetLeft = 30
const blue = '#0095DD'
```

### Initializing state and the game loop

Here we will add three functions, `intiGame()` is responsible for starting the game, `getInitialState()` is responsible for defining the initial state that will be used by the game, and `draw()` is the game loop.

The `initiGame` function should be called once to start the game. This function defines the canvas and canvas context, and calls `getInitialGameState` to get the initial game state. 

The `getInitialGameState` function returns an object that has all of the values used by the game. This object will be passed to other functions as the game runs and will copied and modified when these values change. 

The `draw` function is where the game runs. This function will call functions that draw the ball, bricks and paddle. 

```js
function getInitialState(canvas, ctx) {
  return {
    canvas,
    ctx,
    width: 480,
    height: 320,
    ball: {
      x: canvas.width / 2,
      y: canvas.height - 30,
      dx: 2,
      dy: -2,
      radius: 10,
    },
    paddleX: (canvas.width - constants.paddleWidth) / 2,
    rightPressed: false,
    leftPressed: false,
    score: 0,
    lives: 999999,
    bricks: [],
    color: 'tomato',
    bgColor: '#eee'
  };
}

function draw(state) {
  let newState = state

  requestAnimationFrame(() => {
    draw(newState)
  });
}

function initGame() {
  const canvas = document.getElementById('myCanvas');
  const ctx = canvas.getContext('2d');
  
  const state = getInitialState(canvas, ctx);
  // Start the game loop
  draw(state);
}

initGame();
```

Notice that `getInitialState` returns `state` which is passed to `draw`.

The `draw` function defines `newState`, the game will modify `newState` then call `draw` with `newState` and this process loops for ever. 

## initBricks

Take a close look at `getInitialState`. This function returns an object with all of the vlaues used by the game that might change. This object is always the same so this function can be considered pure. 

There is one thing missing: `initBricks`. The bricks array is empty. We need to create an array bricks to make the game function. 

Add a function to define the array of bricks. 

```JS
function initBricks() {
  const bricks = [];
  for (let c = 0; c < brickColumnCount; c += 1) {
    bricks[c] = [];
    for (let r = 0; r < brickRowCount; r += 1) {
      // Set the x and y when creating the bricks
      const x = (c * (brickWidth + brickPadding)) + brickOffsetLeft;
      const y = (r * (brickHeight + brickPadding)) + brickOffsetTop;
      bricks[c][r] = { x, y, status: true };
    }
  }

  return bricks;
}
```

This should always return an array and it doesn't create any side effects this is a pure function. 

In the `getInitialState` function change the `bricks` property to: 

```JS
bricks: initBricks(),
```

Now the bricks property is defined with an array of bricks. 

### drawBricks

Lets draw the bricks. To do this we need to erase the canvas and then draw the bricks. 

Start by adding a function to draw the background. 

```JS
function drawBackground(state) {
  const { bgColor, ctx, width, height } = state
  ctx.clearRect(0, 0, width, height);
  ctx.fillStyle = bgColor
  ctx.rect(0, 0, width, height)
  ctx.fill()

  return state
}
```

This function uses state but doesn't modify it so it's okay to return state unchanged, there were no side effects. Keep your eye on this, it will not always be the case! 

Draw the background in the `draw` function. 

```JS
function draw(state) {
  let newState = state

  newState = drawBackground(state) // Draw the background

  requestAnimationFrame(() => {
    draw(newState)
  });
}
```

Now draw the bricks. Add a function that draws the bricks. Notice that `drawBackground` is returning state and the value returned is assigned to `newState`. Keep your eye on this pattern. 

```JS
function initBricks() {
  const bricks = [];
  for (let c = 0; c < brickColumnCount; c += 1) {
    bricks[c] = [];
    for (let r = 0; r < brickRowCount; r += 1) {
      // Set the x and y when creating the bricks
      const x = (c * (brickWidth + brickPadding)) + brickOffsetLeft;
      const y = (r * (brickHeight + brickPadding)) + brickOffsetTop;
      bricks[c][r] = { x, y, status: true };
    }
  }

  return bricks;
}
```

This function takes state as an argument and returns state. Since it doesn't modify state state can be returned without making a copy. 

Draw the bricks in the draw function. 

```JS
function draw(state) {
  let newState = state

  newState = drawBackground(state)
  newState = drawBricks(newState) // Draw bricks
  
  requestAnimationFrame(() => {
    draw(newState)
  });
}
```

Add a new line after `drawBackground`. Notice that the you are drawing the background with state, then drawing the bricks with state. State gets passed through these functions. It's not really necessary to pass state through these functions since they do not modify state, but it is the pattern that we will see later!

### updateBall

Next is to draw the ball. To do this add a new function. 

```JS
function updateBall(state) {
	// Get constants
  const { ball, canvas, ctx, color } = state;
	// get mutable values
  let { x, y, dx, dy, radius } = ball;
	// Modify x and y to move the ball
  x += dx;
  y += dy;
	// Check for a collision with the edges of the canvas
  if (x < radius) {
    x = radius;
    dx = -dx;
  } else if (x > canvas.width - radius) {
    x = canvas.width - radius;
    dx = -dx;
  }

  if (y < radius) {
    y = radius;
    dy = -dy;
  } else if (y > canvas.height - radius) {
    y = canvas.height - radius;
    dy = -dy;
  }
	// Draw the ball
  ctx.fillStyle = color
  ctx.beginPath()
  ctx.arc(x, y, radius, 0, Math.PI * 2)
  ctx.fill()

	// update the ball state
  const ballState = { ...ball, x, y, dx, dy }
	// Return a new state object with new ball state
  return { ...state, ball: ballState }
}
```

Take a close look at this function. It takes state as an argument. It uses some values from state as constants: `ball`, `canvas`, `ctx`, `color`. It breaks ball into `x`, `y`, `dx`, `dy`, and `radius`. These properties it may mutate so it stores them as `let`. These properties are on the `ball` object from state. 

Next we modify `x` and `y` to move the ball. 

Next check for collisions with the edges of the canvas. This might mutate `dx` and `dy`.

Next draw the ball. This uses some values that we have stored. 

Last, and this is the most important point, create a new ball state object, and return a new state object that has the updated ball state. 

This function exemplifying function programming. It takes in state, creates a copy of that state and returns new state. 

State is stored in an object with properties. When we create a copy of an object like this we copy those properties. 

```js
const state = { color: 'tomato', width: 480 }
const newState = {...state} // copy state 
```

Here we've created a new object with all of the same properties and values of another object. 

When properties are other reference types like arrays and objects the above example only copies the reference, and doesn't create a new object! 

```js
const state = { color: 'tomato', width: 480, ball: {x: 23, y: 57} }
const newState = {...state} // copy state 
```

### To be continued! 





