Below is a writeup on Ethereum state expiry. The latest version will be available on my [website](https://jacobkaufmann.com/#/artifacts).

# Ethereum State Expiry

## State and History

State refers to the data necessary to process new blocks via transaction execution, block finalization, and the consensus mechanism. This set of data consists of the account state for all existing accounts along with some other data required for consensus.

History refers to the data from older blocks that is not necessary to process new blocks. EVM execution is unaware of this data, so a node can forget history and maintain the ability to process new blocks.

## State Representation

The Ethereum state, or world state, is a mapping between addresses and account states. An address is a 160-bit identifier for some account, and account state is the data structure that represents some account. Account state objects are serialized so that they are stored as byte arrays. Therefore, the world state is a mapping from byte arrays to byte arrays.

The world state is not stored on the blockchain itself. The protocol assumes that the state is stored by a client in the modified Merkle Patricia tree data structure. The root node of this structure is cryptographically dependent on all data in the levels below, such that the hash of the root represents a secure commitment to the entire state.

The account state comprises the following fields: nonce, balance, storage root, and code hash. An account's storage is a mapping between 256-bit integer values, and this mapping is also encoded in a Merkle Patricia tree structure. An individual value in this mapping is known as a storage slot. The storage root is the 256-bit hash of the root node of this tree.

## State Growth

With each new block, transactions access and mutate the state. State mutations may change a value at some existing storage slot or create entirely new state objects. Transactions that create new state objects enlarge the state. This is known as state growth.

While the gas cost for state-growing operations is high relative to other operations, the cost does not fully internalize the social cost that new state objects impose on the network.
The transaction sender pays a one-time cost, but the cost to the network is eternal. The consensus nodes must store the new state object(s) indefinitely in order to process new blocks. If nodes are unable to forget some state, then the burden on those nodes only increases over time.

Not all state is necessary to process new blocks. Only the subset of the state "touched" by a block is necessary to process that block. Some state objects may never be touched again. Effectively, these untouched state objects are artifacts of history that nodes should be able to forget.
We require some mechanism that allows consensus nodes to forget "inactive" state objects. Without such a mechanism, the state that nodes must maintain will continue to grow unbounded.

A larger state imposes higher performance requirements on the hardware that stores and accesses it. If the cost to satisfy those hardware requirements rises, then the number of actors who choose to run nodes will fall. Thus, a solution to state growth is necessary for decentralization.

## State Expiry

The solution to the state growth problem for Ethereum is known as state expiry. With state expiry, state objects that remain "untouched" for some long period of time expire, and a witness (proof) is required to "resurrect" previously expired state objects.

The active state refers to all unexpired state objects, and the inactive state refers to all expired state objects. Nodes who wish to process or create new blocks only require the active state, because any valid transaction that refers to objects in the inactive state must include the corresponding resurrection witnesses.

## Challenges

### Resurrection Conflicts

What happens if a new state object is created at a location where some expired state object was previously created, and then some actor attempts to resurrect the expired object? This is known as a resurrection conflict.

To prevent resurrection conflicts, you must either make it impossible for an expired state object to resurrect at its previous location, or make it impossible for a new state object to be created at a location where some expired state object previously existed.

## The Latest Proposal

The latest state expiry proposal uses a multi-state-tree construction. Instead of a single state tree, we represent the state with a list of state trees. Each tree in the list corresponds to a distinct state period, where the duration of a period is roughly one year.

At the start of each period, initialize a new, empty state tree. Only the state tree for the current period is mutable, and all other state trees (from earlier periods) are immutable (i.e. read only). Full nodes only store the latest two state trees, so transactions that reference state objects that do not exist in either of those two trees must provide witnesses for those objects.

We introduce the notion of an address period. The initial address period is zero, and this is the address period for all pre-existing addresses. To create accounts with a particular address period, we introduce a new CREATE opcode that takes an address period as an argument. The address period becomes part of an account's tree key so that addresses with different address periods can coexist in the same tree without the possibility of collisions.

Suppose

- `(e,s)` denotes a state object where `e` is a nonnegative integer, and `s` is the address of the object.
    - The address `s` may include a storage key if the state object is a storage slot.
- `(S_0,...,S_n)` denotes the list of state trees where `n` is the current state period.

Recall that only `S_n` is mutable, and that full nodes are only required to keep track of `S_{n-1}` and `S_n`.

Then, we have the following rules for creating and modifying some state object `(e,s)` in period `n`:

- `n=e`
  - Create: Modify `S_n` directly
  - Modify: Modify `S_n` directly
- `n=e+1`
  - Create: Modify `S_n` directly
  - Modify: Modify `S_n` directly
- `n>e+1`
  - Create: The transaction sender must provide a witness to prove that `(e,s)` is not in `S_i` for `i=e,...,n-2`
  - `(e,s)` is in `S_n` or in `S_{n-1}`
    - Modify: Modify `S_n` directly
  - `S_x` is the latest tree that contains `(e,s)` with `e <= x<n-1`
    - Modify: The transaction sender must provide a witness to prove both the state of the object in `S_x` and the absence of the object in `S_i` for `i=x+1,...,n-2`

One additional thing to note: in the new state tree construction, "zero" and "absent" are no longer equivalent. "Zero" means that no object exists, and "absent" means that the object may or may not exist in some older tree. This means that an object from some older tree whose post-transaction value is zero must be written into the latest tree.

### Resurrection Conflicts

Under the proposal, resurrection conflicts are not possible. Since the address period is part of a state object's address, it's not possible to create some new state object at an address where some expired state object was previously created.

The proposal also solves a more subtle resurrection issue. Consider an expired state object that has been expired and subsequently resurrected at least once, such that this object has been active in multiple "lifetimes." If an object could be resurrected to some "checkpoint" that did not correspond to its latest expiry, then that object's state changes from any lifetimes after that checkpoint could disappear. This is why the proposal requires the witness to prove the state or absence of the object in some interval when this situation is possible.

### Prerequisites

This proposal assumes the following prerequsites:

- The implementation of Verkle trees
- The extension of addresses from 20 bytes to 32 bytes
