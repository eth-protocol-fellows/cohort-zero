# Ognyan's topics of interest and links

## Portal Network Notes

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

### Questions
1. Definitions of:
- exclusion proofs
- garbage collection in portal clients

2. Cold vs hot state, is there specific distinction?
- Probably not, based on some experiments done, it looks like the state is touched all 
   over the place.

3. What are differences between ddht python implementation and discv5 rust 
   implementation?
- ddht is actually a prototype of a portal client running on a python implementation of 
   discv5(alexandria), trin is a portal rust client running on discv5 rust 
  implementation? Discv5 is the underlying protocol.

4. *"Current mainnet trie is full for the first few layers. Individual proof sizes for single leaf nodes in the account trie are going to be around 3-4kb"*

- How to check to what extent the trie is full?
Maybe spin a node and check the database...

### Actions

1. Read/implement hexary patricia trie with proofs.
2. Look into gossip prototype in GO - Levi. Re-read on transaction gossip and state network:
https://ethresear.ch/t/scalable-gossip-for-state-network/8958
https://ethresear.ch/t/state-network-dht-development-update-2/9005
https://ethresear.ch/t/an-updated-roadmap-for-stateless-ethereum/9046
3. uTP protocol vs udp. libP2P vs uTP over DiscV5.
https://www.bittorrent.org/beps/bep_0029.html
4. Understand how storing merkle trie nodes with the merkle structure vs flat kv store is done in practice.
5. Run trin with ddht.

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

### State Expiry

A state expiry and statelessness roadmap
- https://notes.ethereum.org/@vbuterin/verkle_and_state_expiry_proposal

Verkle tree EIP
- https://notes.ethereum.org/@vbuterin/verkle_tree_eip

State Expiry EIP
https://notes.ethereum.org/@vbuterin/state_expiry_eip