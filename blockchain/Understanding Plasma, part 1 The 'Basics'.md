# Understanding Plasma, part 1: The 'Basics'

https://www.theblockcrypto.com/post/10793/understanding-plasma-part-1-the-basics

- Plasma is a Layer 2 scaling framework that aims to increase transaction throughput

## Introduction

### Background: Layer 2

If you’d like, you can think of a Layer 2 transaction as a check whose account’s funds you can verify directly, without necessarily having to actually deposit it into your bank account. 

The general pattern of Layer 2 systems is: initially, some capital is locked up on the blockchain’s base layer. ***Next, some parties (and not necessarily the same parties who made the deposit) can then transact off-chain with this capital via an overlay system, while only interacting with the mainchain occasionally (if ever).*** At any given point, the proper owner of any capital has assurance in their ability to withdraw all funds they own back onto Layer 1.

### What Makes Plasma Plasma

We’ll hereby  assume broad enough definitions of “Plasma" and "channels" such that the  two encompass the totality of all possible Layer 2 systems.

***One way to delineate these two categories is by the minimum on-chain transactions they require: for a channel transaction to be considered finalized, no interaction with the mainchain is strictly necessary;***

- for a Plasma transaction, *one* interaction with the mainchain is strictly necessary (broadcast by the Plasma operator, not the users, as we’ll see).

The reason Plasma still qualifies as a Layer 2 scaling approach (despite requiring regular on-chain transactions) is that each Layer 1 transaction can effectively finalize *many* transactions in one fell swoop; you can imagine that a bundle of Layer 2 transactions are compressed down into one. Still, this itself seems to be an ipso facto(그 사실 때문에) plus-side to channels; no on-chain block confirmations required means (virtually) instant finality, and less on-chain interaction is generally a good thing. 

On the flip side, a channel requires full consent of all of its participants for any channel-wide state update, which means that having a single channel with many parties gets highly impractical.

- Transacting with parties with whom you *don't* share a channel requires "relaying" transactions through your channel-partners, limiting your financial activity to those with whom you can find these liquidity paths through the channel network's graph.

***In Plasma, however, only a transaction’s sender needs to give consent,*** and no liquidity lock-ups / restrictions are required for all parties involved to enter, exit, and freely transact with each other.

## Minimum Viable Plasma(MVP)

As we saw earlier, the key property of Plasma is that many transactions are compressed down and finalized with only one transaction landing on the mainchain. In MVP, the "compression" is done via a Merkle tree; 

### Life cycle of a typical Plasma transaction

First, Alice deposits Ether into our Plasma chain by sending an on-chain Ether transaction to the contract, which the Operator includes in a Plasma block; this Ether initially  belongs to Alice (obviously) in the form of a UTXO. Alice, as usual,  wants to pay Bob, (note that Bob himself does not necessarily need to  have made any on-chain deposits himself yet, or ever.)

- To do this, she  creates a transaction that spends her UTXO and creates a new one for  Bob, and ***sends this transaction to the Plasma operator.***
- The operator takes this transaction along with a bundle of other (feasibly unrelated) transactions, groups them together into one Plasma block, "Merklizes" them down to their Merkle root, and sends this root — and only this root  — onto the main chain. 

***The operator then sends this Plasma block to all users (including Alice and Bob).***

- Upon receiving the latest block, Alice and Bob validate it on their end; this validation entails ensuring that the transactions themselves are valid and that the block corresponds to the on-chain Merkle root.
- If all of  this checks out, Alice, Bob, and all of the other users can then go happily on with their lives. 

Later on, Alice — feeling she's had herself enough of this crazy Plasma business, say — decides she wants to withdraw her funds back onto the Ethereum chain. ***She initiates this "withdrawal request" via an on-chain transaction (n.b: this withdrawal  request does not require the Operator’s permission.)***

- In her transaction, she includes the Plasma  chain’s UTXO she would like to withdraw, along with the Plasma block number it belongs to and the Merkle path proving inclusion.
- Now, before she has access to her funds, she must wait for the ***"dispute period"*** (one  week, let's just say) to pass. During this period, other users can challenge her exit if they detect foul play. 

Which brings us to:

#### The Happy Case: Everyone Behaves

Upon initiating her exit, other users skim through their copy of the Plasma chain to check and confirm that yes, indeed, the UTXO Alice is trying to exit with does in fact still belong to her. They also verify that all of the blocks are valid in  every other way (though presumably they’ve already done this). Users can now rest assured that Alice is only departing with money that's rightfully hers, and other users' funds are safe. Life can go on. 

#### The Unhappy case: Evil Alice

Alice's exit attempt is "proper enough" to be initially accepted by the smart contract — which is to say, it's a valid transaction, with a Merkle proof that does indeed correspond with an old Merkle root — but it’s actually a double spend;  i.e., she tries to exit with the same UTXO she sent Bob earlier.

But no matter! Bob (or any really any  other user, but let's assume Bob) has one week to take action -Dispute period

- He’ll check Alice’s UTXO against his copy of the Plasma chain and notice that it’s a double spend. To prove maleficence, he submits a "fraud proof" in the form of the old transaction in which Alice previously spent the UTXO in question, along with a Merkle proof of *its* inclusion in a Plasma block. Bob has given cryptographic proof that Alice already spent this money; she's been caught in the act, and her attempt to withdraw this money is canceled. 

#### A Note on “Punishments”

