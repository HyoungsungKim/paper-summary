# UTXO vs Account

Based on https://h2o.law.harvard.edu/text_blocks/30595

## UTXO

- Key base not address base - ***Good privacy as currency***
  - User can use different new address for each transaction
  - linking to user using address is difficult
  - Therefore good for currency
  - ***However not good for dapps***
    - Sometimes, dapp needs to track state of users. It is very complex for UTXO model

## Account

- Large space saving
  - The number of N UTXO -> 1 account
  - Transaction is smaller than UTXO model
- Simple for dapp
  - For dapp in UTXO, including Merkle tree of change-of-application-stat-root is required
  - It is very complicated 
- Good for light client node
  - In account, light client can access all data at any point by scanning down the state tree in a specific direction.
  - If dapp is used in UTXO model, then referencing UTXO is very burdensome While dapp runs for long time. Because, UTXO reference is changed continuously
- ***However It is weak to replay attack***
  - To prevent replay attack, every transaction needs ***nonce***
  - The account keeps track of the nonces used and only accepts a transaction if its nonce is 1 bigger than the last nonce used.
  - [*What happens if I submit a nonce that's higher than the correct next nonce to use?*](https://www.reddit.com/r/ethereum/comments/6ihw6p/can_someone_please_explain_nonce_to_me/)
    - ***The transaction will get stored in the mempool of the nodes in a pending state.*** It will remain in the mempool of the node until transactions  with all nonces between the last valid incremental nonce for the account and the transaction stored in the mempool have been transacted to the network, and then the transaction will be executed as normal. It's possible that the nodes eventually drop your transaction out of the mempool if it's been waiting there for too long.