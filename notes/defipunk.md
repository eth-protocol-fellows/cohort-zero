# Collection of potentially interesting ideas/projects

## Portal Network
* Enable light clients for resource constrained devices.
* DevP2P LES network: 
	* Client/Server
	* Scaling needs more server
* Portal Network:
	* P2P 
	* Adding new clients increases capacity
* Tightly coupled with stateless Ethereum
	* "Two types" of light clients: validators or user-facing


## Gas Limit Coordination

The current method of setting gas limits is not working as intended. It's not really a free movement of the gas limit, but the miners are coordinating to set changes in gas limits.
Generally, the incentives for miners are not always the best incentives for the network.

## Others

* SELFDESTRUCT: See [Albus' notes](./albus.md) for more info. TLDR: SELFDESTRUCT is not needed with state expiry and it has several suboptimal properties (unlimited accesses, violates invariants) 
* Address Space Extension
* to be continued and extended


# Reading material

## Portal Network
* [Portal Network Description](https://github.com/ethereum/stateless-ethereum-specs/blob/master/portal-network.md)
	* More links at end of document
* [Scalable Gossip for State Network](https://ethresear.ch/t/scalable-gossip-for-state-network/8958/)
* [Scalable Transaction Gossip](https://ethresear.ch/t/scalable-transaction-gossip/8660)
* [Gossip Protocol Notes](https://notes.ethereum.org/YtI7e-R5RSK13cklq8j3Ug?both)
* Also see the stateless Ethereum links

## Gas Limit Coordination
* [Original Proposal: "A third way: Coordinating the gas limit"](https://ethresear.ch/t/a-third-way-coordinating-the-gas-limit/9297)
* [Mitigated attack in Berlin](https://blog.ethereum.org/2021/05/18/eth_state_problems/)

### State expiry / statelessness
Partially populated from Albus' list
* [Why Stateless](https://dankradfeist.de/ethereum/2021/02/14/why-stateless.html)
* Stateless Ethereum Roadmap: [Original](https://ethresear.ch/t/complete-revamp-of-the-stateless-ethereum-roadmap/8592) | [Updated](https://notes.ethereum.org/mSOAdx_XT02MEqrt0f2CPA)
* Piper's series of posts / The Winding Road to Functional Light Clients: 
	[1](https://snakecharmers.ethereum.org/the-winding-road-to-functional-light-clients/) 
	[2](https://snakecharmers.ethereum.org/the-winding-road-to-functional-light-clients-part-2/)
	[3](https://snakecharmers.ethereum.org/the-winding-road-to-functional-light-clients-part-3/)
* [Data Availability Problem] (https://ethresear.ch/t/the-data-availability-problem-under-stateless-ethereum/6973)
* [Doing Stateless Ethereum Right](https://notes.ethereum.org/mSOAdx_XT02MEqrt0f2CPA)
* [State expiry paths](https://hackmd.io/@vbuterin/state_expiry_paths)
	* Expire state (use witnesses revive old state)
	* Weak statelessness (only block proposers require state)

### ASE (Address Space Extension)
* [Increasing address size from 20 to 32 bytes (Original post)](https://ethereum-magicians.org/t/increasing-address-size-from-20-to-32-bytes/5485)

### SELFDESTRUCT
* [Pragmatic destruction of SELFDESTRUCT](https://hackmd.io/@vbuterin/selfdestruct)
