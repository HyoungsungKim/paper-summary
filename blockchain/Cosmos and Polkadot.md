## Cosmos and Polkadot

https://medium.com/@juliankoh/5-differences-between-cosmos-polkadot-67f09535594b

Two projects focused on blockchain interoperability

Cosmos with the [Hub-and-Zone](https://medium.com/@juliankoh/a-deep-look-into-cosmos-the-internet-of-blockchains-af3aa1a97a5b) model, Polkadot with the[ Relay Chain/Parachain](https://medium.com/polkadot-network/polkadot-the-foundation-of-a-new-internet-e8800ec81c7) model.

## Difference #1: Local vs Global Security

### Polkadot

parachain에서 transaction이 일어나면 블록을 생성, 검증함. 여러 parachain에서 블록 생성 할 수 있으며 블록이 생성되면 relay chain(parent chain)이 parachain의 블록을 수집하여 global state를 업데이트

- ***Parachains are blockchains within the Polkadot network.***
  - These chains have their own state machine, their own rules, and their own local block producers (collators). Each parachain is essentially an independent state machine, and can utilize any type of unique functionality,  consensus algorithm, transaction cost structure, and so on.
- In the  Polkadot Network, ***all the parachains are children of a parent chain called the Relay Chain,*** which contains some representation of a “global  state” of all the parachains combined.
  - The Relay Chain has its own consensus algorithm called ***GRANDPA consensus,*** which finalizes blocks on the parachains quickly.
    - ***Through this model, parachains in Polkadot operate under a “shared security” model*** — if the Relay Chain is highly secure with 1000s of validators, any parachain will benefit from this strong security by simply connecting to the Relay Chain.
    - This allows parachains to have sovereignty over their state machine and other local rules, as well as strong security shared with hundreds of other chains.
- Relaychain에 1000명의 검증자가 있으면 parachain에서도 1000명의 검증자가 존재 하는 것과 같은 효과
  - 따라서 shared security 모델로 작동

#### Drawback

- The drawback to this model is that the validators in the Relay Chain have the final say over state changes made in any parachain.
  - For example, the validators *could*, for some reason, continually reject blocks that come from collators of a specific parachain and  permanently block the parachain’s progress from being included in the global state.
  - Polkadot tries to reduce this from happening by shuffling validators so that they validate random parachains, lowering the possibility of a specific validator censoring a specific parachain. 
  - Polkadot also has another class of validators called Fishermen, who  continually check the validators for malicious activity.
- Relaychain이 특정 parachain의 검증을 악의적으로 거절 할 수 있음. 따라서 polkadot에서는 검증하는 parachain이 무작위로 섞이도록 하여 이런 상황을 방지 함

### Cosmos

The architecture of the Cosmos Network is fundamentally different.

- In the Cosmos Network, instead of using a local/global model for security, ***every blockchain is independent and secures itself.***
- ***Each blockchain runs its own consensus*** and the validators of each blockchain are responsible for securing that blockchain alone.
- ***The network uses a hub-and-zone model for interoperability,*** where zones (independent  blockchains) can “send tokens” to other zones by routing through a hub (also an independent blockchain).
- ***This protocol is called the IBC  (Inter-Blockchain Communication),*** which is a protocol for sending messages between chains to represent token transfers.
  - The IBC protocol is a [work in progress](https://github.com/cosmos/ics/issues), starting with token transfers and eventually any type of message passing between blockchains.
- 코스모스는 각각의 블록체인이 자신만의 합의 알고리즘을 가지고 있음
  - 코스모스 네트워크는 hub-and-zone이라는 모델을 사용하여 블록체인간에 토큰 전송이 가능해짐
  - 프로토콜은 IBC라고 불리며 현재 개발 진행진행 중

***Comparing this model with Polkadot, the biggest difference here is that the state of each zone is secured by its validators and its validators alone.***

- 폴카닷은 relaychain의 validator에 의해 보안이 유지되지만 코스모스에서는 각각의 체인이 보안을 책임 짐
- If a zone wants to have strong security, it needs to bootstrap and recruit its own validators, which may be difficult for smaller applications.
- However, this is a strong selling point for certain applications that desires more control.
  - For example, Binance is  bootstrapping their DEX by having their own nodes be validators for the Binance Chain as a starting point. This way, they have full control over the chain while they test the DEX and roll out new features. 

## Difference #2: Governance & Membership

### Membership

#### Polkadot

- In the Polkadot network, there is one single Relay Chain and some number of parachains that the validators of the Relay Chain can support.
- The [current estimate](https://medium.com/polkadot-network/a-brief-summary-of-everything-substrate-and-polkadot-f1f21071499d) is that there will be 100 slots for parachains, but this number can shrink or grow in the future. 
- ***The Polkadot Network allocates slots for becoming a parachain via an auction mechanism*** — the highest bidder is able to secure a parachain slot for some fixed time duration by locking up DOTs (the native cryptocurrency of Polkadot) in a Proof-of-stake system.
  - This means that to become a parachain in the Polkadot Network,  ***you would need to purchase a large amount of DOTs and lock them up for as long as you want to continue being a parachain.***
- 폴카닷 네트워크 슬롯은 가장 높은 bidding을 한 사람에게 할당 됨.
  - PoS방식으로 블록 생성.
  - parachain을 유지하고 싶을 때까지 DOTs를 묶어놔야 함

#### Cosmos

***The Cosmos Network on the other hand has no fixed rules of membership*** —  anyone can build a hub or a zone. Hubs are themselves sovereign blockchains built with the intention of connecting a bunch of other blockchains.

- Two examples are the [Cosmos Hub](https://cosmos.bigdipper.live/), which was recently launched by the Tendermint team, and the [Iris Hub](https://www.irisnet.org/), a Hub that plans to connect blockchains which primarily operate in China and other parts of Asia. ***This hub-and-zone model makes inter-chain communication more efficient, because instead of connecting to every other blockchain, each blockchain only needs to connect to a hub.***
- 코스모스에서는 만들고 싶은 사람이 네트워크를 만들수 있음
- 체인을 hub-and-zone에 연결해서 체인끼리 연결 할 필요 없음(존 : 이더리움, 블록체인 등...)
- 첫번째 허브는 코스모스 허브, 토큰은 아톰, 거래 수수료는 여러 토큰으로 지불 됨
- 텐더민트를 사용하지 않는 체인 중 PoW체인은 페그존(Peg-zone)이라는 체인을 거쳐야 함
  - 이더리움은 구현이 쉽지만 비트코인은 어려움
  - 채굴 즉시 거래가 체결되는 합의 알고리즘은 바로 ibc에 연결 가능

### Governence

#### Polkadot

In the Polkadot Network, governance decisions are determined by the amount of DOTs voters have. There will be a formal mechanism for voting on-chain, ***but it has not been finalized*** and the latest updates are [here](https://github.com/paritytech/polkadot/wiki/Governance).

- Other than regular stake-weighted voting, Polkadot also uses the idea of a council to represent passive stakeholders.
  - This council is a group of people, starting with 6 people and adding one every two weeks until  24 people. Each of the members is elected through an [approval vote](https://en.wikipedia.org/wiki/Approval_voting).
  - ***While the specific details of this governance process are yet to be finalized,*** the implications are that there are ways to change parameters in the Relay Chain such as block time, block reward, etc., as well as  ways to change membership rules for parachains.
    - For example, the Polkadot governance process could change the number of DOTs required or the auction mechanism to become a parachain.
    - A common misperception is that DOT holders can vote to kick out parachains at will, ***but in reality DOT holders can only change the process of membership.*** This means that a bonded parachain [stays bonded](https://twitter.com/PAMauric/status/1118545428809568267) throughout its entire lease.
  - 폴카닷에서는 PoS와 council 방법 사용
    - Council은 6명의 사람으로 구성되어있고 2주마다 한명씩 최종적으로 24명까지 늘어남.

#### Cosmos

***There is no single “governance” process for the Cosmos Network.***

- Each hub and zone has its own governance processes and there is no central set of rules that apply to the entire network of blockchains. ***When people talk about “governance of Cosmos”, they are referring to is the governance  of the Cosmos Hub, the blockchain launched by the Tendermint team.***
- The Cosmos Hub has a set of rules that lets anyone send a text proposal, and Atom holders are allowed to vote on it, where their votes are weighted by the number of Atoms they own.
- 코스모스에서 각각의 허브와 존은 자신들만의 governance를 가짐
- 코스모스에서 governance라고 불리는건 텐더민트팀이 개발한 코스모스 허브의 거버넌스를 의미함
- 코스모스에서는 누구나 다 의견을 제시 할 수 있고, 코스모스의 토큰(아톰)을 소유한 사람이 그 의견에 투표함.
  - 아톰의 수량만큼 투표에서 높은 영향력 가짐

## Difference #3: Inter-blockchain Communication

Another differentiator between Polkadot and Cosmos is the architecture of their inter-blockchain communication protocol and its design goals.

The biggest challenge in inter-blockchain communication is not how to represent data on one chain on another, but instead how to handle the case where the chain that the data originates from forks and reorganizes to exclude the transaction. This is where Cosmos and Polkadot differ the most because of the architectural designs.

### Polkadot

***Polkadot is targeting arbitrary message passing between parachains.***

- This means that Parachain A can call a smart contract inside Parachain B, can transfer a token between the chains, or any other type of communication.
- 폴카닷에서는 체인간 통신이 가능 함
  - B체인에 있는 스마트 컨트랙트를 A 체인에서 실행 가능

Polkadot uses two different mechanisms for securing interchain communication.

- First, having shared security makes it easier to exchange messages.
  - ***A  byproduct(부산물) of shared security is that all parachains have uniform levels of security, and as a result each chain can trust one another.***
    - To understand this, let us use the example of getting Ethereum (high  security) and Verge (low security) to interoperate. If we wanted to represent Ethereum on Verge, we could lock up ETH and mint some ETH-XVG  tokens on the Verge blockchain.
    - However, due to low security, an attacker could 51% attack the Verge chain and send a double spend to the Ethereum blockchain, allowing the attacker to withdraw more ETH than he actually owns.
    - Because of this, it is difficult for high-security chains to trust low-security chains when sending each other messages. This becomes even more complicated when messages are passed to multiple different chains of different security levels.
  - 폴카닷에서는 체인이 같은 수준의 보안을 가지고 있음
    - 예를 들어 이더리움과 버지의 토큰 스왑이 일어났을때, 이더리움보다 버지의 보안이 낮기 때문에 버지에서 51% 공격이 발생 할 수 있음

***In theory, having uniform shared security is a nice way of securing interchain communication.*** 

- However, to achieve this, the protocol must be able to shuffle validators assigned to each parachain often and  randomly.
- This leads to the classic “data availability problem”, which is that each validator must constantly download the state for each parachain it is assigned to.
- This is one of the most difficult problems in the space today and it is unclear that Polkadot will be able to solve it.
- 모든 인터체인이 통신에서는 같은 수준의 보안을 갖는게 이론상으로는 좋음
  - 같은 수준의 보안을 갖기 위해서는 parachain을 무작위로 선택해서 검증해야 함
  - 하지만 이럴경우 데이터 가용성 문제가 발생 할 수 있음 -> 검증자는 끊임없이 parachain의 state를 다운 받아야 함(계속 parachain이 바뀌기 때문에)
  - 폴카닷이 이 문제를 풀 수 있을지는 확실하지 않음

***Second, Polkadot uses the concept of Fishermen, which are “bounty-hunters” on the Polkadot Network who watch Parachains for malicious activity.***

- This is, in some sense, a “second line of defense” against malicious activity. In the case that the validators for a specific parachain finalize an invalid block, ***the Fishermen can submit a proof to the relay chain and effectively roll back the entire state of the Polkadot Network and all the parachains within it.***
- During interchain communication, we are most worried about one chain reorganizing and the other progressing as per normal, but Polkadot ensures that *everything* is rolled back if an invalid block is discovered.
- 폴카닷에는 피셔맨이라는 이차 방어선이 존재 함. 
- 특정 parachain의 검증자가 유효하지 않은 블록을 finalize하면, 피셔맨이 relay chain에게 이것을 보고하고, 효과적으로 모든 폴카닷 네트워크와 parachain을 state를 롤백함.
- 인터체인에서 한 체인은 롤백되고 다른 체인은 정상적으로 동작하면 문제가 됨. 폴카닷에서는 모든것을 롤백 함으로서 이 문제를 해결 함.
- Conversely, if an invalid state transition occurs on the Relay Chain (global state) and is *not* picked up by the Fishermen, this could affect every parachain in the Polkadot Network. We cannot assume that parachains are distinctly different objects because they ultimately still share one global state with the rest of the network.
- 하지만 만약 피셔맨이 유효하지 않은 블록을 잡아내지 못한다면 이 블록은 모든 네트워크와 공유되어 구별이 힘들어짐

### Cosmos

***Cosmos on the other hand is focusing on asset transfers between chains, which is a simpler protocol.***

- ***Right now, both communication protocols are fairly under-specified and have not been built.*** More detail about the two specifications can be found here: [IBC](https://github.com/cosmos/ics) (inter-blockchain communication) and [ICMP](http://research.web3.foundation/en/latest/polkadot/ICMP/) (inter-chain messaging among parachains).
- 코스모스는 체인간에 자산 전송에 초점을 맞춤
- 아직까지는 구현이 다 되어 있지 않음

***Cosmos takes an entirely different approach to interchain communication.***

- ***Since each blockchain has its own validators,*** it is entirely possible that there are zones which are “evil” with colluding validators.
  - This means that when one zone wants to communicate with another zone, zone A needs to trust the Cosmos Hub (for routing) and validators in zone B.
  - In theory, it sounds inefficient because people in zone A will have to look up the validators in zone B before they decide to send a message to it.
  - but I believe in practice it will not be so bad. “Famous” validators such as [Polychain Labs](https://hubble.figment.network/cosmos/chains/cosmoshub-2/validators/B1167D0437DB9DF0D533EE2ACDE48107139BDD2E) or Zaki Manian’s [iqlusion](https://hubble.figment.network/cosmos/chains/cosmoshub-2/validators/95E060D07713070FE9822F6C50BD76BCCBF9F17A) will likely validate many different blockchains, and over time build a reputation for being a “good validator”. This means that when zone A sees that zone B is validated by Polychain Labs and iqlusion, they may  decide to trust it.
- 각각의 블록체인이 자신만의 검증자를 가지고 있기 때문에 악의적인 검증자가 존재 할 수 있음
  - 따라서 체인 A에서 B로 전송하기 위해 A가 B의 검증자를 확인해야 하기 때문에 비효율적으로 보일 수 있음
  - 하지만 체인과 연결된 허브에서 서로 다른 많은 블록체인들을 검증하기 때문에 A는 B로 전송 할 때 B가 명성이 있는 허브에게 검증 되었는지 여부만 확인 하면 됨

However, even if people trust a chain, it can still get taken over by malicious actors and cause problems. Let us use this example, which was taken from [this talk:](https://youtu.be/_qUnCUZHc5g)

- 하지만 충분히 악의적인 공격이 가능 함
- Let us say that the small red dots represent a token called ETM, the native currency of the Ethermint zone. Users in Zone A, B and C want to use  ETM for some applications within those zones and they trust the Ethermint zone, so they do an IBC message which transfers ETM to these zones.
  - Now assume that the Ethermint validators collude and start double spending, arbitrarily(제멋대로) moving around coins, and so on. This will have an effect on the rest of the network, since ETM tokens also exist on different zones.
  - However, the *only* people which will be affected by this are people who hold ETM tokens within Ethermint or within other zones.
  - It is not possible for the evil validators within Ethermint to arbitrarily corrupt other zones except itself. This is the  purpose of the Cosmos architecture — to ensure that malicious activity cannot affect the entire network.
  - 토큰이 다른 네트워크로 전송 되었을때 토큰을 발행한 블록체인 내에서 검증자가 의도적으로 double spending attack을 할 수도 있음
  - 이렇게 되면 이 토큰을 가지고 있는 모두에게 손해가 가기때문에, 결국 토큰을 발행한 체인을 망가뜨리는 것과 마찬가지가 됨

## Difference #4: Consensus Algorithms

### Polkadot

The Polkadot Relay Chain uses a consensus algorithm invented by the team called [GRANDPA](https://medium.com/polkadot-network/grandpa-block-finality-in-polkadot-an-introduction-part-1-d08a24a021b5).

- This algorithm allows the Relay Chain to finalize many blocks from all the parachains quickly and can also accommodate a large number of validators (over 1000).
- Simplistically, this is because not all the validators need to vote on every single block — instead, ***validators can vote on a single highest block they think is valid, and the algorithm transitively applies the vote to all ancestors of that block.***
- Through this, the algorithm finds the set of blocks which have a super majority vote and considers that final. GRANDPA is still under development and we do not know how it will perform in the real world.
- 검증자는 자신이 highest 블록이라고 생각하는 블록에 투표를 하고 GRANDPA 합의 알고리즘은 모든 자손 블록에 이 투표를 적용 시킴
- 아직 개발 중

The parachains can use a variety of consensus algorithms to come to local consensus. Polkadot provides a software development kit ([Substrate](https://github.com/paritytech/substrate)) that comes with 3 consensus algorithms out of the box: GRANDPA,  Rhododendron, and Aurand. It is likely that more algorithms will be added to Substrate and will be usable within the Polkadot Network.

- 폴카닷은 3개의 합의 알고리즘을 소프트웨어 개발 킷으로 제공 중

### Cosmos

***On the other hand, each blockchain in the Cosmos Network can use any consensus algorithm that adheres to a certain specification known as the [ABCI](https://tendermint.com/docs/spec/abci/)(Application Blockchain Interface) spec.***

- This specification is created to standardize communication between chains. ***Right now, only the Tendermint algorithm fits this spec, but there are [other efforts](https://twitter.com/sunnya97/status/1113454367548379136) to create other consensus algorithms that fit this specification.***
- At a high-level, the Tendermint algorithm works by having every validator talk to each other to approve/reject any single block, creating finality on a per-block level.
- The algorithm is fast and has been stress-tested in a live environment with 200 validators and ***6-second block times*** during [Game of Stakes](https://github.com/cosmos/game-of-stakes). The Cosmos team also provides a software development kit with the Tendermint algorithm being usable out-of-the-box. [This blog post](https://medium.com/tendermint/a-to-z-of-blockchain-consensus-81e2406af5a3) is a good primer on consensus algorithms, and the features of Tendermint that make it useful.
- ABCI를 통해 블록체인에 어떤 합의 알고리즘이든 사용 할 수 있음
- 현재 이 스펙에 맞춘건 텐더민트 뿐

***The biggest downside of Tendermint is that it has a high communication overhead between validators.***

- This means that while it could work fairly fast with ~200 validators, it will be much slower with 2000 validators.
-  However, the trade-off here is that you get safety in asynchrony.
  - This means that in a network partition, instead of having 2 different histories of transactions which will eventually merge (and 1 history will get discarded in the process), the network will halt instead.
  - This is important because if you see a transaction that is “finalized”, it will *never* be reversed even in the worst network conditions.
- 텐더민트에서는 검증자가 증가 할 수록 오버헤드 역시 증가함. 하지만 검증자가 적을때 비동기성에 안전해짐

## Difference #5: Substrate vs Cosmos SDK

Both Polkadot and Cosmos offer a software development kit, called [Substrate](https://www.parity.io/substrate/) and the [Cosmos SDK](https://cosmos.network/sdk) respectively. They are both intended to make it easy for developers to start building their own chains, and include various modules out-of-the-box, such as governance modules (voting systems), staking  modules, authentication modules, and so on.

- The main difference between  the two is that the Cosmos SDK supports Go, whereas Substrate supports any language that compiles to WASM (Web Assembly), giving more flexibility to developers.

## Conclusion

### Polkadot 

1. **Application developers do not need to bootstrap their own security**
2. **If they can solve data availability, interchain messaging under shared security is easier**
3. **They seem to be more ambitious with Substrate (WASM, more consensus algorithms & modules out-of-the-box)**
4. **Focus on arbitrary message passing better for cross-parachain contract calls. (Still unsure of use case today)**
5. **Seems to have** [**more developers**](https://twitter.com/web3jp/status/1113440496116846592) **building version 1.0**(다양한 언어로 개발되고 있음. cosmos는 오직 go로만 개발)

### Cosmos

1. **Cosmos is live. Polkadot is not.**
2. **Polkadot has a restrictive & possibly expensive parachain membership process**
3. **More customizability is better for specific projects (e.g, Binance)**
4. **Evil validators of parachains could spread corruption throughout entire  network. Cosmos restricts corruption to only within the zone &  corresponding assets**
5. **Cosmos SDK used by** [**many projects**](https://cosmos.network/ecosystem) **already**
6. **Focus on asset transfers simpler & easier to get right. Proven use case today.**