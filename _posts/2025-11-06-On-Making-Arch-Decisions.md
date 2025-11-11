---
layout: blog_post
title: "On Evaluating Options and Making Decisions"
date: 2025-11-06
tags:
  - engineering
  - team
---

_This was originally written for an intern on my team as some general advice/guidance before they started their first project. Some examples and references have been modified to exclude non-public information and make more sense in a general context._

---

These are some incoherent thoughts to hopefully help you when making decisions on modeling and different options. **Take it with a grain of salt; take advice from others.**

I generally find it helpful to think about the code in terms of 2 stakeholders and their “interfaces” or how they experience your feature or interact with your model. There may be more, or these groups may change depending on what you’re building but in general they are:

1. Customers
1. Other engineers

When you’re evaluating a new model/payment type/pattern, you’re often making tradeoffs between these 2 groups, specifically their interfaces to the system.

These “interfaces” can vary, but many times making the user experience better for one side is zero-sum for the other. Those “ah-ha” moments are when you figure out how to deliver a good experience for both.

## For customers

### Detailed error messages

An example could be “This id belongs to a Counterparty; you probably meant to use an External Account”.<sup>1</sup> Think about the tradeoff here: we have to issue extra queries against the db to know for sure. Is it worth it? I think so. Should we do this for all ids? Probably not.

There are second order effects too. Being able to validate Being able to validate payments better results in errors further upstream. Once the payment goes to the bank and there’s an issue, we often have to get Customer Support and Engineering involved. So would you rather have a few more queries and messy logic, or spend an afternoon debugging a payment days after it was sent?

### A single payment order API

This one sounds trivial and is table stakes for us, but many banks (including major ones) have extremely disparate API products. This could mean different endpoints, request shapes, formats, and responses. To offer our customers unified interfaces to each feature we have to both deal with these disparities on the “backend” and write a lot of logic.

At Modern Treasury, a couple examples of this are:

- Our complex payment validation system, which validates payment instructions against the originating bank's requirements for the specific rail.
- Our payment routing service, which determines the correct set of account and routing information (i.e. a "route") a payment should use.

Ultimately, this wins deals. We accept the tradeoff for engineering so that our customers don’t have to care, and as a result, the upside for customers that use multiple rails and/or multiple banks is huge.

### The least surprising thing

This is something I think about a lot when deciding how something should _behave._ You’ve worked with many APIs, both good and bad. What kinds of responses and behaviors make you go “wtf?”. Surprising is generally not good. Often it’s easier to code something in a way that makes sense from the engineer’s interface, but results in surprising behavior to the customer.

## For other engineers

### How easy is it to add the next thing?

This applies a lot when you’re establishing a pattern or introducing a new model. How much pain (read: new code) is the next engineer that uses this going to go through? How much of that code is going to be “hacky”? Think a couple use cases into the future.

The tradeoff here is over-engineering aka [yagni](https://martinfowler.com/bliki/Yagni.html). This applies to a lot of development, but remember — we’re an infrastructure layer. We’re always going to have to support newer and more convoluted use cases, so sometimes (tasteful) foresight pays off.

Again, consider the tradeoff for customers. Overly generic things are often not useful. Think about banks that respond with a single error or status code regardless of which field in the API call was invalid. Think about how annoying it is to add or edit things in the 1Password editor (lol). How does this affect the product or feature’s usefulness?

### How easy is it to understand?

Simple is good and usually means less bugs. Lean in to Ruby patterns, the language is pretty powerful, and flexible enough to implement a lot of common and complex patterns. How easy something is to understand often correlates to how likely the next engineer is to actually use it. I won’t say complexity results in more bugs, but it definitely makes it harder to _debug_. Simplifying code will often allow more engineers to engage with it.

Balance simplicity with actually supporting all the use cases that you need to (duh) as well as performance. Simplicity and performance don’t have to be mutually exclusive. Your PM is a good resource to lean on to understand the _how_ and _why_ behind what the customer is doing and wants to do. Remember: "sexy" code doesn’t matter if no one uses it.

Few options are ever perfect, and even if they are it never lasts. There will always be a new customer, bank, or something in between that breaks the pattern that’s “worked for so long”. Do some research, think through the tradeoffs, make a decision, and start implementing.

With every project there are “unknown unknowns” i.e. weird stuff that you’ll only uncover as you start writing code and hitting their APIs. Don’t get stuck on a single decision; no decision has to be final. You can always make changes as you get new info.

Good luck 🤙🏽

## Notes

1. At Modern Treasury, two of our core [payments API](https://docs.moderntreasury.com/payments/docs/quickstart) models were Counterparties (individuals or businesses that you want to transact with) and External Accounts (actual bank accounts owned by a Counterparty). A common mistake we saw customers make was attempting to create a payment against a Counterparty [instead of a specific External Account](https://docs.moderntreasury.com/payments/docs/quickstart#4-create-a-payment-order).
