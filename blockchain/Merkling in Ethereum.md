# Merkling in Ethereum

https://blog.ethereum.org/2015/11/15/merkling-in-ethereum/

First, the basics. A Merkle tree, in the most general sense, is a way of hashing a large number of “chunks” of data together which relies on splitting the chunks into buckets, where each bucket contains only a few chunks, then taking the hash of each bucket and repeating the same process, continuing to do so until the total number of hashes remaining  becomes only one: ***the root hash.***

A user who wants to do a key-value lookup on the database (eg. “tell me the object in position 85273”) can ask for a Merkle proof, and upon receiving the proof verify that it is correct, and therefore that the value received *actually is* at position 85273 in the database with that particular root.

## Merkle Proofs in Bitcoin

The benefit that this provides is the concept that Satoshi described as  “simplified payment verification”:

- Instead of downloading *every* transaction and every block, ***a “light client” can only download the chain of block headers, 80-byte chunks of data for each block that contain only five things:***
  - A hash of the previous header
  - A timestamp
  - A mining difficulty value
  - A proof of work nonce
  - A root hash for the Merkle tree containing the transactions for that block.

***If the light client wants to determine the status of a transaction, it can simply ask for a Merkle proof showing that a particular transaction is in one of the Merkle trees whose root is in a block header for the main chain.***

***This gets us pretty far, but Bitcoin-style light clients do have their limitations.***

- One particular limitation is that, while they can prove the inclusion of transactions, ***they cannot prove anything about the current state (eg. digital asset holdings, name registrations, the status of  financial contracts, etc).***
- How many bitcoins do you have right now? A  Bitcoin light client can use a protocol involving querying multiple  nodes and trusting that at least one of them will notify you of any particular transaction spending from your addresses, and this will get you quite far for that use case, but for other more complex applications it isn’t nearly enough;
- The precise nature of the effect of a transaction can depend on the effect of several previous transactions, which themselves depend on previous transactions, and so ultimately you would have to authenticate every single transaction in the entire chain.

To get around this, Ethereum takes the Merkle tree concept one step further.

## Merkle Proofs in Ethereum

Every block header in Ethereum contains not just one Merkle tree, but ***three trees for three kinds of objects:***

- Transactions
- Receipts (essentially, pieces of data showing the *effect* of each transaction)
- State

This allows for a highly advanced light client protocol that allows  light clients to easily make and get verifiable answers to many kinds of queries:

- Has this transaction been included in a particular block?
  - Transaction tree
- Tell me all instances of an event of type X (eg. a crowdfunding  contract reaching its goal) emitted by this address in the past 30 days
  - Receipt tree
- What is the current balance of my account?
  - State tree
- Does this account exist?
  - State tree
- Pretend to run this transaction on this contract. What would the output be?
  - State tree, but the way that it is computed is more complex.
  - Here, we need to construct what can be called a **Merkle state transition proof**.

To compute the proof, the server locally creates a fake block, sets the state to S, and pretends to be a light client while applying the transaction.

- That is, if the process of applying the transaction requires the client to determine the balance of an account, the light  client makes a balance query.
- If the light client needs to check a particular item in the storage of a particular contract, the light client makes a query for that, and so on.
  - The server “responds” to all of its own queries correctly, but keeps track of all the data that it sends back.
  - The server then sends the client the combined data from all of these requests as a proof.
  - The client then undertakes the exact same procedure, but *using the provided proof as its database*;
  - If its result is the same as what the server claims, then the client accepts the proof.

## Patricia Trees

The trees used in Ethereum are more complex - this is the “Merkle Patricia tree”.

Binary Merkle trees are very good data structures for authenticating information that is in a “list” format; essentially, a series of chunks one after the other. For transaction trees, they are also good because it does not matter how much time it takes to *edit* a tree once it’s created, as the tree is created once and then forever frozen solid.

***Unlike transaction history, however, the state needs to be frequently updated:*** the balance and nonce of accounts is often changed, and what’s more, new accounts are frequently inserted, and keys in storage are frequently inserted and deleted.

- What is thus desired is a data structure where we can quickly calculate the new tree root after an insert, update edit or delete operation, without recomputing the entire tree.

 There are also two highly desirable secondary properties:

- ***The depth of the tree is bounded,*** even given an attacker that is deliberately crafting transactions to make the tree as deep as possible.
  - Otherwise, an attacker could perform a denial of service attack by manipulating the tree to be so deep that each individual update becomes extremely slow.
  - Tree balance가 안좋으면 검색, 삭제 등등에 시간이 오래 걸림
- ***The root of the tree depends only on the data,*** not on the order in which updates are made. Making updates in a different order and even recomputing the tree from scratch should not change the root.