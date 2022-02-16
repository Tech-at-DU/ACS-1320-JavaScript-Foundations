<!-- .slide: data-background="./Images/header.svg" data-background-repeat="none" data-background-size="40% 40%" data-background-position="center 10%" class="header" -->
# ACS 1320 - Components and State

<!-- Put a link to the slides so that students can find them -->

<!-- ➡️ [**Slides**](/Syllabus-Template/Slides/Lesson1.html ':ignore') -->

<!-- > -->

## Why you should know this? 🤔

**State** is a key part of React components. To fully understand React you have to undestand state. 

<!-- > -->

## Learning Objectives 😎

1. Describe State
1. Compare and contrast state and props
1. Use State in components

<!-- > -->

## React Component State ⚛️

<!-- > -->

🧪

State in React represents a value that is held/stored by a component, and triggers a render when the value changes. 

<!-- > -->

🧑‍🔬

Imagine state as something that is connected to the user interface and when a change to state occurs you would want the user interface to update.

<!-- > -->

🔭

Props are values that come from outside of a component. When a component receives props is renders.

<!-- > -->

🔬

State is stored internally by the component. When state changes the component is rendered. 

<!-- > -->

There are two ways to handle state. With a class based component, or with a hook. Hooks are a new feature. 

🪝

We will use for all of the examples. 

<!-- > -->

We haven't seen state yet but here is an example of props: 

```JS
function Heading(props) {
  return <h1>{props.title}</h1>
}
```

Notice that props is a parameter. The Heading function is called and props are passed. 

<!-- > -->

### Defining state 🔬

<!-- > -->

🪝

Hooks are functions that begin with "use". Use the "useState" hook to define a state variable and setter function. 

```JSX
import { useState } from 'react'

function Counter() {
  const [count, setCount] = useState(0)

  return (
    <h1>{count}</h1>
  )
}
```

<small>Notice that state is defined inside the component!</small>

<!-- > -->

### useState Hook 🪝

<!-- > -->

The `useState` hook returns an array with two values. The first is a value, and the second is a function. 

```JSX
const [count, setCount] = useState(0)
console.log(count)    // 0
console.log(setSound) // function
```

Here `count` is 0 and `setCount` is a function. 

<small>The argument passed to `useState` is value returned for first value is the array. </small>

<!-- > -->

We use the function to returned from useState to update the value.

We call this a setter function. 

<!-- > -->

Yes, useState returns an array! You could do this: 


```JSX
const arr = useState(0)
console.log(arr[0]) // 0
console.log(arr[1]) // function
```

<!-- > -->

It's easier to destructure the array into two variables, like this! 

```JSX
const [count, setCount] = useState(0)
```

<!-- > -->

What does state do?

Why not just make a variable? 

<!-- > -->

🧐

State stores a value and updates a component when that value changes. 

😮

If you just made a variable and changed the value the component would NOT render!

<!-- > -->

Imagine you need to create a counter. With a button that increments the counter with each click. 

🤔

<!-- > -->

```JSX
import { useState } from 'react'

function Counter() {
  const [count, setCount] = useState(0)

  return (
    <div>
      <h1>{count}</h1>
      <button 
        onClick={() => setCount(count + 1)}
      >+</button>
    </div>
  )
}
```

Each time the button is clicked count is updated and the component is rendered. 

<!-- > -->

🙌

To update state you must call the setter function!

When state is updated the component renders!

<!-- > -->

## Lab

Try these challenges during lab:

<!-- > -->

### Challenge 0 - Counter

Create the Counter component as outlined above. 

Make an instance of your counter in the App component and test it to make sure it's working.

```JS
<Counter />
```

<!-- > -->

### Challenge 1 - decrement 

A counter that only counts up might be useful but might be more sueful if there was a button that added 1 and another button that subtracted 1. 

The new button can use the same setter like this: 

```JS
onClick={() => setCount(count - 1)}
```

<!-- > -->

### Challenge 2

State is stored internally by each component. You can test this by making multiple copies of your counter component. 

```JS
<Counter />
<Counter />
<Counter />
```

Each should track it's own count independent of the others!

<!-- > -->

Here is what your component might look like at this point. 

```JS
function Counter(props) {
  const [count, setCount] = useState(0)

  return (
    <div>
      <button onClick={() => setCount(count + 1)}>+</button>
      <h1>{count}</h1>
      <button onClick={() => setCount(count - 1)}>+</button>
    </div>
  )
}
```

<!-- > -->

### Challenge 3 

Style your component. Add a stylesheet and some styles. Import the stylesheet into the component. 

Notife all of the components share the styles. 

YOu only need one Counter component but you can create as many instances as you need. 

<!-- > -->

Make a stylesheet: Counter.css

Assign the outermost element a class name to to match your component: 

```JS
<div className="Counter">...</div>
```

<!-- > -->

Add some selectors: 

```CSS
.Counter {
  /* Container styles */
}

.Counter > h1 {
  /* count display */
}

.Counter > button {
  /* Button styles */
}

.Counter > button:first-child {
  /* left button */
}

.Counter > button:last-child {
  /* Right button */
}
```

<!-- > -->

div Container styles: 

```CSS
.Counter {
  display: flex;
}
```

<!-- > -->

h1 count display styles: 

```CSS
.Counter > h1 {
	padding: 0.5em;
	margin: 0;
	border-top: 3px solid;
	border-bottom: 3px solid;
}
```

<!-- > -->

General button styles: 

```CSS
.Counter > button {
	font-size: 1em;
	width: 50px;
	margin: 0;
	background-color: #eee;
	border: none;
	border-top: 3px solid;
	border-bottom: 3px solid;
}
```

<!-- > -->

Left (-) button styles: 

```CSS
.Counter > button:first-child {
	border-left: 3px solid;
	border-top-left-radius: 1em;
	border-bottom-left-radius: 1em;
}
```

<!-- > -->

Right (+) button styles: 

```CSS
.Counter > button:last-child {
	border-right: 3px solid;
	border-top-right-radius: 1em;
	border-bottom-right-radius: 1em;
}
```

<!-- > -->

### Challenge 4

Modify the counter component so that it can count in any increment. For example each click adds 1, 3, or 5.

Pass the "step" amount to each component as a prop. Like this:  

```js
<Counter step={1} /> // Counts in 1s - 1, 2, 3...
<Counter step={3} /> // Counts in 3s - 3, 6, 9, ...
<Counter step={5} /> // Counts in 5s - 5, 10, 15, ...
```

<!-- > -->

### Challenge 5 

Counters might be more useful if you could limit the minimum and maximum values. 

Add two props: `min` and `max`. Your component should prevent state from going above or below the max and min values. 

```js
<Counter 
  step={1} 
  min={0}
  max={3}
/> // Counts in 1s - 1, 2, 3, 3, 3, ...
```

<!-- > -->

## Defining your final project

Your goal is to take the ideas from class and create your project. Revisiting the ideas from class and building something new. 

Your final project must: 

- Use React
- Use React Router
- Be published to GitHub Pages

<!-- > -->

## Wrap Up (5 min)

- Review State
- Compare Props and State
- Update tracker and answer questions about homework

<!-- > -->

## Additional Resources

1. https://reactjs.org/docs/faq-state.html

<!-- > -->
<!-- 
## Minute-by-Minute

| **Elapsed** | **Time** | **Activity** |
| ----------- | -------- | ------------ |
| 0:00 | 0:05 | [Learning Objectives](#learning-objectives) |
| 0:05 | 0:15 | [Why you should know this](#why-you-should-know-this)] |
| 0:20 | 0:30 | In Class Activity I |
| 0:50 | 0:10 | BREAK |
| 1:00 | 0:45 | In Class Activity II |
| 1:45 | 0:05 | Wrap up review objectives | -->