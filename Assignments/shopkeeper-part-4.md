# Shopkeeper — Part Four

From hard-coded to data-driven

Goal

Make the game support any number of products without copy/paste.

In this part, you will refactor three areas:
	1.	Wholesale costs (ordering)
	2.	Inventory + price inputs (rendering)
	3.	Daily report (rendering)

Not included yet: generalizing demand and events.

## STEP 1 — Create a single source of truth for products

Create `src/products.js:`

```js
export const PRODUCTS = [
  { id: "coffee", name: "Coffee", wholesaleCents: 150 },
  { id: "bagel",  name: "Bagel",  wholesaleCents: 100 },
];
```

### ✅ Checkpoint

What you created
  - An array of objects
  - A single place to define product info

What you should see
  - Nothing changes yet. This file will be used in later steps.


## STEP 2 — Replace the hard-coded wholesale cost logic

Right now "ORDER_STOCK" uses a hard-coded chain like this:

```js
const costPerItem =
  item === "coffee" ? 150 :
  item === "bagel" ? 100 :
  null;

if (costPerItem === null) {
  newState.log.push("Invalid item.");
  return newState;
}
```

This works, but it does not scale.

Instead, we will look up the product data from PRODUCTS.

### 2.1 Import PRODUCTS

In `reducer.js`, add this at the top with your other imports:

```js
import { PRODUCTS } from "./products.js";
```

### 2.2 Replace the cost block inside ORDER_STOCK

Inside your "ORDER_STOCK" action, find this block:

```js
const costPerItem =
  item === "coffee" ? 150 :
  item === "bagel" ? 100 :
  null;

if (costPerItem === null) {
  newState.log.push("Invalid item.");
  return newState;
}

const totalCost = costPerItem * qty;
```

**Delete** it, and replace it with:

```js
const product = PRODUCTS.find(p => p.id === item);

if (!product) {
  newState.log.push("Invalid item.");
  return newState;
}

const costPerItem = product.wholesaleCents;
const totalCost = costPerItem * qty;
```

That’s it. The rest of "ORDER_STOCK" stays the same.

### ✅ Checkpoint

What you covered
  - Using `find()` to locate an object by id
  - Replacing hard-coded conditionals with a data lookup

What you should see
  - Ordering still works exactly as before

Common bug
  - `find()` can return undefined. If you skip if (!product), your code will crash.

## STEP 3 — Render inventory using PRODUCTS + a loop

Right now, renderInventory is hard-coded with specific products. That creates copy/paste every time you add a new item.

We will generate the inventory + price inputs using PRODUCTS.

**Update** `render.js`

At the top of `render.js`, import:

```js
import { PRODUCTS } from "./products.js";
```

Then **rewrite** `renderInventory(state)`:

```js
function renderInventory(state) {
  const inventory = document.getElementById("inventory");

  const rows = PRODUCTS.map(p => `
    <div class="row">
      <p>${p.name}: ${state.inventory[p.id]}</p>

      <label>
        ${p.name} Price (¢):
        <input
          class="price-input"
          data-item="${p.id}"
          type="number"
          value="${state.prices[p.id]}"
        >
      </label>
    </div>
  `).join("");

  inventory.innerHTML = `
    <h2>Inventory</h2>
    ${rows}
  `;
}
```

Important change: we no longer use ids like price-coffee.
We now use:
  - `class="price-input"`
  - `data-item="coffee"`

That’s what makes it scalable.


### ✅ Checkpoint

What you covered
  - Building HTML using map() + join("")
  - Dynamic property access: state.inventory[p.id]
  - Using data-* attributes to store product ids in the DOM

What you should see
  - Inventory still displays
  - Price inputs still display
  - The page looks almost the same, but your code is now generic


## STEP 4 — Update event delegation to use data-item

Your current price handler probably checks specific element ids. That won’t scale.

Update your price listener to look for `.price-input` and read `data-item`.


**Update** `main.js`

Replace the price handler with:

```js
const inventoryEl = document.getElementById("inventory");

inventoryEl.addEventListener("change", (e) => {
  if (!e.target.classList.contains("price-input")) return;

  const item = e.target.dataset.item;
  const price = Number(e.target.value);

  dispatch({ type: "SET_PRICE", item, price });
});
```


### ✅ Checkpoint

What you covered
	•	Event delegation using a class selector
	•	Reading values from dataset
	•	Generalizing to any number of products

What you should see
	•	Changing prices still works
	•	No focus issues
	•	No new listeners needed when you add products


## STEP 5 — Render the report using PRODUCTS + a loop

Your report is currently hard-coded with coffee and bagels.

To make it scalable, we’ll change the report shape to store sales “by item.”


5.1 **Update** `simulateDay` report shape

At the end of `simulateDay`, change your report from:

```js
state.lastReport = {
  coffeeSold,
  bagelSold,
  revenue
};
```

To:

```js
state.lastReport = {
  soldByItem: {
    coffee: coffeeSold,
    bagel: bagelSold
  },
  revenue
};
```

(Yes, this is still hard-coded inside simulateDay. That’s okay for now. We’ll generalize demand later.)

5.2 **Update** renderReport(state) in `render.js`

```js
function renderReport(state) {
  const report = document.getElementById("report");

  if (!state.lastReport) {
    report.innerHTML = `<h2>Report</h2><p>No report yet.</p>`;
    return;
  }

  const lines = PRODUCTS.map(p => {
    const sold = state.lastReport.soldByItem[p.id] ?? 0;
    return `<p>${p.name} sold: ${sold}</p>`;
  }).join("");

  report.innerHTML = `
    <h2>Report Day ${state.day}</h2>
    ${lines}
    <p>Revenue: $${(state.lastReport.revenue / 100).toFixed(2)}</p>
  `;
}
```

### ✅ Checkpoint

What you covered
	•	Designing state shapes that scale
	•	Rendering from data instead of hard-coding HTML
	•	Using ?? 0 to handle missing items safely

What you should see
	•	The report lists every product in PRODUCTS


## STEP 6 — Add a new item (now it’s easy)

Now adding a product should be simple: update the data and state defaults.


Update `products.js`

```js
export const PRODUCTS = [
  { id: "coffee", name: "Coffee", wholesaleCents: 150 },
  { id: "bagel",  name: "Bagel",  wholesaleCents: 100 },
  { id: "tea",    name: "Tea",    wholesaleCents: 120 },
];
```

**Update** `state.js`

```js
inventory: { coffee: 5, bagel: 5, tea: 5 },
prices:    { coffee: 300, bagel: 250, tea: 275 },
```


### ✅ Checkpoint

What you should see
  - Tea appears automatically in inventory
  - Tea has a price input automatically
  - Ordering logic accepts "tea" correctly (because costs come from PRODUCTS)
  - The report lists Tea (even if Tea never sells yet)

What’s still hard-coded
  - Demand / sales rules inside simulateDay (coffee and bagels only)

That’s intentional. This part was about representation + rendering.


## STEP 7 — Fix the order menu (make it data-driven)

Your order <select> is still hard-coded in HTML. That’s why Tea can’t be ordered.


**Update** `index.html`

Replace the hard-coded options:

```html
<select id="order-item">
  <option value="coffee">Coffee</option>
  <option value="bagel">Bagel</option>
</select>
```

With:

```html
<select id="order-item"></select>
```


## STEP 8 — Render the order dropdown from PRODUCTS

Add an order panel renderer so the dropdown updates automatically when products change.


**Update** `render.js`

Add this function:

```js
function renderOrderPanel(state) {
  const select = document.getElementById("order-item");
  if (!select) return;

  // Preserve selection between renders
  const previous = select.value;

  select.innerHTML = PRODUCTS.map(p =>
    `<option value="${p.id}">${p.name}</option>`
  ).join("");

  if (previous) select.value = previous;

  const orderBtn = document.getElementById("order-button");
  if (orderBtn) {
    orderBtn.disabled = !!state.orderedToday || !!state.gameOver;
  }
}
```

Then make sure your main `render(state)` calls it:

```js
export function render(state) {
  // If your project has renderStatus(), keep it.
  // If not, remove this line.
  renderStatus?.(state);

  renderInventory(state);
  renderReport(state);
  renderLog(state);
  renderOrderPanel(state);
}
```

If you don’t want to use `renderStatus?.(state)`, just call `renderStatus(state)` if it exists in your file. The key point: don’t call a function you haven’t defined.


### ✅ Checkpoint

What you covered
  - Making UI match data automatically
  - Rendering <select> options from an array
  - Avoiding annoying UI resets by preserving selection

What you should see
  - Tea appears in the order dropdown
  - Ordering Tea works

**Optional** challenges (after Part Four)

These are harder. Do them only after everything above works.
  - Challenge A: Generalize demand so Tea can sell too
  - Challenge B: Make raccoon steal a random product
  - Challenge C: Make promo affect demand as a multiplier instead of +1
