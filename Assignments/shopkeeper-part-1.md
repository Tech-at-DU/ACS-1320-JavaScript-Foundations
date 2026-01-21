# Shopkeeper - Part One

What you are building

You are building a small shop simulation game using JavaScript.

Each day:
  -	You can clean your shop
  -	You can order more items
	-	You can change prices
	-	You can open the shop and see what sells

This is a turn-based game where clicking a button advances the day.

The goal is to practice JavaScript fundamentals:
	-	objects
	-	functions
	-	conditionals
	-	arrays
	-	events
	-	updating the DOM

How the program works (important)

This project follows one simple rule:

All game data lives in one object called state.

It works like this:

User clicks a button
→ an action is sent
→ state is updated
→ the page re-renders

You will see this pattern repeated over and over.

Folder and file structure:

```
shopkeeper/
  index.html
  style.css
  src/
    main.js
    state.js
    reducer.js
    render.js
    economy.js
    events.js
    storage.js
    utils.js
```

## Quick explanation: JavaScript modules

In this project, each `.js` file is treated like a separate script with its own _scope_. That means:
	-	Variables and functions in one file are not automatically available in another file.
	-	If you want to use something from another file, you must **`export`** it from that file and **`import`** it where you need it.

**Why we use modules**

Modules help you organize code and avoid “giant file” projects. They also prevent accidental name collisions like having two different functions named `render`.

How modules work in this project

Look at this line in index.html:

```js
<script type="module" src="src/main.js"></script>
```

type="module" tells the browser:
	•	This file can use import / export
	•	Each file has its own scope
	•	Imports must use a real path like `./state.js` (including the ./)

**Example from this project**

In `src/state.js`:

```js
export function makeInitialState() {
  return { ... };
}
```

The word `export` makes `makeInitialState` available to other files.

In `src/main.js`:

```js
import { makeInitialState } from "./state.js";
```

That line means: “Bring in the exported function named `makeInitialState` from the file `state.js`.”

Common module mistakes (that cause errors)
	-	Forgetting `type="module"` → imports won’t work at all
	-	Forgetting `./` in a path:
	-	✅ `./state.js`
	-	❌ `state.js`
	-	Misspelling the imported name (must match the export exactly)
	-	Opening the file in a way that blocks modules (some setups require a local server)

If something breaks, always check the browser console first. Module errors show up there clearly.

## STEP 1 - setup files

**Goal**: Open `index.html` in your browser and see:
	-	Day number
	-	Cash amount

`index.html`

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <title>Shopkeeper</title>
  <link rel="stylesheet" href="style.css" />
</head>
<body>

  <h1>Shopkeeper</h1>

  <section id="status"></section>
  <section id="inventory"></section>
  <section id="controls"></section>
  <section id="report"></section>
  <section id="log"></section>

  <script type="module" src="src/main.js"></script>
</body>
</html>
```

`src/state.js`

```js
// This file defines the game state.
// Think of this like the "database" for the game.

export function makeInitialState() {
  return {
    day: 1,
    cashCents: 2500, // $25.00
    cleanliness: 60,

    inventory: {
      coffee: 5,
      bagel: 5
    },

    prices: {
      coffee: 300, // cents
      bagel: 250
    },

    lastReport: null,
    log: ["Welcome to your new shop!"]
  };
}
```

`src/render.js`

```js
// This file updates the page using the state object.

export function render(state) {
  renderStatus(state);
}

function renderStatus(state) {
  const status = document.getElementById("status");
  status.innerHTML = `
    <p><strong>Day:</strong> ${state.day}</p>
    <p><strong>Cash:</strong> $${(state.cashCents / 100).toFixed(2)}</p>
  `;
}
```

`src/main.js`

```js
import { makeInitialState } from "./state.js";
import { render } from "./render.js";

let state = makeInitialState();

// Draw the page for the first time
render(state);
```

### ✅ Checkpoint

In this step, you set up the basic structure of the program and proved that  JavaScript is successfully:
	-	Loading as a module (`type="module"`)
	-	Importing functions from other files
	-	Creating a state object that holds all game data
	-	Rendering values from that state into the HTML

You also established the most important rule of this project: The state object stores game data, and the page only displays it.

At this point:
	-	Nothing is interactive
	-	No buttons
	-	The game is not “running” yet

That’s okay. Right now, your goal is visibility, not interaction.

What you should see at this point

When you open `index.html` in your browser, you should see:
	-	The title "Shopkeeper"
	-	The text:
    -	Day: 1
    -	Cash: $25.00

If you do not see these:
	-	Check that `main.js` is loading
	-	Check that `render(state)` is being called
	-	**Check the browser console for errors**

If you change values in `makeInitialState()` (for example, set day: 5), you should see the page update after refreshing.
This confirms that rendering is driven by state.

## STEP 2 — Buttons & actions

**Goal**: Clicking a button changes the state.

**Update** `index.html`

Add this inside `<section id="controls">`:

```html
<button id="next-day">Next Day</button>
<button id="clean">Clean Shop</button>
```

Add in `src/reducer.js`

```js
// This file updates state based on actions.

export function update(state, action) {
  // Make a copy so we don’t change the original
  const newState = structuredClone(state);

  if (action.type === "NEXT_DAY") {
    newState.day += 1;
    newState.log.push("A new day begins.");
  }

  if (action.type === "CLEAN") {
    newState.cleanliness += 10;
    if (newState.cleanliness > 100) {
      newState.cleanliness = 100;
    }
    newState.log.push("You cleaned the shop.");
  }

  return newState;
}
```

**Update** `src/main.js`

```js
import { makeInitialState } from "./state.js";
import { render } from "./render.js";
import { update } from "./reducer.js";

let state = makeInitialState();
render(state);

function dispatch(action) {
  state = update(state, action);
  render(state);
}

document.getElementById("next-day").addEventListener("click", () => {
  dispatch({ type: "NEXT_DAY" });
});

document.getElementById("clean").addEventListener("click", () => {
  dispatch({ type: "CLEAN" });
});
```

**Add Inventory display**

**Goal**: Show what items you have.

**Update** `src/render.js` 

```js
export function render(state) {
  renderInventory(state);
  renderLog(state);
}

function renderInventory(state) {
  const inventory = document.getElementById("inventory");
  inventory.innerHTML = `
    <h2>Inventory</h2>
    <p>Coffee: ${state.inventory.coffee}</p>
    <p>Bagels: ${state.inventory.bagel}</p>
  `;
}

function renderLog(state) {
  const log = document.getElementById("log");
  log.innerHTML = `
    <h2>Log</h2>
    <ul>
      ${state.log.map(msg => `<li>${msg}</li>`).join("")}
    </ul>
  `;
}
```

### Quick explanation: Event listeners

JavaScript can “listen” for things that happen in the browser, like:
	-	a button click
	-	typing in an input box
	-	changing a dropdown
	-	submitting a form

An event listener is how you tell the browser:

“When this event happens on this element, run this function.”

**The pattern**:

```js
element.addEventListener("eventName", () => {
  // code that runs when the event happens
});
```

**Example from this project (buttons)**:

```js
document.getElementById("next-day").addEventListener("click", () => {
  dispatch({ type: "NEXT_DAY" });
});
```

This means:
	-	Find the button with id "next-day"
	-	When it’s **clicked**:
	-	send an action into the game using `dispatch`

That click triggers the main loop:

```
click → dispatch(action) → update(state, action) → render(state)
```

**Why we use event listeners here**

Because the game is turn-based. Nothing happens unless the user chooses an action (clicks a button, changes a price, etc.). Event listeners connect the UI to your game logic.

One important rule about listeners in this project

An event listener can only attach to an element that exists right now.

So this will fail if the button doesn’t exist yet:

```js
document.getElementById("open-shop").addEventListener("click", ...)
```

because `getElementById("open-shop")` would return null.

**A second important rule**

If you replace HTML using innerHTML, you destroy old elements and create new ones.

That means:
	-	the old elements (with listeners) are gone
	-	the new elements do not have listeners attached

That’s why your price input listeners break in STEP 3. It’s a common issue and later you’ll learn two standard solutions:
	-	event delegation
	-	or not re-rendering the input area while editing

For now, it’s enough to know why it happens.

**“Sanity check” you can add (fast debugging habit)**

If an event listener doesn’t work, do this:
	1.	Open DevTools → Console
	2.	Look for red errors
	3.	If there’s no error, add a temporary console.log(...) inside the event listener:

```js
document.getElementById("clean").addEventListener("click", () => {
  console.log("Clean clicked"); // Add a console.log message
  dispatch({ type: "CLEAN" });
});
```

If you don’t see “Clean clicked”, the listener isn’t attached (wrong id, code ran too early, element replaced, etc.).

### ✅ Checkpoint

What you covered in this step

In this step, you made the game interactive for the first time.

You introduced several critical ideas:
	-	Actions: plain objects that describe what happened
	-	dispatch: a function that sends actions into the system
	-	update (reducer): a function that takes state + action and returns new state
	-	event listeners that connect buttons to actions

This step is the core pattern you will repeat for the entire project:

```
click → dispatch(action) → update(state, action) → render(state)
```

You also learned:
	-	How to copy state safely using structuredClone
	-	How to modify numbers inside objects
	-	How to clamp values so they stay in a valid range

Importantly, you did not touch the DOM inside the reducer.
The reducer only changes data. Rendering is handled elsewhere (`render(state)` function).

What you should see if you test now

You should now be able to:
	-	Click Next Day
	-	The day number increases by 1
	-	A message appears in the log
	-	Click Clean Shop
	-	Cleanliness increases (up to a maximum of 100, you won't see this)
    - **CHALLENGE**: display the cleaniness value in the log message. Do this by editing the log message added in the reducer for the "CLEAN" action.
	-	A message appears in the log

Nothing visual changes yet except:
	-	The day number
	-	The log entries

That’s expected. Cleanliness exists in state, but you are not displaying it yet.

If clicking buttons does nothing:
	-	Check that dispatch is being called
	-	Check that update is imported correctly
	-	**Check the browser console for errors**

Add Inventory Display

What you covered in this step

In this step, you expanded rendering beyond a single value and started:
	-	Reading nested data from state (`state.inventory`)
	-	Rendering multiple related values to the page
	-	Using `.map()` to turn arrays into HTML
	-	Keeping rendering logic separate from game logic

You also introduced a log panel, which is extremely important for debugging and understanding what the game is doing.

This step reinforces an important idea:

If something is in state, it should be visible somewhere on the page.

If players can’t see it, they can’t reason about it.

What you should see if you test now

You should now see:
	-	An Inventory section showing:
    -	Coffee `count`
    -	Bagel `count`
  -	A Log section showing:
    -	“Welcome to your new shop!”
    -	Messages from clicking buttons

When you click:
	-	Next Day → a new log entry appears
	-	Clean Shop → a new log entry appears

Inventory numbers do not change yet.
That’s correct — you haven’t written any code that modifies inventory.

## STEP 3 — Prices & inputs

**Goal**: Change prices using <input> fields.

**Update** `render.js`, replace `renderInventory(state)` with: 

```js
function renderInventory(state) {
  const inventory = document.getElementById("inventory");
  inventory.innerHTML = `
    <h2>Inventory</h2>

    <p>Coffee: ${state.inventory.coffee}</p>
    <label>
      Coffee Price (¢):
      <input id="price-coffee" type="number" value="${state.prices.coffee}">
    </label>

    <p>Bagels: ${state.inventory.bagel}</p>
    <label>
      Bagel Price (¢):
      <input id="price-bagel" type="number" value="${state.prices.bagel}">
    </label>
  `;
}
```

**Update** `reducer.js`, add this inside `function update(state, action)` before `return`:

```js
if (action.type === "SET_PRICE") {
  const { item, price } = action;
  newState.prices[item] = price;
}
```

Notice that you are adding new ways for the reducer to handle different actions inside the reducer. The reducers job is to make changes to state based on the action. 

**Update** `main.js`

```js
document.getElementById("price-coffee").addEventListener("change", e => {
  dispatch({
    type: "SET_PRICE",
    item: "coffee",
    price: Number(e.target.value)
  });
});

document.getElementById("price-bagel").addEventListener("change", e => {
  dispatch({
    type: "SET_PRICE",
    item: "bagel",
    price: Number(e.target.value)
  });
});
```

### ✅ Checkpoint

In this step, you connected form inputs to your game state.

You learned how to:
	-	Read values from `<input>` elements
	-	Convert input strings into numbers
	-	Dispatch actions in response to input changes
	-	Update part of state without advancing the game

This is an important distinction:

Changing prices does not consume a day.

Prices are part of setup, not gameplay progression.

You also practiced:
	-	Passing extra data inside actions
	-	Updating only one field in a larger object
	-	Treating inputs as views of state, not sources of truth

What you should see if you test after this step

You should now be able to:
	-	Change the coffee or bagel price
	-	See no immediate visual change
	-	Open the browser console and verify:
	-	`state.prices.coffee` and `state.prices.bagel` update correctly

This might feel anticlimactic — that’s normal.

Prices only matter when the shop opens, which comes next.

If changing inputs causes errors:
	-	Make sure you used `Number(e.target.value)`
	-	Check that your action object matches what the reducer expects

**Note!** There is a problem here. `renderInventory(state)` overwrites the DOM with new input elements. When the DOM overwites an element, even if it has the same id name, listeners attached to the element are not attached to the newly created elements. 

I Left this problem in the tutorial because it is a common problem encountered, and there are standard solutions available, which we will explore in the future.

## STEP 4 — Open the shop (simple sales)

**Goal**: Sell items when the shop opens.

Add the following to `src/economy.js`:

```js
export function simulateDay(state) {
  let revenue = 0;

  // Coffee sales
  let coffeeSold = 0;
  if (state.prices.coffee <= 350) {
    coffeeSold = Math.min(3, state.inventory.coffee);
  } else {
    coffeeSold = Math.min(1, state.inventory.coffee);
  }

  state.inventory.coffee -= coffeeSold;
  revenue += coffeeSold * state.prices.coffee;

  // Bagel sales
  let bagelSold = 0;
  if (state.prices.bagel <= 300) {
    bagelSold = Math.min(2, state.inventory.bagel);
  } else {
    bagelSold = Math.min(1, state.inventory.bagel);
  }

  state.inventory.bagel -= bagelSold;
  revenue += bagelSold * state.prices.bagel;

  state.cashCents += revenue;
  state.lastReport = {
    coffeeSold,
    bagelSold,
    revenue
  };
}
```

**Update** `reducer.js`.

Add this to the top of the page: 

```js
import { simulateDay } from "./economy.js";
```

Add this inside `update(state, action)` before `return`:

```js
if (action.type === "OPEN_SHOP") {
  simulateDay(newState);
  newState.day += 1;
  newState.log.push("You opened the shop.");
}
```

Add button in `index.html` in the controls section:

```html
<button id="open-shop">Open Shop</button>
```

Wire it in `main.js`

```js
document.getElementById("open-shop").addEventListener("click", () => {
  dispatch({ type: "OPEN_SHOP" });
});
```

**Render status**

Edit `render.js`. 

Updated `redner(state)`, add `renderStatus(state);`
```js
export function render(state) {
  renderStatus(state); // add this line
  renderInventory(state);
  renderLog(state);
}
```

Add the `renderStatus(state)` function after the `render(state)` function:

```js
function renderStatus(state) {
  const status = document.getElementById("status");
  status.innerHTML = `
    <h2>Status</h2>
    <p><strong>Day:</strong> ${state.day}</p>
    <p><strong>Cash:</strong> $${(state.cashCents / 100).toFixed(2)}</p>
  `;
}
```

### ✅ Checkpoint

This is the step where the project becomes a game.

You introduced the idea of a simulation step:
	-	The game looks at current state (prices, inventory)
	-	It calculates what sells
	-	It updates inventory and cash
	-	It generates a report of what happened

Key ideas introduced here:
	-	Separating calculation logic (simulateDay) from control logic ("OPEN_SHOP")
	-	Mutating state only inside the reducer flow
	-	Using `Math.min()` to prevent negative inventory
	-	Storing results in lastReport instead of logging everything immediately

This is the first time:
	-	Inventory goes down
	-	Cash goes up
	-	Player choices actually matter

What you should see if you test at this step

You should now be able to:
	1.	Click Open Shop
	2.	See:
    -	The day number increase
    -	Inventory counts decrease
    -	Cash increase
    -	A log message saying you opened the shop

If prices are low:
	-	More items sell
	-	Cash increases more

If prices are high:
	-	Fewer items sell
	-	Cash increases less

If inventory reaches zero:
	-	Items stop selling
	-	Cash stops increasing

At this stage:
	-	Cleanliness does not affect sales yet
	-	Random events do not exist yet
	-	The report is stored but not fully displayed yet

That’s intentional. You now have a working core loop, which everything else will build on.
