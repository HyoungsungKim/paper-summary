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