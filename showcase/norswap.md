# Nicolas "Norswap" Laurent

- email: norswap [at] gmail
- [github](https://github.com/norswap)
- [personal site](https://norswap.com)
- twitter: [@norswap](https://twitter.com/norswap)
- [CDAP notes]

[CDAP notes]: https://norswap.notion.site/Ethereum-Apprenticeship-0834d0470c8746c2aeeef75b62bef6c9

## About Me

I'm a software engineer & researcher. I used to work for Oracle Labs, but now
work for [Optimism PBC](https://www.optimism.io/) thanks to CDAP! 🙏

You can learn more about me [here](https://norswap.com/about/).

## My CDAP Project

I set out to help with [state expiry]. After writing [a review] of related
concepts and initiatives, I decided to work towards implementing [Verkle trees]
as my CDAP project. This was somewhat ambitious for someone whose sum total
knowledge about blockchains was captured in [this blogpost], and whose
cryptography experience was one postgraduate course.

To learn about Ethereum, I read the yellowpaper, and started implementing
[nanoeth], a attempt at re-implementing parts of the execution layer — in
particular block validation. For this, I had to implement [Merkle trees], a good
first step on the way to Verkle trees.

Verkle trees remain a work of progress — I will continue working on them and
post my progress on [this branch]. At the time of writing I have implemented the
[Bandersnatch] elliptic curve ([implementation][bs-implem]),  as well as most of
the [inner product argument] ([implementation][ipa-implem]), a zero-knowledge
scheme used to implement the vector commitment scheme in Verkle trees.

[state expiry]: https://notes.ethereum.org/@vbuterin/verkle_and_state_expiry_proposal
[a review]: https://norswap.notion.site/State-Expiry-Statelessness-in-Review-8d531abcc2984babb9bf76a44459e611
[Verkle trees]: https://vitalik.ca/general/2021/06/18/verkle.html
[this blogpost]: https://norswap.com/blockchain-how/
[nanoeth]: https://github.com/norswap/nanoeth/
[Merkle trees]: https://github.com/norswap/nanoeth/tree/master/src/com/norswap/nanoeth/trees/patricia
[this branch]: https://github.com/norswap/nanoeth/tree/verkle
[Bandersnatch]: https://ethresear.ch/t/introducing-bandersnatch-a-fast-elliptic-curve-built-over-the-bls12-381-scalar-field/9957
[bs-implem]: https://github.com/norswap/nanoeth/tree/11c530587e0e5c94ca560ad5f2bd7f4e6ada5a83/src/com/norswap/nanoeth/crypto
[inner product argument]: https://dankradfeist.de/ethereum/2021/07/27/inner-product-arguments.html
[ipa-implem]: https://github.com/norswap/nanoeth/tree/11c530587e0e5c94ca560ad5f2bd7f4e6ada5a83/src/com/norswap/nanoeth/trees/verkle

Along the way, I also engaged with the Ethereum research community and wrote
multiple articles about my findings and thoughts on some topics of interest:

- [State Expiry & Statelessness in
  Review](https://norswap.notion.site/State-Expiry-Statelessness-in-Review-8d531abcc2984babb9bf76a44459e611)
- [Thoughts on Address Space Extension
  (ASE)](https://ethereum-magicians.org/t/thoughts-on-address-space-extension-ase/6779)
- [Mitigating Sandwich Attacks at the Protocol
  Level](https://hackmd.io/@norswap/mev)
- [Merkle Patricia Tree: Uses &
  Implementations](https://github.com/norswap/nanoeth/tree/cc5d94a349c90627024f3cd629a2d830008fec72/src/com/norswap/nanoeth/trees/patricia#readme)
- [How rollups scale Ethereum](https://hackmd.io/@norswap/rollups) (at Optimism)

## Development Updates

You can read more about the work I did during the program in my development
updates:

- [Update 1]: Wrote the [state management
  review](https://www.notion.so/norswap/Dev-Update-1-5d4177c99f6b4c9ba856a5b870f9871a)
  and worked on [nanoeth](https://github.com/norswap/nanoeth).
- [Update 2]: Implemented transaction signing in nanoeth, read up on the Ethereum
  ecosystem and attended EthCC.
- [Update 3]: Wrote about [address space extension
  (ASE)](https://ethereum-magicians.org/t/thoughts-on-address-space-extension-ase/6779)
  and [how to mitigate MEV sandwich attacks at the protocol
  level](https://ethresear.ch/t/mitigating-sandwich-attacks-at-the-protocol-level/10298/).
- [Update 4]: Implemented partial block validation in nanoeth, implemented Merkle trees
  & working on debugging them.
- [Update 5]: Read on SNARKs & the beacon chain, finished Merkle tree
  implementation.
- [Update 6]: Merkle proofs, Merkle tree revamp, implementing cryptography for Verkle
  trees.

[Update 1]: https://norswap.notion.site/Dev-Update-1-5d4177c99f6b4c9ba856a5b870f9871a
[Update 2]: https://norswap.notion.site/Dev-Update-2-e48c8384b1424d81b774b56c3720f7b8
[Update 3]: https://norswap.notion.site/Dev-Update-3-4028a0bd1dd74983acd90c471daae80d
[Update 4]: https://norswap.notion.site/Dev-Update-4-a2359d2c75274578a3e37b1ee368987f
[Update 5]: https://norswap.notion.site/Dev-Update-5-404512ed7ae844769a76a3c7df6b890e
[Update 6]: https://norswap.notion.site/Dev-Update-6-22c8c2a7236947f493c3e5f42252e717

## Job Stuff

Having already found a job thanks to CDAP, I'm not looking at the moment!
