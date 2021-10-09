# Note
The apprenticeship program has provided me with an opportunity to dive deeper and enhance my knowledge on the Ethereum and blockchain in general.

There are a couple of topics which I was really curious about that the beginning stage of the program.

## Smart contract

Smart contracts are codes that have been compiled and have its bytecode stored into the blockchain and anywhere that has access to the network is also able to execute bytecode.

Understanding the storage of smart contract especially on how, bytes, string, arrays and mapping have a unique way of storing data in the blockchain

Link:
https://programtheblockchain.com/posts/2018/03/09/understanding-ethereum-smart-contract-storage/

Understanding how to decode for transaction input, meaning of methodId of functions and event logs.

MethodId is the first 4 bytes sha256 hash of the function name + its parameter types. Because its only 4 bytes instead of the full length, methodid collision do exist 

Despite having used this site alot, I recently only realized its made by Piper
https://www.4byte.directory/

Event logs are being emitted by the smart contract and has a 4 indexed topic. In solidity, the topic0 is the full length of sha256 of the event name and its parameter type. We are also able to emit event via the LOG opcode as well and in that case the topic0 can be freely determined by the author of the contract.

5 part series that IMO has a very clear explaining on everything EVM inside out
https://medium.com/@hayeah/diving-into-the-ethereum-vm-6e8d5d2f3c30

https://ethereum.org/en/developers/docs/smart-contracts/anatomy/

The contract data are stored in a hexanary merkle trie with its root being stored in the field of a an account state

https://www.researchgate.net/figure/Ethereum-block-header-and-state-merkle-tree_fig1_330752524

A nice youtube video:
https://www.youtube.com/watch?v=RxL_1AfV7N4


## Ethereum state
Ethereum stores states using the Patricia Merkle trie
https://medium.com/codechain/modified-merkle-patricia-trie-how-ethereum-saves-a-state-e6d7555078dd

The root hash of the state is contained in the block header for each block along with transaction root and receipt root.

Understanding the workings of the current structure of the ethereum state led me to understand its ineffiency in tranversing down the state down to query for data. 

To query for a state which is a few layers down, the client will need to know the hashes of 16 of its neighbour for each layer, resulting in a slow and expensive search

Hence the proposal of Verkle Tries


## Verkle Trie
IIUC verkle trie along with code chunking (merkleizing the smart contract https://eips.ethereum.org/EIPS/eip-2926) allows client to be able to prove that a leaf node is part of the root using vector commitments. (pederson/bulletproof seems to be the prefered choice as far)

The vector commitments are much easier to compute without having to know all the neighbours of the node and so each layer instead of having 16 nodes (in merkle), verkle trie can have more nodes (proposed to 257)and thereby also reducing the depth that a tree needs

https://www.youtube.com/watch?v=RGJOQHzg3UQ (basics of verkle trie)

https://www.youtube.com/watch?v=HipI3vCd8_Q (analysis of code chunking, how it may affect smart contract to break (exceeded gas limit), and making smart contract more/less gas efficient)

https://ethereum-magicians.org/t/proposed-verkle-tree-scheme-for-ethereum-state/5805

https://dankradfeist.de/ethereum/2021/06/18/verkle-trie-for-eth1.html

Verkle trie will then leads to statelessness and state expiry
