# Plasma Group’s Plasma Spec

https://medium.com/plasma-group/plasma-spec-9d98d0f2fccf

## Introcude

A multi-chain approach for parallelizing transactions is a promising way  to increasing throughput… unfortunately, it also imposes significant  challenges:

- We don’t want to divide security, e.g. 100 chains with 1% of total security each.
- Advanced solutions like sharding are promising, but not yet ready.

We believe that the strongest candidate to meet these criteria is a  network of chains, each secured to mainnet via the Plasma framework.

## Properties of our Plasma Chain Implementation

**Our specification has the following properties:**

- Single transactions over large ranges of coins, solving the fixed-denomination” problem in Plasma Cash.
- Block size which ***scales with the number of transactions,*** not the number of deposits.
- Light client proofs which scale in the logarithm of the block size and linear in blocks since deposit, making the operator the system’s only  (computational) bottleneck.
  - 라이트 클라이언트의 증명 시간에는 블록 사이즈에는 log로 deposit에는 선형으로 증가 함
- A simplified, optimistic ***exit procedure that allows exits to specify only the most recent transaction,*** instead of both a transaction and its parent.
- Interchain atomic swaps, which lay the groundwork(준비작업) for decentralized exchange protocols.
- Unlimited deposit capacity.

**However, a few disclaimers before we dig in:**

- The main difference (explained below!) between this protocol and other Plasma implementations is the block structure: a Merkle *sum* tree. This has significant benefits, but adds complexity. Plasma is already complex in comparison to sidechains.
- Code has yet to be audited or formally verified, and has not undergone any optimization.
- While the operator is the only *computational* bottleneck, the primary performance limit today remains bandwidth.  Custody proofs require downloads which are linear in the number of blocks. Our code is an improvement per block, but still linear. This [active](https://ethresear.ch/t/rsa-accumulators-for-plasma-cash-history-reduction/3739) [area](https://ethresear.ch/t/log-coins-sized-proofs-of-inclusion-and-exclusion-for-rsa-accumulators/3839/7) of [research](https://ethresear.ch/t/plasma-prime-design-proposal/4222) is yet unready for implementation.
- Though our safety mechanisms and exit games are both implemented and tested,  we have not yet built an automated guard service, meaning challenges and responses must be manually constructed.



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

***The off chain data is always for the purpose of “disputing” incorrect on-chain claims.***

- The dispute for a particular claim is basically just some proof that the state referenced in the claim is outdated.
- A dispute about ownership proves that the user making the claim later transferred ownership to someone else.
  - A dispute about kitty fur color requires proving that the kitty’s fur color has been changed.

***Our primary breakthrough was realizing that dispute conditions don’t  need to be checked in the main smart contract.*** 

- Instead, we could have other smart contracts implement a function that would tell us if the dispute was valid or not. We could add new functionality by creating a  new contract that implemented the necessary logic for that functionality and then have our main contract reference the new one.
  - We call these external contracts **‘predicates’**.
  - 거래를 사실이라고 단정 지음
  - 메인 스마트 컨트랙트에서는 확인 될 필요가 없기 때문에
- But we still needed a way to know *which* predicate to use for a particular state update.
  - Simple! ***We just add the requirement that the state update also says which predicate it’s subject to.*** Now the update message that changes a kitty’s fur color could be, “I’m making this kitty’s fur color blue and disputes can be handled by the predicate sitting at (`0x123…`)”