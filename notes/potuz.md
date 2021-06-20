Here are some quickly written notes for the first cohort meeting. I will slowly populate this file with some more projects and status reports depending on time availability.
# Mammon: the greedy consensus client

## Issue:
There are a few places in the ETH 2.0 specs where we rely on honest, and often times altruistic behavior from stakers. To name a few
- Keeping historical blocks:
A node can sync from a trusted state and simply discard any blocks beyond the last finalization checkpoint, thus not helping those nodes that need to sync from genesis or have fallen out of sync since before the last finalized epoch. There is no specified punishment for this behavior.
- Vote for arbitrary ETH1 data:
There is no penalty for voting arbitrarily when proposing a block. The only case is when one needs to include deposits. There is an issue about this that has been raised [Is it even worth running an Eth1 node...](https://github.com/ethereum/eth2.0-specs/issues/2152).
- Consider all `ExecutionPayload` as valid:
There is no penalty for attesting on blocks without checking their excution validity after the merge.
- Propose blocks late:
Proposers are rewarded by the attestations included, not by the votes they get. It is beneficial for a proposer to submit their block late enough after the attesters may have voted for it and before the next proposer sees it. This is specially true for `Phase0` and loses importance after the merge. An analysis and a proposal was posted [here](https://ethresear.ch/t/reward-proposers-by-received-votes/8588/2)

## Objectives:

To obtain a real life quantification of the incentives to *cheat*. Measure direct economical incentives like proposing late in `Phase0` or indirect ones like the ones obtained from lower resource consumption and lower latency.

## Method:

We plan on building a C++ client from scratch that's optimized for the above objectives. We implement the features mentioned above. In order to do so we need to implement some mitigation techniques to prevent peers from dropping us while we gain simplicity of the code since we do not need to worry about certain scenarios. Some examples
- We need to fake our p2p id and change it often in case we are booted for not propagating useful old blocks.
- We do not need to worry about fork transition logic
- We do not need to worry about long consensus forks nor periods with no finalization. This allows us to keep the entire non-finalized tree of blocks in-memory without caring about disk access nor databases.

## Status:

The project is in its infancy, we have implemented the basic types and containers, SSZ serialization. After the beacon-chain logic is complete, we plan to release the code and ask for help from the comunity dealing with the `libp2p` stack. There is a C++ implementation [here](https://github.com/libp2p/cpp-libp2p) which has GossipSub marked as WIP. 
