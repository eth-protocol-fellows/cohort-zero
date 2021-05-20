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