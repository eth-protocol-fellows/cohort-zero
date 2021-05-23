Notes by @sblOWPCKCR

# Project ideas

# Area of interest: code guarantees >> economic incentives

There's a lot of things outside Ethereum core that rely on incentivising users to perform
actions that keep a protocol alive. Examples: DEX liquidations, 'cron'-like protocols.

If we can bring execution of those actions to Ethereum itself (think: native transactions
scheduling, native transaction triggers), we can rely less on economics and more on stronger
guarantees provided by contracts.

Is it possible? For some usecases - probably, for some - likely not.

## 1. Contract scheduled/automatic invocations

[Basically, this](./virepri.md#2-contract-scheduled-invocations)

To explore: is it possible/valuable to extend 'scheduled' beyond time axis, and have
some kind of automatic triggers for transactions?

Condition A is met -> triggers transaction X. It is possible in current EVM (every time A can be
met, we check it and CALL X), but is not gas efficient (if my transaction triggers A, I don't
want to pay for X).

Need to come up with a few *concrete* usecases first.


# Area of interest: rollups

Do we need to extend/reduce EVM to make full EVM-compatible rollups easier to implement? Any other
interesting things?

# Area of interest: developer productivity

Debugging failing transactions is pain. Looking at debug_traceTransaction is pain, is it possible to
build something better? How about source maps?