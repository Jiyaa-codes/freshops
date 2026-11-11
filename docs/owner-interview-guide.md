# Grocery owner interview guide

Spend about 20 minutes with the person who places orders. Ask them to walk through one recent order for a perishable product, such as milk or bread. Use that example to understand what they knew at the time, how they chose a quantity, and what happened afterward.

Suggested opening: “I’m building a student project about how small grocery shops decide what to order. Could you walk me through a recent order for something that expires quickly? Approximate quantities are fine.” Ask before taking notes and keep identifying details and private business records out of the public repo.

| Time | Ask | Listen for |
|---|---|---|
| 5 min | “Think of the last time you ordered this item. What did you check before you picked a quantity? When did you place the order, and when did it arrive?” | The person making the decision, the current rule, the order cutoff, and time to delivery. |
| 5 min | “When did this item last run out or expire? How did you find out? What, if anything, was recorded?” | Whether the shop can distinguish a sellout from a quiet day, and whether it tracks expiry by batch or records discarded units. |
| 5 min | “Can you remember an order where you wanted a different quantity but couldn’t get it? What determined what you actually ordered?” | Constraints that affected a real decision. If needed, ask about supplier pack sizes, minimum orders, storage space, and budget. |
| 5 min | “Where are yesterday’s sales and today’s stock recorded? Can you get them into a spreadsheet? What would you check before using a suggested order, and when would you change it?” | How much effort data entry would take and what the owner needs to trust or override a suggestion. |

Ask which product causes the most trouble if the first example turns out to be routine. Ask what happens in a typical week before assuming that one unusual order represents the normal workflow.

## Notes to fill in after the conversation

- **Context:** anonymous shop label, role of the person who orders, and product category.
- **Recent order:** trigger, inputs they checked, how they decided, when they ordered, and when it arrived.
- **Outcome:** what sold, when stock ran out or expired if known, and what was recorded.
- **Data available:** where sales, stock, expiry, and stockouts are kept. Mark each as observed in a record, described by the owner, or unknown.
- **Binding constraint:** the specific limit that changed the quantity in the example.
- **Review and override:** what the owner would check, what they would change, and whether a CSV upload fits their routine.
- **Implication for FreshOps:** one proposed change or assumption to test next, tied to a specific observation.

## How the answers inform the build

| If the interview reveals… | Decision to revisit |
|---|---|
| The owner can export sales but rarely records when stock runs out. | Treat missing availability as unknown. Test whether a simple manual stockout note is feasible. |
| The supplier routinely delivers after the next day. | Revisit the planning horizon before training a model around next-day purchasing. |
| The owner can see total stock but has no batch expiry record. | Check whether manual expiry entry is realistic before making it a required input. |
| The supplier sells in fixed packs and the shop has limited storage. | Prioritize the constraints that changed the example order. |
| Sales are kept on paper or a CSV export takes too much effort. | Test a small manual entry workflow before committing to CSV as the only input. |
