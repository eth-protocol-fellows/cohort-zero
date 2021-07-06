# Ethereum Portal Network

## Stateless Ethereum Clients

A stateless client is a client that can perform block validation without any state. Stateless clients are possible with block witnesses, proofs for the state values referenced by the execution of a block. Effectively, a block witness is a minimal, portable, and verifiable database that only contains the subset of state necessary to execute the respective block.

Statelessness lowers disk requirements at the cost of higher bandwidth requirements. For stateless clients to be viable, the size of the witnesses must be sufficiently low. Otherwise, the network would not be able to reliably propagate witnesses within the block time.

The Ethereum developers and researchers have made meaningful progress toward sufficiently small witnesses, but several other aspects about the roadmap to stateless clients remain unresolved.

A stateless client requires a separate gossip network to propagate block witnesses, because without state and history, a client cannot participate in the existing `eth` protocol. Even with a special gossip network for block witnesses, a stateless client is particularly limited. It cannot construct transactions or participate in transaction gossip, and notably, it cannot serve the majority of the JSON-RPC API.

One motivation for stateless clients is the decentralization of the Ethereum network through lower barriers to entry. Stateless clients are fundamentally "lighter" than a full node that holds the entirety of the Ethereum state and history. If you want to interact with the Ethereum network today, you can either operate a full node and join the network, or you can rely on a centralized provider. This binary lies at the extremes of the "light" versus "heavy" and decentralized versus centralized spectrums.

If stateless clients are dysfunctional from the perspective of most actors, in the sense that stateless clients cannot provide the data of interest, then we are left with this binary decision. When faced with the decision between these two options, we expect most actors to choose the simpler option and rely on a centralized provider. Thus, the scope of the stateless client effort should expand to the extent necessary to deliver stateless clients that are both "lightweight" and "functional." A lightweight client is a client with minimal hardware requirements. A functional client is a client that can participate in gossip networks and serve the standard JSON-RPC API. To differentiate between the minimal stateless client and the stateless client we desire, we define a "practical stateless client" as a stateless client that satisfies both lightweight and functional properties.

At minimum, a stateless client requires a separate gossip network to receive block witnesses. A practical stateless client necessarily requires additional infrastructure. A practical stateless client should be able to participate in gossip networks without full state and full history.  Those gossip networks should propagate transactions, blocks, and block witnesses. To serve the JSON-RPC API, a practical stateless client requires on-demand access to state and history. The architecture necessary to support practical stateless clients is fundamentally different from the architecture that supports existing clients, and we should not expect the client teams to make such fundamental changes to their existing implementations.

## Ethereum Protocol Monolith

The Ethereum wire protocol, or `eth`, facilitates the exchange of Ethereum blockchain information among nodes in a P2P network. The protocol assumes that all participating nodes are "full" nodes, meaning that each node holds the full world state and chain history. The protocol is monolithic in the sense that it is responsible for all Ethereum data. Nodes must propagate new transactions and blocks as well as serve all state and history. The homogeneity of the network makes it easier to reason about, but the ongoing growth of Ethereum continuously raises the barriers to entry to operate a full node.

## Light Ethereum Subprotocol

The Light Ethereum Subprotocol, or `les`, is a protocol for "light" clients. The `les` protocol is intended to enable resource-constrained devices to securely access the Ethereum blockchain. A light client downloads block headers as they appear. For all other data, a light client requests it on-demand from a `les` server. A node may participate as a server node in the `les` protocol and as a full node in the `eth` protocol. In order to distribute its finite capacity, a server node limits how much data it serves per unit time to each light client. A light client who exceeds its limit will suffer throttling or disconnection.

If there are too many light clients for some given amount of server nodes, then the light clients will overwhelm the server nodes, and the network will become unavailable. In the `eth` protocol, each node consumes and provides network capacity, so there is balance. In the `les` protocol, a light client only consumes network capacity. Thus, each additional light client diminishes the performance of the network. The capacity of the `les` protocol network is a common resource and subject to tragedy of the commons problems.

## Portal Network

The Portal Network is an effort to enable resource-constrained devices to access and support the `eth` protocol. The term "portal" indicates that the network provides a view into the protocol, but exists separately. The network will be a composition of one or more decentralized P2P networks. Together, the network will provide the data necessary to expose the standard JSON-RPC API.

Without additional infrastructure, a stateless client cannot participate in the `eth` protocol. Meanwhile, the `les` protocol is unable to support large numbers of light clients due to its client-server architecture. The Portal Network consists of infrastructure to support practical stateless clients and allow resource-constrained devices to contribute what they can to the network.

Once stateless clients become available, we expect the majority of nodes to be stateless. There will be some actors with economic incentives to continue to operate full nodes (e.g. miners, infrastructure providers), but we cannot rely on a small number of full nodes to support a large number of stateless clients. If we have asymmetric protocol participation, then the situation devolves to the situation in the `les` network. If we rely on economic incentives, such that nodes must maintain a balance to pay for their requests, then we raise the barriers to entry and lower adoption. The Portal Network attempts to address these issues so that stateless nodes can exist alongside full nodes without tragedy of the commons or free rider problems.


### Transactions

We noted above that a stateless client cannot participate in transaction gossip. Stateless clients do not hold the data necessary to participate in the `eth` protocol, so it cannot participate in transaction gossip. In order to participate in transaction gossip, a node must be able to validate the transaction before propagating it to its peers. For stateless clients, transaction messages would require accompanying witnesses (for account balances and nonces) so that the client could reason about whether a transaction is valid. Another item of concern is that a lightweight node may be unable to maintain the entire transaction pool. The proposed transaction gossip network gives nodes a mechanism to express how much of the transaction pool they would like to maintain.

Each node in the network has a `node_id`, and each transaction has a `tx_hash`. Define the distance between a node and a transaction as the absolute value of `node_id - tx_hash` where `node_id` and `tx_hash` are interpreted as integers. Each node publishes a `radius` to express the sphere of transactions that it is interested in. A node only propagates a transaction to its neighbor if the distance between the neighbor and the transaction falls within the neighbor's `radius`. A node should prefer to establish connections with nodes whose sphere of interest overlaps with its own and nodes with similar `radius` values.

If you connect to a node whose sphere of interest overlaps with your own, then that connection will receive and propagate transactions of interest to you. If you connect to a node whose `radius` value is similar to yours, then you should be able to support the volume of transactions that you must propagate to it. Weaker nodes with small `radius` can connect to other small `radius` nodes without taking on network responsibilities that are too demanding.

The peer connection preferences would lead to a network topology with a "core" of high `radius` nodes and a "periphery" of low `radius` nodes. A transaction that originates from a low `radius` node would take more time to reach a high `radius` node because it would likely require several intermediate hops between other lower `radius` nodes.

Can the network tolerate misbehavior? A low `radius` node will be more vulnerable to DoS attacks, because those nodes may lack the visibility necessary to detect spam transaction messages. Also, a low `radius` node may be unable to update the transaction witnesses if those witnesses become out of date. However, the core of the network, made up of high `radius` nodes, should not be vulnerable to these issues due to their greater visibility. Another type of misbehavior occurs when a node publishes a high `radius`, so that it can attract connections from nodes in the core, but does not support its entire `radius`. This behavior should be easy to detect and does not present much of a concern.


### State

TODO

### History

TODO
