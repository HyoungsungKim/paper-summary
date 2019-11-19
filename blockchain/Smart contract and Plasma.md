# Smart contract and Plasma

## Why is EVM-on-Plasma hard?

https://medium.com/@kelvinfichter/why-is-evm-on-plasma-hard-bf2d99c48df7

Lots of people want to know if it’s possible to create a Plasma chain that allows users to deploy EVM smart contracts. Unfortunately, this is a lot more complicated than it might seem for reasons that probably aren’t obvious if you don’t spend a lot of time working with Plasma.

The type of EVM smart contracts you might be familiar with, like a simple ***multisig account.***

- If this account currently sits on the Plasma chain, then we need to provide some way for the wallet to be “withdrawn” to the root chain.
  - However, we still want to keep the funds in a multisig — we effectively want to move the entire smart contract back onto the root chain.
  - Remember that each EVM contract has state, balance, and code. So what we’re really doing is moving the state, balance, and code of this multisig contract from the Plasma chain to the root chain.
  - The contract should then be able to operate normally on the root chain.

So who actually gets to decide to move stuff from the Plasma chain to the root chain?

### First big problem

***Multisig***

- ***If we’re talking about a multisig account,*** then we could design a few different mechanisms to determine when the multisig is moved to the root chain.
  - Maybe every user in the multisig needs to sign off(승인하다), maybe n-of-m users need to sign off, or maybe only one user needs to sign off.
  - All of these are potentially valid mechanisms — we start to see that it makes sense for the designers of the multisig to decide which mechanism is most appropriate.

But here’s the problem — ***it’s not always clear who gets to move a contract from the Plasma chain to the root chain***.

- Imagine we have a smart contract (on some Plasma chain) that represents lots of virtual kitties.
  - Let’s say the consensus mechanism goes “bad” and everyone needs to leave the Plasma chain to keep their funds safe. What do we do about our virtual kitty contract?
  - We need to move the contract back onto the root chain. Unfortunately, if we allowed just anyone to withdraw the contract, then (rude) users would almost definitely just withdraw as many contracts as possible to make the Plasma chain unusable. We have to come up with a better mechanism.
- One seemingly obvious mechanism might be a voting mechanism that decides when the contract can be withdrawn. A small set of eligible voters would highly centralize control over the contract.
  - However, the more users eligible to vote, the more expensive the voting mechanism.

### Second big problem

***If anyone can modify the state of the contract, then anyone can block an exit***.

We need to validate that the state change presented in a challenge is actually a valid state change.

- However **validating EVM state changes inside the EVM is hard**. 
  - For complex EVM contracts, things get much more expensive.
  - State 변화가 정말 검증된 변화인지 확인 해야 함
  - 하지만 EVM에서 state 변화를 검증하기 힘듬
- If we want to validate single EVM steps in a trustless way, ***then we need to implement the EVM inside the EVM***

## Plasma Leap - a State-Enabled Computing Model for Plasma

https://ethresear.ch/t/plasma-leap-a-state-enabled-computing-model-for-plasma/3539

### Problem Statement

**Enforcing off-chain computation** 

- The verification needs to be economically feasible inside the Ethereum Virtual Machine (EVM) and should not exceed the capacity of the Ethereum network at any time

**Exit authorization**

- ***If funds are controlled by complex rules and conditions for spending with multiple owners or even no specific owners,*** these owners might not agree on the availability of data or there might be no-one with the authority to exit the specific funds.
- Thus, exits can be hard or impossible to coordinate if funds have multiple or undefined ownership

**Spending authorization**

- The possibility for a grieving attack emerges where anyone can spend such funds and cancel the exit 

### Computation Verification Game

The ***TrueBit protocol 5*** proposes a scheme to verify correct execution of computation in witness of a computationally bounded judge.

- ***The protocol can reduce the verification effort of off-chain execution to the execution of a single operation on-chain.***
- While the original protocol works with WASM VM 2 we adapt the protocol to be able to verify EVM computation using SolEVM 8.
- To enable it, the following requirements need to be met by the  implementation to make the verification of Plasma computation feasible:

## LeapDAO

https://leapdao.org/blog/Smart-Contracts-on-Plasma/

Lifting the execution of smart contracts onto a layer-2 blockchain is  one of the core goals of the community which holds a huge potential.

At the heart of this task is the [SolEVM Enforcer](https://github.com/leapdao/solEVM-enforcer), a computation verification game that allows for enforcing the off-chain execution of EVM bytecode. This SolEVM is composed of 3 parts:

- **On-chain stepper** - Enforcer contracts.
- **Off-chain interpreter** - ECMAScript implementation with the same behavior as the Solidity contracts, for performant off-chain usage.
- **Library** with an easy to understand interface for developers.

LeapDAO community is developing two independent systems with the intention to integrate them

### The Plasma Roadmap

https://leapdao.org/blog/Plasma-Roadmap/

This post will introduce the milestones that we defined: from launching Plasma on mainnet to a fully-fledged censorship resistant, smart contract executing child chain that is supposed to scale Ethereum and  increase its usability.

- ***SolEVM Enforcer*** is the verification engine enforcing the correct execution of smart contracts on the Plasma chain.
- ***The Plasma chain, built on the More Viable Plasma dialect,*** will start off with transfer-only abilities and later integrate with SolEVM at the Metaverse/WardKeeper milestones to gain computation abilities.

###  2.0 Metaverse

While enabling the execution of smart contracts on layer-2 blockchains, ***the contracts will be limited in their expressiveness to stateless scripts.***

- State가 없는 간단한 스마트 컨트랙트만 가능...