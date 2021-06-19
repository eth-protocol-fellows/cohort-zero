# Project ideas

## Getting rid of SELFDESTRUCT

There is a wish to remove or neuter the `SELFDESTRUCT` opcode. See [here](https://hackmd.io/@vbuterin/selfdestruct) for context. The reasons for getting rid of `SELFDESTRUCT` can be summarized as follows:

* `SELFDESTRUCT` is not needed after [state expiry](https://hackmd.io/@vbuterin/state_expiry_paths). One of the reasons for having the ability to `SELFDESTRUCT` was to be able to remove state, but this is handled automatically with state expiry.
* `SELFDESTRUCT` is the only opcode which causes an unbounded number of state objects to be altered in a single block. This operation is especially costly after migrating the state trie to a [Verkle trie](https://ethereum-magicians.org/t/proposed-verkle-tree-scheme-for-ethereum-state).
* `SELFDESTRUCT` is the only opcode which can cause the code of a contract to change.
* `SELFDESTRUCT` is the only opcode which can change other accounts’ balances without their consent.

The aim of this project is to

1. Do a comprehensive analysis on the contracts deployed on mainnet to identify where and how `SELFDESTRUCT` is used.
2. Evaluate the different ways to get rid of `SELFDESTRUCT` with the above analysis in mind, and try to find out how the existing contracts may be affected.

### Analysis of existing uses of `SELFDESTRUCT`

Sub-tasks:
* Extract the contract code of all contracts on Mainnet. Can use [Ethereum ETL](https://ethereum-etl.readthedocs.io/) for this.
* Exclude all contracts that we can prove to be non-selfdestructable. We can prove that a contract is non-selfdestructable if
  1. the contract code does not contain the `SELFDESTRUCT`, `DELEGATECALL` or `CALLCODE` opcodes, or
  2. the contract code does not contain `SELFDESTRUCT`, and the set of contracts that are called with `DELEGATECALL` or `CALLCODE` can be determined statically, and are proven to be non-selfdestructable.
* Use static analysis on the remaining contracts to classify the different ways `SELFDESTRUCT` are used. Possible things to check for are:
  1. Under which conditions are `SELFDESTRUCT` executed?
  2. Who is the receiver of the contract balance?
* Identify upgradeable contracts, which are selfdestructable contracts that were deployed with `CREATE2` by another contract that allows new deployments at the same address.

# Reading material

### State expiry / statelessness
* [State size management](https://hackmd.io/@vbuterin/state_size_management)
* [Resurrection-conflict-minimized state bounding, take 2](https://ethresear.ch/t/resurrection-conflict-minimized-state-bounding-take-2/8739)
* [State expiry paths](https://hackmd.io/@vbuterin/state_expiry_paths)
* [Witness gas cost 2](https://notes.ethereum.org/@vbuterin/witness_gas_cost_2)
* [Why it's so important to go stateless](https://dankradfeist.de/ethereum/2021/02/14/why-stateless.html)
* [The Portal Network](https://github.com/ethereum/stateless-ethereum-specs/blob/master/portal-network.md)
* [Doing Stateless Ethereum Right](https://notes.ethereum.org/mSOAdx_XT02MEqrt0f2CPA)

### KZG (Kate) commitments and Verkle tries
* [Kate polynomial commitments](https://dankradfeist.de/ethereum/2020/06/16/kate-polynomial-commitments.html)
* [Kate commitments in ETH](https://hackmd.io/yqfI6OPlRZizv9yPaD-8IQ)
* [Verkle trie for Eth1 state](https://notes.ethereum.org/_N1mutVERDKtqGIEYc-Flw)
* [Proposed Verkle tree scheme for Ethereum state](https://ethereum-magicians.org/t/proposed-verkle-tree-scheme-for-ethereum-state)

Implementations:
* https://github.com/gballet/go-verkle
* https://github.com/crate-crypto/rust-verkle
* https://github.com/ethereum/research/tree/master/verkle_trie

### ASE (Address Space Extension)
* [Increasing address size from 20 to 32 bytes (Original post)](https://ethereum-magicians.org/t/increasing-address-size-from-20-to-32-bytes/5485)
* [ASE (Address Space Extension) with Translation Map (Follow-up)](https://notes.ethereum.org/@ipsilon/address-space-extension-exploration)

### Other
* [Pragmatic destruction of SELFDESTRUCT](https://hackmd.io/@vbuterin/selfdestruct)
