# CDAP Development Update #2

Date: 5 August 2021

Author: Ying Tong (yingtong@z.cash)

## Progress

### 8-bit word encoding
An EVM word (256 bits) is represented as a linear combination of 32 bytes:
        `encode(word) = b_0 + r * b_1 + ... + r^{31} * b_{31}`,
where `word` is a 256-bit word, b_i's are bytes, and `r` is a random factor.
This helper returns an expression of `encode(word)`.
In the zkevm circuit, this `encode(word)` expression will not be directly
looked up. Instead, it will be folded into the bus mapping lookup.

Merged PR: https://github.com/appliedzkp/zkevm-circuits/pull/21

### `ADD` opcode
Previously, we had planned to use lookup tables to implement byte-wise addition
of two EVM words. However, we realised that this can be handled more directly
by a custom gate.

WIP PR: https://github.com/appliedzkp/zkevm-circuits/pull/15

## Blockers

### Bus mapping
The "bus mapping" data structure must be consistent across the EVM circuit and
the state circuit. It is derived from the execution trace and consists of the
actual operations executed in the block. Further, it is consumed by subcircuits
such as `ADD`, and thus is a blocker.

The contents of the bus mapping will possibly change as we build out the circuits;
I think one way forward is to use an incomplete bus mapping for now to unblock
other circuits, and adapt the struct as needed later on.

WIP PR: https://github.com/appliedzkp/zkevm-circuits/pull/20
