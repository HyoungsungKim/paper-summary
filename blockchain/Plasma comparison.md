# Plasma comparison

https://www.learnplasma.org/en/learn/mvp.html

## Plasma MVP

Plasma MVP is a design for an extremely simple UTXO-based plasma chain. It does not support more complicated constructions like scripts or smart contracts.

### Consensus

Plasma chains are special types of blockchains that can guarantee the safety of user funds ***even if the plasma consensus mechanism fails.***

- As a result, the simplest version of MVP relies on something called an **operator**
- This is what we generally mean when we say that MVP relies on **Proof-of-Authority**.
- The unique design of plasma ensures that ***user funds are safe even if the operator attempts to misbehave.***

### Deposit

Users start using the plasma chain by **depositing** funds into a smart contract on Ethereum.

- The basic MVP specification only allows users to deposit ETH, but the spec can be easily extended to support ERC20s.

### Transactions

Users can transact on the plasma chain by spending an output they own and creating new outputs.(UTXO)

- This transaction (and the corresponding signature) is then sent off(발송되다) to the **operator**.
- The operator will receive a bunch of transactions and then include them into an ordered list of transactions called a **block**. 
- Once the operator receives enough transactions to fill a block (although they can always include less), the operator will submit a commitment to this block to Ethereum.

### Merkle Trees

We can later prove that a specific transaction is in that set of transactions. ***This is exactly what the operator does***

- Every block is composed of a set of transactions, which is turned into a Merkle tree.
- The root of this tree is the **commitment** that gets published to Ethereum along with each plasma block.

### Withdrawals

Users need to be able to withdraw their funds from the plasma chain(exit).

#### Starting an Exit

In order to start a withdrawal, a user needs to submit a **Merkle Proof** along with the exit.

- The smart contract checks this proof to make sure that the transaction that created the output was actually included in some block.
- The contract then also checks that the output is owned by the user who started the exit.

#### Challenging an Exit

Users would be able to withdraw outputs they’d already spent!

- We want to make sure that the output being referenced is actually unspent, so we introduce a **challenge period**. 

#### Exit Priority

Unfortunately, the plasma operator is allowed to do evil things, like include double-spending transactions, and we can’t really do anything to stop them. The operator can even start a withdrawal from an output created by an invalid transaction.

- Conveniently, we only need to add a few rules to make sure user funds are safe.
  - The first of these rules is that UTXO have an “exit priority” based on when they were included in the plasma chain.
    - ***The exact priority is based on the “position” of the UTXO in the blockchain.***
    - This position is first determined by the block,
    - then the index of the transaction in the block,
    - then the index of the output in the transaction.
  - This gives us a unique, static position for every single UTXO.

> Priority
>
> 1. 몇번째 블록에 있는지
> 2. 블록 안에서 몇번째 트랜젝션에 들어있는지
> 3. 트랜잭션 안에서 몇번째 아웃풋으로 나오는지

***Note then that “older” UTXOs withdraw before newer ones.***

- That means that if an invalid transaction is ever included in the blockchain, then all transactions that occurred before the invalid transaction will be processed before that invalid one.

We’ve solved half of our problem!

#### Confirmation Signatures

What happens if a transaction gets included **after** the bad transaction?

- This can totally happen if the operator puts an invalid transaction before the user’s valid transaction.
- Users could try to exit from the inputs to the transaction, but that exit could be challenged by revealing the signed spend.

***We deal with this scenario by requiring that transactions are invalid until they’re signed twice.***

- Whenever a user makes a transaction, they’ll ***sign a first signature to have that transaction included in a block.***
- ***Then, once the transaction is included in a valid block, the user will sign a second signature,*** called a **confirmation signature**.
- Users correctly following this rule will never sign a confirmation signature unless they know that their transaction was included in a valid block.

### Watching the Plasma Chain

In order to keep their funds completely safe, users need to watch the plasma chain every once in a while.

### More Viable plasma

Confirmation signatures make for pretty bad user experience. 

***More Viable Plasma,*** also known as MoreVP, is an extension to Minimal Viable Plasma that removes the need for confirmation signatures.

- MoreVP modifies the process through which users can withdraw their funds. The ordering of each withdrawal becomes ***based on the position of the youngest input to the transaction that created an output.***
- An updated version of the MoreVP specification is currently being maintained and expanded by OmiseGO.

## Plasma Cash

Plasma Cash is a plasma design primarily built for storing and transferring ***non-fungible tokens.*** Plasma Cash was originally designed to address the mass exit problem in Plasma MVP.

### Consensus

This mechanism can be anything from a single operator (Proof-of-Authority) to a large set of validators (Proof-of-Stake). ***The design of Plasma Cash ensures that user funds are always safe, even if the consensus mechanism misbehaves.***

### Deposits

Unlike Plasma MVP, ***each Plasma Cash asset is represented by a non-fungible token.***

- For example, if a user deposits 10 ETH into the contract, that user will receive a token worth 10 ETH. Each token is given a unique identifier.

### Blocks

***Plasma Cash blocks are very different from Plasma MVP blocks.***

Each Plasma Cash block has a slot for every token in existence.

- Whenever a token is spent, a record of that transaction is placed at the corresponding slot.
- This special structure gives us something really cool.
  - In addition to being able to show that a token was spent in a specific block
  - We can also show that the token *didn’t* change hands in that block.
- So whereas Plasma MVP blocks form standard Merkle trees, Plasma Cash blocks form *sparse* Merkle trees.
  - ***Standard Merkle trees don’t give us a good way to prove that something isn’t part of a specific block***
  - But sparse Merkle trees do
- What’s cool about this is that users don’t actually need to keep track of every single token

### Transactions

Since users are only keeping track of their own tokens, they don’t know who owns any of the other tokens. 

- When a user wants to send their token to another user, ***they need to prove that they actually own that token***
- If the history is correct, then it should show the list of owners ending with the sender.
- ***To prove that history is actually correct,*** the user needs to provide additional proof that each transaction in the history was correctly included in a block. 
- Additionally, to show that there aren’t any missing transactions, ***the user also needs to provide a proof that the token wasn’t spent in any other block.***

> 1. 마지막 거래자를 보여야 함
> 2. 각 걱래가 올바르게 블록에 포함 되었는지 증명해야 함
> 3. 토큰이 다른 블록에서 사용되지 않았는지 증명해야 함
>
> -> 증명해야 할 것들이 많음

### Withdrawals

#### Starting an Exit

- When a user wants to withdraw a token, they need to submit the two latest transactions in the token’s history.
- The user also needs to submit Merkle proofs that show both transactions were included in the blockchain.

#### Challenging an Exit

Withdrawals can be immediately blocked if someone proves that the withdrawing user actually spent the token later on.

- Withdrawals can also be immediately blocked if someone shows that there’s transaction *between* the parent and the child transactions, meaning the withdrawing user provided an invalid parent.
- Someone can also challenge the withdrawal by providing some other transaction in the token’s history.
- This type of challenge doesn’t immediately block a withdrawal.
  - Instead, ***the withdrawing user is forced to respond with the transaction that comes after the provided transaction.***

### Pros and Cons

Plasma Cash is highly scalable because users only ever need to keep track of their own tokens. However, there’s a trade-off being made between scalability and flexibility. 

- Tokens always have a fixed denomination(액면가) - there’s (currently) no good way to spend a fraction of a token without going into something like Plasma Debit (which we’ll discuss in the next section).
- This makes Plasma Cash unsuitable for use cases where fractions of tokens are necessary, like exchanges.
- The proofs that need to be sent along with each transaction can grow pretty quickly.
  - These proofs need to go all the way back to the block in which the token was deposited.
  - Once the Plasma Chain has been running for a while, these proofs might get prohibitively large.
  - 체인이 길어질수록 토큰이 발행 됐을때 생성된 slot찾으로 뒤로 가야 함

## Plasma Debit

Plasma Debit is like Plasma Cash, except every token is a payment channel between the user and the chain operator. ***It’s sort of like a big Lightning hub,*** but the channels can be transferred just like a Plasma Cash token

### Consensus

 Plasma Debit is more suited for single operators than for lots of validators

### Deposits

Deposits in Plasma Debit are basically the same as deposits in Plasma Cash.

- Unlike Plasma Cash, this token is also a payment channel with the consensus mechanism
  - 플라즈마 캐시에서는 채널이 없고 블록에 슬롯이 있음
- It’s hard to have a payment channel with lots of people simultaneously, ***so this really lends itself to single operators.***

### Transactions

When a user wants to pay another user, they simply pay the operator and have the operator pay the other user simultaneously.

- The problem here is that the recipient needs to have a payment channel with the operator too
  - More specifically, the recipient needs to have a channel *where the operator has funds*. 
  - This is a huge user experience issue if the recipient doesn’t already have a payment channel with the operator.
- Operator가 여러개의 채널을 생성해 놓고 거래 할 사람에게 채널을 전송함.
  - 이더를 받을 사람의 채널 연결을 기다릴 필요가 없음

### Withdrawals

Withdrawals in Plasma Debit are also basically the same as withdrawals in Plasma Cash.

- However, remember that Plasma Debit payment channel transactions allow you to spend fractional parts of your tokens.
- So instead of having to withdraw entire tokens, users are allowed to withdraw fractions of tokens.
- Generally the exit challenges stay the same as in Plasma Cash. 
- What if a user tries to withdraw an entire token when they already spent half of it?
  - Plasma Debit solves this by ***adding one more challenge*** that blocks the exit if someone reveals a later balance signed by the withdrawing user.

### Pros and Cons

Pros

- Plasma Debit is a big improvement over Plasma Cash. Because it acts like a big Lightning hub.
  - Best of all, you still only need to keep track of your own channels.

Cons

- However, the design does have its downsides. Users still need to transmit a proof whenever they want to transfer a channel to someone else, just like in Plasma Cash. 
- Operators will probably create lots of channels in advance that can be transferred to new users.
  - ***Each of these channels requires the operator lock up some funds.*** Depending on how big the network gets, that might be a lot locked up funds!
- Plasma Debit isn’t much better than Plasma Cash for cross-currency payments. There currently aren’t any great Plasma Debit DEX proposals.