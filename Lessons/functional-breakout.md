# Breakout Functional

This tutorial is an introduction to to functional programming and general JavaScript. The goal of the tutorial is to create the game breakout using functional programming ideas. 

## What is functional programming? 

Check out this article on functional programming with JS. 

https://www.toptal.com/javascript/functional-programming-javascript

> Functional Programming is a paradigm of building computer programs using expressions and functions without mutating state and data.

## Key concepts

**Use pure functions.** A pure function takes some input and returns some output and causes no side effects. 

```JS
// impure function
let x = 120
let dx = 2
function moveBall() {
	x += dx
}

moveBall()

// pure function
let ballState = { x: 120, dx: 2 }
function moveBall(state) {
	const newState = { ...state }
	newState.x =+= newState.dx
	return newState
}

ballState = moveBall(ballState)
```

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

Pass state to each of these functions and each function returns state. If the function modifies state you have to copy state and return the copy! 

Why make a copy when you could just modify the existing object? The function would not be considered "pure" since it would be causing a side effect by modifying a value outside the function. 

Keep an open mind and make this a game of sticking to the rules of pure functions and immutable values. As you work ask yourself if a function has a side effect? If the answer is yes, ask yourself how you can avoid this. 

This is an open ended challenge, have fun and pay attention to the how this program differs from the OOP version!
