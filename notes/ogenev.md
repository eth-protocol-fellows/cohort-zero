# Ognyan's topics of interest and links

## Project candidates

## 1. Proof of concept for code chunk tracking

#### Project summary
https://github.com/ethereum-cdap/cohort-zero/issues/62

Write POC EVM modification in a client to keep track of code chunks access.

#### Acceptance criteria

To do

#### Questions
1. Is code merkleization implemented in any of the clients, other than
https://github.com/s1na/go-ethereum/tree/code-merkleization-ssz-stats ?
2. Are there any prototype implementations of verkle trie in geth? I have found only [this PR](https://github.com/ethereum/go-ethereum/pull/22999).
3. What is meant by "keep track of which chunks of code you access"? What is the acceptance criteria?

#### Actions
1. Set clear project goals and acceptance criteria.
2. Read on SSZ vs rlp.
3. Check EVM implementations (py-evm/geth)

#### Links

Code merkleization:
https://hugo-dc.github.io/cm-docs/

https://blog.ethereum.org/2021/04/26/ef-supported-teams-research-and-development-update-2021-pt-1/#code-merkleization

Implemented code merkleization in geth:
https://github.com/s1na/go-ethereum/tree/code-merkleization-ssz-stats

Verkle tree in go:
https://github.com/gballet/go-verkle

A state expiry and statelessness roadmap:
https://notes.ethereum.org/@vbuterin/verkle_and_state_expiry_proposal

Verkle tree EIP:
https://notes.ethereum.org/@vbuterin/verkle_tree_eip

State Expiry EIP:
https://notes.ethereum.org/@vbuterin/state_expiry_eip

[State expiry zoom recording](https://consensysmesh.zoom.us/rec/play/VoftjeRO0xNFHObLZpot-r0hTj0U6ZG0fUKORwQzy2qoh-JlweRgL6hj5UxqC8WsdYW3IOVBc6l_912R.ND8fJB672c-EpG1T?_x_zm_rhtaid=872&_x_zm_rtaid=Jhbf_Q6fTx-tPJB7vFllMg.1623863913448.e6b0045131eec8ea5666c579c3b613b7&autoplay=true&continueMode=true&startTime=1623848674000)

EVM:
https://blog.trustlook.com/understand-evm-bytecode-part-1/

## 2. Analyze how changes to the gas cost schedule in state expiry will effect existing contracts
 https://github.com/ethereum-cdap/cohort-zero/issues/64

Why not start with project 1) and if enough capacity after project 1) is completed, continue with project
2). One of the benefits of this is that a POC prototype will be implemented in a 
client for tracking code chunks. Then we can use the same modified client to also analyse 
the new gas schedules by tracing the transactions, similar to [this analysis](https://notes.ethereum.org/@ipsilon/code-chunk-cost-analysis).

Notes:

Gas is charged in three cases: 1) fee intrinsic to the computation of the operation (Appendix G) 2) message call or contract creation CREATE, CREATE2, CALL and CALLCODE. 3) increase in the usage of memory
 
EVM gas cost Appendix H

#### Questions

To Do

#### Actions

1. Explore transaction tracing APIs.

#### Links

Code chunk cost analysis:
https://notes.ethereum.org/@ipsilon/code-chunk-cost-analysis

# Portal Network Notes

Those notes are based on my current understanding of the network.

### Definitions
- `archive node`
Full sync node, stores the full archive state
- `state network`
An idea for how we could distribute the state among across all the nodes in a DHT and make it discoverable and retrievable on demand.
- `tx gossip`
P2P network functionality that facilitates distributing new transactions to all nodes in the network.
- `chain history`
Combines block history, transactions receipt history, state history
- `exclusion proofs`
Shows a leaf is not present in a trie root
- `garbage collection in portal clients`
Allowing nodes to discard old trie data

## Resources

### Stateless Ethereum
Specifications for the Stateless Ethereum research effort
- https://github.com/ethereum/stateless-ethereum-specs

An updated roadmap for Stateless Ethereum
- https://ethresear.ch/t/an-updated-roadmap-for-stateless-ethereum/9046

Why it is so important to go stateless
- https://dankradfeist.de/ethereum/2021/02/14/why-stateless.html

The Data Availability Problem under Stateless Ethereum
- https://ethresear.ch/t/the-data-availability-problem-under-stateless-ethereum/6973

#### 1. Portal network

Weekly Network Research Call Notes
- https://notes.ethereum.org/kva0bUSARI-I2VLx4eb6mw?view

Ethereum portal client in rust
- https://github.com/carver/trin/

State Network DHT - Development Update #2
- https://ethresear.ch/t/state-network-dht-development-update-2/9005

Doing Stateless Ethereum Right
- https://notes.ethereum.org/mSOAdx_XT02MEqrt0f2CPA

The winding road to functional light clients

- part 1: 
https://snakecharmers.ethereum.org/the-winding-road-to-functional-light-clients/
- part 2:
https://snakecharmers.ethereum.org/the-winding-road-to-functional-light-clients-part-2/
- part 3:
https://snakecharmers.ethereum.org/the-winding-road-to-functional-light-clients-part-3/

Testing Portal Network with trin & ddht
- https://gist.github.com/carver/f3c26d9adb28ea2b4e5d5bbe42b4f4b7

#### 2. Gossip
Scalable gossip for state network
- https://ethresear.ch/t/scalable-gossip-for-state-network/8958

Scalable Transaction Gossip
- https://ethresear.ch/t/scalable-transaction-gossip/8660

#### 3. Witnesses

Ethereum Witness Protocol (wit)
- https://github.com/ethereum/devp2p/blob/master/caps/wit.md

Redesign how witness gas costs work
- https://notes.ethereum.org/@vbuterin/witness_gas_cost_2

#### 4. Beam Sync

Intro to Beam Sync
https://medium.com/@jason.carver/intro-to-beam-sync-a0fd168be14a

Beam Sync in 80 Seconds, Using Meta-Witnesses
https://snakecharmers.ethereum.org/beam-sync-in-80-seconds-using-meta-witnesses/