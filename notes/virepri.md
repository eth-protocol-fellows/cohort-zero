# Project ideas

Almost all of these will focus on long-term usage of Ethereum and the staying-power of DApps.

## 1. Off-chain communication

Sometimes, users on the Ethereum network may have an interest in communicating with eachother. For instance, match-making in games that may be implemented as dApps, IM on Ethereum, etc.

I'd like to borrow an idea from Beam, namely something akin to [SBBS](https://beam.mw/beampedia-item/sbbs) for this purpose, and to build atop Ethereum's existing P2P network.

## 2. Contract scheduled invocations

Right now, in cases where contracts need future invocations, there are no really good options aside from manual scheduled invocations, which require off-chain resources.

In essence, the idea is adding some alternative to the existing `CALL` EVM opcode that would include a block height and proposed gas settings (treated as a maximum). This would draw from a contract's ether stores, rather than the existing execution context.

It is important that these calls get their own execution context, akin to being called by a user.

### Early concerns

- If a contract schedules a call but the intended gas price is low enough, it could just never execute.
  - Perhaps a user could specify -1 for a "floating" gas price. That being, that it'd just take the network average at time of invocation.
- Contracts could possibly spam up the mempool and try to force gas price inflation.
  - Technically, an average user could do this too without a contract... So I'm less concerned about it. But fundamental limits could also be placed per contract, in terms of how many scheduled executions there can be at a time.

Potential spamming attacks

## 3. Privacy in transacting & executing

Ethereum has similar issues to Bitcoin and other major cryptocurrencies in that it is _wildly_ traceable. Entire companies exist to capitalize not on what these things _can_ do, but what they _cannot_ do.

Treat this as a stub. Currently, I don't much have a theory or a plan for solving this. Ethereum relies a lot on addresses, as it is currently (especially in contracts). I'd hate for this to be a layer-2 type (or optional) solution, since that always sticks out like a sore thumb when it's used.
