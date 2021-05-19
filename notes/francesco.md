# Francesco's notes and project ideas

Credits to `@pipermerriam` for suggesting the below ideas.

## Eth1 specs

Here are [Eth2.0 specifications](https://github.com/ethereum/eth2.0-specs).

They are written in Markdown and have executable Python parts, and those parts can be [tested](https://github.com/ethereum/eth2.0-specs/blob/dev/tests/core/pyspec/README.md) to verify compliance with the spec. Tests can also be ran in generator mode, outputting tests that clients can consume to verify their compliance with the spec.

A project idea would be doing the same work for Eth1, essentially writing the specs for the entire protocol in easily understandable code and language.

### Benefits

- Very effective way to deeply learn about the whole protocol in a bottom-up way.

    _TODO: Who else would benefit from this work and why? How are Eth1 client tests currently created?_

### Gathered information

- We already have all the information (Yellow Paper, EIPs) and tested Python implementations (`py-evm`, `trinity`, `py-trie`) we need as a starting point. Looks like the core of it would be understanding the concepts and re-writing the code in the most readable, easy-to-understand way. Probably does not need a lot of hand holding.
- The Eth2.0 specs repo looks complex. A deep dive is needed to fully understand how it is organized and how the tests are generated.
- Looks like a lot of work to tackle alone. It'd be nice to have at least two people working together on this.

## EIP-1559 `py-evm` implementation

[EIP-1559](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1559.md) is introducing a new transaction pricing mechanism which will be effective from the London hard fork. [py-evm](https://github.com/ethereum/py-evm) needs to keep up with mainnet changes, so work is needed to integrate the EIP into it.

### Benefits

- Very effective way to learn about the protocol and `py-evm` in a top-down way.
- A way to contribute to keeping `py-evm` relevant in the future.

### Gathered information

- The spec was still being updated as of mid-May. A `go-ethereum` implementation is available [here](https://github.com/ethereum/go-ethereum/pull/22837/files).
- A WIP PR is already open [here](https://github.com/ethereum/py-evm/pull/2005). General structure is there, here's what still needs to be done:
  1. Make sure `LondonVM` passes all existing tests (still a few to tackle);
  2. Create new tests for EIP-1559 transactions;
  3. Periodically sync with the `py-evm` team to discuss general direction as well as implementation details (mostly about new block headers, transactions). The codebase is not hard to understand once you spend some time with it, but there are many ways to implement the same thing and experience is needed to understand the best one;
  4. Understand a lot of small but important issues (what are the default values for new fields? what should happen in edge cases? how should the old block -> new block jump happen?). The `go-ethereum` implementation as well as discussions happening in `#1559-dev` and `#1559-fee-market` could be helpful;
  5. Organize and separate commits in a way that makes sense.