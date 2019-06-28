# Plasma: Scalable Autonomous Smart Contracts

## Abstract

We propose a method for decentralized autonomous applications to scale to process not only financial activity, but also construct economic incentives for globally persistent data services, which may produce an alternative to centralized server farms.

Plasma is composed of two key parts of the design:

- Reframing all blockchain computation into a set of MapReduce functions
- An optional method to do Proof-of-Stake token bonding on top of existing blockchains with the understanding that the Nakamoto Consensus incentives discourage block withholding.

We compose blockchains into a tree hierarchy, and treat each as an individual branch blockchain with enforced blockchain history and MapReducible computation committed into merkle proofs.

> reducible : 축소 시킬 수 있는
>
> enforcement : 집행
>
> revolves : 돌다
>
> mitigations : 경감, 완화

The greatest complexity around global enforcement of non-global data revolves around data availability and block withholding attacks, Plasma has mitigations for this issue by allowing for exiting faulty chains while also creating mechanisms to incentivize and enforce continued correct execution of data.

***As only merkleized commitments are broadcast periodically to the root blockchain (i.e. Ethereum) during non-faulty states, this can allow for incredibly scalable, low cost transactions and computation.*** Plasma enables persistently operating decentralized applications at high scale.

## 1 Scalable Multi-party Computation

***With blockchains, the solution for enforcing correctness has generally been having every participant validate the chain themselves. To accept a new block requires one to fully validate the block to ensure correctness.*** Many efforts to scale blockchain transactional capacity (e.g. Lightning Network) requires using time commitments to build a fidelity bond, (an assert/challenge agreement) so that the asserted data must be subject to a dispute period for participants on the blockchain to enforce the state. This assert/challenge construction allows one to assert a particular state is correct, and if the value is incorrect, then a dispute period exists where another observer can provide a proof challenging that assertion before a certain agreed time. In the event of fraudulent or faulty behavior, the blockchain can then penalize the faulty actor. This creates a mechanism for participants to be encouraged to enforce if-and-only-if the incorrect state is asserted. By having this assert/challenge-proof construction, interested participants can be able to assert ground truths to non-interested participants on the root blockchain 

> fidelity : 충실도
>
> enforcing : 강화
>
> correctness : 정확성
>
> adjudication : 판결

This structure can be used not only for payments, but extended to computation itself so that the blockchain is the adjudication layer for contracts. However, the presumption would be that all parties are participants in validating the computation. In Lightning Network, for example, the construction makes it so that one can establish commitments to computing contract state (e.g. with pre-signed tree of multisignature transactions of conditional state).

These constructions allow for highly powerful computation at scale, however there are some issues which require the summation of a lot of external state (i.e. summation of entire systems/markets, computation of a large amount of shared/incomplete data, large number of contributors).

This form of commitment to multiparty off-chain state (”state channels”) requires participants to fully validate the computation, or else there are significant amount of trust established in the computation itself, even in single-round games. Additionally, there is usually a presumption of ”rounds” whereby the execution path must be completely unrolled before contract initiation, which gives participants the opportunity to exit and force expensive computation on-chain (as it is not possible to prove which party is halting).

***Instead, we seek to design a system whereby computation can occur off-blockchain butultimately enforcible on-chain which is scalable to billions of computations per second with minimal on-chain updates.***

These state updates occur across an autonomous set of proof-of-stake validators who are incentivized towards correct behavior enforced by fraud proofs, which allow for computation to occur without a single actor being able to easily halt the computation service.

This needs to be able to minimize issues around the data availability problem (i.e. block withholding), minimizing the state updates in the root blockchain necessary in the event of byzantine actors to prevent risk-discounted transaction fees on the root chain, and a mechanism to enforce state changes. Similar to the Lightning Network, ***Plasma is a series of contracts which runs on top of an existing blockchain to ensure enforcement while ensuring that one is able to hold funds in a contract state with net settlement/withdrawal at a later date.***

## 2 Plasma

> autonomously : 자율적으로

Plasma is a way to do scalable computation on the blockchain with the structure of creating economic incentives to autonomously and persistently operate the chain without active state transition management by the contract creator. The nodes themselves are incentivized to operate the chain.

Additionally, significant scalability is achieved by minimizing the funds represented in a spend from a contract to a single bit in a bitmap, so that one transaction and signature represents a payment coalesced with many participants.

> coalesced : 합병된

We combine this with a `MapReduce` framework to be able to construct scalable computation enforced by bonded smart contracts. Plasma instead runs on top of an existing blockchain so that one does not need to create transactions on the underlying chain for every state update (including adding new users’ ledger entries), with minimal data on-chain for coalesced state updates.

Plasma is composed of five key components:

- An incentive layer for persistently computing contracts in an economically efficient manner
- Structure for arranging child chains in a tree format to maximize low-cost efficiency and net-settlement of transactions
- A `MapReduce` computing framework for constructing fraud proofs of state transitions within these nested chains to be compatible with the tree structure while reframing the state transitions to be highly scalable,
- A consensus mechanism which is dependent upon the root blockchain which attempts to replicate the results of the Nakamoto consensus incentives
- A bitmap-UTXO commitment structure for ensuring accurate state transitions off the root blockchain while minimizing mass-exit costs.

Allowing for exits in data unavailability or other Byzantine behavior is one of the key design points in Plasma’s operation.

### 2.1 The Plasma Blockchain, or Externalized Multiparty Channels

***We propose a method whereby multiparty off-chain channels can hold state on behalf of others. We call this framework a Plasma blockchain.***

> Plasma blockchains are a chain within a blockchain. The system is enforced by bonded fraud proofs. The Plasma blockchain does not disclose the contents of the blockchain on the root chain (e.g. Ethereum). Instead, ***blockheader hashes are submitted on the root chain and if there is proof of fraud submitted on the root chain, then the block is rolled back and the block creator is penalized.*** This is very efficient, as many state updates are represented by a single hash (plus some small associated data). This update can represent balances which are not represented on the root blockchain (Alice does not have her ledger balance on the root chain, her ledger is on the Plasma chain, and the balance in the root chain is representing a smart contract enforcing the Plasma chain itself).
>
> disclose : 밝히다
>
> How plasma contract is verified?

For funds held in the Plasma chain, this allows for deposit and withdrawal of funds into the Plasma chain, with state ㅣtransitions enforced by fraud proofs. This allows for enforcible state and fungibility since one is able to deposit and withdraw, with accounting of the Plasma block matching the funds held in the root chain (Plasma is not designed to be compatible with fractional reserve banking designs).

> fungibility : 바꿀수 있음
>
> fraud proofs : 오류 검증

Incredibly high amount of transactions can be committed on this Plasma chain with minimal data hitting the root blockchain. Any participant can transfer funds to anyone, including transfers to participants not in the existing set of participants. These transfers can pay into and withdraw (with some time delay and proofs) funds in the root blockchain’s native coin(s)/token(s).

We construct a series of fraud proofs as smart contracts on the root blockchain which enforce state in this channel so that attempts at fraud or non-Byzantine behavior can be slashed. These fraud proofs enforce an interactive protocol of fund withdrawals. Similar to the Lightning Network, when withdrawing funds, ***the withdrawal requires time to exit.***

We construct an interactive game whereby the exiting party attests to a bitmap of participants’ ledger outputs arranged in an UTXO model which requests a withdrawal.

> attests : 증명하다

Anyone on the network can submit an alternate *bonded* proof which attests whether any funds have already been spent. In the event this is incorrect, anyone on the network can attest to fraudulent behavior and slash the bonds to roll back the attestation.

Plasma allows for an interactive mechanism between n participants. The primary difference between Lightning and Plasma is that

- not all participants need to be online to update state
- the participants do not need a record of entry on the root blockchain to enable their participation – one can place funds on Plasma without direct interaction on-chain, with minimal data to confirm transactions when constructing these Plasma chains in a tree format.

### 2.2 Enforcible Blockchains in Blockchains

> Plasma composes blockchains in a tree. Block commitments flow down and exits can be submitted to any parent chain, ultimately being committed to the root blockchain.

If Lightning Network uses an adjudication layer for payments which is ultimately enforcible on the root blockchain, we create a system of higher and lower courts to maximize availability and minimize costs in non-Byzantine states. 

> adjudication : 판결

we are able to create state transitions which are only periodically committed to parent chains (which then flows to the root blockchain).

This child blockchain runs on top of a root blockchain (e.g. Ethereum) and from the root blockchain's perspective, is only seeing periodic commitments with the tokens bonded in the contract for enforcement of the Proof-of-Stake consensus rules and business logic of the blockchain.

This has significant advantages in maximizing block availability and minimizing stake for validation of one's coins. However, since not all data is being propagated to all parties (only those who wish to validate a particular state), parties are responsible for monitoring the particular chain they are interested in periodically to penalize fraud, as well as personally exiting the chain rapidly in the event of block withholding attacks.

### 2.3 Plama Proof-of-Stake

We propose a method whereby a single party can enforce state with a set of validators, often in a proof-of-stake framework requiring either ETH bonding, or bonding in a token (e.g. ERC-20). The consensus mechanism for this proof of stake system, is again, enforced in an on-blockchain smart contract.

Proof-of-stake coalitions face this issue since it's possible if one does straight leader election, block withholding attacks by majority cartels (also generalized as the ”data availability problem”) become magnified.

> mitigate : 완화시키다.

We can mitigate this in Plasma Proof-of-Stake by allowing stakeholders to publish on  the root blockchain or parent Plasma chain which contains a committed hash of their new block.