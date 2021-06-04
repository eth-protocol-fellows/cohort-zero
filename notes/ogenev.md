# Ognyan's topics of interest and links

## Stateless Ethereum
Specifications for the Stateless Ethereum research effort
- https://github.com/ethereum/stateless-ethereum-specs

An updated roadmap for Stateless Ethereum
- https://ethresear.ch/t/an-updated-roadmap-for-stateless-ethereum/9046

Why it is so important to go stateless
- https://dankradfeist.de/ethereum/2021/02/14/why-stateless.html

The Data Availability Problem under Stateless Ethereum
- https://ethresear.ch/t/the-data-availability-problem-under-stateless-ethereum/6973

### 1. Portal network
Ethereum portal client
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

### 2. Gossip
Scalable gossip for state network
- https://ethresear.ch/t/scalable-gossip-for-state-network/8958

Scalable Transaction Gossip
- https://ethresear.ch/t/scalable-transaction-gossip/8660

### 3. Witnesses

Ethereum Witness Protocol (wit)
- https://github.com/ethereum/devp2p/blob/master/caps/wit.md

Redesign how witness gas costs work
- https://notes.ethereum.org/@vbuterin/witness_gas_cost_2

### 4. Beam Sync

Intro to Beam Sync
https://medium.com/@jason.carver/intro-to-beam-sync-a0fd168be14a

Beam Sync in 80 Seconds, Using Meta-Witnesses
https://snakecharmers.ethereum.org/beam-sync-in-80-seconds-using-meta-witnesses/

## State Expiry

A Theory of Ethereum State Size Management
- https://hackmd.io/@vbuterin/state_size_management

A few paths to statelessness and state expiry
- https://hackmd.io/@vbuterin/state_expiry_paths

Alternative bounded-state-friendly address scheme
- https://ethresear.ch/t/alternative-bounded-state-friendly-address-scheme/8602

Resurrection-conflict-minimized state bounding, take 2
- https://ethresear.ch/t/resurrection-conflict-minimized-state-bounding-take-2/8739

### ReGenesis

ReGenesis Explained:
- https://medium.com/@mandrigin/regenesis-explained-97540f457807

ReGenesis - resetting Ethereum to reduce the burden of large blockchain and state:
- https://ethresear.ch/t/regenesis-resetting-ethereum-to-reduce-the-burden-of-large-blockchain-and-state/7582

ReGenesis Plan:
- https://ledgerwatch.github.io/regenesis_plan.html

Regenesis Phase0:
https://ethresear.ch/t/regenesis-phase0/8627


## Problems to solve

1. Alexandria DHT - current roadmap?
2. Client prototypes for both state and chain history networks
3. Client prototypes for gossip network? What is the current state of the specifications of the gossip network?
4. Writing tests for block withess specifications ? Are specifications still in progress?
5. Prototype for “bridge” from ETH DevP2P network

## Questions

1. What is the current roadmap and goals for `trin`?
2. As some of the information online looks outdated and everything moves in a fast pace, what is the latest developement updates on "portal network" and lightweight stateless clients?
