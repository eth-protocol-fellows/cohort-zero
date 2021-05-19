# Aaryamann's Notes

---


## Key Areas of interest

- Layer 2 Technologies
- State Channels to prevent state bloat
- Decentralized Staking Pools
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

[Todo]


---
## Decentralized Staking Pools
[Todo]

---
## Underlying Network infrastructure
[Todo]

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