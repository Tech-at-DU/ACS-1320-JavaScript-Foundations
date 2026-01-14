# Shopkeeper — Part Two

Making choices matter

At the end of Part One, you built a working core loop:
	-	Buttons dispatch actions
	-	State updates correctly
	-	Rendering reflects state
	-	Opening the shop sells items and changes cash and inventory

In this part, you will:
	-	Fix a real UI bug you’ve already encountered
	-	Add rules that make choices matter
	-	Introduce randomness
	-	Add a clear win / lose condition

Nothing in this section changes the overall structure.
We are adding rules, not rewriting the system.

## STEP 5 — Fixing the price input problem (event delegation)

The problem you already saw

In **STEP 3**, you added price inputs inside renderInventory().

This caused two problems:
	1.	Event listeners sometimes stop working
	2.	Inputs lose focus while you are editing them

This happens because:
	-	`renderInventory()` uses `innerHTML`
	-	`innerHTML` destroys and recreates DOM elements
	-	When an element is destroyed, its event listeners are destroyed too

This is a very common problem in JavaScript projects.

The solution: **event delegation**

Instead of attaching listeners to each input directly, we attach one listener to their parent element and let events “bubble up.”

This way:
	-	Inputs can be recreated
	-	The listener still works
	-	Focus is not lost unnecessarily

**Update** `main.js`

**Remove** the individual listeners for "price-coffee" and "price-bagel".

**Add** this instead:

```js
const inventoryEl = document.getElementById("inventory");

inventoryEl.addEventListener("change", (e) => {
  if (e.target.id === "price-coffee") {
    dispatch({
      type: "SET_PRICE",
      item: "coffee",
      price: Number(e.target.value)
    });
  }

  if (e.target.id === "price-bagel") {
    dispatch({
      type: "SET_PRICE",
      item: "bagel",
      price: Number(e.target.value)
    });
  }
});
```

### ✅ Checkpoint

What you covered in this step:
	-	Why `innerHTML` can break event listeners
	-	How events bubble up through the DOM
	-	How event delegation solves listener problems
	-	How to handle multiple inputs with one listener

What you should see now:
	-	Price inputs no longer lose focus when edited
	-	Changing prices reliably updates state
	-	No need to re-attach listeners after render

_This is a real-world fix, not a workaround._

## STEP 6 — Cleanliness affects sales

Right now, cleaning the shop feels pointless.
Let’s fix that.

New rules
	•	If cleanliness < 40 → sell 1 fewer item
	•	If cleanliness > 70 → sell 1 more item

_This affects both coffee and bagels._

Update `simulateDay(state)` in `economy.js`.

At the top of the function, after `let coffeeSold = 0;`, add:

```js
let cleanlinessModifier = 0;

if (state.cleanliness < 40) {
  cleanlinessModifier = -1;
}

if (state.cleanliness > 70) {
  cleanlinessModifier = 1;
}
```

Then apply it to both coffee and bagel sales.

Add this after determining `coffeeSold`:

```js
// Apply cleanliness, then clamp to [0, inventory]
coffeeSold = Math.max(0, coffeeSold + cleanlinessModifier);
coffeeSold = Math.min(coffeeSold, state.inventory.coffee);
```

Add this after determining `bagelSold`:

```js
// Apply cleanliness, then clamp to [0, inventory]
bagelSold = Math.max(0, bagelSold + cleanlinessModifier);
bagelSold = Math.min(bagelSold, state.inventory.bagel);
```

How to approach this step (important). You are not rewriting the simulation. You are adding a rule to an existing process.

That means:
	-	Do not change how simulateDay is called
	-	Do not move logic into the reducer
	-	Do not add new functions

Your job is to:
	1.	Identify where coffeeSold and bagelSold are calculated
	2.	Adjust those values after demand is calculated
	3.	Ensure the result is still valid (not negative, not more than inventory)

If you find yourself changing code outside simulateDay, you are going too far.

Where the cleanliness logic belongs

Think in terms of phases inside simulateDay:
	1.	Decide how many items will sell (demand)
	2.	Adjust that number based on cleanliness
	3.	Clamp the result so it makes sense (can't be less than 0, or more than inventory)
	4.	Update inventory and revenue

Cleanliness only affects step 2.

It should not:
	-	Change prices
	-	Change inventory directly
	-	Skip the simulation entirely

After you finish this step check your work:
	-	If cleanliness is high, do I sell more? Yes
	-	If cleanliness is low, do I sell less? Yes
	-	Can sales ever be negative? No
	-	Can sales ever exceed inventory? No

Read your code carefully. Refactor, incorporating the code above. The goal is to get the following logic working: 

- Declare and set revenue = 0
- Declare and set cleanliness = 0
- Declare and set coffeeSold = 0
- Declare and set bagelSold = 0
- Set cleanlinessModifier based on cleanliness
  - If cleanliness < 40 
    - cleanlinessModifer = -1
  - else if cleanliness > 70
    - cleanlinessModifer = 1
- Set coffeeSold based on demand 
  - If price is <= 350 
    - coffeeSold = 3
  - else 
    - coffeeSold = 1
- Set bagelSold based on price 
  - If price <= 300 
    - bagelSold = 2
  - else 
    - bagelSold = 1
- Modify the amount of coffee and bagels sold based on inventory. Clamp the results to 0 and the amount of inventory
  - Set coffeeSold = Max(0, coffeeSold + cleanlinessModifer)
  - Set coffeeSold = Min(coffeeSold, coffee inventory)
  inventory. Clamp the results to 0 and the amount of inventory
  - Set bagelSold = Max(0, bagelSold + cleanlinessModifer)
  - Set bagelSold = Min(bagelSold, bagel inventory)
- Update inventory and revenue for coffee and bagels
  - Set coffee (inventory) -= coffeeSold
  - Set revenue += coffeeSold * coffee (price);
  - Set bagel (inventory) -= coffeeSold
  - Set revenue += bagelSold * bagel (price);

### ✅ Checkpoint

What you covered in this step:
	-	Adding rules that depend on existing state
	-	Modifying simulation results without changing structure
	-	Preventing negative values using `Math.max`

What you should see now:
	-	Cleaning the shop increases sales
	-	Ignoring cleanliness hurts sales
	-	Cleaning is now a meaningful choice

## STEP 7 — Promotions

Let’s add a short-term boost that costs money.

New rules
	-	Running a promo costs $3
	-	A promo lasts 2 days
	-	While active: sell 1 extra item per product

**Update** `state.js`

**Add** this to the initial state:

```js
promoDaysLeft: 0,
```

**Update** `reducer.js`

**Add** a new action:

```js
if (action.type === "PROMO") {
  if (newState.cashCents >= 300) {
    newState.cashCents -= 300;
    newState.promoDaysLeft = 2;
    newState.log.push("You ran a promotion.");
  } else {
    newState.log.push("Not enough cash to run a promotion.");
  }
}
```

In the "OPEN_SHOP" action, after `simulateDay(newState)` **add**:

```js
if (newState.promoDaysLeft > 0) {
  newState.promoDaysLeft -= 1;
}
```

**Update** `economy.js`

Inside `simulateDay`, apply the promo. To do this you need to refactor your code. Add the code below to update the number of coffee and bagels sold after the `coffeeSold` and `bagelSold` variables are declared and before `revenue` is calculated and inventory is updated. 

```js
if (state.promoDaysLeft > 0) {
  coffeeSold += 1;
  bagelSold += 1;
}
```

**Add** button in `index.html`, in the cotrols section:

```html
<button id="promo">Run Promotion</button>
```

**Add** a new listener in `main.js`:

```js
document.getElementById("promo").addEventListener("click", () => {
  dispatch({ type: "PROMO" });
});
```

### ✅ Checkpoint

What you covered in this step:
	-	Temporary state effects
	-	Countdown-style logic
	-	Making money vs growth tradeoffs

What you should see now:
	-	Promotions increase sales
	-	Promotions cost money
	-	Promotions expire after two days

**Note!** You'll run out of inventory pretty fast since the default state sets the inventory for coffee and bagels to 5. You can increase this number for testing. Later we will add a system to buy more inventory. 

## STEP 8 — Random events

Now we add uncertainty.

New events (one per day, maybe)
	-	Tour bus arrives → +1 sale to everything
	-	Raccoon steals food → −1 random inventory
	-	Helpful neighbor → cleanliness +10
	-	Bad weather → no bagel sales today

**Create** `events.js`. 

**Add** this to `events.js`

```js
export function randomEvent(state) {
  if (Math.random() > 0.3) return null;

  const roll = Math.floor(Math.random() * 4);

  switch (roll) {
    case 0:
      state.log.push("A tour bus arrives!");
      return { demandBoost: 1 };

    case 1:
      state.log.push("A raccoon steals some food!");
      return { steal: true };

    case 2:
      state.log.push("A helpful neighbor cleans up.");
      state.cleanliness += 10;
      return null;

    case 3:
      state.log.push("Bad weather scares away bagel customers.");
      return { noBagels: true };
  }
}
```

**Edit** `reducer.js`.

**Import** `randomEvent` at the top of the page: 

```js
import { randomEvent } from './events.js'
```

**Edit** the "OPEN_SHOP" action. **Before** `simulateDay(newState)` add:

```js
const event = randomEvent(newState);
```

### CHALLENGE!

Your goal is to get the random events to affect sales and inventory. 

**Understanding the event object**

`randomEvent(state)` can return one of two things:
	-	`null` → no event happened
	-	an event object → something special happened today

Example event objects:

```js
{ demandBoost: 1 } // Tourists demand coffee and bagels!
{ steal: true }    // Raccoons break in!
{ noBagels: true } // Too cold for bagels today
```

The event object is data, not behavior.

That means:
	-	`randomEvent` decides what happened
	-	`simulateDay` decides how that affects sales

This separation is intentional.

**Where events should be applied?**

Events should affect the same place as other rules:
inside `simulateDay`.

That means:
	-	Do NOT update inventory in `randomEvent`
	-	Do NOT change prices in the `reducer`
	-	Do NOT add special logic inside "OPEN_SHOP"

Instead:
	1.	"OPEN_SHOP" generates the event
	2.	`simulateDay` reacts to it

**How to pass the event into simulateDay**

You will need to modify the function signature.

Change this:

```js
export function simulateDay(state) { ... 
```

To this:

```js
export function simulateDay(state, event) { ...
```

Then update the call site:

```js
const event = randomEvent(newState);
simulateDay(newState, event); // add event here
```

Now simulateDay has access to the event for that day only.

**How to think about applying events**

Inside `simulateDay`, ask:
	-	Does this event change demand?
	-	Does it prevent something from selling?
	-	Does it remove inventory?

Examples:
	-	Tour bus arrives -> Increase demand *before* clamping to inventory
	-	Bad weather -> Force bagelSold = 0
	-	Raccoon steals food -> Reduce inventory after sales are calculated

**Starter hint (do not copy blindly)**

```js
if (event?.demandBoost) {
  coffeeSold += event.demandBoost;
  bagelSold += event.demandBoost;
}

if (event?.noBagels) {
  bagelSold = 0;
}
```

This is not a full solution.
It is a direction.

Common mistakes to avoid
	•	❌ Applying events inside randomEvent and simulateDay
	•	❌ Mutating inventory before demand is calculated
	•	❌ Forgetting to handle null events
	•	❌ Letting events permanently change the game (they should be temporary)

If an event’s effect lasts more than one day, you’ve probably put it in the wrong place.

**Self-check questions**

After implementing events:
	-	Do events affect only one day?
	-	Can the game still run when no event occurs?
	-	Do logs explain what happened?
	-	Does the core simulation still work without events?

If you remove the call to randomEvent, the game should still function.

### ✅ Checkpoint

Check your work. Since events are random you will need to refresh the page, and click open shop more than a few times! You can also modify your code to generate a specific event for testing. In this case you should remove those chnages when you are sure things are working. 

How can you tell if an event is having the correct effect? 

- Bad weather: coffee - 3, bagels - 0, cash +$9
- Raccoons: coffee - 3, bagels - 4 (2 stolen), cash +$14 
- Tourists: coffee - 4, bagels - 3, cash +$19.5
- No event: coffee - 3, bagels - 2, cash +$14
