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

### 2.3 Plasma Proof-of-Stake

We propose a method whereby a single party can enforce state with a set of validators, often in a proof-of-stake framework requiring either ETH bonding, or bonding in a token (e.g. ERC-20). The consensus mechanism for this proof of stake system, is again, enforced in an on-blockchain smart contract.

Proof-of-stake coalitions face this issue since it's possible if one does straight leader election, block withholding attacks by majority cartels (also generalized as the ”data availability problem”) become magnified.

> coalitions : 연합, 제휴
>
> mitigate : 완화시키다.
>
> suboptimal : 차선의, 최적이 아닌

We can mitigate this in Plasma Proof-of-Stake by allowing stakeholders to publish on  the root blockchain or parent Plasma chain which contains a committed hash of their new block. ***Validators only build upon blocks which they have fully validated. They can build upon blocks in parallel (to encourage maximum information sharing).***

We create incentives for validators to represent the past 100 blocks to match the current staker ratio (i.e. if one stakes 3 percent of the coins, they should be 3 percent of the past 100 blocks), by rewarding more transaction fees to be paid out to accurate representation.

Excess fees (due to suboptimal behavior by stakers) goes to a pool to pay out fees in the future. A commitment exists in every block which includes data from the past 100 blocks (with a nonce). The correct chain tip is the chain with summed weight of the highest fees. After a period of time, the blocks are finalized.

### 2.4  Blockchains as MapReduce

By constructing computation in a MapReduce format, ***it is also easy to design computation and state transitions in a hierarchical tree.***

***MapReduce gives a framework for high scale computation across thousands of nodes.*** The blockchain faces similar issues in meeting computational scale, but has additional requirements in generating proofs of computation.

Our construction enables incredible high-scale computation, with time or speed trade-offs. ***These trade-offs produce a network where nodes assert computation and participants are responsible for verifying them.***

It enables the ability to compress computation into bonded proofs. These bonded proofs encourage participants to only attest to things honestly. If no one is watching/enforcing the computation, it's presumed to be correct, or it simply doesn't matter what the result may be.

Computation can be watched by any participant in open networks, but participants who hold balances and/or require correct computation will periodically watch the chain to ensure correctness.

The scaling benefit comes from removing the requirement to watch the chains one is not economically impacted by, one should watch the chains where one wishes to enforce correct behavior.

> One only needs to watch the data which one wants to enforce.

### 2.5 A Description of Economic Incentives around Persistent decentralized Autonomous Blockchains

We propose a structure whereby one can create economic incentives to persistently keep a child blockchain running.

To incentivize avoidance of Byzantine states, especially around correctness and liveness, it may be ideal to create a token per contract. This token represents the network effects in operating the contract, and creates an incentive to maximize security of this contract.

***As the Plasma chain requires the token to secure the network in a Proof-of-Stake structure,*** stakers are disincentivized against Byzantine behaviors or faults as that would cause a loss in value of the token.

> PoS는 무엇에 대한 지분으로 진행?? 토큰 보유량으로 PoS? -> 이더리움으로...
>
> bond : 채권

With simple contracts and business logic such as a basic contract account holding funds on behalf of its users, ***an Ethereum bond can represent a stake in the Plasma chain.***

The stakes who put up bonds (whether it be a token or ETH) have incentives to continue operating the network as they receive transaction fees for operating the network.

## 3 Design Stack and Smart Contract

***Plasma is not designed to reach assured finality rapidly, even though transactions are confirmed in the child chains rapidly, it requires it to be finalized on the underlying root blockchain.*** Channels are necessary to be able to have rapid local finality of payments and contracts (enforcible on-chain).

***In smart contracts, there is an issue of the ”free option problem” whereby the receiver (second or last signer) of a smart contract offer is needed to sign and broadcast the contract in order to enforce it*** – during that time the receiver of the contract may treat it as a free option and refuse to sign the contract if the activity does not interest them. This is exacerbated as smart contracts are most effective when dealing with counterparties who are untrusted (as that creates minimization in counterparty risk and thereby information costs).

With Lightning (including Lightning on top of Plasma), it's possible to do incredibly rapid updates with reasonable sense of localized finality. ***Instead of having a single payment which gives optionality to the last party, a payment can instead be split into many small payments.***

This minimizes the free option to the amount per split fraction. Since the second party of the smart contract only has the free option on the amount in the split fraction, the value of the free option is minimized. Within the above use cases, it's possible that ***Lightning may be a primary interface layer for rapid financial payments/contracts on top of Plasma, as Plasma allows for ledger updates with minimal root chain state commitments.***

### 3.1 The Most Significant Problem in Sharding is Information

With sharded data sets, there is a significant risk of individual shards to refuse to disclose information. It would thereby be impossible to produce fraud proofs. We attempt to resolve this using 3 strategies:

- A new Proof-of-Stake mechanism which encourages block propagation. The underlying mechanism does not entirely rely upon correct functionality of incentives. However, this should significantly decrease faulty behavior.
- Significant withdrawal delays which allow for accurate withdrawal proofs. In the event of block withholding, plasma chains can immediately lock up funds via a proof, preventing an attacker from submitting fraudulent withdrawal proofs.
- Creating child chains whereby transactions can be propagated in any parent chain. ***For this reason, participants on networks will desire to submit transactions to deep child chains.*** People are therefore encouraged to create deeply nested child chains which represent significant value. ***Note that there is some presumption about reputation around chain selection for individuals holding very small balances which cannot be on the root blockchain transaction fees, however is mitigated by having deeply nested chains. This security model is the key novelty of plasma chains.***

## 4 Related Work

Plasma uses a merkleized proof to enforce child chains.

### 4.1 TrueBit

Plasma shares a great degree of similarity in the reliance of fraud proofs as TrueBit. Similar assumptions as TrueBit applies, namely that computational state must be computable and broadcastable online, data availability problems needs to be mitigated, failure must be disclosed. We attempt to mitigate these problems, especially the latter two.

The primary aspect which Plasma attempts to build upon TrueBit is the notion of multiparty participants which need to compute on shared state.

### 4.2 Blockchain Sharding

If the root blockchain is sharded, then the Plasma chain can run on top of this for greater scalability and other benefits. This can also be a testbed for different sharding techniques as there are no consensus changes necessary in Ethereum and other rich stateful blockchains to begin basic operation.

### 4.4 Merge-Mined Blockchain

Examples include Namecoin, which create concurrent blocks with the parent blockchain. This presumes full validation of the blockchain, thus does not provide scalability benefits.

The goal of Plasma is to ensure that only users and miners need to validate the chains relevant to themselves.

### 4.7 Cosmos/Tendermint

Cosmos arranges blockchains in a Cosmos ”Hub” and has child blockchains ”Zones” validated over a proof of stake system. ***Significant similarity with the construction of child blockchains exist, however Plasma is reliant upon construction fraud proofs to enforce state in child chains and is genericized to be applicable to many chains.*** The proof of stake construction for Cosmos presumes a 2/3 honest majority of validators, including validators of its Cosmos Zone.

### 4.8 Polkadot

Polkadot also constructs a structure for a hierarchy of blockchains. There is some similarity with the design of Polkadot. Instead of a structure with ”fishermen” validators ensuring block accuracy, we construct a series of child blockchains which enforce state via merkle proofs. The Polkadot construction is reliant on the child blockchains (”parachains”) state and information availability being enforced by the fishermen.

## 5 Multiparty Off-chain State

The goal is to construct a method whereby participants can hold funds in the native underlying coin/token of the blockchain, without significant on-chain state. Plasma begins to blur the line between on-chain and off-chain (e.g. are shards on-chain or off-chain?).

There are two common issues in efforts to establish off-blockchain multiparty channels.

- The first is the need to do synchronized state update amongst all participants when there needs to be an update on the system (or otherwise make trade-offs on availability of global state updates) ***and therefore must be online.***
- The second is that adding and removing participants in the channel require a large on-blockchain update, enumerating all participants which are added and removed.

The general construction is a child blockchain which allows for holding balances represented in a smart contract on the root blockchain (e.g. Ethereum). The balances of the smart contract are represented and allocated to the balances of finalized blocks in the child Plasma blockchain. This allows one to hold the native coin in the child blockchain with full representation of balances on the root blockchain, allowing for withdrawals after a dispute mediation period.

In order to achieve this, we construct a UTXO (Unspent Transaction Output) model for the ledger. While this is not an explicit requirement, it is easier to reason about with rapid withdrawals. The rationale for a UTXO model is that it is easy to compactly represent whether a particular state has been spent or not. This can be represented within a trie for merkleized proofs, and as a bitmap for a compact representation parsable by others. In other words, the smart contracts are held in accounts on the root chain, but ***the Plasma chain maintains a UTXO set of balances for the allocation of the balances held in the root chain account.*** For child chains which do not have significant requirements around state transitions, it is possible to use an account model for more complex or frequent state transitions, however there is more reliance on block space availability in the parent blockchain(s).

***The blocks are propagated to the participants who wish to observe the blocks, including participants who hold balances or want to observe/enforce computing on the individual Plasma chain.*** While there is minimal complexity in maintaining deposits of off-chain state, state transitions and withdrawals create greater complexity.

### 5.1 Fraud Proof

All states within this child blockchain is enforced via fraud proofs which allows for any party to enforce invalid blocks, presuming block data availability.
However, the greatest difficulty in this construction is that there are no explicit guarantees around data/block availability.

The fraud proofs ensure that all state transitions are validated. Example fraud proofs are proof of transaction spendability (funds are available in the current UTXO), proof of state transition (including checking the signature for the ability an output can be spent, proof of inclusion/exclusion across blocks, and deposit/withdrawal proofs.)

In order for this construction to have minimal proofs, though, all blocks must provide a commitment to a merkleized trie of the current state, a trie of outputs spent, a merkle tree of transactions, and a reference to the prior state being modified.

The fraud proofs ensure that a coalition of participants are not able to create fraudulent blocks without getting penalized. ***In the event a fraudulent block is detected and proven on the root blockchain (or parent Plasma chains), the invalid block is rolled back.*** The result is highly-scalable state transitions are capable in the Plasma blockchain while ensuring that observers who have access to block data are able to prove (and therefore discourage) invalid state transition. In other words, ***payments can occur in this chain with only periodic commitments on the root chain.***

### 5.2 Deposits

Deposits from the root chain are sent directly into the master contract. The contract(s) are responsible for tracking current state commitments, penalization of invalid commitments using fraud proofs, and processing withdrawals. As the child Plasma blockchain is a full validator of the root blockchain, the incoming transaction must be processed using a two-phase lock-in. Deposits must include the destination chain blockhash to specify the destination child chain and is achieved using a multi-step process to ensure coins are not unrecoverable.

1. The coins/tokens (e.g. ETH or ERC-20 token) are sent into the Plasma contract on the root blockchain. The coins are recoverable within some set time period for a challenge/response.
2. ***The Plasma blockchain includes an incoming transaction proof.*** At this point, the Plasma blockchain is committing to the fact that the transaction is incoming and will be spendable in the event of either a lock-in transaction or spend initiated by the depositor. When this is included, the blockchain is committing to the fact that it will honor a withdrawal request. ***However, there is no confirmation yet that the depositor has sufficient information to generate a fraud proof, so there is not yet a commitment from the depositor.*** This block includes the addition in the state tree, bitmap, and transaction tree, so that there is a compact proof of correct inclusion.
3. Depositor signs a transaction on the child Plasma blockchain, activating the transaction, which includes a commitment that they have seen the block with the chain's commitment in Phase 2. ***The role of this phase is the depositor is attesting to the fact that they have sufficient information to withdraw funds.***

### 5.3 Mass Withdrawals and Bitmapped State

The primary concern around this system is around the inability to verify state.

***To be able to conduct maximum compaction of state transaction, outputs may be optionally represented in a bitmap. This is necessary for withdrawal proofs which may be too expensive to conduct on the root chain.***

The goal of this construction allows for holding small balances on the Plasma chain. These balances are held in full reserve on the contract on the root blockchain, ***but the full ledger is not on the blockchain.*** The primary attack which needs to be mitigated is withheld invalid blocks (with commitments to the root chain). In the event the system observes invalid state transitions, the participants conduct a mass exit of the transactions.

> compaction : 꽉 채움

With a bitmap construction, a withdrawal includes a bitmap of signed transactions which wish to exit. A game/protocol is constructed which is enforced by a smart contract to ensure correct information. ***The bitmap ensures that everyone is able to reason about what outputs are being spent.*** As this is a bitmap, it necessitates that the state be represented in an unspent transaction output data structure (UTXO) for maximum efficacy of small-value balances. Spentness can be compactly proven, and a large set of state transitions can be cleanly enforced. After a predefined settlement time period, the bits can be reused.

For those holding balances which can be enforced on the root chain, the requirement to conduct a UTXO bitmapped format is not necessary. However, for those holding balances and enforcement is only viable if the 1-2 bit transaction gas/fee on the root blockchain is sufficiently low.

### 5.4 State Transactions

By default, state transitions in the Plasma chain run in a similar multi-phase process as the deposit. ***However, unlike with the deposit construction, once a transaction is signed and included in a block, there is a commitment to participate.*** For this reason, state transitions should include a signature, state updates (e.g. destination, amount, token, and any other associated state data), as well as some kind of TTL for expiration and a commitment to a particular block. This TTL, while not required, should be below the time to construct exit proofs to ensure that adversarial exit conditions are known.

The commitment to a block is a commitment by the spender that the entity broadcasting the transaction in the Plasma chain has observed the chain up to that point and is able to enforce proofs and must be after the block in which the output being spent has occurred.

The multi-phase commitment occurs as follows for fast finalization:

1. Alice wants to spend her output in the Plasma chain to Bob in the same Plasma chain (without the full transaction record being submitted on the blockchain). She creates a transaction which spends one of her outputs in the Plasma chain, signs it, and broadcasts the transaction.

2. The transaction is included in a block by validator(s) of the Plasma chain. ***The header is included as part of a block in the parent Plasma chain or root blockchain, ultimately being committed and sealed in the root blockchain***
3. Alice and Bob observes the transaction and signs an acknowledgement that he has seen the transaction and block. This acknowledgement gets signed and included in another Plasma block.(거래가 잘 진행 됐는지 acknowledgement로 확인)

***For slow finalization, only the first step needs to occur. After the acknowledgement occurs, the transaction is considered finalized.***

- The reason the third step exists is to ensure that block availability is ensured with the participants (Alice and Bob).

***This third step is not required, but there will be significant delays in finality.*** The rationale is that a transaction should not be viewed as finalized until the block validity and information availability can be proven by all parties relevant to the transaction.

If blocks are being withheld before finalization (between step 1 and 2) and Alice and/or Bob observes this, then Alice may withdraw her unfinalized funds. If blocks are being withheld after step 2 but before step 3, then it is presumed that Bob has sufficient information to withdraw funds, but since neither Alice nor Bob have fully committed to the payment, then it is not treated as complete, depending on information availability either party may theoretically be able to claim the funds. If both parties sign off on Step 3, then it is presumed that it is truly finalized.

***Pay-to-contract-hash enforcement occurs after this step has been completed, specifically when the signatures are provably observed on-chain.*** In the event one party refuses to sign or blocks are being withheld, it is conditional upon a redemption proof. As all states are eventually committed to the chain via merkle proofs, there is less of a reliance on pay-to-contract-hash as payments are provable and enforcible after finalization.

Note that Step 3 can be conditional upon a smart contract instead of a signature by both parties. This allows for mutli-chain or multi-transaction atomicity. Contract creation complexity may be increased, and writing higher level languages/tooling around this may be needed if these features are desired.

> atomicity : 하나의 원자 트랜젝션은 모두 성공하거나 모두 실패해야 함. (e.g., 항공권의 날짜 선택 - 좌석 선택 - 지불)

### 5.5 Periodic Commitments to the Root Chain

The Plasma chain must be able to create ordering of the blockchain. ***In a Plasma chain, there is ordering within blocks, but the blocks are not attested to and ordered themselves on its own.*** As a result, it's necessary to create a commitment on the root blockchain. The Plasma chain publishes its block header on the root chain and its header is enforced by the fraud proofs.

### 5.6 Withdrawals

Plasma allows one to deposit funds of the native coin and tokens (i.e. ETH and ERC-20 tokens) off the root blockchain. It additionally allows for state transitions within the Plasma blockchain whose state is enforced by the root blockchain provided there is information availability. In the event of information availability failures, there is a need to do a mass exit on this Plasma chain. Finally, it's also possible to do a simple withdrawal of funds held in the Plasma chain.
However, in normal operation, one can do a simple withdrawal.

#### 5.6.1 Simple Withdrawal

***For a simple withdrawal, one is only allowed to withdraw funds which have been committed in the root blockchain and ultimately finalized in the Plasma chain.***

Withdrawals are the most critical component, as this ensures the fungibility(바꿀 수 있음) of coins between the root blockchain and the child Plasma chains. If one is able to deposit funds onto the Plasma chain, do state transitions (i.e. transfer coins to other parties), and those parties capable of withdrawing funds, then the value should closely map to the value of coins on the root chain. In some cases, funds on Plasma can be more useful, as it has greater transaction capacity, while the security is ultimately dependent upon the root chain.

All participants of the Plasma chain MUST validate all parent Plasma chains and the root blockchain to ensure that there are no withdrawals in-progress for particular accounts/outputs when updating state. ***If a withdrawal is in progress, a subsequent block cannot spend*** the coins/tokens, any byzantine behavior here violates consensus and is subject to fraud proof, penalization, and block reversal per the Plasma contract in the root blockchain.

A withdrawal occurs in the following steps:

1. A signed withdrawal transaction is submitted to the root blockchain or parent Plasma chain. ***The amount being withdrawn must be whole outputs (no partial withdrawals). Multiple outputs may be withdrawn, but they must all be within the same Plasma chain.*** The output bitmap position is disclosed as part of the withdrawal. An additional bond is placed as part of the withdrawal to penalize false withdrawal requests.
2. A predefined timeout period exists to allow for disputes. This is similar to the dispute period in Lightning Network. In this case, ***if anyone can prove an output has already been spent in the chain being withdrawn to (in many cases, the root blockchain), then the withdrawal is canceled and the bonded withdrawal request is lost.*** 
3. A second time delay exists to wait for timeouts of any other withdrawal requests with a LOWER block confirmation height. This is to force ordered withdrawal in a particular Plasma chain or root chain.
4. If the agreed dispute time period defined in the Plasma smart contract has elapsed and no fraud proofs are provided on the root or parent chain, then it is presumed that the withdrawal is correct and the withdrawer will be able to redeem their funds on the root/parent chain. Withdrawals are processed in the order of old to new in terms of the UTXO/account age.

#### 5.6.2 Fast Withdrawal

***A fast withdrawal is the same construction as the simple withdrawal, but the funds are being sent to a contract which operates an atomic swap.*** A fast withdrawal is not instant. However, it significantly reduces the time to withdrawal down to the time for transaction finality provided that the Plasma chain is not Byzantine (including conducting block withholding). For this reason, a ***fast withdrawal swap is not possible during block withholding attacks, and a slow mass withdrawal request would instead be necessary.***
A fast withdrawal occurs in the following steps:

1. Alice wants to withdraw funds to the root blockchain but doesn't want to wait. ***She's willing to pay the time-value for that convenience.*** Larry (the Liquidity Provider) is willing to provide this as a service. Alice and Larry coordinate to do a withdrawal to the root blockchain. The Plasma blockchain is presumed to be non-Byzantine.
2. Funds are locked to a contract on a particular output in the Plasma chain. This occurs in a manner similar to a normal transfer, in that both parties broadcast a transaction, and then later commit that they have seen the transaction in a Plasma block. The terms of this contract is that if a contract is broadcast on the root blockchain and has been finalized, then the payment will go through in the Plasma chain. If no transaction proof can be provided, then Alice can redeem the funds.
3. After the above Plasma block has finalized and Larry is confident he can redeem funds in the event the contract conditions are met, Larry creates an on-chain contract which enables payment to Alice for the specified amount (the amount he will receive less the fee he charges for this service)

> Hash Time Lock Contract(HTLC) : 블록체인간 암호화폐를 교환 할 때 해시타임락을 이용한 거래 방식. 한쪽이 암호화폐를 전송했는데 일정 시간 안에 상대방이 코인을 전송하지 않으면 자동으로 거래가 취소 됨.

### 5.7 Adversarial mass Withdrawal