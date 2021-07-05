# Ethereum Portal Network

## Stateless Ethereum Clients

A stateless client is a client that can perform block validation without any state. Stateless clients are possible with block witnesses, proofs for the state values referenced by the execution of a block. Effectively, a block witness is a minimal, portable, and verifiable database that only contains the subset of state necessary to execute the respective block.

Statelessness lowers disk requirements at the cost of higher bandwidth requirements. For stateless clients to be viable, the size of the witnesses must be sufficiently low. Otherwise, the network would not be able to reliably propagate witnesses within the block time.

The Ethereum developers and researchers have made meaningful progress toward sufficiently small witnesses, but several other aspects about the roadmap to stateless clients remain unresolved.

A stateless client requires a separate gossip network to propagate block witnesses, because without state and history, a client cannot participate in the existing `ETH` protocol. Even with a special gossip network for block witnesses, a stateless client is particularly limited. It cannot construct transactions or participate in transaction gossip, and notably, it cannot serve the majority of the JSON-RPC API.

One motivation for stateless clients is the decentralization of the Ethereum network through lower barriers to entry. Stateless clients are fundamentally "lighter" than a full node that holds the entirety of the Ethereum state and history. If you want to interact with the Ethereum network today, you can either operate a full node and join the network, or you can rely on a centralized provider. This binary lies at the extremes of the "light" versus "heavy" and decentralized versus centralized spectrums.

If stateless clients are dysfunctional from the perspective of most actors, in the sense that stateless clients cannot provide the data of interest, then we are left with this binary decision. When faced with the decision between these two options, we expect most actors to choose the simpler option and rely on a centralized provider. Thus, the scope of the stateless client effort should expand to the extent necessary to deliver stateless clients that are both "lightweight" and "functional." A lightweight client is a client with minimal hardware requirements. A functional client is a client that can participate in gossip networks and serve the standard JSON-RPC API. To differentiate between the minimal stateless client and the stateless client we desire, we define a "proper stateless client" as a stateless client that satisfies both lightweight and functional properties.

At minimum, a stateless client requires a separate gossip network to receive block witnesses. A proper stateless client necessarily requires additional infrastructure. A proper stateless client should be able to participate in gossip networks without full state and full history.  Those gossip networks should propagate transactions, blocks, and block witnesses. To serve the JSON-RPC API, a proper stateless client requires on-demand access to state and history. The architecture necessary to support proper stateless clients is fundamentally different from the architecture that supports existing clients, and we should not expect the client teams to make such fundamental changes to their existing implementations.

## Ethereum (ETH) Protocol Monolith

TODO

### LES

## Portal Network

TODO

### Transactions

### State

### History
