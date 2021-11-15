# Albus Dompeldorius

- email: albus.dompeldorius@gmail.com
- [github](https://github.com/adompeldorius)
- location: Oslo, Norway
- CDAP notes: [`./notes/albus.md`](./../notes/albus.md)

## About Me

I am a mathematician and software developer with a long-time interest in Ethereum. I have a PhD in math, where I did topological data analysis on neural data, and I have worked as a data engineer/scientist at an energy company. Now I want to start working in the Ethereum space.

## My CDAP Project

### `SELFDESTRUCT` analysis

In the first half of my time in the CDAP I was analysing existing usage of the `SELFDESTRUCT` opcode in existing contracts on Ethereum. This opcode is responsible for [a lot of complexities in clients](https://hackmd.io/@vbuterin/selfdestruct), and is currently causing problems when the representation of the Ethereum state is switching from Merkle Patricia tries to Verkle tries. Because of this, the [current proposal for Verkle tries](https://notes.ethereum.org/@vbuterin/verkle_tree_eip#Miscellaneous) requires removing most of the functionalities of `SELFDESTRUCT`.

I analysed how `SELFDESTRUCT` is currently used, and if neutering `SELFDESTRUCT` would lead to breaking changes in existing contracts.

My analysis revealed a [security issue in an existing contract](https://nbviewer.jupyter.org/github/adompeldorius/selfdestruct-analysis/blob/main/analysis.ipynb). Based on this, I wrote a [modification of the Verkle tree specification](https://github.com/adompeldorius/selfdestruct-analysis/blob/main/verkle_trie_with_selfdestruct_eip.md) that supports `SELFDESTRUCT`.

### Springrollup: A zk-rollup where sender can batch unlimited number of transfers with only 6 bytes of calldata

In the second half of my time in the CDAP I worked on a [new zk-rollup](https://github.com/adompeldorius/springrollup) (see also [ethresear.ch post](https://ethresear.ch/t/springrollup-a-zk-rollup-that-allows-a-sender-to-batch-an-unlimited-number-of-transfers-with-only-6-bytes-of-calldata-per-batch/11033)) where a sender can batch an arbitrary number of transfers with only 6 bytes of calldata. I wrote a description of the protocol, and started working on a POC.

## Development Updates

You can read more about the work I did during the program in my development updates:

- [Update 1]: Extracted contract data, and performed some static analysis on `SELFDESTRUCT` usage.
- [Update 2]: More analysis on self-destructed revealed a security issue in Pine Finance if `SELFDESTRUCT` is neutered.
- [Update 3]: Wrote a modified version of the Verkle tree proposal that preserves `SELFDESTRUCT`.
- [Update 4]: Some more analysis on the Pine Finance case and started working on Springrollup.
- [Update 5]: Finished specification of how deposits and withdrawals work in Springrollup.
- [Update 6]: Managed to reduce calldata usage from 8 bytes to 6 bytes per batch, finished first specification, and posted on ethresear.ch. Also started on implementing a POC.
- [Update 7]: Working on implementation and simplified the spec. Found a way to have dynamically sized rollup batches.

[Update 1]: https://hackmd.io/@albus/HJ6EBiTn_
[Update 2]: https://hackmd.io/@albus/rkAbjAsWF
[Update 3]: https://hackmd.io/@albus/rJRfbIRft
[Update 4]: https://hackmd.io/@albus/rJAnxRlVF
[Update 5]: https://hackmd.io/@albus/S1E22SmBF
[Update 6]: https://hackmd.io/@albus/BkADtsrLF
[Update 7]: https://hackmd.io/@albus/H1sDKmdwF

## Job Stuff

- I am not willing to relocate
- I am interested in remote work
- I am open to part-time positions, contract work, and full time positions starting next year
