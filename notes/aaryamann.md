# Aaryamann's Notes

---


## Key Areas of interest

- Layer 2 Technologies
- State Channels to prevent state bloat
- PoS Mechanism
- Underlying Network infrastructure of Ethereum

---


## Layer 2 Technologies
- This is a scaling solution that leverages the security that the Ethereum network provides. Nearly all of the L-2 solutions depend on the mainnet


- Major Types of L-2 Solutions:

	1. Rollups

		- The main types of rollups differ in 2 ways, that are - Data Storage and Computation. Some of the solutions involve data storage off-chain, which leads to a significantly higher TPS

		- The attribute that all rollups have in common is that they settle the transactions on mainnet 

		- Each type of Rollup comes with its own Pros and Cons, and as the ecosystem moves forward, it is imperative that there is a diverse set of solutions that all work toward scaling Ethereum
 
	2. Sidechains

		- Sidechains are independent chains that run parallel to mainnet

		- They are employed if the usecase requires a different consensus algorithm, or block parameters

		- If the block parameters are configured incorrectly, it could lead to unintended consequences such as [empty blocks](#why-empty-blocks-are-bad), state bloat, 51% attacks, etc

		- These Sidechains communicate with mainnet via duplex Bridges that ideally should be operated by existing node-operators to increase the decentralization of the bridge operation

	3. State Channels

		- The concept of State channels is like that of Bitcoin's lightning network, but with added functionality like smart contract interactions

		- Users "lock" in their funds in a multi-sig contract
		- They then perform all their transactions off-chain, by signing transactions regularly

		- When all parties are ready to complete the main transaction, they pay a fee to "unlock" the funds with the new state

		- Hence, even though there might have been more than 10k transactions, only 2 are recorded on mainnet, i.e the initial and final state.

---
## State Channels to prevent state bloat

- One of the requirements of a good payment system involves high throughput of transactions, i.e people should not wait too long for their transaction to go through

- However, the downside to having a high TPS, is that the hardware required to store such a large amount of transactions becomes more expensive and inaccessible to the regular end user

- A consequence of this, directly, is larger node operators start to make a monopoly on node services

- This also results in reduced decentralization and significant portions of the hash rate will be owned by corporations, very much the opposite of what was intended by a decentralized "everyone's network"

- A Proposal could involve having state channels baked into the ethereum core protocol which would come with its own advantages, i.e

	- Tuned performance
	- Would lead to true decentralization of state channels
	- Could be a stepping stone to truly stateless clients as highlighted [here](https://ethresear.ch/t/the-stateless-client-concept/172)


- Much like the recent [EIP-2930](https://eips.ethereum.org/EIPS/eip-2930) that was introduced in the [Berlin Hard Fork](https://blog.ethereum.org/2021/03/08/ethereum-berlin-upgrade-announcement/), an EIP could be introduced that promotes users (that may transact many times) to make use of state channels and slashes gas costs as an incentive

- The above potential EIP could be used in many cases like

	- Governance, where many users transact in a short time with a voting contract

	- IoT Devices that distribute certificates/data to other devices


	- Decentralized Exchanges that depend on the security provided by mainnet and would benefit from reduced gas prices, example - [Raidex](https://raidex.io/)


---
## PoS Mechanism
[Todo]

---
## Underlying Network infrastructure


- The bread and butter for the network's operation is [devp2p](https://github.com/ethereum/devp2p)

- devp2p makes use of a set of network protocols, like libp2p
- Unlike libp2p, devp2p was made for the ethereum network, and could be applied for a few similar use-cases as well

- There are many implementations of the protocol, as highlighted [here](https://github.com/ethereum/devp2p#implementations)

- There are 3 big pieces of devp2p:
	1. The Node Record (What can you tell me about your node?)

		- The node record essentially behaves like a node's business card. It tells you it's name (nodeId), where you can find it (IP(v4 & v6) address), and it's role in the organization (the network)

		- There are multiple reasons as to why it is important to know all details about the node you are connecting to

		- For example, while syncing an archive node, the node would need to connect to other archive nodes to sync state. It can only do this if it knows the type of node on the other end

		- The structure of the node record can be found [here](https://github.com/ethereum/devp2p/blob/master/enr.md#record-structure)

		- The node signs its record and broadcasts it to other nodes to initiate the connection. The process of signing and encoding the node record can be found [here](https://github.com/ethereum/devp2p/blob/master/enr.md#rlp-encoding)


	2. The discovery protocol (How do I find nodes?)

		- Due to the decentralization of routing, each node must maintain its own [DHT](https://docs.ipfs.io/concepts/dht/), that stores node records

		- There are various types of packets that are sent to and fro between nodes to establish and secure the connection between them. Technical spec regarding the packets can be found [here](https://github.com/ethereum/devp2p/blob/master/discv4.md#ping-packet-0x01)

		- The records are stored in a [Kademlia table](#Kademlia-tables) 

		- As messages are exchanged between nodes, buckets that store node Ids is filled up from least-recently seen to most-recently seen

		- These buckets are DoS resistant due to the design of the bucket structure

		- As time progresses, the Nodes DHT starts to fill up with "nearby" nodes, and can easily connect to nodes which are "active"
  
	3. The transport protocol (How do I send information to other nodes?)

		- RLPx is used as the transport protocol to facilitate secure communication between nodes

		- Since each node holds a private key, it is used for signing messages that prove its identity, which is highlighted [here](https://github.com/ethereum/devp2p/blob/master/rlpx.md#initial-handshake)

		- Framing is a method of encapsulating data that is to be communicated. It is primarily used to support multiplexing multiple protocols over one connection. Technical spec [here](https://github.com/ethereum/devp2p/blob/master/rlpx.md#framing)

		- Any message that is sent after the handshake comes with a "capability", which is used to denote the functionality associated with the nodes

		- The multiplexing of connections is done based off the message ID, and each capability is given only as much message id space as it needs

Relevant Links:

1. [RLPx implementation in Rust](https://docs.rs/rlpx/0.4.1/rlpx/)
2. [Devp2p implementation in Rust](https://github.com/rust-ethereum/sentry)
3. [Whisper Protocol](https://geth.ethereum.org/docs/whisper/whisper-overview)

---
### Extras

#### Why empty blocks are bad
- To build on top of the existing chain, nodes need to collect and verify transactions, and the previous block header.
- Many Mining pools take advantage of the fact that they can produce blocks with no transactions
- Blocks require the state root to produce the hash (which is different than Bitcoin)
- This requires nodes to be up to date with the latest blocks
- You can notice that empty blocks are mined directly after a mining pool mines a block with actual transactions
- They do this so as to extend the chain, thereby reducing the chance of their mined block being an uncle block, and hence collect profits
- Unfortunately, this leads to transactions being piled up and eventually drives up gas prices a little bit (which also favors miners)
- An interesting thread realting to this, [here](https://ethresear.ch/t/a-proposal-to-alleviate-the-empty-block-problem/6191/9)

#### Kademlia tables


- My thoughts after reading this [paper](https://pdos.csail.mit.edu/~petar/papers/maymounkov-kademlia-lncs.pdf)
- A Kademlia table is like a binary tree, with each node as a leaf
- The node id (ENR) is used to find the spot of the given node within the tree
- The protocol ensures that each node knows about other nodes that belong to its subtrees, if they exist
- This leads to a quick search for nodes
- An advantage of this protocol, is that it enables nodes to query any node in the network, or multiple nodes, at the same time so that it can find the fastest route to the destination node
- Kademlia proposes the use of XOR to find the "distance" between 2 nodes
- This leads to an efficient check to decide the nodes you should connect to for a quick sync
