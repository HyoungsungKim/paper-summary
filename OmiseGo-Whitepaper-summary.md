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
> - Approve : Prevent fake token -> check total amount of token and reject or conclude contransaction 
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

However, direct crosses between eWallet fiat tokens may not be desirable, as there maybe  too  many. It would be necessary to use ctyptocurrency for a liquid market without single preference.  By bonding Ethereum into a smart contract  (or Bitcoin-like tokens into bonded clearinghouses), it is possible to lock up Ether onto the activity of the OMGchain to allow for eWallet pairs to occur over Ether or other cryptocurrencies, creating  liquid  market  (if  every  pair  crosses  with  ETH,  spreads  would  be  much  smaller  provided low  currency  volatility).  

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
> Combine eWallet and exchange for market liquidity
>
> &&  It need high liquidity and prefer high-volume && micorpayments 

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



