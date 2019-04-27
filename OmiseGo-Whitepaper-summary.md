# OmiseGo whitepaper summary

***Decentralized Exchange and Payment Platform***

***[ERC-712 will be supported (2019.04.14 new roadmap)](https://blog.omisego.network/omisego-roadmap-update-25075416d6f1)***

What is ERC-712?

> TBW
>
> TBW
>
> TBW
>
> TBW

## Abstract

OmiseGO is building a decentralized exchange, liquidity provider mechanism, clearing house messaging network, and asset-backed blockchain gateway. ***OmiseGo is an open distributed network of vaildators which enforce behavior of all participants*** . It uses the mechanism of a protocol token to create a proof-of-stake blockchain to enable enforcement of market activity amongst participants. 

> ERC-20 token : Ethereum Request for Comment
>
> Attribute of ERC-20
>
> - Total Supply : How many token is minted
> - Transfer : A token can be sent 
> - Balance Of : Return token balance of account
> - Transfer From : Token can be transfered
> - Approve : Prevent fake token -> check total amount of token and reject or conclude contraction
> - Allowance : If someone try to send a tokens which are more than their balance, then transaction is canceled or not allowed


## 1. Introduction adn Problem Statement ##

The primary role of blockchains are solve coordination problems among multilateral agreements between a network of participants.

Any single participant is more willing to use systems where the business processes and mechanisms itself are not owned by any other single participants. There is a fundamental coordination problem amongst payment processor, gateways, and financial institutions. These are usually constructed by creating a clearinghouse which manages the interchange, usually via a messaging  network with either a central counter-party clearinghouse or nostro/vostro account.

These  centralized  networks  allow  for  the  controlling  entity  to arbitrarily change the mechanisms, which result in significant amount of transaction costs via information costs, due diligence, and contractual enforcement between all parties.

***We believe that there is currently a large emerging market of disruption in digital payments with new payment platforms (e.g.  Venmo, Alipay, etc.).***

Blockchains allows society to externalize the world’s business processes from single centralized corporations into open, decentralized computing networks. OmiseGO (OMG)is a network which decentralizes market liquidity, order book matching and execution, clearinghouse custodianship, and high-scalability payments to ***help resolve payments across these emerging eWallet payment networks.***



## 2 Design Approach

As  it’s  a  core  function  for  this  decentralized  network  to  do  eWallet  interchange,  a blockchain ledger on OmiseGO is necessary to hold the general balance of funds per eWallet service (or any user/node). This ledger must be able to hold funds across many assets/commodities. However, merely holding a ledger is insufficient for interchange. The mechanism must also allow to trade these assets/commodities.

In order to perform interchange, it requires an order to be placed across many different pairs on an open public market. ***This requires a decentralized orderbook and trading engine.*** The trading engine is built in to the OMG blockchain,  ***orders are published and matches are performed as part of every block  when a matched order has reached sufficient number of validation confirmations.*** This  results  in  a  non-custodial  ***decentralized  exchange  held  by a  single  party  where  the  eWallet  platforms  may  exchange  onto  other  eWallet  platforms without centralized trust on a single entity.***

However, direct crosses between eWallet fiat tokens may not be desirable, as there maybe  too  many. It would be necessary to use cryptocurrency for a liquid market without single preference.  By bonding Ethereum into a smart contract  (or Bitcoin-like tokens into bonded clearinghouses), it is possible to lock up Ether onto the activity of the OMGchain to allow for eWallet pairs to occur over Ether or other cryptocurrencies, creating  liquid  market  (if  every  pair  crosses  with  ETH,  spreads  would  be  much  smaller  provided low  currency  volatility).  

> liquid market means Market Liquidity?
>
> Market Liquidity : It is shown how much assets can be converted into cash

 there’s strong incentive to use decentralized tokens for settlement due to coordination/trust advantages related to programmatic adjudication.

> what is a proof of this sentence?

eWallet fiat tokens may also cross using other eWallet tokens. By  allowing  for  cryptocurrencies  to  be  the  backing  for eWallet platforms, ***the platforms can be assured of an even playing field between eWallet interchange activities.*** This requires a greater degree of liquidity in funds locked up, and the ***OmiseGO decentralized exchange may not be desirable to transact for low-value interchange activity (e.g.for high-volume micropayments)***

> degree of liquidity : 유동성 지표
>
> eWallet with exchange system?
>
> 
>
> eWallet과 거래소를 결합하여 유동성을 확보하겠다.  
>
> && 큰 유동성이 확보 되고, 소수의 거래가 아니라 다수의 소액 거래가 좋음
>
> 
>
> (Combine eWallet and exchange for market liquidity)
>
> &&  (It need high liquidity and prefer high-volume && micorpayments) 

 There is an expectation, that eWallets will hold some reserve of fiat tokens of other eWallets, ready to be used for smaller transfers in popular directions. ***Constructions  such  as  Lightning  Network  allow  for payments  to  occur off-chain  when eWallets hold balances to facilitate rapid payments.***  Implementations allow for payments across Bitcoin and Ethereum, which can be easily ported to the OMG chain for eWallet balances

> Constructions  such  as  Lightning  Network  allow  for payments  to  occur off-chain  when eWallets hold balances to facilitate rapid payments
>
> Is it possible?

The result of the OmiseGO blockchain construction is it allows for eWallet interchange,supported by a decentralized exchange, ***cryptocurrency (e.g.  ETH) matching***, orderbook, and clearinghouses ***without full-custody trust.***

> full-custody : 완전 양육권 -> 완전 양도 의미하는 듯
>
> ->완전 양도를 신탁 없이 진행(신탁 : 재산을 운용 할 수 없을때 신회 할 수 있는 개인에게 그 재산의 관리 또는 처분을 의뢰하는 것)
>
> -> it is ***necessary a centralized something to change assets to cash*** But ***omiseGo will implement decentralized exchange.***

### 2.1 Decentralized Liquidity Hub for Channels

The construction has the additional benefit of allowing for a decentralized liquidity pool to be created for use with payment channels on various cryptocurrencies, such as Bitcoin 

> For faster exchange, Lightning Network is needed
>
> -> Ordinary exchange is take a time or computer power

However, Lightning Network faces significant pressure around network effects with capital, it’s desirable to prevent liquidity pools from centralizing to a single trusted entity. 

***We can create a Lightning Network hub which is not owned by any single individual on tokens which support more complex smart contracts.***  For currencies with  simple smart contracts, ***any node on  the network (e.g.  Bitcoin network) can act as a gateway into the OMG chain pool*** and cross back with any other participant.  

 -> This allows the OmiseGO chain to offload a lot of on-chain activity, while encouraging decentralization.

For Ethereum in particular (and other full-featured smart-contract scripting blockchains), all participants  set up channels into an ETH smart contract operating as a single pool of funds. ***The chain state of the OMG chain reflects the current balance of participants.*** This allows for any participant to supply liquidity onto this network which can be allocated in accordance to the OMG-chain consensus rules (limits may be in place early on to prevent this blockchain from sucking up all the spare liquidity from the cryptocurrency space if this construction is successful before robust testing/validation overtime).  These funds can thereby be used for any liquidity activity on the OMG chain.

> If this construction is successful before robust testing/validation overtime, limits may be in place early on to prevent this blockchain (from sucking up all the spare liquidity) from the cryptocurrency space 

> suck up : 아첨하다

## 3. Blockchain Overview and Mechanism

The above mechanisms require significant volume of activity (with a large amount of state),

> The more nodes can make blockchain robust

And is not at this time suitable for all activity to occur on the Ethereum main chain, ***however the construction would be to bond trading activity in the public Ethereum chain with contract execution input being provided by the OMG chain.*** We are building a blockchain which hooks into other blockchains ***to allow for trading across token/asset classes, largely backed by Ether.***

  The OMG chain validates the activity of the

***The OMG chain validates the activity of the behavior of all participants (including activity on other chains).***  In other words, the role of the OMG token is providing computation and enforcement. The token itself acts as a bond for its activity on this blockchain, ***improper activity results in the token/bond being burned on the OMG chain.*** By creating a custom chain with deep enforcement, we are able to construct a system where consensus rules optimize for high-performant activity.

The design optimizes for rapid execution and clearing, with slower settlement. ***Future iterations may include sharding of the OMG chain, but the initial iteration will presume high-throughput capacity for block propagation.***

> performant : 성능 기준에 맞는
>
> Sharding 
>
> - In DB, It is the way for distributing date save system
> - Shading is a kind of solutions for solving scalability problem like Raiden network or Plasma
>
> Off-chain
>
> Off-chain system is designed for reduce the number of transaction which are written in block. For example make a kind of box(It is called channel in blockchain system) and payer and payee put there cryptocurrency in the box. Therefore,   in box, there are no transaction fee and waiting for block confirmation . After finishing all of transactions, they only send a final result of transactions to nodes. So It can reduce transaction fee and waiting time
>
> Raiden network
>
> : It is off-chain system like lightning network of bitcoin & lightcoin. Transaction channel will be network For example, A connect with B, and B connect with C. then as a result A is connected with C. like this off-chain system will be a netwrok
>
> Plasma
>
> : Raiden network is using off-chain system, but Plasma is using Child-chain. Currently DApp is writing all of data in etherium chain. it makes system slow, increase block size and gas consumption.
>
> So Plasma makes separate channel and only minimum data is synced with etherium main blockchain.
>
> - Reduce size of etherium
> - Fast DApp
> - Reduce gas wasting
>
> Sharding
>
> :Similar with DB shadding -> separate data for boosting speed
>
> ***Need all of these project for synergy effect***

Owning  OMG  tokens  buys  the  right  to  validate  this  blockchain, within its consensus rules. Transaction fees on the network including (but not limited to) payment, interchange, trading, and clearinghouse use, are given to non-faulty validators who enforce bonded contract states

***The token will have value derived from the fees derived from this network, with the obligation/cost of providing validation to its users.***  This token must have value, to prevent low-cost attacks and is necessary to enforce this network.

> low-cost attack?

As this will be designed as a high-performant system, an linked-via-proof  blockchain construction is necessary.   We expect that this system will be able to handle extremely high volumes of transactions and hence, will only do final delivery over Ethereum. 

Consensus rules are enforced via this proof-of-stake network.  ***As part of the consensus rules of this network, it is required that all OMG (Omise GO) validators also run the Ethereum network to validate  in  parallel, resulting in Ethereum as a first-class citizen with regards to inter-blockchain validation***

> BLS : Boneh — Lynn — Shacham

The OMG blockchain manages matching and managing order execution on the Ethereum chain.  Activity on  the OMG ensures the validator activity also may be enforced on the Ethereum chain via native Ethereum smart contracts. 

For Bitcoin and Bitcoin-like systems, we allow for trading via a clearinghouse network on the Lightning Network. The blockchain enforces activity on this network via committed proofs. ***While not as robust as Ethereum’s network,  it allows for near-instantaneous clearing and settlement of activity orchestrated on  the OMG chain without full-node validation.***  We expect to do partial validation in the future for nodes which do not allow for blockchain reorganization.

> instantaneous : 즉각적인
>
> orchestrate : (오케스트라용으로) 편곡하다, (은밀히)조직하다

 We hope OmiseGO and its distributed exchange will be a critical core in helping to lead the way in providing base-layer technologies/infrastructure which can spark and launch the entire protocol token ecosystem.  ***Initial versions of OmiseGO may use aspects of Tendermint consensus***

### 3.1 Light Client Validation

It will become necessary to produce light client proofs for partial validation,as well as for external smart contract enforcement. 

The current state can be acquired by any node by downloading the recent block state commitment and any blocks between then. ***As the recent block state includes a tree of the recent state, clients are able to get a view of the recent commitment without downloading the entire chain.***

 Note that this is only possible as there is sufficient economic incentive against reorganization and halting attacks.

***The OMG chain is designed to heavily disincentive block reorganizations via bonded proofs, but does not provide guarantees around the need for block confirmation.*** Similar to current SPV Bitcoin validation implementations, there is some trust given to full nodes with regards to censorship risk; 

> SPV(Simplified Payment Verification) Bitcoin validation :
>
> ***Using Bitcoin without running a full network node. By default, upon receiving a new transaction a node must validate it***: in particular, verify that none of the transaction's inputs have been previously spent.(거래의 인풋이 이전에 사용되지 않았는지 검증) ***To carry out that check the node needs to access the blockchain.*** Any user who does not trust his network neighbors, should keep a full local copy of the blockchain, so that any input can be verified.
>
> none : 아무것도...않다
>
> ***It is possible to verify bitcoin payments without running a full network node. And this is called simplified payment verification or SPV.***
>
>   A user or user’s bitcoin spv wallet ***only needs a copy of the block headers of the longest chain***, which are available by querying network nodes until it is apparent that the longest chain has been obtained. Then, wallet using spv client get the Merkle branch linking the transaction to its block. Linking the transaction to a place in the active chain demonstrates that a network node has accepted it, and blocks added after it further establish the confirmation.

## 4 eWallets

***While OmiseGO supports payments, is not designed first and foremost a payment processor within a specific eWallet payment providers (EPP).***

However, due to the need for transactions between EPPs, payment activity may be conducted over a blockchain. This blockchain allows for the EPP to provide token issuance on OmiseGO. This allows for fiat-denominated currencies backed by fiat on the platform,or for any asset class (such as loyalty points). 

> issuance : 배급
>
> denominated  : 명명 된

***OmiseGO is an open system allowing for anyone to issue assets, but it is up to individual users (or  EPPs acting on behalf of  the users) to ensure correct issuance/auditing.***

In the default configuration, it is presumed that an EPP holds funds directly on behalf of its users for ease-of-use.  This is similar to full-custodian cryptocurrency wallets such as Coinbase or many centralized exchanges today. This allows for the EPP to construct fee-free transactions within their own network, as it doesn't result in blockchain activity.  ***However, it may also be possible to withdraw directly from the EPP and transact their issued token(e.g. fiat currency) on the OmiseGO chain (but transfers may result in on-chain fees if not transferred within an EPPs custodial account on-chain).  This allows for decentralized transfers, as well as serving the needs of some EPPs which need zero-fee transactions on their own network.***

***By building an eWallet platform as part of the blockchain***, it will be possible to directly exchange fiat-backed tokens with decentralized currencies and protocol tokens on the OMG blockchain.

## 5. Decentralized Exchange

A decentralized exchange may be ideal for eWallet interchange, as they may have different underlying representations of value, and even when transacting with the same underlying, there’s different counter party risk and costs. 

***The decentralized exchange will initially use a batch-auction construction where every round exchange matching occurs.*** It is possible to buy into particular rounds (block-heights), or to leave open orders on rounds until the order is filled. A batch auction allows for orders to be placed and execution happens at once at a specific interval. This construction allows for higher assurance and performance over a decentralized  network. 

While it is desirable to be able to perform low-latency high-frequency order execution, there are significant impediments to doing so in a decentralized network.  It is a necessary function of order matching that execution is occurs at a single point.   

> sybll attack: attack of one person but it looks like attack of many people.

An alternative which allows for fast execution with low-latency would be allowing for external centralized venues, however, this establishes trust in execution on a single entity. As trading liquidity naturally centralizes (far stronger than payment centralization), there are significant trust/coordination problems, which end up looking like current cryptocurrency exchanges  (with the only difference being that it is non-custodial)

 This  construction, however, ***does not resolve significant coordination problems around participants needing to resolve a coordination issue around not wanting to trade on a single trusted vendor.***

This decentralized exchange is designed to be high-performant where orders are propagated over the proof-of-stake network.

5 장 처음부터 다시 읽기