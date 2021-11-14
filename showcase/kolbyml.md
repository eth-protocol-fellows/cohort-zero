# Kolby ML (Moroz Liebl)

- email: kolbymoroz@hotmail.ca
- [github](https://github.com/KolbyML)
- [resume](https://drive.google.com/drive/folders/1ho_H_0bp4JEqpiWgfwN3uPLWnYfwuVdK?usp=sharing)
- location: Alberta, Canada
- CDAP notes: [`./notes/KolbyML.md`](./../notes/KolbyML.md)

## About Me

I'm a software engineer who has been looking for fun projects to work on, often relating to blockchain technology. I wanted to learn how to write software since I was 10 years old, but only actually got off the ground 4 years ago after years of trying to learn the trade. I learnt about cryptocurrencies in 2014 and would mine as a hobby in the computer lab at my school. Following this interest getting into blockchain development ended up being a very clear path for me.


## My CDAP Project

During the course of the apprenticeship program, my main focus in the program was working on the [Portal Network](https://github.com/ethereum/portal-network-specs) helping to implement it in a project called [Trin](https://github.com/ethereum/trin) a Portal Network client written in Rust.

Quickly beginning my journey in CDAP Piper quickly pointed me to a project I might be interested in, implementing [uTP](https://github.com/ethereum/portal-network-specs) which peeked my interest. I started by learning Rust. After that I worked towards getting a deep grasp of TCP, because the uTP specs assume you understand TCP, this makes a lot of sense because uTP is practically a TCP clone with a custom Congestion Control Protocol. The first issue I ran into was that Trin didn't support compiling to Windows, my main development platform. So I made a PR adding support for Windows and also added CircleCI tests well I was at it. After that I begin implementing uTP, gaining a deep understanding of TCP on the way.

After I completed and merged the initial uTP implementation. I began working on implementing the 7th and 8th message of [portal wire protocol](https://github.com/ethereum/portal-network-specs/blob/master/portal-wire-protocol.md) Offer and Accept. This really suited me, because the Offer/Accept messages had parts that required uTP which I wrote the implementation of. Finishing the [Offer/Accept Implementation](https://github.com/ethereum/trin/pull/84) I think I might write a integration test framework, since it would be super useful for the continued development of Trin.

## Development Updates

You can read more about the work I did during the program in my development updates:

- [Update 1]: Start learning Rust and learning how uTP/TCP works
- [Update 2]: Implement Windows build support in Trin and CircleCI tests for windows, start implementing uTP
- [Update 3]: Continue working on uTP implementation, add fast resends, extensions, fully implement how acks are handled
- [Update 4]: Read how TCP is implemented in Rust, and work to base uTP implementation after its interfaces
- [Update 5]: Wrote unit tests for uTP implementation and finished the initial PR
- [Update 6]: Research how Discv5 handles Request/Responses and Start to implement Offer/Accept messages in Trin
- [Update 7]: Finish Offer/Accept PR and begin working on integration tests for Trin

[Update 1]: https://hackmd.io/eCBG9ANoT9my0St87Y6GEg
[Update 2]: https://hackmd.io/vBINdl8YTh-gxOfDr7ZUHA
[Update 3]: https://hackmd.io/TJ7AKs4-QBmdPGGxEuY2ig
[Update 4]: https://hackmd.io/nJEBr6OBRa6z4C4ZxOrf8w
[Update 5]: https://hackmd.io/pcz_FSjlSWuGD-EML6hFEg
[Update 6]: https://hackmd.io/V2kK5kcIQs-pMhzjTplvwg
[Update 7]: https://hackmd.io/DN-mXxJZRi6dlrJM5mIXyw


## Job Stuff

- I am willing to relocate
- I am interested in remote work
- I am open to part-time positions, and full time positions starting in May
