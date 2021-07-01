# CDAP Development Update #1

Date: 1 July 2021

Author: Ying Tong (yingtong@z.cash)

## Background
I'm contributing to zkEVM, a project led by the EF's Applied ZKP group. The aim of this project is to verify a valid Ethereum state transition inside a SNARK circuit. This extends the functionality of zkRollups beyond simple transfers, to support Ethereum transactions.

- WIP spec: https://hackmd.io/Hy_nqH4yTOmjjS9nbOArgw
- WIP state proof spec: https://hackmd.io/Nwd0e5AgTVSBWRQlp-Ving
- WIP circuits: https://github.com/appliedzkp/zkevm-circuits

We are writing our circuits using the UltraPLONK arithmetisation as implemented in `halo2`: https://zcash.github.io/halo2/concepts/arithmetization.html

## Progress
### 1. High-level design
> Note: Many parts of the proposed design are still in flux, but the high-level architecture is stable. I document here my high-level understanding.

The validity proof is actually composed of two proofs: the "state proof" and the "EVM proof". These proofs share the same public input: a commitment to the "bus mapping", i.e. the witnessed operations involved in the state transition.

A `global_counter` is initialised for each proof. This counts the number of operations (`read` / `write` / `push` / `pop`) performed in the circuit.

#### a) State proof
The state proof checks that the message calls perform legal reads and writes on memory, storage, and the stack. This includes checking that:
- all opcodes used are legal;
- all addresses accessed are legal;
- a `READ` does not alter the value accessed;

To check whether a value is legal, we pre-load lookup tables of allowed values, and constrain witnessed values to be a subset of these tables.

Operations in the state proof are ordered first by the address they access, and then by `global_counter`.

#### b) EVM proof
The EVM proof checks that the witnessed values correspond to valid operations. This includes checking:

- the business logic of message calls. For example, for a message call involving the opcode `ADD`, the EVM proof checks that the value of the witnessed sum is consistent with the values of the witnessed summand.
- the `program_counter`, which is the byte offset in a smart contract's bytecode, is updated from one operation to another in an expected way. For example, a `PUSHX` should increase the `program_counter` by `++ (1 + X)`; a `JUMP` should set the `program_counter` to the location of the jump.

Operations in the EVM proof are ordered by `global_counter`.

### 2. (PoC) State proof memory subcircuit
This is a partial implementation of the memory subcircuit in the state proof:
https://github.com/appliedzkp/zkevm-circuits/blob/main/src/state_circuit/memory.rs

### 3. Possibilities for inter-operability
The zkEVM circuit is designed with zkRollups in mind; separately, I'm interested in how it can be used for interoperability. For example, two EVM-compatible chains could each verify transactions on the other, without having to re-execute the transactions. This would mean that any contract on Chain A could directly use the outcome of a transaction on Chain B.
