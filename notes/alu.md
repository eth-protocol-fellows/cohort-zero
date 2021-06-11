# Stateless Ethereum

Stateless Ethereum is **not** a solution for the following:
- stateless block mining
- [dynamic state access](https://github.com/ethereum/stateless-ethereum-specs/wiki/Glossary#dynamic-state-access-dsa): caller cannot accurately predict what state would be touched
- submitting witnesses along with each transaction for the purpose of execution

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
- any interactions with inactive state require a proof to become active again

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

February 2021: [Resurrection-conflict-minimized state bounding](https://ethresear.ch/t/resurrection-conflict-minimized-state-bounding-take-2/8739)

