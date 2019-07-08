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