# Cosmos - A Network of Distributed Ledger

## 1. Introduction

An ideal solution is one that allows multiple parallel blockchains to interoperate while retaining their security properties. This has proven difficult, if not impossible, with proof-of-work. Merged mining, for instance, allows the work done to secure a parent chain to be reused on a child chain, but transactions must still be validated, in order, by each node, and a merge-mined blockchain is vulnerable to attack if a majority of the hashing power on the parent is not actively merge-mining the child. An academic review of alternative blockchain network architectures is provided for additional context, and we provide summaries of other proposals and their drawbacks in Related Work.

Here we present Cosmos, a novel blockchain network architecture that addresses all of these problems. ***Cosmos is a network connecting many independent blockchains, called zones.***

-  The zones are powered by Tendermint Core, which provides a high-performance, consistent, secure PBFT(*Practical Byzantine Fault Tolerance*)-like consensus engine, where strict fork-accountability guarantees hold over the behaviour of malicious actors.

Tendermint Core's BFT consensus algorithm is well suited for scaling public proof-of-stake blockchains. ***Blockchains with other consensus models, including proof-of-work lockchains like Ethereum and Bitcoin can be connected to the Cosmos network using adapter zones.***

***The first zone on Cosmos is called the Cosmos Hub.*** 

- The Cosmos Hub is a multi-asset proof-of-stake cryptocurrency with a simple governance mechanism which enables the network to adapt and upgrade.
- In addition, the Cosmos Hub can be extended by connecting other zones.

***The hub and zones of the Cosmos network communicate with each other via an inter-blockchain communication (IBC) protocol, a kind of virtual UDP or TCP for blockchains.***

- Tokens can be transferred from one zone to another securely and quickly without the need for exchange liquidity between zones. Instead, ***all inter-zone token transfers go through the Cosmos Hub, which keeps track of the total amount of tokens held by each zone.***
- The hub isolates each zone from the failure of other zones. Because anyone can connect a new zone to the Cosmos Hub, ***zones allow for future-compatibility with new blockchain innovations.***

> zone에 블록체인이 존재하고 이 블록체인들을 연결해주는게 cosmos

## 2. Tendermint

### 2.1 Validators

In classical Byzantine fault-tolerant (BFT) algorithms, each node has the same weight.

In Tendermint, nodes have a non-negative amount of *voting power*, and nodes that have positive voting power are called *validators*. Validators participate in the consensus protocol by broadcasting cryptographic signatures, or *votes*, to agree upon the next block.

- Validators' voting powers are determined at genesis, or are changed deterministically by the blockchain, depending on the application.  For example, in a proof-of-stake application such as the Cosmos Hub, the voting power may be determined by the amount of staking tokens bonded as collateral.

*NOTE: Fractions like ⅔ and ⅓ refer to fractions of the total voting power, never the total number of validators, unless all the validators have equal weight. >⅔ means "more than ⅔", ≥⅓ means "at least ⅓".*

### 2.2 Consensus

***Tendermint is a partially synchronous BFT consensus protocol*** derived from the DLS(Depth Limit Search) consensus algorithm. 

- The protocol requires a fixed known set of validators, where each validator is identified by their public key.
- Voting for consensus on a block proceeds in rounds.
  - ***Each round has a round-leader, or proposer, who proposes a block.***
  - The validators then vote, in stages, on whether to accept the proposed block or move on to the next round.
  - The proposer for a round is chosen deterministically from the ordered list of validators, ***in proportion to their voting power.***

Tendermint’s security derives from its use of ***optimal Byzantine fault-tolerance via super-majority (>⅔) voting and a locking mechanism.***  Together, they ensure that:

- ≥⅓ voting power must be Byzantine to cause a violation of safety, where more than two values are committed.
- If any set of validators ever succeeds in ***violating safety***, or even attempts to do so, ***they can be identified by the protocol.*** This includes both voting for conflicting blocks and broadcasting unjustified votes

### 2.3 Light Clients

A major benefit of Tendermint's consensus algorithm is simplified light client security, making it an ideal candidate for mobile and internet-of-things use cases.  While a Bitcoin light client must sync chains of block headers and find the one with the most proof of work, ***Tendermint light clients need only to keep up with changes to the validator set, and then verify the >⅔ PreCommits in the latest block to determine the latest state***

### 2.4 Preventing Attacks

These are discussed more fully in the [appendix](https://github.com/cosmos/cosmos/blob/master/WHITEPAPER.md#appendix).

### 2.5 ABCI

***The Tendermint consensus algorithm is implemented in a program called Tendermint Core.***

- Tendermint Core is an application-agnostic "consensus engine" that can turn any deterministic blackbox application into a distributedly replicated blockchain.
- Tendermint Core connects to blockchain applications via the Application Blockchain Interface (ABCI). 
  - ***Thus, ABCI allows for blockchain applications to be programmed in any language, not just the programming language that the consensus engine is written in.*** 
  - Additionally, ABCI makes it possible to easily swap out the consensus layer of any existing blockchain stack.

If one wanted to create a Bitcoin-like system on top of ABCI, Tendermint Core would be responsible for

- Sharing blocks and transactions between nodes
- Establishing a canonical/immutable order of transactions (the blockchain)

Meanwhile, the ABCI application would be responsible for

- Maintaining the UTXO database
- Validating cryptographic signatures of transactions
- Preventing transactions from spending non-existent funds
- Allowing clients to query the UTXO database

Tendermint is able to decompose the blockchain design by offering a very simple API between the application process and consensus process.

## 3. Cosmos Overview

>Cosmos : network 이름
>
>Cosmos hub : network의 첫번째 블록체인
>
>Tendermint : 합의 알고리즘 이름

***Cosmos is a network of independent parallel blockchains that are each powered by classical BFT consensus algorithms like Tendermint***

***The first blockchain in this network will be the Cosmos Hub.***

- The Cosmos Hub connects to many other blockchains (or *zones*) via a novel inter-blockchain communication protocol.
- The Cosmos Hub tracks numerous token types and keeps record of the total number of tokens in each connected zone. ***Tokens can be transferred from one zone to another securely and quickly without the need for a liquid exchange between zones, because all inter-zone coin transfers go through the Cosmos Hub.***

### 3.1 Tendermint-BFT

***The Cosmos Hub is the first public blockchain in the Cosmos Network, powered by Tendermint's BFT consensus algorithm.***

Unlike other blockchain consensus systems, ***Tendermint offers instant and provably secure mobile-client payment verification.***

- Since the Tendermint is designed to never fork at all, mobile wallets can receive instant transaction confirmation, which makes trustless and practical payments a reality on smartphones.

Validators in Cosmos have a similar role to Bitcoin miners, but instead use cryptographic signatures to vote. Validators are secure, dedicated machines that are responsible for committing blocks.

-  ***Non-validators can delegate their staking tokens (called "atoms") to any validator to earn a portion of block fees and atom rewards***
- But they incur the risk of getting punished (slashed) if the delegate validator gets hacked or violates the protocol.
- The proven safety guarantees of Tendermint BFT consensus, and the collateral(담보물, 부수적인, 이차적인) deposit of `stakeholders--validators` and `delegators--provide` provable, quantifiable security for nodes and light clients.

### 3.2 Governance

***Distributed public ledgers should have a constitution(구조) and a governance system.*** 

Validators and delegators on the Cosmos Hub can vote on proposals that can change preset(미리 결정하다) parameters of the system automatically (such as the block gas limit), coordinate upgrades, as well as vote on amendments to the human-readable constitution that govern the policies of the Cosmos Hub.

Each zone(Blockchain) can also have their own constitution and governance mechanism as well. For example, the Cosmos Hub could have a constitution that enforces immutability at the Hub (no roll-backs, save for bugs of the Cosmos Hub node implementation), while each zone can set their own policies regarding roll-backs.

By enabling interoperability among differing policy zones, the Cosmos network gives its users ultimate freedom and potential for permissionless experimentation.

## 4. The Hub and Zones

>hub : 최초의 블록체인
>
>zoens : 그 후에 생성된 다른 블록체인

Here we describe a novel model of decentralization and scalability.  Cosmos is a network of many blockchains powered by Tendermint.  While existing proposals aim to create a "single blockchain" with total global transaction ordering, Cosmos permits many blockchains to run concurrently with one another while retaining interoperability.

At the basis, ***the Cosmos Hub manages many independent blockchains called "zones" (sometimes referred to as "shards", in reference to the database scaling technique known as "sharding").*** 

- A constant stream of recent block commits from zones posted on the Hub allows the Hub to keep up with the state of each zone.
- Likewise, each zone keeps up with the state of the Hub (but zones do not keep up with each other except indirectly through the Hub).
- ***Packets of information are then communicated from one zone to another by posting Merkle-proofs as evidence that the information was sent and received.*** 
  - This mechanism is called ***inter-blockchain communication, or IBC for short.***

***Any of the zones can themselves be hubs to form an acyclic graph, but for the sake of clarity we will only describe the simple con guration where there is only one hub, and many non-hub zones.***

### 4.1 The hub

***The Cosmos Hub is a blockchain that hosts a multi-asset distributed ledger,*** where tokens can be held by individual users or by zones themselves.

- These tokens can be moved from one zone to another in a special IBC packet called a ***"coin packet"***.
-  The hub is responsible for preserving the global invariance of the total amount of each token across the zones. IBC coin packet transactions must be committed by the sender, hub, and receiver blockchains.

***Since the Cosmos Hub acts as the central ledger for the whole system, the security of the Hub is of paramount(다른 무엇보다 중요한) importance.*** 

- While each zone may be a Tendermint blockchain that is secured by as few as 4 (or even less if BFT consensus is not needed)
- The Hub must be secured by a globally decentralized set of validators that can withstand the most severe attack scenarios, such as a continental network partition or a nation-state sponsored attack.

> zone은 최소 4개 필요,
>
> Hub는 globally decentralized

### 4.2 The Zones

***A Cosmos zone is an independent blockchain that exchanges IBC messages with the Hub.*** 

From the Hub's perspective

- A zone is a multi-asset dynamic-membership multi-signature account that can send and receive tokens using IBC packets. Like a cryptocurrency account
- A zone cannot transfer more tokens than it has, but can receive tokens from others who have them.
- A zone may be designated as an "source" of one or more token types, granting it the power to inflate that token supply.

***Atoms of the Cosmos Hub may be staked by validators of a zone connected to the Hub.***  While double-spend attacks on these zones would result in the slashing of atoms with Tendermint's fork-accountability(책임이 있음), a zone where >⅔ of the voting power are Byzantine can commit invalid state.

***The Cosmos Hub does not verify or execute transactions committed on other zones, so it is the responsibility of users to send tokens to zones that they trust.***

## 5. Inter-Blockchain Communication

Now we look at how the Hub and zones communicate with each other.

For example, if there are three blockchains, "Zone1", "Zone2", and "Hub", and we wish for "Zone1" to produce a packet destined for "Zone2" going through "Hub".

- ***To move a packet from one blockchain to another, a proof is posted on the receiving chain.*** The proof states that the sending chain published a packet for the alleged destination.
- ***For the receiving chain to check this proof, it must be able keep up with the sender's block headers.*** This mechanism is similar to that used by sidechains, which requires two interacting chains to be aware of one another via a bidirectional stream of proof-of-existence datagrams (transactions).

The IBC protocol can naturally be defined using two types of transactions:

- An `IBCBlockCommitTx` transaction, which allows a blockchain ***to prove to any observer of its most recent block-hash***
- And an `IBCPacketTx` transaction, which allows a blockchain ***to prove to any observer that the given packet was indeed published by the sender's application, via a Merkle-proof to the recent block-hash.***

By splitting the IBC mechanics into two separate transactions, we allow the native fee market-mechanism of the receiving chain to determine which packets get committed (i.e. acknowledged), while allowing for complete freedom on the sending chain as to how many outbound packets are allowed.

In the example above, in order to update the block-hash of "Zone1" on "Hub" (or of "Hub" on "Zone2"),

- an `IBCBlockCommitTx` transaction must be posted on "Hub" with the block-hash of "Zone1" (or on "Zone2" with the block-hash of "Hub"). 

## 6. Use Cases

### 6.1 Distributed Exchange

we can make exchanges less vulnerable to external and internal hacks by running it on the blockchain.  We call this a distributed exchange.

***What the cryptocurrency community calls a decentralized exchange today are based on something called "atomic cross-chain" (AXC) transactions.***  With an AXC transaction, two users on two different chains can make two transfer transactions that are committed together on both ledgers, or none at all (i.e. atomically).  For example, two users can trade bitcoins for ether (or any two tokens on two different ledgers) using AXC transactions, even though Bitcoin and Ethereum are not connected to each other.  The benefit of running an exchange on AXC transactions is that neither users need to trust each other or the trade-matching service. ***The downside is that both parties need to be online for the trade to occur.***

Another type of decentralized exchange is a mass-replicated distributed exchange that runs on its own blockchain.  Users on this kind of exchange can submit a limit order and turn their computer off, and the trade can execute without the user being online. The blockchain matches and completes the trade on behalf of the trader.

A centralized exchange can create a deep orderbook of limit orders and thereby attract more traders. Liquidity begets more liquidity in the exchange world, and so there is a strong network effect (or at least a winner-take-most effect) in the exchange business. ***For a decentralized exchange to compete with a centralized exchange, it would need to support deep orderbooks with limit orders. Only a distributed exchange on a blockchain can provide that.***

***Tendermint provides additional benefits of faster transaction commits.*** By prioritizing fast finality without sacrificing consistency, zones in Cosmos can finalize transactions fast -- for both exchange order transactions as well as IBC token transfers to and from other zones.

### 6.2 Bridging to Other Cryptocurrencies

A privileged zone can act as the source of a bridged token of another cryptocurrency. ***A bridge is similar to the relationship between a Cosmos hub and zone;*** both must keep up with the latest blocks of the other in order to verify proofs that tokens have moved from one to the other.

***A "bridge-zone" on the Cosmos network keeps up with the Hub as well as the other cryptocurrency.***

-  The indirection through the bridge-zone allows the logic of the Hub to remain simple and agnostic to other blockchain consensus strategies such as Bitcoin's proof-of-work mining.

#### 6.2.1 Sending Tokens to the Cosmos Hub

>Application Blockchain Interface (ABCI). 

Each bridge-zone validator would run a Tendermint-powered blockchain with a special ABCI bridge-app, but also a full-node of the "origin" blockchain.

- ***When new blocks are mined on the origin, the bridge-zone validators will come to agreement on committed blocks by signing and sharing their respective local view of the origin's blockchain tip.***
- When a bridge-zone receives payment on the origin (and sufficient confirmations were agreed to have been seen in the case of a PoW chain such as Ethereum or Bitcoin), a corresponding account is created on the bridge-zone with that balance.

In the case of Ethereum, the bridge-zone can share the same validator-set as the Cosmos Hub. On the Ethereum side (the origin), a bridge-contract would allow ether holders to send ether to the bridge-zone by sending it to the bridge-contract on Ethereum. The bridge-contract tracks the validator-set of the bridge-zone, which may be identical to the Cosmos Hub's validator-set.

In the case of Bitcoin, the concept is similar except that instead of a single bridge-contract, each UTXO would be controlled by a threshold multisignature P2SH pubscript.  ***Due to the limitations of the P2SH system, the signers cannot be identical to the Cosmos Hub validator-set.***

> 이더리움은 되는데 비트코인은 안됨

#### 6.2.2 Withdrawing Tokens from Cosmos Hub

Ether on the bridge-zone ("bridged-ether") can be transferred to and from the Hub, and later be destroyed with a transaction that sends it to a particular withdrawal address on Ethereum. An IBC packet proving that the transaction occurred on the bridge-zone can be posted to the Ethereum bridge-contract to allow the ether to be withdrawn.

In the case of Bitcoin, the restricted scripting system makes it difficult to mirror the IBC coin-transfer mechanism.  Each UTXO has its own independent pubscript, so every UTXO must be migrated to a new UTXO when there is a change in the set of Bitcoin escrow signers. ***One solution is to compress and decompress the UTXO-set as necessary to keep the total number of UTXOs down.***

> 이더리움은 contract로 되는데 비트코인은 좀 힘듬

#### 6.2.3 Total Accountability(책임) of Bridge Zones

***The risk of such a bridgeging contract is a rogue validator set.  ≥⅓ Byzantine voting power could cause a fork, withdrawing ether from the bridge-contract on Ethereum while keeping the bridged-ether on the bridge-zone.*** Worse, >⅔ Byzantine voting power can steal ether outright from those who sent it to the bridge-contract by deviating from the original bridgeging logic of the bridge-zone.

***It is possible to address these issues by designing the bridge to be totally accountable.***

- For example, all IBC packets, from the hub and the origin, might require acknowledgement by the bridge-zone in such a way that all state transitions of the bridge-zone can be efficiently challenged and verified by either the hub or the origin's bridge-contract.
- The Hub and the origin should allow the bridge-zone validators to post collateral(담보물), and token transfers out of the bridge-contract should be delayed (and collateral unbonding period sufficiently long) to allow for any challenges to be made by independent auditors.
- We leave the design of the specification and implementation of this system open as a future Cosmos improvement proposal, to be passed by the Cosmos Hub's governance system.

> 아직 보안은 완벽하지 않음

### 6.3 Ethereum Scaling

Though the Cosmos Hub and IBC packet mechanics does not allow for arbitrary contract logic execution per se, it can be used to coordinate token movements between Ethereum contracts running on different zones, providing a foundation for token-centric Ethereum scaling via sharding.

#### 6.3.1 Multi-Application Integration

As a multi-asset blockchain, a single transaction may contain multiple inputs and outputs, where each input can be any token type, enabling Cosmos to serve directly as a platform for decentralized exchange, though orders are assumed to be matched via other platforms. Alternatively, a zone can serve as a distributed fault-tolerant exchange (with orderbooks), which can be a strict improvement over existing centralized cryptocurrency exchanges which tend to get hacked over time.

>  Bitcoin이나 ethereum 같은 여러 mainnet들이 tendermint consensus를 통해 zone이 될 수 있음

#### 6.3.2 Network Partition Mitigation

Some claim that a major problem with consistency-favouring consensus algorithms like Tendermint is that any network partition which causes there to be no single partition with >⅔ voting power (e.g. ≥⅓ going offline) will halt consensus altogether. ***The Cosmos architecture can help mitigate this problem by using a global hub with regional autonomous zones, where voting power for each zone are distributed based on a common geographic region.***  Note that this allows real geological, political, and network-topological features to be considered in designing robust federated fault-tolerant systems.

#### 6.3.3 Federated Name Resolution System

NameCoin was one of the first blockchains to attempt to solve the name-resolution problem by adapting the Bitcoin blockchain. Unfortunately there have been several issues with this approach.

With Namecoin, we can verify that, for example, *@satoshi* was registered with a particular public key at some point in the past

- but ***we wouldn’t know whether the public key had since been updated recently unless we download all the blocks since the last update of that name.***
- This is due to the limitation of Bitcoin's UTXO transaction Merkle-ization model, where only the transactions (but not mutable application state) are Merkle-ized into the block-hash.

This lets us prove existence, but not the non-existence of later updates to a name. ***Thus, we can't know for certain the most recent value of a name without trusting a full node, or incurring significant costs in bandwidth by downloading the whole blockchain.***

Even if a Merkle-ized search tree were implemented in NameCoin, its dependency on proof-of-work makes light client verification problematic.

- Light clients must download a complete copy of the headers for all blocks in the entire blockchain (or at least all the headers since the last update to a name).
- This means that the bandwidth requirements scale linearly with the amount of time. In addition, name-changes on a proof-of-work blockchain requires waiting for additional proof-of-work confirmation blocks, which can take up to an hour on Bitcoin.

***With Tendermint, all we need is the most recent block-hash signed by a quorum of validators (by voting power), and a Merkle proof to the current value associated with the name.*** This makes it possible to have a succinct, quick, and secure light-client verification of name values.

## 7 Issuance and Incentive

### 7.1 The Atom Token

While the Cosmos Hub is a multi-asset distributed ledger, there is a special native token called the ***atom***. Atoms are the only staking token of the Cosmos Hub. Atoms are a license for the holder to vote, validate, or delegate to other validators. Additional inflationary atoms and block transaction fees are rewarded to validators and delegators who delegate to validators.

### 7.2 Limitations on the Number of Validators

Unlike Bitcoin or other proof-of-work blockchains, ***a Tendermint blockchain gets slower with more validators due to the increased communication complexity.*** Fortunately, we can support enough validators to make for a robust globally distributed blockchain with very fast transaction confirmation times, and, as bandwidth, storage, and parallel compute capacity increases, we will be able to support more validators in the future.

On genesis day, the maximum number of validators will be set to 100, and this number ***will increase at a rate of 13% for 10 years, and settle at 300 validators.***

```
Year 0: 100
Year 1: 113
Year 2: 127
Year 3: 144
Year 4: 163
Year 5: 184
Year 6: 208
Year 7: 235
Year 8: 265
Year 9: 300
Year 10: 300
...
```

### 7.3 Becoming a Validator After Genesis Day

Atom holders who are not already can become validators by signing and submitting a `BondTx` transaction.  The amount of atoms provided as collateral(담보물) must be nonzero. ***Anyone can become a validator at any time, except when the size of the current validator set is greater than the maximum number of validators allowed.*** 

- In that case, the transaction is only valid if the amount of atoms is greater than the amount of effective atoms held by the smallest validator, where effective atoms include delegated atoms.
- When a new validator replaces an existing validator in such a way, the existing validator becomes inactive and all the atoms and delegated atoms enter the unbonding state.

### 7.4 Penalties for Validators

There must be some penalty imposed on the validators for any intentional or unintentional deviation from the sanctioned protocol. Such evidence will result in the validator losing its good standing and its bonded atoms as well its proportionate share of tokens in the reserve pool -- collectively called its "stake" -- will get slashed.

Sometimes, validators will not be available, either due to regional network disruptions, power failure, or other reasons.  If, at any point in the past `ValidatorTimeoutWindow` blocks, a validator's commit vote is not included in the blockchain more than `ValidatorTimeoutMaxAbsent` times, that validator will become inactive, and lose `ValidatorTimeoutPenalty` (DEFAULT 1%) of its stake.

Some "malicious" behavior does not produce obviously discernible evidence on the blockchain. ***In these cases, the validators can coordinate out of band to force the timeout of these malicious validators, if there is a supermajority consensus.***

***In situations where the Cosmos Hub halts due to a ≥⅓ coalition of voting power going offline, or in situations where a ≥⅓ coalition of voting power censor evidence of malicious behavior from entering the blockchain, the hub must recover with a hard-fork reorg-proposal.***

### 7.5 Transaction Fees

Cosmos Hub validators can accept any token type or combination of types as fees for processing a transaction. Each validator can subjectively set whatever exchange rate it wants, and choose whatever transactions it wants, as long as the `BlockGasLimit` is not exceeded. ***The collected fees, minus any taxes specified below, are redistributed to the bonded stakeholders in proportion to their bonded atoms, every `ValidatorPayoutPeriod` (DEFAULT 1 hour).***

> 1시간마다 수수료 분배 됨

Of the collected transaction fees, `ReserveTax` (DEFAULT 2%) will go toward the reserve pool to increase the reserve pool and increase the security and value of the Cosmos network. These funds can also be distributed in accordance with the decisions made by the governance system.

Atom holders who delegate their voting power to other validators pay a commission to the delegated validator.  The commission can be set by each validator.

### 7.6 Incentivizing Hackers

The security of the Cosmos Hub is a function of the security of the underlying validators and the choice of delegation by delegators. In order to encourage the discovery and early reporting of found vulnerabilities, the Cosmos Hub encourages hackers to publish successful exploits via a `ReportHackTx` transaction that says, "This validator got hacked.  Please send bounty to this address".  Upon such an exploit, the validator and delegators will become inactive, `HackPunishmentRatio` (default 5%) of everyone's atoms will get slashed, and `HackRewardRatio` (default 5%) of everyone's atoms will get rewarded to the hacker's bounty address.  The validator must recover the remaining atoms by using their backup key.

> Hacking된 검증자 발견하고 보고하면 보상 지금

In order to prevent this feature from being abused to transfer unvested atoms, the portion of vested vs unvested atoms of validators and delegators before and after the `ReportHackTx` will remain the same, and the hacker bounty will include some unvested atoms, if any.

### 7.7 Governance Specification

The Cosmos Hub is operated by a distributed organization that requires a well-defined governance mechanism in order to coordinate various changes to the blockchain, such as the variable parameters of the system, as well as software upgrades and constitutional amendments.

All validators are responsible for voting on all proposals. ***Failing to vote on a proposal in a timely manner will result in the validator being deactivated automatically for a period of time called the `AbsenteeismPenaltyPeriod` (DEFAULT 1 week).***

Delegators automatically inherit the vote of the delegated validator. This vote may be overridden manually. Unbonded atoms get no vote.

***Each proposal requires a deposit of `MinimumProposalDeposit` tokens, which may be a combination of one or more tokens including atoms.*** For each proposal, the voters may vote to take the deposit. If more than half of the voters choose to take the deposit (e.g. because the proposal was spam), the deposit goes to the reserve pool, except any atoms which are burned.

For each proposal, voters may vote with the following options:

- Yea
- YeaWithForce
- Nay
- NayWithForce
- Abstain

A strict majority of Yea or YeaWithForce votes (or Nay or NayWithForce votes) is required for the proposal to be decided as passed (or decided as failed), but 1/3+ can veto the majority decision by voting "with force".  When a strict majority is vetoed, everyone gets punished by losing `VetoPenaltyFeeBlocks` (DEFAULT 1 day's worth of blocks) worth of fees (except taxes which will not be affected), and the party that vetoed the majority decision will be additionally punished by losing `VetoPenaltyAtoms` (DEFAULT 0.1%) of its atoms.

## 8. Related Work

There have been many innovations in blockchain consensus and scalability in the past couple of years.  This section provides a brief survey of a select number of important ones.

### 8.1 Consensus Systems

#### 8.1.1 Classic Byzantine Fault Tolerance

Early solutions were discovered for synchronous networks where there is an upper bound on message latency. It was not until the late 90s that Practical Byzantine Fault Tolerance (PBFT) was introduced as an efficient partially synchronous consensus algorithm able to tolerate up to ⅓ of processes behaving arbitrarily.  PBFT became the standard algorithm, spawning many variations, including most recently one created by IBM as part of their contribution to Hyperledger.

***The main benefit of Tendermint consensus over PBFT is that Tendermint has an improved and simplified underlying structure, some of which is a result of embracing the blockchain paradigm.*** Tendermint blocks must commit in order, which obviates(제기하다) the complexity and communication overhead associated with PBFT's view-changes. 

In addition, the batching of transactions into blocks allows for regular Merkle-hashing of the application state, rather than periodic digests as with PBFT's checkpointing scheme.

- This allows for faster provable transaction commits for light-clients and faster inter-blockchain communication.

Tendermint Core also includes many optimizations and features that go above and beyond what is specified in PBFT.

- For example, the blocks proposed by validators are split into parts, Merkle-ized, and gossipped in such a way that improves broadcasting performance.
- Also, Tendermint Core doesn't make any assumption about point-to-point connectivity, and functions for as long as the P2P network is weakly connected.

#### 8.1.2 Stellar

Building on an approach pioneered by Ripple, Stellar refined a model of Federated Byzantine Agreement wherein the processes participating in consensus do not constitute(~을 구성하다) a fixed and globally known set.

- Rather, each process node curates one or more "quorum slices", each constituting a set of trusted processes.
- A "quorum" in Stellar is defined to be a set of nodes that contain at least one quorum slice for each node in the set, such that agreement can be reached.

***The security of the Stellar mechanism relies on the assumption***

- The intersection of any two quorums is non-empty, while the availability of a node requires at least one of its quorum slices to consist entirely of correct nodes, ***creating a trade-off between using large or small quorum-slices that may be difficult to balance without imposing significant assumptions about trust.***

  > 만약 quorum이 크면 합의가 쉽지만 cheating도 쉬워짐
  >
  > 만약 quorum이 작으면 cheating이 힘들어지지만 합의도 어려워짐
  >
  > 맞나...?

- Ultimately, nodes must somehow choose adequate quorum slices for there to be sufficient fault-tolerance (or any "intact nodes" at all, of which much of the results of the paper depend on), and the only provided strategy for ensuring such a configuration is hierarchical and similar to the Border Gateway Protocol (BGP), used by top-tier ISPs on the internet to establish global routing tables, and by that used by browsers to manage TLS certificates; both notorious for their insecurity.

The criticism in the Stellar paper of the Tendermint-based proof-of-stake systems is mitigated by the token strategy described here, wherein a new type of token called the *atom* is issued that represent claims to future portions of fees and rewards. The advantage of Tendermint-based proof-of-stake, then, is its relative simplicity, while still providing sufficient and provable security guarantees.

#### 8.1.3 BitcoinNG

BitcoinNG is a proposed improvement to Bitcoin that would allow for forms of vertical scalability, ***such as increasing the block size, without the negative economic consequences typically associated with such a change, such as the disproportionately large impact on small miners.*** This improvement is achieved by separating leader election from transaction broadcast: leaders are first elected by proof-of-work in "key-blocks", and then able to broadcast transactions to be committed until a new key-block is found. This reduces the bandwidth requirements necessary to win the PoW race, allowing small miners to more fairly compete, and allowing transactions to be committed more regularly by the last miner to find a key-block.

#### 8.1.4 Casper

***Casper is a proposed proof-of-stake consensus algorithm for Ethereum.*** Its prime mode of operation is "consensus-by-bet". ***By letting validators iteratively bet on which block they believe will become committed into the blockchain based on the other bets that they have seen so far, finality can be achieved eventually.*** This is an active area of research by the Casper team.  The challenge is in constructing a betting mechanism that can be proven to be an evolutionarily stable strategy. ***The main benefit of Casper as compared to Tendermint may be in offering "availability over consistency" -- consensus does not require a >⅔ quorum of voting power -- perhaps at the cost of commit speed or implementation complexity.***

### 8.2 Horizontal Scaling

#### 8.2.1 Interledger Protocol

The Interledger Protocol is not strictly a scalability solution. It provides an ad hoc(ad hoc : 즉석) interoperation between different ledger systems through a loosely coupled bilateral relationship network. 

- Like the Lightning Network, the purpose of ILP is to facilitate payments
- but it specifically focuses on payments across disparate ledger types, and extends the atomic transaction mechanism to include not only hash-locks, but also a quorum of notaries (called the Atomic Transport Protocol).

The latter mechanism for enforcing atomicity in inter-ledger transactions is similar to Tendermint's light-client SPV mechanism, so an illustration of the distinction between ILP and Cosmos/IBC is warranted, and provided below.

> notary : 공증인

1. The notaries of a connector in ILP do not support membership changes, and do not allow for flexible weighting between notaries. ***On the other hand, IBC is designed specifically for blockchains, where validators can have different weights, and where membership can change over the course of the blockchain.***

2. As in the Lightning Network, the receiver of payment in ILP must be online to send a confirmation back to the sender. In a token transfer over IBC, the validator-set of the receiver's blockchain is responsible for providing confirmation, not the receiving user.

3. The most striking difference is that ILP's connectors are not responsible or keeping authoritative state about payments, ***whereas in Cosmos, the validators of a hub are the authority of the state of IBC token transfers as well as the authority of the amount of tokens held by each zone (but not the amount of tokens held by each account within a zone).*** This is the fundamental innovation that allows for secure asymmetric transfer of tokens from zone to zone; the analog to ILP's connector in Cosmos is a persistent and maximally secure blockchain ledger, the Cosmos Hub.

   > ILP는 책임을 지지 않지만 hub가 IBC토큰 전송 상태의 권한 뿐만 아니라, 토큰의 양에도 권한을 가짐

4. The inter-ledger payments in ILP need to be backed by an exchange orderbook, as there is no asymmetric transfer of coins from one ledger to another, only the transfer of value or market equivalents.

#### 8.2.2 Sidechains

Sidechains are a proposed mechanism for scaling the Bitcoin network via alternative blockchains that are "two-way pegged" to the Bitcoin blockchain. (Two-way pegging is equivalent to bridging. In Cosmos we say "bridging" to distinguish from market-pegging). ***Sidechains allow bitcoins to effectively move from the Bitcoin blockchain to the sidechain and back, and allow for experimentation in new features on the sidechain.***

***As in the Cosmos Hub, the sidechain and Bitcoin serve as light-clients of each other, using SPV proofs to determine when coins should be transferred to the sidechain and back.***  Furthermore, this is a Bitcoin-maximalist solution that doesn't natively support a variety of tokens and inter-zone network topology as Cosmos does. That said, the core mechanism of the two-way peg is in principle the same as that employed by the Cosmos network.

#### 8.2.3 Ethereum Scalability Efforts

##### Cosmos vs Ethereum 2.0 Mauve

Cosmos and Ethereum 2.0 Mauve have different design goals.

- Cosmos is specifically about tokens. Mauve is about scaling general computation.
- Cosmos is not bound to the EVM, so even different VMs can interoperate.
- Cosmos lets the zone creator determine who validates the zone.
- Anyone can start a new zone in Cosmos (unless governance decides otherwise).
- The hub isolates zone failures so global token invariants are preserved.

### 8.3 General Scaling

#### 8.3.1 Lightning Network

***The Lightning Network is a proposed token transfer network operating at a layer above the Bitcoin blockchain (and other public blockchains), enabling improvement of many orders of magnitude in transaction throughput by moving the majority of transactions outside of the consensus ledger into so-called "payment channels".*** 

- This is made possible by on-chain cryptocurrency scripts, which enable parties to enter into bilateral( 쌍방의) stateful contracts where the state can be updated by sharing digital signatures, and ***contracts can be closed by finally publishing evidence onto the blockchain, a mechanism first popularized by cross-chain atomic swaps.***
- By opening payment channels with many parties, participants in the Lightning Network can become focal points for routing the payments of others, leading to a fully connected payment channel network, at the cost of capital being tied up on payment channels.

While the Lightning Network can also easily extend across multiple independent blockchains to allow for the transfer of *value* via an exchange market, it cannot be used to asymmetrically transfer *tokens* from one blockchain to another.

***The main benefit of the Cosmos network described here is to enable such direct token transfers.*** That said, we expect payment channels and the Lightning Network to become widely adopted along with our token transfer mechanism, for cost-saving and privacy reasons.

#### 8.3.2 Segregated Witness

***Segregated Witness is a Bitcoin improvement proposal that aims to increase the per-block transaction throughput 2X or 3X, while simultaneously making block syncing faster for new nodes.***  The brilliance of this solution is in how it works within the limitations of Bitcoin's current protocol and allows for a soft-fork upgrade (i.e. clients with older versions of the software will continue to function after the upgrade).

Tendermint, being a new protocol, has no design restrictions, so it has a different scaling priorities. Primarily, Tendermint uses a BFT round-robin algorithm based on cryptographic signatures instead of mining, which trivially allows horizontal scaling through multiple parallel blockchains, while regular, more frequent block commits allow for vertical scaling as well.