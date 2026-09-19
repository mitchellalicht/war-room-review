# Example review: Fernwood checkout (fictional)

> Fictional artifact, written to test the skill. Fernwood is an invented online plant shop. Nothing here refers to a real product.

## The artifact

**Fernwood** sells live houseplants online and delivers them in insulated boxes. Mobile web checkout, four screens. Guest checkout is supported.

**1. Cart**
- Line items with photo, name, size, price, quantity stepper
- Promo code field, open by default, above the order total
- Order total reads "Shipping calculated at next step"
- Two filled buttons of equal size: **Proceed to checkout** and **Continue shopping**

**2. Delivery**
- Address form (name, street, city, postcode, phone)
- Date picker showing only Tuesdays and Thursdays (plants ship on those days), no explanation of why other days are greyed out
- Toggle: **Gift?** reveals a message box
- Any invalid field shows "Invalid input" in red under the field
- Button: **Continue**

**3. Payment**
- Banner at top: "Your plants are reserved for 10:00", counting down live
- Order total now shows **Shipping $12.95** for the first time
- Card form, plus a checkbox labeled "Use Fernwood credit"
- Button: **Pay now**
- On decline: red banner reading "Payment failed."

**4. Confirmation**
- "Order placed! 🌿"
- Order number
- No delivery date, no mention of a confirmation email

**Stage:** pre-ship, launching in two weeks. **Constraints:** existing payment provider, must ship in three languages, guest checkout is non-negotiable. **Author is unsure about:** the delivery date picker.

---

## Context

Full review of a four-screen mobile checkout, pre-ship, launching in two weeks, with a fixed payment provider and three languages. Weighted toward the delivery date picker, per the author. Two Blocking notes, both about the same countdown timer, seen from engineering and from the user.

## Assumptions

- Reviewed from the written spec above, not a rendered design. Visual notes are inferred from the described layout and will need a check against the real screens.
- Assumed the 10-minute reservation timer is real inventory logic, not decoration.
- Assumed "Fernwood credit" is a stored balance from refunds or gift cards, and that guests may not have any.

## What's working

- **Guest checkout with no forced account.** It removes the single biggest cause of checkout abandonment, and it's the constraint that most often gets quietly dropped in the last sprint. Protect it.
- **Only offering real delivery days.** Showing just Tuesdays and Thursdays is honest about how the business works. The problem is the missing explanation, not the restriction.
- **Four screens with one job each.** Cart, delivery, payment, confirmation. The structure is right, so most of the fixes are inside screens, not to the flow.

## Product management

- **Should fix—The shipping fee first appears on the payment screen.** A $12.95 surprise at the last step is the classic cause of abandonment at checkout. It also makes the cart total a number the customer can't trust. Show shipping, or a clear estimate, on the cart.
- **Should fix—No success metric is stated for any screen.** Pick one per step: cart-to-delivery rate, delivery-to-payment rate, payment success rate, and the share of orders that get a "where is my order" ticket.
- **Consider—Promo code open by default.** It sends customers off to search for a code, and some never come back. The smallest test is collapsing it and comparing completion.

## Engineering

- **Blocking—The countdown timer and the payment form have no defined behavior at zero.** If the timer hits 0:00 while a card is being authorized, the inventory can be released and the charge can still go through. That's an oversold order and a refund. It needs a defined rule (freeze the timer during authorization, or extend on activity), plus a recovery path so a customer who times out mid-typing doesn't lose their cart. *Also raised by: product management (no recovery path, and no stated metric for what the timer is meant to lift), product design (it's the loudest element on the payment screen).*
- **Should fix—Missing states across the flow.** Not drawn: empty cart, an item that sells out while in the cart, a postcode outside the delivery area, no available dates in the next two weeks, offline or slow payment response, and a 3-D Secure challenge that opens in a new context. The date picker is the most likely to be hit: what does it show when the next Tuesday is full?
- **Should fix—"Invalid input" across five fields means the validation returns no reason.** If the API returns field-specific reasons, use them. If it doesn't, a small change to the response is much cheaper than writing generic fallback copy in three languages. *Also raised by: content design, who owns the messages themselves (below).*
- **Consider—The Gift toggle reveals a text box.** Confirm the message length limit, what happens to emoji, and whether the packing slip can print it. Cheap to define now, expensive to discover after launch.

## Content design

- **Should fix—"Payment failed." is a dead end.** It names the problem and gives no remedy, on the screen where the customer is most anxious about money. Try: "Your payment didn't go through. Check your card details, or try another card. You haven't been charged." The last sentence is the one that matters.
- **Should fix—"Invalid input" says nothing.** Each field needs its own message. Postcode: "Enter a postcode like 90210." Phone: "Add a phone number so the courier can reach you." Name what to do, not what went wrong. *Also raised by: engineering, who owns the field-level reasons behind these messages (above).*
- **Should fix—"Use Fernwood credit" is unexplained.** Credit from where, how much, and does it cover shipping? Rename to show the amount: "Apply $20.00 Fernwood credit." If a guest has none, don't show the checkbox at all.
- **Should fix—"Order placed! 🌿" carries too little.** A confirmation screen is where customers look for the promise: the date, the address, the email. The emoji is doing the work the missing information should do. Try: "Order placed. Your plants ship Thursday, May 14 to 12 Elm Street. We've emailed your receipt to sam@example.com."
- **Consider—The greyed-out days need one line of explanation.** Customers will wonder why Monday is unavailable. Try: "We ship on Tuesdays and Thursdays so plants aren't stuck in a warehouse over the weekend."

## Product design

- **Should fix—Two equal filled buttons on the cart.** **Proceed to checkout** and **Continue shopping** compete, and half of customers will hit the wrong one. Make **Continue shopping** a text link or an outline button. One primary per screen.
- **Should fix—The countdown timer is the loudest element on the payment screen.** *(Behavior at zero is covered under Engineering.)* Full-width, top of the page, moving. It outweighs the card form, which is the actual task. A calm, small timer near the total would do the same job without raising the customer's pulse.
- **Should fix—Greyed-out dates look broken, not intentional.** Disabled dates need to read as "unavailable by design," and the selected date needs a state that isn't only a color change. Show the ship days as the only tappable ones, and the rest as plain unstyled numbers.
- **Consider—The Gift toggle sits in the middle of the address form.** It interrupts a task with a different one. It reads better as a section after the date picker, with a clear heading.

## The user

- **Blocking—I'm using a screen reader and a live countdown announces itself every second.** I can't finish typing my card number over the noise, and I can't tell how much time I have. A timer needs to announce only at meaningful moments (say, at 2 minutes and 30 seconds) and let me extend it.
- **Should fix—I picked a delivery date and I can't tell which one I picked.** On the calendar the selected date changes color and nothing else. In direct sun on my phone, I can't see it. I would go back to check on the next screen, and that screen doesn't show the date.
- **Should fix—I paid, and I don't know if it worked.** "Order placed!" without a date or an email note means I'll open my inbox to check, find nothing yet, and worry I've been charged for nothing. I'd contact support.
- **Should fix—I made a typo in my postcode and the form said "Invalid input" under one of five fields.** I don't know which character is wrong or what a valid postcode looks like. I'd retype everything.

## Where they disagree

**PM wants the timer to stay; the user and engineering want it constrained or gone.** PM's case: a live reservation creates urgency, and urgency lifts completion on stock-limited items like live plants. The user and engineering say it breaks in two real ways: it's unusable with a screen reader, and it has no defined behavior when it expires mid-payment. *Kind: answerable.* Test it. Run the checkout with and without the timer on a slice of traffic and compare completion and oversold-order rate. If the timer stays, design the recovery first: freeze during authorization, warn at 2 minutes, one-tap extend. PM owns the decision, and the data should be in hand before launch.

**Engineering wants to keep the error copy minimal; content design wants specific messages.** Engineering's point: every error variant is a string in three languages, forever. Content's point: "Invalid input" and "Payment failed." are why customers contact support. *Kind: false conflict.* Most of the cost is in the API not returning reasons, not in writing strings. Have validation return a reason code per field (about a day of backend work), then write four short messages: required, format, out of area, and declined. That's small enough to translate and specific enough to be useful. No one has to lose.

**PM wants shipping shown on the cart; engineering says the fee can't be known until the address exists.** PM's point: the surprise is the biggest abandonment risk in the flow. Engineering's point: shipping depends on postcode and box size, and neither is known at the cart. *Kind: real tradeoff.* Options are a flat estimate on the cart ("Shipping from $9.95, final price after you enter your address"), free-shipping threshold messaging, or moving the address step first. Each trades accuracy, effort, and flow order. The PM should decide, with a designer proposing which option keeps the flow at four screens.

**Product design wants one calm primary action per screen; PM wants to keep "Continue shopping" prominent.** PM's implicit reason: it recovers customers who aren't ready to buy. Design's reason: two equal buttons split attention at the moment of commitment. *Kind: false conflict.* An outlined or text-style **Continue shopping** stays visible and recoverable while **Proceed to checkout** is the only filled button. The evidence that would confirm it is the click ratio between the two, before and after.

## If you fix three things

1. **Define what the timer does, and make it accessible.** Freeze it during card authorization, announce only at 2:00 and 0:30, add a one-tap extend, and decide what happens at 0:00. This is the only issue that both loses money and blocks a user, so it goes first.
2. **Show shipping before the payment screen.** Even as "from $9.95, final after you enter your address." Removes the top abandonment driver and makes the total believable.
3. **Fix the two dead-end messages.** Replace "Payment failed." and "Invalid input" with specific, actionable ones, backed by field-level reasons from the API. Add the delivery date and the email line to the confirmation screen so a paid customer knows it worked.

## Open questions

- Is the 10-minute reservation a real inventory hold, and does authorization interact with it? (Engineering to confirm.)
- What is the current abandonment rate by step, and how much of it is at payment? (PM to pull.)
- Does the payment provider return decline reasons that are safe to show to the customer? (Engineering to confirm with the provider.)
- Who holds "Fernwood credit," and can guests have any? (PM to answer, since it decides whether the checkbox exists.)
