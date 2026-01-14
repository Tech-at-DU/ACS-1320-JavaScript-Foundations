# Shopkeeper — Part Three

**Restocking, reporting, and making the game winnable**

At the end of Part Two, you added real strategy:
	-	Cleanliness affects sales
	-	Promotions boost sales temporarily
	-	Random events change outcomes
	-	Price inputs work reliably using event delegation

But the game still has a major problem: You can’t keep playing because you run out of inventory.

Part Three fixes that by adding:
	-	A simple ordering system (restock)
	-	A daily report display (so you can see what happened)
	-	A daily cost (so decisions matter)
	-	Win / lose conditions (now that the game can run long enough)

Nothing here changes the core pattern:

```
click → dispatch(action) → update(state, action) → render(state)
```

## STEP 9 — Order stock (restocking system)

**What you’re adding**

A small UI where the player can:
	-	choose an item (coffee or bagel)
	-	choose a quantity
	-	click “Order”

That will update state:
	-	cash decreases
	-	inventory increases

**New rules**
	-	Coffee wholesale cost: 150¢ each
	-	Bagel wholesale cost: 100¢ each
	-	You can order once per day
	-	This prevents spamming the order button
	-	It forces choices

**Update** `state.js`

**Add** these fields to the initial state:

```js
orderedToday: false,
```

**Update** `index.html`

**Add** this inside the controls section (under your buttons):

```html
<section id="orders">
  <h2>Order Stock</h2>

  <label>
    Item:
    <select id="order-item">
      <option value="coffee">Coffee</option>
      <option value="bagel">Bagel</option>
    </select>
  </label>

  <label>
    Quantity:
    <input id="order-qty" type="number" value="1" min="1" max="20">
  </label>

  <button id="order-button">Place Order</button>
</section>
```

**Create** `src/utils.js`

```js
export function clampNumber(value, min, max) {
  return Math.min(max, Math.max(min, value));
}
```

**Update** `reducer.js`

At the top (with your other imports), add:

```js
import { clampNumber } from "./utils.js";
```

**Edit** `update(state, action)`, add a new action:

```js
if (action.type === "ORDER_STOCK") {
  const item = action.item;
  const qty = clampNumber(action.qty, 1, 20);

  if (newState.orderedToday) {
    newState.log.push("You already placed an order today.");
    return newState;
  }

  const costPerItem =
    item === "coffee" ? 150 :
    item === "bagel" ? 100 :
    null;

  if (costPerItem === null) {
    newState.log.push("Invalid item.");
    return newState;
  }

  const totalCost = costPerItem * qty;

  if (newState.cashCents < totalCost) {
    newState.log.push("Not enough cash to place that order.");
    return newState;
  }

  newState.cashCents -= totalCost;
  newState.inventory[item] += qty;
  newState.orderedToday = true;

  newState.log.push(`Ordered ${qty} ${item}(s) for $${(totalCost / 100).toFixed(2)}.`);
}
```

**Important**: We return early in failure cases so state doesn’t change accidentally.

**Update** `main.js`

Add a listener for the order button:

```js
document.getElementById("order-button").addEventListener("click", () => {
  const item = document.getElementById("order-item").value;
  const qty = Number(document.getElementById("order-qty").value);

  dispatch({
    type: "ORDER_STOCK",
    item,
    qty
  });
});
```

### ✅ Checkpoint

What you covered in this step:
	-	Reading values from `<select>` and `<input>`
	-	Validating user input (clamping quantity)
	-	Adding a new action to the reducer
	-	Introducing a constraint (orderedToday) to prevent spam

What you should see now:
	-	Clicking “Place Order” reduces cash and increases inventory
	-	If you try to order twice in the same day, you get a log message
	-	If you can’t afford the order, it fails safely (no negative cash)
  - Currently you can only place one order. You'll fix that next.

## STEP 10 — Reset the “once per day” order rule

Right now, orderedToday never resets, which would block all future orders.

We need to reset it when a new day happens.

In this project, the “day happens” when you click Open Shop.

**Update** `reducer.js` (inside OPEN_SHOP)

In your "OPEN_SHOP" block, add this line after the day advances:

```js
newState.orderedToday = false;
```

A good location is right **after**:

```js
newState.day += 1;
```

### ✅ Checkpoint

What you covered:
	-	Resetting temporary state values
	-	Thinking about “what counts as a new day”

What you should see:
	-	You can order once, open shop, then order again the next day

## STEP 11 — Display the daily report

You already store state.lastReport, but the player can’t see it.

Let’s render it.

**Update** `render.js`

**Add** this to `render(state)`:

```js
renderReport(state);
```

Then **add** this function, below the other functions:

```js
function renderReport(state) {
  const report = document.getElementById("report");

  if (!state.lastReport) {
    report.innerHTML = `<h2>Report</h2><p>No report yet. Open the shop!</p>`;
    return;
  }

  report.innerHTML = `
    <h2>Report</h2>
    <p>Coffee sold: ${state.lastReport.coffeeSold}</p>
    <p>Bagels sold: ${state.lastReport.bagelSold}</p>
    <p>Revenue: $${(state.lastReport.revenue / 100).toFixed(2)}</p>
  `;
}
```

## ✅ Checkpoint

What you covered:
	-	Rendering conditional UI (no report yet vs report exists)
	-	Reading nested state (state.lastReport)

What you should see:
	-	After you open shop, a report appears showing what sold and revenue

## STEP 12 — Add a daily cost (rent)

Right now, it’s too easy to profit just by ordering and selling.
A real shop has costs even on “good” days.

**New rule**

Every time you open shop, you pay rent:
	-	Rent cost: 200¢ per day

**Update** `reducer.js` (inside "OPEN_SHOP")

After `simulateDay(newState)` and before you increment the day, subtract rent:

```js
const rentCents = 200;
newState.cashCents -= rentCents;
newState.log.push(`Paid rent: $${(rentCents / 100).toFixed(2)}.`);
```

## ✅ Checkpoint

What you covered:
	-	Costs that happen automatically each day
	-	How costs affect win/lose strategy

What you should see:
	-	Each “Open Shop” day reduces cash by $2.00 after sales
	-	If you don’t sell enough, cash can drop over time

## STEP 13 — Win and lose conditions

Now the game can run for many days because you can restock.

Rules
	-	Lose if cash < 0
	-	Win if day > 10 AND cash ≥ $60

**Update** `state.js`

**Add**:

```js
gameOver: false,
```

**Update** `reducer.js`

At the end of `update()` (right before `return newState;`) add:

```js
if (newState.cashCents < 0) {
  newState.gameOver = true;
  newState.log.push("You went bankrupt. Game over.");
}

if (newState.day > 10 && newState.cashCents >= 6000) {
  newState.gameOver = true;
  newState.log.push("You ran a successful shop! You win!");
}
```

**Update** `main.js` (disable controls when game ends)

**Edit** `dispatch(action)`, after rendering, check gameOver:

```js
function dispatch(action) {
  state = update(state, action);
  render(state);

  if (state.gameOver) {
    disableControls();
  }
}

function disableControls() {
  document.querySelectorAll("button, input, select").forEach(el => {
    el.disabled = true;
  });
}
```

### ✅ Checkpoint

What you covered:
	-	End-game conditions
	-	Preventing further actions after the game ends
	-	“Win state” and “Lose state” as part of state

What you should see:
	-	If you go negative, the game ends and controls disable
	-	If you survive past day 10 with enough cash, you win and controls disable
	-	The log clearly explains why the game ended

## STEP 14 — Save and load

Now that your game can be played, saving matters.

**Update** `index.html`

**Add** these buttons to your controls section:

```js
<button id="save">Save</button>
<button id="load">Load</button>
<button id="new-game">New Game</button>
```

**Create** `storage.js`

**Add**:

```js
export function saveState(state) {
  localStorage.setItem("shopkeeper", JSON.stringify(state));
}

export function loadState() {
  const data = localStorage.getItem("shopkeeper");
  if (!data) return null;
  return JSON.parse(data);
}
```

**Update** `main.js`

**Import** storage helpers:

```js
import { saveState, loadState } from "./storage.js";
```

**Add** listeners:

```js
document.getElementById("save").addEventListener("click", () => {
  saveState(state);
  // log without dispatch: this is UI-only feedback
  state.log.push("Game saved.");
  render(state);
});

document.getElementById("load").addEventListener("click", () => {
  const loaded = loadState();
  if (!loaded) {
    state.log.push("No save found.");
    render(state);
    return;
  }
  state = loaded;
  state.log.push("Game loaded.");
  render(state);

  if (state.gameOver) disableControls();
});

document.getElementById("new-game").addEventListener("click", () => {
  // reset everything
  state = makeInitialState();
  render(state);
});
```

## ✅ Checkpoint

What you covered:
	-	Persisting state with localStorage
	-	Restoring a saved object
	-	Handling missing saves safely

What you should see:
	-	Save stores your progress
	-	Load restores it after refreshing
	-	New Game resets state

Final takeaway (Part Three)

By the end of Part Three, you have a complete playable loop:
	-	You can restock inventory
	-	You can earn and lose money over time
	-	The game reports what happened each day
	-	The game ends with clear win/lose conditions
	-	You can save and load progress

You now have a full project you can extend — and the structure is still simple enough to understand.

Optional challenges (if you finish early)
	-	Add “Order cost” display before placing order
	-	Make the raccoon steal a random item (coffee or bagel)
	-	Add a “Low stock” warning in the inventory panel
	-	Add a “Day summary” log line: items sold, rent paid, ending cash
