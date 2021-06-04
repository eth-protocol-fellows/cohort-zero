# Andrew Cudworth (lammatail22) notes

---

## Interests
- EVM generally
- Op Codes properties

## Open Questions

- What properties of the EVM can we formally define from the Opcodes
	-example: arithemtic is fully defined by  Opcodes 0x00-0x0B
	-the EVM can do keccak256 because of 0x20
	-can we infer DOS vectors from opcode definitions?
- How do changes in gas prices effect the formal properies of the EVM
- Can we simulate op code definition changes on historical transcation data
	-I'd really like to define and/or build a client with wtih flexible opcode definitions that will rerun transcations to check
	for backwards compatibility and change in execution properies
	
## Useful Links
https://ethervm.io/#opcodes
https://ethereum.github.io/yellowpaper/paper.pdf?
https://github.com/djrtwo/evm-opcode-gas-costs/blob/master/opcode-gas-costs_EIP-150_revision-1e18248_2017-04-12.csv
https://takenobu-hs.github.io/downloads/ethereum_evm_illustrated.pdf
https://blog.mycrypto.com/the-history-of-ethereums-block-size-block-gas-limit/