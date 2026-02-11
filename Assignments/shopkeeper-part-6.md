# Shopkeeper — Part Six

Depth, economy, and UI polish

**Goal**

Make the game deeper and more playable by extending:
- purchasing + supply chain
- economy systems (pressure + tradeoffs)
- usability and clarity

Try as many of the challenges below as you can! 

## Add Styles 

Start with some general styles. Choose a `font-family`, and set that as the base font on the body element. This allows all other elements to inherit this font. Adjust the `line-height`. Adding or removing space between lines will make your pages easier to read. 

Set the `color` (text) and `background-color`.

Use CSS to style your shop. Think about how the shop is used, and what is the most important information. Style these things so that standout and easily identified. 

Use: 
- `font-size`
- `font-weight`
- `color`
- `line-height`

Think about the layout. If you followed the tutorial closely, the content is organized in `<section>` elements. Use this as a startng point for your layout. Imagine that each section might be arranged in columns or a grid. 

use:
- `display: grid` and/or `display: flex`

## Major Challenge 1 — Expand purchasing and economy

Pick one direction:

### 1A. Delivery delay (recommended)

Orders don’t arrive instantly. They arrive next day.

Requirements
- Add incomingOrders to state
- When you order: store it
- When you open shop: apply deliveries first

Why it’s good
- Forces planning
- Makes inventory management real

State Addition: 

```js
state.incomingOrders = []   // list of { dayArrives, itemId, qty }
```

Ordering flow (ORDER_STOCK action)
```
WHEN player places an order:
    IF already ordered today
        LOG "Already ordered today"
        STOP

    CALCULATE totalCost from wholesale price * qty
    IF cash < totalCost
        LOG "Not enough cash"
        STOP

    SUBTRACT totalCost from cash
    SET orderedToday = true

    CREATE orderRecord:
        dayArrives = state.day + 1
        itemId = chosen item
        qty = chosen qty

    ADD orderRecord to state.incomingOrders
    LOG "Order placed, arrives tomorrow"
```

Delivery application (before selling when OPEN_SHOP)

```
WHEN shop is opened:
    APPLY deliveries FIRST:

    FOR each order in incomingOrders
        IF order.dayArrives == state.day
            ADD order.qty to inventory[order.itemId]
            LOG "Delivery arrived: +qty item"
        ELSE
            KEEP order for later

    REMOVE delivered orders from incomingOrders

    THEN run simulateDay()
    THEN advance day
```

### 1B. Supplier pricing changes

Wholesale costs vary by day.

Requirements
- Add a daily “market” multiplier (e.g. 0.9–1.2)
- Show today’s wholesale prices in the UI
- Players must decide whether to buy now or wait

Why it’s good
- Real tradeoffs
- Encourages reading the report/log

Major Challenge 1B — Supplier pricing changes (daily wholesale market)

```js
state.marketMultiplier = 1.0
```

Start-of-day market roll (happens once per day)
```
AT the start of each new day:
    SET state.marketMultiplier = random value between 0.9 and 1.2

    IF marketMultiplier < 1.0
        LOG "Wholesale market is cheap today"
    ELSE IF marketMultiplier > 1.0
        LOG "Wholesale market is expensive today"
    ELSE
        LOG "Wholesale market is normal"
```

Compute wholesale price “today”

```
FUNCTION todayWholesale(product):
    RETURN ROUND(product.wholesaleCents * state.marketMultiplier)
```

Ordering logic (ORDER_STOCK action)

```
WHEN ordering stock:
    LOOK UP product by id
    SET costPerItem = todayWholesale(product)
    SET totalCost = costPerItem * qty

    IF cash < totalCost
        LOG "Not enough cash"
        STOP

    SUBTRACT totalCost from cash
    ADD qty to inventory[itemId]
    LOG "Bought qty item at today's wholesale price"
```

UI requirement

```
RENDER an "Order Panel" that shows:
    for each product:
        name
        today's wholesale price
```

### 1C. Storage limits + spoilage

Limit how much you can store; some products spoil.

Requirements
- Each product has maxStock
- Bagels spoil daily if unsold (or after 2 days)

Why it’s good
- Prevents “buy infinite stock” strategy
- Creates risk

Product config additions

```
PRODUCTS entries include:
    maxStock
    spoilageRule (optional)
```

Storage limits (ORDER_STOCK action)

```
WHEN ordering stock:
    LOOK UP product
    SET current = inventory[itemId]
    SET limit = product.maxStock

    IF current + qty > limit
        SET qty = limit - current

    IF qty <= 0
        LOG "Storage full for this item"
        STOP

    CALCULATE totalCost and apply purchase
    ADD qty to inventory
    LOG "Ordered qty (clamped by storage limit)"
```

Spoilage model option 1 (simple daily spoilage)

```
WHEN opening shop (before selling OR after selling, but be consistent):
    FOR each product:
        IF product is perishable (ex: bagel)
            spoiled = MIN( spoilAmount, inventory[bagel] )
            inventory[bagel] -= spoiled
            LOG "Bagels spoiled: -spoiled"
```

Spoilage model option 2 (aging inventory, advanced)

```
INSTEAD of inventory counts:
    inventory[itemId] is a list of batches:
        [{ qty, ageDays }, ...]

EACH day:
    INCREMENT ageDays for each batch
    REMOVE batches older than allowed
```

## Major Challenge 2 — Expand events into a real subsystem

Pick one:

### 2A. Event deck with weights

Some events are common, some rare.

Requirements
- Events have weight
- Random selection respects weights

Event data structure

```JS
events = [
    { name, weight, effectFunction OR effectData }
]
```

Weighted selection algorithm

```
FUNCTION pickWeightedEvent(events):
    totalWeight = SUM of all event.weight

    roll = random number between 0 and totalWeight

    running = 0
    FOR each event in events:
        running += event.weight
        IF roll <= running:
            RETURN event
```

Applying event (OPEN_SHOP)

```
WHEN opening shop:
    event = pickWeightedEvent(events)
    LOG event.name

    effect = event.effectData OR event.effectFunction(state)

    PASS effect into simulateDay OR store it in state for render/report
```

### 2B. Reputation system

Events and cleanliness affect reputation; reputation affects demand.

Requirements
- Add reputation (range -5 to +5)
- Reputation changes from:
- cleanliness
- certain events
- price spikes
- Reputation changes demand multiplier

Why it’s good
- Adds delayed consequences
- Gives the player a long-term goal besides cash

State addition

```js
state.reputation = 0   // range -5..+5
```

Clamp helper

```
FUNCTION clampReputation(value):
    IF value < -5 RETURN -5
    IF value > 5 RETURN 5
    RETURN value
```

Reputation changes (where they come from)

Cleanliness effect (each day):

```
AT end of day OR after opening shop:
    IF cleanliness <= 40:
        reputation -= 1
        LOG "Customers noticed the mess (-rep)"
    ELSE IF cleanliness >= 80:
        reputation += 1
        LOG "Customers love the clean shop (+rep)"

    reputation = clampReputation(reputation)
```

Price spikes:

```
WHEN setting price:
    IF new price is much higher than yesterday price:
        reputation -= 1
        LOG "Price gouging hurts reputation"
```

Events:

```
WHEN certain events occur:
    reputation += or -= based on event
    clamp reputation
```

Reputation affects demand

```
FUNCTION reputationMultiplier(rep):
    // choose a simple mapping:
    // rep -5 => 0.75 demand
    // rep  0 => 1.00 demand
    // rep +5 => 1.25 demand

    RETURN 1.0 + (rep * 0.05)
```

In simulateDay:

```
FOR each product:
    sales *= reputationMultiplier(state.reputation)
```

## Required UI & usability upgrade (pick at least one)

This is not optional. Your game is now too complex to play without feedback.

### UI Option A — Make “status” real

Show at least:
- cash
- day
- cleanliness
- promo days left
- reputation/mood if you added it

```
RENDER status panel:
    day
    cash
    cleanliness
    promoDaysLeft
    reputation (if used)
    marketMultiplier (if used)
    incomingOrders count (if used)
```

### UI Option B — Warnings

Add warnings like:
- “Low stock”
- “Not enough cash to order”
- “Shop is dirty — customers unhappy”
Must appear without alerts (no alert()).

State addition (recommended)

```
state.warnings = []   // list of strings
```

Build warnings during render OR during update

```
CLEAR warnings list

IF cash is low:
    ADD "Low cash" warning

IF any inventory item below threshold:
    ADD "Low stock: item" warning

IF cleanliness is low:
    ADD "Shop is dirty — customers unhappy" warning
```

Render warnings

```
RENDER warnings section as a list (always visible)
```

### UI Option C — Better report

Show:
- sold per item
- revenue
- costs (rent, promo spend, spoilage)
- net profit (revenue - costs)

Report shape (recommended)

```
lastReport = {
    soldByItem,
    revenue,
    costs: {
        wholesaleSpent,
        rent,
        promoSpent,
        spoilageLoss
    },
    netProfit
}
```

Net profit calculation

```
netProfit = revenue - (sum of all costs)
```

Render report

```
SHOW:
    sold per item
    revenue
    each cost line
    net profit
```

### UI Option D — Disable invalid actions

Disable buttons when:
- game over
- already ordered today
- promo already active
- inventory is empty (optional)

Part Six Submission
1. A list of the challenges you chose
2. A short explanation of your new systems
3. Evidence it’s playable:
  - screenshots OR a short screen recording OR a written “play log”

```
IN renderControls:
    IF gameOver: disable all buttons

    IF orderedToday: disable order button
    IF promoDaysLeft > 0: disable promo button

    IF cash too low for minimum order: disable order button OR show warning
```

