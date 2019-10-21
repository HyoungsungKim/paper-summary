# Let's read papers and articles together :)

## Something to read(papers)

[Secure High-Rate Transaction Processing in Bitcoin](https://fc15.ifca.ai/preproceedings/paper_30.pdf)

[Plasma:  Scalable Autonomous Smart Contracts](https://plasma.io/plasma.pdf)

[cosmos whitepaper](https://cosmos.network/cosmos-whitepaper.pdf)

[tokamak network - onther](http://tokamak.network/)



## Something to read(articles, posts)

- [Bitcoin](#bitcoin)
- [Ethereum](#ethereum)
- [Stackexchange questions](#stack-exchange-questions)
- [Etc](#etc)

### Bitcoin

[Understanding Segwit Block Size](https://medium.com/@jimmysong/understanding-segwit-block-size-fd901b87c9d4)

> ***The scriptSig part of Segwit transactions is called the “witness data”. When Segwit transactions are sent to Legacy nodes the witness data is stripped.*** The Segwit blocks are restricted by something called *Block Weight*. 
>
> weight = (tx size *with witness data stripped*) * 3 + (tx size)
>
> Non-Segwit transactions have zero witness data, so the weight for a non-Segwit transaction is exactly 4 times the size.
>
> e.g) Non-Segwit tx : (1 + 0) -> 1 : transaction size, 0 : segwit size. therefore 1 * 3 + 1 = 4

[What is the current maximum Bitcoin block size in MB?](https://bitcoin.stackexchange.com/questions/69468/what-is-the-current-maximum-bitcoin-block-size-in-mb)

[After Segwit Activation, what is the largest block size possible?](https://bitcoin.stackexchange.com/questions/54948/after-segwit-activation-what-is-the-largest-block-size-possible)

[Block weight](https://en.bitcoinwiki.org/wiki/Block_weight)

> **Block weight** prior to [SegWit](https://en.bitcoinwiki.org/wiki/SegWit), there was a max block size of 1MB. After SegWit, the concept of max block size was removed and replaced with *max block weight*. The cryptocurrent max block weight is 4MB.

[How long does block validation take?](https://bitcoin.stackexchange.com/questions/50349/how-long-does-block-validation-take)

### Ethereum

[How does Ethereum work, anyway?](https://medium.com/@preethikasireddy/how-does-ethereum-work-anyway-22d1df506369)

> merkle tree : save data using hash
>
> patricia  : searching algorithm using trie
>
> merkle patricia algorithm : save using hash(merkle) and searching value using patricia
>
> ## ***Concept of gas : gas is unit not price***
>
> ***That is why gasLimit is multiplied with gasPrice***
>
> transaction fee = gasLimit * gasPrice ($gas * ether/gas = ether$)

[How secure is Ethereum 2.0 consensus? Or, what is Casper finality?](https://medium.com/@ralexstokes/how-secure-is-ethereum-2-0-consensus-41523a59f270)

[The finality gadget Putting eth2.0 to work…](https://medium.com/@ralexstokes/the-finality-gadget-2bf608529e50)

[Sharding - vitalik](https://vitalik.ca/files/Ithaca201807_Sharding.pdf)

[Sharding, Casper → Serenity!](https://medium.com/onther-tech/sharding-casper-serenity-e25dec162845)

[Ethereum wire protocol](https://github.com/ethereum/devp2p/blob/master/caps/eth.md)

### Stack exchange questions

[Concept of Block weight and segwit are still unclear](https://bitcoin.stackexchange.com/questions/87970/concept-of-block-weight-and-segwit-are-still-unclear)

[What is a relationship with total hash rate(hash power) and average block generation time?](https://bitcoin.stackexchange.com/questions/87971/what-is-a-relationship-with-total-hash-ratehash-power-and-average-block-genera)

### Etc

### Consensus algorithm
[ConsensusPedia: An Encyclopedia of 30+ Consensus Algorithms](https://hackernoon.com/consensuspedia-an-encyclopedia-of-29-consensus-algorithms-e9c4b4b7d08f)  

#### Cosmos & Polkadot

[5 Differences between Cosmos & Polkadot](https://medium.com/@juliankoh/5-differences-between-cosmos-polkadot-67f09535594b)

[twitter](https://twitter.com/web3jp/status/1113440485698179072?s=19)

[Cosmos vs. Polkadot Comparison - reddit](https://www.reddit.com/r/dot/comments/b8tw3g/cosmos_vs_polkadot_comparison/?utm_source=share&utm_medium=ios_app)
