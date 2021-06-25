# Kolby Moroz Liebl (KolbyML) notes

## An introduction

So to begin I have just been reading a lot of papers to get a high-level understanding of the space since I have only ever worked with Bitcoin. I like the program because of the freedom it gives you, but at the same time being so early on it is hard to know what you should work on. I really found it interesting how diverse the interests people had for projects and such. It was crazy to see how people were interested in stuff I would never think about doing or want to work on in years. This just shows how having different people work on projects can be beneficial.

At first, I thought of documenting all of the articles I read, but after some thought. I thought it would probably not be that beneficial. At the same time, it took me a lot of thought about how a document like this should look saying I have never done something like this before.

My interests were mostly on the implementation rather than the research end of things. I would rather contribute to clients, read their code, contribute to them, or just write my own client from the ground up.

---

# Projects

## Implementing uTP SubProtocol for Discovery V5
The goal of adding uTP is to standardize how clients send data that exceeds the limit on packet size. Clients that wish to send data that exceed this limit are often left to implement their own solutions for splitting these payloads. Well uTP and TCP could solve these problems Discovery 5 is built upon UDP and uTP could be built on top of it to solve these problems.

I am pretty happy Piper pointed me towards this after I addressed my interests. Well, there are some concerns how this does violate the discv5 specification, I am still willing to implement it, although it seems as a contentious problem which may not see fruition rather being replaced by an alternative.

There are three paths I could go down. Implement it for the Trin client (in Rust), implement it for the Nim-Eth client (in Nim), or just write a proof of concept. I would like to implement this in one of the projects that already supports discv5 because I want it to be used/useful. Those two clients are the only ones I have found so far which support discv5, but there may be others. Trin seems like the most willing to want this done because of its correlation with trinity. Although I do have a good bit of time to further look into it before making a final decision.

Prior work:
- https://github.com/ethereum/stateless-ethereum-specs/blob/master/discv5-utp.md
- https://github.com/ethereum/devp2p/blob/6eddaf50298d551a83bcc242e7ce7024c6cc8590/discv5/discv5.md
- https://www.bittorrent.org/beps/bep_0029.html

