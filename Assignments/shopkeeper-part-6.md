# Shopkeeper — Part Six

Depth, economy, and UI polish

Goal

Make the game deeper and more playable by extending:
	•	purchasing + supply chain
	•	economy systems (pressure + tradeoffs)
	•	usability and clarity

Choose two major challenges + one UI/usability upgrade.

⸻

Major Challenge 1 — Expand purchasing and economy

Pick one direction:

1A. Delivery delay (recommended)

Orders don’t arrive instantly. They arrive next day.

Requirements
	•	Add incomingOrders to state
	•	When you order: store it
	•	When you open shop: apply deliveries first

Why it’s good
	•	Forces planning
	•	Makes inventory management real

⸻

1B. Supplier pricing changes

Wholesale costs vary by day.

Requirements
	•	Add a daily “market” multiplier (e.g. 0.9–1.2)
	•	Show today’s wholesale prices in the UI
	•	Players must decide whether to buy now or wait

Why it’s good
	•	Real tradeoffs
	•	Encourages reading the report/log

⸻

1C. Storage limits + spoilage

Limit how much you can store; some products spoil.

Requirements
	•	Each product has maxStock
	•	Bagels spoil daily if unsold (or after 2 days)

Why it’s good
	•	Prevents “buy infinite stock” strategy
	•	Creates risk

⸻

Major Challenge 2 — Expand events into a real subsystem

Pick one:

2A. Event deck with weights

Some events are common, some rare.

Requirements
	•	Events have weight
	•	Random selection respects weights

⸻

2B. Reputation system

Events and cleanliness affect reputation; reputation affects demand.

Requirements
	•	Add reputation (range -5 to +5)
	•	Reputation changes from:
	•	cleanliness
	•	certain events
	•	price spikes
	•	Reputation changes demand multiplier

Why it’s good
	•	Adds delayed consequences
	•	Gives the player a long-term goal besides cash

⸻

Required UI & usability upgrade (pick at least one)

This is not optional. Your game is now too complex to play without feedback.

UI Option A — Make “status” real

Show at least:
	•	cash
	•	day
	•	cleanliness
	•	promo days left
	•	reputation/mood if you added it

UI Option B — Warnings

Add warnings like:
	•	“Low stock”
	•	“Not enough cash to order”
	•	“Shop is dirty — customers unhappy”
Must appear without alerts (no alert()).

UI Option C — Better report

Show:
	•	sold per item
	•	revenue
	•	costs (rent, promo spend, spoilage)
	•	net profit (revenue - costs)

UI Option D — Disable invalid actions

Disable buttons when:
	•	game over
	•	already ordered today
	•	promo already active
	•	inventory is empty (optional)

⸻

Part Six Submission
	1.	A list of the challenges you chose
	2.	A short explanation of your new systems
	3.	Evidence it’s playable:
	•	screenshots OR a short screen recording OR a written “play log”

⸻

Instructor note (what to watch for)

Most students will:
	•	scale numbers but forget to scale modifiers (game becomes flat)
	•	add randomness without logging it (feels like a bug)
	•	let events mutate state in messy, inconsistent ways
	•	add UI “info” that contradicts state (wrong values)

Your rubric should reward:
	•	consistency
	•	clarity
	•	predictable state flow
