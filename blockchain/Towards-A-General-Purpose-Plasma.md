# Towards A General Purpose Plasma

https://medium.com/plasma-group/towards-a-general-purpose-plasma-f1cc4d49c1f4

## Clap for the Plapps

Writing a new plapp is as simple as writing a special type of smart contract, called a ***predicate contract\***, and deploying it to Ethereum. Anyone can use your plapp by interacting with your predicate contract, and it all happens *on the plasma chain —* where it’s much, much cheaper.

- Plapps need to implement a standard predicate contract interface, and individual transactions must still fit within the ethereum gas limit.

# Understanding the Generalized Plasma Architecture

https://medium.com/plasma-group/plapps-and-predicates-understanding-the-generalized-plasma-architecture-fc171b25741

## Layer 2: Claims about State

The core idea behind “Layer 2” is that we can use data outside of a main blockchain (“off-chain data”) to provide guarantees about assets sitting on the main blockchain (“on-chain data”).

- The plasma smart contract on Ethereum records the order in which plasma blocks are committed by the operator, and prevents commitments from ever being overwritten.

## Abstract Offchaindataism

Whether it’s changing the kitty’s fur color or eye color, the smart contract back on Ethereum needs to have a way of understanding these changes.

- ***Each new piece of functionality — or new type of “state  transition” — requires a change to the logic followed by the plasma contract.***

In previous plasma specs, adding a feature like this meant that one would have to re-deploy the *entire* plasma contract and migrate everyone’s assets from the old plasma chain to the new one. ***This is not secure, scalable, or upgradeable.***

## Predicates: Making Plasma Useful

After some brainstorming, we realized there was an easy way to add new functionality ***without changing the main plasma chain contract.***