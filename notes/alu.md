# Stateless Ethereum

Stateless Ethereum is **not** a solution for the following:
- stateless block mining
- [dynamic state access](https://github.com/ethereum/stateless-ethereum-specs/wiki/Glossary#dynamic-state-access-dsa): caller cannot accurately predict what state would be touched
- submitting witnesses along with each transaction for the purpose of execution

## Problem Statement

Ethereum state size is growing too quickly.
- 35GB for state
- 100GB if include all merkle proofs
- increasing by half that amount each year
- *participants pay a one time cost to burden consensus nodes forever*
- this affects scalability + sustainability

## Important Milestones

1. Witness Implementation
2. State Expiry
3. Portal Network
4. Block Level Access Lists
5. [Extending Address from 20 to 32 bytes](https://ethereum-magicians.org/t/increasing-address-size-from-20-to-32-bytes/5485)
## General Roadmap and Overview

1. [The Witness](https://github.com/ethereum/stateless-ethereum-specs/wiki/Glossary#witnesses)

**Goal**: validators can validate blocks without the burden of state 

There are 2 types of witnesses: 
1. Block Witness: provides all data needed to perform block execution
2. Transaction Witness: provides all data needed to perform EVM execution for a specific transaction

**How**: clients use witnesses to validate resulting state root after block execution

**Problem A**: small witness size
Solution: [Verkle Tries](https://notes.ethereum.org/nrQqhVpQRi6acQckwm1Ryg)
- VT reduce proof overhead to constant size: 800k upper bound, average witness size of 200k based off gas limit of 12.5 million
- pending removal of `SELFDESTRUCT` opcode

**Problem B**: availability of witnesses
Solution: witnesses become a part of the protocol
- enforce witnesses in access lists in block header, anyone who receives this proof can verify its correctness
- further research: production of witnesses and availability over gossip

2. State Expiry

- state will become "inactive" after a period of time (~12 months)
- any interactions with inactive state require a witness to become active again
- reduces state to constant 20-50GB

milestones:
1. [regenesis-like epoch-based expiry scheme](https://ethresear.ch/t/resurrection-conflict-minimized-state-bounding-take-2/8739)
- an epoch of roughly 1 year in length
- extended address scheme: tuple (e,s) where e is an epoch and s is a subcoordinate
- state is a list of state trees (S1, S2, S3...) where during each epoch, only Se can be modified
    - old trees are frozen and only accessible via witnesses
    - full nodes only store state roots
    - block produce nodes store S(e-1) for easy witness production (users only need to produce witnesses for epochs > -2)
- accounts can be accessed in any epoch >= creation epoch, and always stored in tree at position hash(e,s)
- account editing rules:
    - in epoch e: direct account (e,s) modification thru tree Se
    - in epoch f (f > e): direct accoount (e,s) modification thru tree Sf
    - account creation of (e,s) during epoch f must provide witness of activity absense in Se...S(f-1)
    - account (e,s) modified during epoch f > e, not in tree Sf, in Se', must provide witness of state in trees Se...Se' and absence of state in trees S(e'+1)...Sf
- in-depth scenarios in article! thanks v
- adding storage slots: 
    - storage slots must be treated as independent accounts
    - SSTORE and SLOAD opcodes modified to add epoch parameter
    - each account would need separate storage tree for each epoch
    - 

3. Portal Clients

- current devp2p eth protocol is not well suited for stateless clients, and cannot be easily modified to do so
- solution: build a networking infrastructure necessary to support widely deployed ultra-lightweight portal clients 
- portal: clients *view* the network and accompanying data, but don't meaningfully participate in the protocol
- participation includes:
    - retrieve arbitrary state on demand
    - retrieve arbitrary chain history on demand
    - participate in transaction gossip without access to state
    - participate in block gossip without implicit requirements imposed by devp2p


- what is gossip in p2p networks?
    - transaction gossip: distributes new transactions to all nodes in network; depends on [transaction validation](https://github.com/ethereum/stateless-ethereum-specs/wiki/Glossary#transaction-validation)
    - block gossip: distributes new blocks; depends on [block validation](https://github.com/ethereum/stateless-ethereum-specs/wiki/Glossary#Block-Validation)

## Resources

### General Statelessness

March 2021: [Updated Roadmap For Stateless Ethereum](https://ethresear.ch/t/an-updated-roadmap-for-stateless-ethereum/9046)

February 2021: [Importance of Statelessness](https://dankradfeist.de/ethereum/2021/02/14/why-stateless.html)

January 2021: 

[ETH 1.x Glossary](https://github.com/ethereum/stateless-ethereum-specs/wiki/Glossary#Witness)

[Complete revamp of the stateless ethereum roadmap](https://ethresear.ch/t/complete-revamp-of-the-stateless-ethereum-roadmap/8592)

### Verkle Tries

June 2021: [Verkle Tree Scheme for Etherem State](https://ethereum-magicians.org/t/proposed-verkle-tree-scheme-for-ethereum-state/5805)


March 2021: [Verkle Multiproofs](https://notes.ethereum.org/nrQqhVpQRi6acQckwm1Ryg)

February 2021: [Verkle Trie for Eth1 State](https://notes.ethereum.org/_N1mutVERDKtqGIEYc-Flw)


### State Expiry

March 2021: [A few paths to statelessness and state expiry](https://hackmd.io/@vbuterin/state_expiry_paths)

February 2021: [Resurrection-conflict-minimized state bounding](https://ethresear.ch/t/resurrection-conflict-minimized-state-bounding-take-2/8739)

[A Theory of Ethereum State Size Management](https://hackmd.io/@vbuterin/state_size_management)


## Extended Address Scheme

February 2021: [Increasing Address Size from 20 to 32 bytes](https://ethereum-magicians.org/t/increasing-address-size-from-20-to-32-bytes/5485)
