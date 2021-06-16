# Stateless Ethereum

## Problem Statement

### What is state in Ethereum?

State is all the information needed by a node to be able to process new transactions and blocks.

State includes:
- account balances and nonces
- contract code and storage
- consensus-related data (recent block hashes, uncles, proof-of-stake consensus data: validator pubkeys, activity records on beacon chain, etc) <-- this is relatively small

State grows during:
- new account creation
- new contract creation
- first receipt of new tokens
- first participation in contracts

Practically, this results in "Junk State" -- old account/contracts are never used again, but left in the state forever. **This is a fundamental economic imbalance in state management: users pays a one time gas cost for a transaction that imposes permanent ongoing costs to the network.**

State is currently 35GB, and 100GB if you include all merkle proofs. It's increasing by half that amount each year, and is the main reason why we cannot keep increasing the gas limit. 

### What is Stateless Ethereum?

Nodes *verifying* blocks do not need to store the full state. New blocks will come with "witnesses" that prove the values of the state that got accessed and the correctness of the new state root after processing the block. 

This can be achieved in 2 main ways:
1. Weak Statelessness: block *producers* store the full state to generate witnesses for each block that is created 
2. Strong Statelessness: neither block *producers* or *verifiers* need to store the full state; transaction senders are responsible for providing witnesses and storing portions of the state they care about

While strong statelessness is ideal, it requires some incentive protocol for people to maintain state for users that do not run nodes or need to interact with unexpected accounts. Strong statelessness also increases the data bandwidth requirements of the network via access lists--transactions must declare which accounts and storage keys they interact with.

Stateless Ethereum is **not** a solution for the following:
- stateless block mining
- [dynamic state access](https://github.com/ethereum/stateless-ethereum-specs/wiki/Glossary#dynamic-state-access-dsa): caller cannot accurately predict what state would be touched
- submitting witnesses along with each transaction for the purpose of execution

### Solution A: State Expiry

- State must be continually accessed to remain "active", and state that is untouched for a period of time is "inactive" or "expired". 
- State must be renewed explicitly. 
- The creation of new state objects only burden nodes for a limited period of time. 
- Inactive state is never deleted, but must be "resurrected" via a witness. 

There were 4 proposed expiry schemes, and core development has chosen the regenesis expiry scheme detailed below.

### Solution B: Weak Statelessness

Only block proposers are required to store state. All other nodes can verify blocks statelessly.
## Technical Milestones

- Verkle Trie
- Witness Implementation
- Regenesis Expiry Scheme
- Portal Network
- Block Level Access Lists
- [Extending Address from 20 to 32 bytes](https://ethereum-magicians.org/t/increasing-address-size-from-20-to-32-bytes/5485)

## [Witness Implementation](https://github.com/ethereum/stateless-ethereum-specs/wiki/Glossary#witnesses)

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

## Regenesis Expiry Scheme

- state will become "inactive" after a period of time (~12 months)
- any interactions with inactive state require a witness to become active again
- reduces state to constant 20-50GB

### [Regenesis-like Epoch-based Expiry Scheme](https://ethresear.ch/t/resurrection-conflict-minimized-state-bounding-take-2/8739)
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

### Account-level vs Storage-slot-level expiry

Account-level (currently chosen)

The case for storage-slot-level:
- contract accounts have unbounded number of storage slots that arbitrary users can come and increase
- rent fee needs to be proportional to number of storage slots --> user can pay one-time cost to impose permanent ongoiong cost to contract + users
- contracts need to add complex internal logic to pass on cost of storage slot to users or redesign to use CREATE2 for new accounts and use them as storage slots
- contracts that keep using storage slots need an eviction policy for storage a user is not paying for
- weakness: each storage slot must have metadata on expiry date
- resurrection conflicts affect accounts + storage slots

### Removing from tree vs Retiring parts of tree

The "one tree" vs "two tree" technical dichotomy: Do we removed expired state from the main state tree to a separate data structure that contains only expired objects or maintain a single state tree but mark parts as expired?

one tree: each node is marked with an expiry date (both leaf *and* intermediate nodes)

![one tree](https://i.imgur.com/Bw5NvJo.png) 

*active nodes in white, expired nodes in grey*

- benefits: similar tree structure, update expiry-date parameter during expiry and resurrection
- drawbacks:
    - requires tree structure capable of stooring intermediate information in nodes
    - does not extend well to verkle trees
    - additional primitive of merkle proofs: must also stop at intermediate points to prove portion of state is expired

two trees (currently chosen): 

![two trees](https://i.imgur.com/TNE1Tpt.png)

*active state in white, tree containing expired state in grey*

- benefits: dooes not need per-node metadata, works out of box with state accumulators
- drawbacks:
    - requires deeper change of protocol to implement 
    - explicit procedure to expire part of state
    - does not have built-in solution to resurrection conflicts 
- note: expired state does not need to be a tree, can be merkle proof pointing to receipt when an object expired + cryptographic proof not resurrected before or re-expired more recently

### Resurrection Conflicts

An account created at address A expires. A new account gets created at address A via CREATE2. What happens when a resurrection of the original account is attempted?

Solutions
1. explicit account merge procedure 
    - applications must add merging logic
2. eliminate same-address recreation by adding current year to CREATE2 
    - hard for accounts that have not yet been "registered" to accumulate assets
    - imposes a 1 year time limit: if user does not send a transaction, can lose funds
3. add a "stub" to the position (the one tree approach automatically does this)
    - to keep track of N expired state objects, state size is still O(N)
    - tree rot: try to cover more than N accounts 
    ![tree rot](https://i.imgur.com/0xjhLXs.png)
    - to open parts of the tree back up: can use this [scheme](https://ethresear.ch/t/alternative-bounded-state-friendly-address-scheme/8602/13) or keep track of it for witness creation
    - requires explicit data structure for storing and checking ranges (eg tree with node-level data)
4. require new account creations to come with witnesses proving non-prior-expiry (currently chosen)

## Portal Clients

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

## Meetings

### June 16, 2021 
- the plan is to attempt both state expiry and statelessness together
- [roadmap](https://notes.ethereum.org/@vbuterin/verkle_tree_eip): 
    - period 1 hard fork: 
        - 2 state trees: frozen hexary Patricia tree and new Verkle tree
    - address space extension: 
        - more collision resistance (26 bytes for hash)
        - backwards compatible: 32 byte addresses can interact with 20 byte addresses
    - period 2 hard fork: 
        - transition period 0 to a verkle tree, clients only need to store root hash
- [verkle tree eip](https://notes.ethereum.org/@vbuterin/verkle_tree_eip):
    - smaller witness sizes
    - code verklization 
- [state expiry eip](https://notes.ethereum.org/@vbuterin/verkle_tree_eip):
    - prereqs: verkle tree + address expansion

## Resources

### General Statelessness

March 2021: [Updated Roadmap For Stateless Ethereum](https://ethresear.ch/t/an-updated-roadmap-for-stateless-ethereum/9046)

February 2021: [Importance of Statelessness](https://dankradfeist.de/ethereum/2021/02/14/why-stateless.html)

January 2021: 

[ETH 1.x Glossary](https://github.com/ethereum/stateless-ethereum-specs/wiki/Glossary#Witness)

[Complete revamp of the stateless ethereum roadmap](https://ethresear.ch/t/complete-revamp-of-the-stateless-ethereum-roadmap/8592)

### Verkle Tries

June 2021: 

[Verkle Tree Scheme for Etherem State](https://ethereum-magicians.org/t/proposed-verkle-tree-scheme-for-ethereum-state/5805)

[Verkle Tree Spec](https://notes.ethereum.org/4BPLDyGnQ0WY12kVbOuvxw)

[Proposed Verkle Tree Scheme for Ethereum State](https://notes.ethereum.org/_N1mutVERDKtqGIEYc-Flw)

March 2021: [Verkle Multiproofs](https://notes.ethereum.org/nrQqhVpQRi6acQckwm1Ryg)

February 2021: 

[Verkle Trie for Eth1 State](https://notes.ethereum.org/_N1mutVERDKtqGIEYc-Flw)

### State Expiry

June 2021: [State Expiry EIP](https://notes.ethereum.org/@vbuterin/state_expiry_eip)

March 2021: [A few paths to statelessness and state expiry](https://hackmd.io/@vbuterin/state_expiry_paths)

February 2021: [Resurrection-conflict-minimized state bounding](https://ethresear.ch/t/resurrection-conflict-minimized-state-bounding-take-2/8739)

[A Theory of Ethereum State Size Management](https://hackmd.io/@vbuterin/state_size_management)


### Extended Address Scheme

February 2021: [Increasing Address Size from 20 to 32 bytes](https://ethereum-magicians.org/t/increasing-address-size-from-20-to-32-bytes/5485)

January 2021: [Alternative bounded-state-friendly address scheme](https://ethresear.ch/t/alternative-bounded-state-friendly-address-scheme/8602)

### Stateless Clients 

July 2020: [Stateless Client Witnesses](https://www.youtube.com/watch?v=kbcj1TKXaN4)
