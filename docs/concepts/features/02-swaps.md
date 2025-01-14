---
description: High level overview of swapping between risky and stable tokens in the Engine
---

# Swaps

### Two token pools and a way to swap between them

Primitive Pools have two tokens, which are called a risky asset and stable asset. The core smart contract exposes a low level `swap` call, allowing the exchange between the two tokens as long as the _**trading invariant** _is preserved.

### How much gas does swapping cost?

A low-level swap is about 100k gas on average. A single route swap using the PrimitiveHouse averages 144k gas.

Learn more about the trading invariant in the research page:

[Research](../advanced)

### Source Code: Low-level Swap&#x20;

[Swap](/protocol/core/swap)





