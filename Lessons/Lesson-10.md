<!-- .slide: data-background="./Images/header.svg" data-background-repeat="none" data-background-size="40% 40%" data-background-position="center 10%" class="header" -->
# ACS 1320 - Lesson 11 - Forms and User inputs

<!-- Put a link to the slides so that students can find them -->

<!-- ➡️ [**Slides**](/Syllabus-Template/Slides/Lesson1.html ':ignore') -->

<!-- > -->

## Why you should know this

Collecting user data is crucial to many apps you will create. React is a library for creating user interfaces and creating interfaces for forms is an art in itself. React handles forms a little differently from how you have handled forms in other systems. 

<!-- > -->

## Learning Objectives

1. Use State to manage form input
1. Implement the controlled component patern
1. Create forms using components

<!-- > -->

## Review Your Work

Pair up and review projects show your work. Walk through all of the features you have implemented.

Make a list of Everything you need to complete to show your work in class on Tue, March 3.

Identify the three most important things on your list that need to get done.

Identify one or more items on the list that you can accomplish in class today during lab time.

<!-- > -->

# React and Forms

When using forms and inputs React has a special pattern. It's called the controlled component pattern. 

<!-- > -->

The controlled component pattern uses state to hold the value of an input. 

You should try it for yourself with these challenges. 

<!-- > -->

## Using Input with React

<!-- > -->

When using input fields/form elements with React you'll use a special pattern called the controlled component pattern. 

https://reactjs.org/docs/forms.html

<!-- > -->

The idea is that the value you enter into a form element is stored in state and it's that value that is displayed by the form element. 

<!-- > -->

Imagine you have a for where a user will input their name.

```JS 
import { useState } from 'react'

function NameForm() {
  const [name, setName] = useState('')

  return (
    <input 
      type="text"
      value={name}
      onChange={(e) => setName(e.target.value)}
    />
  )
}
```

<!-- > -->

Here is the process. 

- You enter a character in the field
- onChange event fires
- Call `setName` to change the value of state
- set the value of the input to state

<!-- > -->

### Forms 

<!-- > -->

The example above is only the input. Often you'll want to wrap your inputs in a form element. 

<!-- > -->

This is a more complete example: 

```JS
import react, { useState } from 'react'

function FormThing() {
 const [name, setName] = useState('')
 const [email, setEmail] = useState('')

  return (
    <form>
      <input 
        type="text"
        value={name}
        onChange={(e) => setName(e.target.value )}
      />

      <input 
        type="email"
        value={email}
        onChange={(e) => setEmail(e.target.value )}
      />
    </form>
  )
}
```

Notice here we have two inputs. Each has a state variable that tracks it's value. 

<!-- > -->

### Challenge 1

<!-- > -->

Use the Controlled Component pattern. The goal of this challenge is to create a component with an input field and an H1. As you enter text in the input the H1 should display the same text

- Make a React Project
- Make a New Component with an input
- Use Conotrolled Component pattern to store the input value on state
- Display the value of state in the H1.

<!-- > -->

### Challenge 2

<!-- > -->

Make the a tip calculator. Google Tip Calculator and use the Google tip calculator as a model.

Here is an example of what you are trying to make: https://www.google.com/search?q=tip+calculator

<!-- > -->

You'll use the controlled component pattern for the tip calculator inputs. This should store the bill, tip%, and split vlaues on state. In the render method calculate the tip amount from the values in state and display it.

- You'll need inputs for:
  - Bill
  - Tip %
  - Split (number of ways to split the bill)
- Dipslay:
  - Tip amount
  - The bill total
  - Per person amount
- These values should update as you change the values

### Stretch Challenges

If you've got the tip calculator working try these stretch goals. 

- Use CSS to style the tip calculator
- Format the Bill with a decimal
- Create an input for the tip amount that calculates the tip percent

<!-- .slide: data-background="#087CB8" -->
## [**10m**] BREAK

<!-- > -->

# Topic 2

<!-- > -->

## Wrap Up (5 min)

- Continue working on your current tutorial
- Complete reading
- Complete challenges

<!-- > -->

## Additional Resources

1. Links to additional readings and videos

<!-- > -->

<!-- ## Minute-by-Minute

| **Elapsed** | **Time** | **Activity** |
| ----------- | -------- | ------------ |
| 0:00 | 0:05 | [Why you should know this](#why-you-should-know-this) |
| 0:05 | 0:15 | [Learning Objectives](#learning-objectives) |
| 0:20 | 0:30 | In Class Activity I |
| 0:50 | 0:10 | BREAK |
| 1:00 | 0:45 | In Class Activity II |
| 1:45 | 0:05 | Wrap up review objectives | -->
