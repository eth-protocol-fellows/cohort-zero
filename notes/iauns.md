# Scope

These notes primarily focus on Ethereum 1.x and Python/C++. In a [2019 blogpost](https://medium.com/@pipermerriam/stateless-clients-a-new-direction-for-ethereum-1-x-e70d30dc27aa) Piper discusses the history of Eth1.x and Eth 2. The gist being that work and research is needed for Eth 1.x.

Under the umbrella of ETH 1.x we'll further refine the focus of these notes to Python and C++.

These notes are a work-in-progress. At present, you'll find primarily reference links.

# ETH 1.x Projects

## Stateless Clients

Within ETH 1.x there's talk about a light/stateless client. As far back as November 2015 Vitalik mentioned using the Hexary Trie for a [light client](https://youtu.be/gjwr-7PgpN8?t=1960). Piper's 2019 blog post above also mentions 'beam sync' and stateless clients. Stateless clients appear to be a major theme in 1.x development.

A good starting point for learning about stateless clients are the [stateless client specs](https://github.com/ethereum/stateless-ethereum-specs). I've also found the [glossary](https://github.com/ethereum/stateless-ethereum-specs/wiki/Glossary) useful.

Here's a collection of links I've accumulated on the subject.

### Ethereum 1.x
  * https://blog.ethereum.org/2019/12/30/eth1x-files-state-of-stateless-ethereum/
  * https://medium.com/@akhounov/the-shades-of-statefulness-in-ethereum-nodes-697b0f88cd04
  * https://ethresear.ch/t/the-stateless-client-concept/172
  * https://ethresear.ch/t/complete-revamp-of-the-stateless-ethereum-roadmap/8592
  * https://ethresear.ch/t/complete-revamp-of-the-stateless-ethereum-roadmap/8592/3
  * https://notes.ethereum.org/mSOAdx_XT02MEqrt0f2CPA
  * https://github.com/ethereum/stateless-ethereum-specs/blob/master/portal-network.md
  * https://snakecharmers.ethereum.org/the-winding-road-to-functional-light-clients/
  * https://snakecharmers.ethereum.org/the-winding-road-to-functional-light-clients-part-2/
  * https://snakecharmers.ethereum.org/the-winding-road-to-functional-light-clients-part-3/
  * https://github.com/ethereum-cdap/cohort-zero/blob/main/notes/milan.md
  * https://github.com/ethereum-cdap/cohort-zero/blob/main/notes/ogenev.md

### Ethereum 2.0
  * https://github.com/ethereum/eth2.0-specs/blob/dev/specs/altair/sync-protocol.md

## Ethereum Virtual Machine

### References
  * Tool for assembly / dissassembly. https://github.com/quilt/etk
  * https://github.com/pirapira/awesome-ethereum-virtual-machine

# Ecosystems

## C++ Ecosystem

  * https://github.com/ethereum-mining/ethminer
  * https://github.com/ethereum/aleth
  * https://github.com/ethereum/evmc
    * https://github.com/ethereum/aleth/tree/master/libaleth-interpreter
  * https://github.com/ethereum/evmone
  * https://github.com/ethereum/solidity
    * https://ethereum.stackexchange.com/questions/93916/adding-new-custom-opcode-to-solidity
  * ETC labs and not directly related to ETH 1.x: https://github.com/etclabscore/evm_llvm
    * https://lists.llvm.org/pipermail/llvm-dev/2019-March/131334.html
  * Transferring to ewasm for ETH2 https://ewasm.readthedocs.io/en/mkdocs/README/
    * This is ethereum flavored web assembly
    * https://github.com/ewasm/hera
    * https://github.com/ewasm/evm2wasm
    * https://arxiv.org/ftp/arxiv/papers/2007/2007.15510.pdf
  * https://github.com/microsoft/eEVM

## Python Ecosystem

  * https://medium.com/@pipermerriam/the-python-ethereum-ecosystem-101bd9ba4de7
  * [Trinity](https://github.com/ethereum/trinity)
  * https://discord.com/channels/809793915578089484/809793916077342752
  * https://github.com/ethereum/py-evm
  * https://ethereum.org/en/developers/docs/programming-languages/python/
  * https://github.com/ethereum/web3.py

# Implementations

## RLP

### Aleth Implementation (C++)

* https://github.com/ethereum/aleth
  * Implementation in libdevcore/RLP.h

## Hexary Trie

AKA, Merkle Patricia Trie

### References
  * Intro to how ethereum encodes its trie: https://gist.github.com/ftruzzi/fe6c6735dbbfafa4994985879254a871
  * https://medium.com/@eiki1212/ethereum-state-trie-architecture-explained-a30237009d4e
  * https://en.wikipedia.org/wiki/Merkle_tree
  * https://en.wikipedia.org/wiki/Radix_tree
  * https://medium.com/codechain/modified-merkle-patricia-trie-how-ethereum-saves-a-state-e6d7555078dd
  * https://gist.github.com/ftruzzi/fe6c6735dbbfafa4994985879254a871

### Turbo-geth
  * Uses the path to the leaf node, instead of the hash, as the key. So it can be looked up directly.
  * Uses 'turbo-geth' implementation: https://github.com/ledgerwatch/erigon
  * https://blog.ethereum.org/2019/12/30/eth1x-files-state-of-stateless-ethereum/

# General References (Unsorted)


  * Architecture, https://ethereum.stackexchange.com/questions/268/ethereum-block-architecture
    * https://youtu.be/gjwr-7PgpN8?t=1123
    * https://medium.com/@eiki1212/ethereum-state-trie-architecture-explained-a30237009d4e
  * Yellow paper, https://ethereum.github.io/yellowpaper/paper.pdf
  * Yellow paper cheat sheet, https://github.com/benjaminion/YellowPaper_CheatSheet/blob/master/YPCheatSheet.pdf
  * Yellow paper walkthrough, https://www.lucassaldanha.com/ethereum-yellow-paper-walkthrough-1/
  * Learning solidity, https://dev.to/odyslam/learning-solidity-in-public-58a3
  * Paper on Verkle Trees, https://vitalik.ca/files/misc_files/verkle.pdf
  * Verkle multiproofs, https://notes.ethereum.org/nrQqhVpQRi6acQckwm1Ryg
  * Ethereum research forum, https://ethresear.ch/
  * Ethereum improvement proposals, https://eips.ethereum.org/
  * Stateless Ethereum (?) Glossary, https://github.com/ethereum/stateless-ethereum-specs/wiki/Glossary
  * Kate commitments in ETH, https://hackmd.io/yqfI6OPlRZizv9yPaD-8IQ?view
  * Ethereum Virtual Machine opcodes (EVM), https://ethervm.io/#0B
  * Spin up your own node, https://ethereum.org/en/developers/docs/nodes-and-clients/run-a-node/
  * Gas limits and how gas is controllered, https://youtu.be/gjwr-7PgpN8?t=1000
  * Logs, https://youtu.be/gjwr-7PgpN8?t=1283
  * Uncles, https://youtu.be/gjwr-7PgpN8?t=1843
