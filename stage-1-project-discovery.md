# Stage 1: Project Discovery

> WORK IN PROGRESS: none of this should be considered final

> Previous: [Stage 0: Getting Started](./stage-0-getting-started.md) | Next: [Stage 2: Project Exploration](./stage-2-project-exploration.md)

In this stage you will work to gain a high level overview of what sort of
projects there are to work on.  The goal is to avoid diving too deep on any
single subject.

Along the way, the hope is for you to start building and improving the following skills.

- Feeling comfortable in the R&D Discord
- Flex your writing skills.  Note taking and writing down what you know in an organized manner.
- Asking well informed questions.


## Identifying Potential Projects

Your goal is to identify potential projects for improving the core Ethereum protocol.  You should spend enough time with each project idea to gain a high level understanding of it.  If you already **know** what you want to work on, you may choose to skip this stage, however, there is value in gaining a broad view of the Ethereum protocol.

## Deliverable

The deliverable will come in two parts.

### 1. Documenting Project Ideas

First you will be hunting for possible projects. You should take a lot of notes.  When you find a potential project, start writing things down and aggregating information.  You can organize this information however you choose, but you should try to make it publicly consumable.  Each project idea will be different, but in general, you should probably try to gather at minimum:

- A "name" for the project
    - e.g. "Removing Gas Observability aka Ungas", "State Expiry", "Meta Transactions"
- Prior work: Links to existing forum posts or other documents that people have produced.
- A high level description of the project/topic/subject.
    - Even if someone else has already done this, try to do so again using your own words as this is a good way to gauge whether you understand the topic.
- Anything else that feels relevant.

If you haven't already, fork this repository. You are free to organize your work however you wish, as long as you do so in a public way.  I'll recommend that you create a document under `./notes/<your-name>.md` where you can take notes and commit.

### 2. Aggregating a master list

Second we want to collaborate on combining everyone's work into a single *master* list of potential projects in this document.  As you identify projects, open a pull request adding them to the master list.  If someone has already added it, look into whether it makes sense to merge your work with theirs, or to keep the two separate.  This is a collaborative effort.

## Finding Potential Projects

If you know very little about the Ethereum protocol, expect for this stage to be mostly you going back over old forum posts, blog posts, etc.  Leaning on the work of those that came before is to be expected.

Even if you are reasonably well versed in the Ethereum protocol, don't feel like you need to re-invent the wheel.

- New ideas are great.
- Executing on an existing idea that isn't actively being worked on is great.
- Joining someone else who is already working on an idea is great.

Here are a few places to start:

- List of EVM features we would like to remove: https://hackmd.io/@vbuterin/evm_feature_removing
- Core Paper: https://corepaper.org/ethereum/
- Stateless Ethereum: https://ethresear.ch/t/an-updated-roadmap-for-stateless-ethereum/9046
    - The Portal Network: https://github.com/ethereum/stateless-ethereum-specs/blob/master/portal-network.md
    - Block Level Access Lists: https://ethresear.ch/t/block-access-list-v0-1/9505
    - State Expiry: - https://hackmd.io/@vbuterin/state_expiry_paths
    - Extending Address to 32 bytes: https://ethereum-magicians.org/t/increasing-address-size-from-20-to-32-bytes/5485

## Project Master List

- [State Expiry](./notes/piper.md#state-expiry)
    - *this is just here as a very roughly sketched out example...*
- [Off-chain communication](./notes/virepri.md#1-off-chain-communication)
- [Scheduled Invocations](./notes/virepri.md#2-contract-scheduled-invocations)
- [Privacy in transacting](./notes/virepri.md#3-privacy-in-transacting--executing)
    - *this is currently a stub*
