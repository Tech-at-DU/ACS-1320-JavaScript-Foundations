# Shopkeeper — Part Five

Make it feel like a real simulation

Goal

Right now, Shopkeeper works correctly — but it still feels mechanical.

Numbers change, but nothing feels uncertain or dynamic. Once a player finds a “good” setup, the game plays itself.

In this part, you will make the game feel more like a simulation by introducing:
  - Scale — larger sales volumes and cash flow
  - Uncertainty — days that go better or worse than expected
  - Pressure — events that force adaptation instead of optimization

You must complete Step A and Step B.
Then choose one option from Step C.

Do not rewrite the architecture.
Do not add new UI controls yet.
You are changing how the existing system behaves.

## Step A — Scale the numbers (required)

**The problem** 

Selling 0–5 items per day is too small.

At that scale:
  - Modifiers like “+1 sale” barely matter
  - Randomness feels cosmetic
  - Events feel weak or pointless

This makes the game easy to solve and boring to play.

**Your task**

Rescale the simulation so that a normal day might sell 50–200 units, depending on the product.

This does not mean making the game more realistic.
It means making percentage-based changes meaningful.

**Constraints**
  - Do not add new buttons or controls
  - Do not rewrite the core game loop
  - Stop using “+1 item” style modifiers. Instead use a multiplier, for example: `* 1.2` is like +20%.

**Required changes**

1. Add base demand to each product
In `PRODUCTS`, add a `baseDemand` value. This determines the amount of that product that might be sold on any day. 

**Example** (you choose the numbers):

```js
{ id: "coffee", name: "Coffee", wholesaleCents: 150, baseDemand: 120 }
```

This number represents typical daily demand before modifiers.

Different products should have different base demand.

2. Switch from additive to multiplicative modifiers
At this scale, modifiers must work as percent changes, not flat additions.

Instead of:
  - “+1 item”
  - “−1 item”

Think in terms of:
  - “+25% demand”
  - “−15% demand”

Example modifier effects (conceptual, not exact requirements):
  - Promotion → demand × 1.25
  - Very clean shop → demand × 1.15
  - Dirty shop → demand × 0.85

You will combine these into a final multiplier and apply it to baseDemand.

Here is some pseudo code that outlines a solution. This would be used to refactor `simulateDay(state)` in `economy.js`.

```
FOR each product
    SET base sales = product.baseDemand

    SET daily sales = base sales

    APPLY daily randomness
        daily sales *= random factor

    APPLY cleanliness
        IF shop is dirty
            daily sales *= cleanliness penalty
        ELSE IF shop is very clean
            daily sales *= cleanliness bonus

    APPLY promotions
        IF promo active
            daily sales *= promo multiplier

    ROUND daily sales to whole number

    LIMIT daily sales by available inventory

    SUBTRACT sales from inventory
    ADD sales * price to cash
```

**Deliverable**

Add a short comment or README note explaining:
  - Your base demand numbers
  - Your demand multipliers
  - Why you chose those values

Success looks like
  - Sales numbers feel large enough to swing meaningfully
  - A 10–20% change is visible and impactful
  - Inventory and ordering decisions matter more

## Step B — Add more chance (required)

**The problem**

Right now, players can compute the “best” strategy and repeat it forever.

If prices and cleanliness don’t change, results don’t change.

That’s not how simulations — or games — behave.

**Your task**

Add day-level randomness so that:
  - Some days are better than expected
  - Some days are worse
  - Outcomes vary even when the player makes good decisions

This randomness should affect demand, not bypass the system.

**Important clarification** (read carefully)

This step uses two separate ideas:
  1. Chance — how often something happens
(“10% of days are terrible”)
  2. Effect — what happens when it does
(“sales drop to 70% of normal”)

These are not the same thing.

Constraints
  - Roll randomness once per day, not per product
  - The result must be explained in the log
  - Demand must still be clamped by inventory

If the player cannot tell why sales changed, it feels like a bug.

**Suggested models** (choose one)

Model 1 — Day type table (recommended)
Each day, roll once to determine the type of day:

Chance	Day type	Effect on demand
10%	Terrible day	demand × 0.70 (30% lower)
20%	Slow day	demand × 0.85 (15% lower)
55%	Normal day	demand × 1.00
15%	Great day	demand × 1.25 (25% higher)

**Example interpretation:**

“10% chance” means how often the day occurs
“×0.70” means demand is reduced to 70% on that day

Model 2 — Noise (simpler)
Instead of named day types:
  - Generate a random multiplier between 0.85 and 1.15
  - Multiply demand by that value

This creates smaller but constant variation.

Here is a psuedo code description of solution: 

```
AT start of day
    ROLL random number between 0 and 1

    IF roll is in terrible range
        SET day multiplier to low value
        LOG "Terrible day"
    ELSE IF roll is in slow range
        SET day multiplier to slightly low
        LOG "Slow day"
    ELSE IF roll is normal
        SET day multiplier to 1.0
        LOG "Normal day"
    ELSE
        SET day multiplier to high value
        LOG "Great day"
```

**Deliverable**

Your log must show why demand changed, for example:
  - “Slow day — fewer customers than usual.”
  - “Great day — lunch rush was huge.”
  - “Demand was about 12% lower than average today.”

Common mistakes to avoid
  - Treating “10% chance” as “+10% demand”
  - Adding randomness after inventory is clamped
  - Rolling randomness separately for each product
  - Hiding randomness from the player

## Step C — Expand events (choose one)

**The problem**

Right now, events feel inconsistent and underpowered.
They don’t meaningfully interact with the rest of the system.

**Your task** (choose one approach)

C1. Make events data-driven
  - Store events in an array of objects
  - Randomly select one
  - Each event returns a structured effect, not direct mutations

Example effects:
  - demand multiplier
  - cleanliness change
  - temporary status

```
DEFINE list of events
    each event has:
        name
        chance
        effect description
        effect data

ROLL random number

FOR each event
    IF roll matches event chance
        APPLY event effect
        LOG event name
        STOP checking further events
```

C2. Add event chains
Events can affect future days.

Examples:
  - “Bad weather” → lower demand tomorrow
  - “Local festival” → demand boost for 2 days

This introduces delayed consequences.

```
WHEN event happens
    SET state.effectDaysLeft = N
    SET state.effectMultiplier

AT start of each day
    IF effectDaysLeft > 0
        APPLY effect
        effectDaysLeft -= 1
    ELSE
        CLEAR effect
```

C3. Add player-respondable events
Some events require a choice.

Example:
  - “Health inspector arrives”
  - Pay $10 bribe
  - Lose reputation
  - Close shop for the day

Each option must have a clear cost.

```
WHEN special event occurs
    PAUSE simulation

    SHOW choices to player
        choice A: cost money
        choice B: reduce demand
        choice C: close shop

    WAIT for player decision

    APPLY consequences
    RESUME simulation
```

Constraints
  - Events should not “do everything” themselves
  - Events produce effects; the simulation applies them
  - Every event must be explained in the log

## Part Five Submission

You must submit:
  - Your updated PRODUCTS config (with baseDemand)
  - Log output showing randomness and events
  - A short paragraph answering:

What change made the game feel more alive, and why?
