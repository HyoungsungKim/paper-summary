# Understanding Plasma, Part 2: Make Plasma Fungible Again 

https://www.theblockcrypto.com/post/16705/understanding-plasma-part-2-make-plasma-fungible-again

## Less Client-Side Data, Strategy 1: Checkpoints

What we saw in (what we'll hereby refer  to as) Vanilla Plasma Cash was that the amount of data each client needs to store grows linearly over time; i.e., for each coin, one Merkle proof of either inclusion (coin was spent) or exclusion (coin stayed put) is required for every Plasma block. 

The idea is, once this "On Chain Thing" is finalized, it would serve as a historical checkpoint: future proofs of ownership now only have to go back as far as this point; no disputes involving any prior history are allowed. Thus, the Merkle proofs of all prior blocks can be safely discarded for good. This would put a concrete upper-bound on the size of the requisite client-side data.

- The first thing we need to make these checkpoints work is a way for the Plasma operator (or anyone else, but  we'll assume the operator for simplicity) to attest to the full state of coin ownership at a given Plasma block.

Suppose a malicious/compromised/bored operator broadcasts the Merkle root for a checkpoint, but then fails to share any of the Merkle proof data with the users. Now what? 

- We could point out that users still have the option to safely withdraw their funds back onto the mainchain, which is technically true.
- But the problem is, now every user would have to stampede onto the mainchain, and they would have to do so within some bounded, two-week window *because of the checkpoints we just introduced.* The "mass-exit" corner of the carpet we so cleverly pressed down in part 1 pops up again. 

In essence, we modify the ownership-attestation slightly such that the operator requires the signature of each coin's owner in the checkpoint; in other words, ***the operator can only checkpoint a coin with its owner's explicit consent.*** The operator then also posts some additional data on chain: an attestation of which coins are included in the checkpoint (imagine each coin represented in a list of binary digits, with a "1" signifying consent/inclusion and a "0" exclusion.)

## Less Client-Side Data, Strategy 2: Proof Compression 

***A different, more advanced approach for minimizing storage requirements is to effectively compress many Plasma block proofs down into one.*** Again, recall that for a Vanilla Plasma Cash coin, one exclusion or inclusion Merkle proof per block is required.

The idea now is to add a requirement for *inclusion:* Not only do we require the on-chain Merkle root and off-chain Merkle branch,

- We *also* require an on-chain “RSA accumulator,” (a secondary way of proving membership using pure number theory)
- And an off-chain “proof of knowledge” (details explained shortly, just roll with it for now).

***Since both of these are required to prove inclusion, only one is required to prove exclusion;*** i.e., a coin is either included in a block or it isn't; there are no other possibilities, and the negation of “Both A and B (for inclusion)” is “Either A or B (for exclusion)”. 

