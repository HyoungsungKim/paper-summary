# A Vademecum on Blockchain Technologies:When, Which and How

***Marianna Belotti, Nikola Božić, Guy Pujolle, Stefano Secci, Senior Member, IEEE***

The actual advantages in using blockchain instead of any other traditional solution (such as centralized databases) are not completely understood to date, or at least there is a strong need for a vademecum(필휴 안내서) guiding designers toward the right decision about when to adopt blockchain or not, which kind of blockchain better meets use-case requirements, and how to use it.

## II. DISTRIBUTED LEDGER TECHNOLOGY (DLT)

Today, computational capabilities are distributed on the clients, the clients facilities, and on distant servers. This approach gave rise to the ‘client-server’ architecture which supported the development of the Internet and relational database systems.

- Massive data sets, originally housed on mainframes, can move onto a distributed architecture, with data replicated from node to node, or server to server, and subsets of the data can be accessed and processed on clients, and then, synced back to one of the servers.

Currently, we are witnessing the transition from centralized computing, storage, and processing to decentralized
architectures and systems. The DLT is the key innovation making this shift possible.

- Some distributed systems (e.g.,permissionless blockchains) aim to give the control of digital assets to end users without the need for intermediate nodes.
- Others (e.g., permissioned blockchains), attempt at maintaining a logical centralization of some information while adopting a decentralized architecture. 

Not all DLTs make use of a block architecture and can therefore be defined as ‘blockchains’.

### A. Terminology

- A ***distributed ledger*** is a type of digital data structure residing across multiple computer devices, generally at
  geographically distinguished locations
- ***Distributed Ledger Technology (DLT)*** designs a type of technology enabling storing and updating a distributed ledger in a decentralized manner.
  - While distributed ledgers existed prior to Bitcoin, the Bitcoin blockchain was novel in that since marking:
    - The convergence of a set of existing technologies (including timestamping of transactions, P2P networks, cryptography, and shared computational power)
    - Enabling data sharing
    - ***Storage without entrusting any central party for the ledger maintenance.***
  - DLTs consist of three basic components:
    1. A ***data model*** that captures the current ledger state;
    2. A ***communication language*** defined by transactions that change the ledger state;
    3. A ***protocol*** used to build consensus among participants around which transactions are accepted by the ledger and in which order.
- A ***blockchain*** is a P2P DLT structured as a chain of blocks, forged by consensus, which can be combined with a data model and a communication language enabling smart contracts and other assisting technologies.
- A ***smart contract*** is a computer program that executes predefined actions when certain conditions within the system are met. Smart contracts provide the transactions language allowing the ledger state to be modified. They can facilitate the exchange and transfer of any asset (e.g. shares, currency, content, property). They reside into the blockchain structure and are triggered along with transactions. Smart contracts can be imagined as digital protocols used to facilitate and enforce the negotiation of a legal contract. Actions carried out by trusted third-parties during a trade are replaced by pieces of code.

### B. Permissionless and permissioned participation modes

With respect to accessing the blockchain network, there are two main modes of operation: permissionless and permissioned. These are often referred to as public and private blockchains,

## III. JOURNEY OF A TRANSACTION

The usage of blockchain transactions is not limited to the simple assets exchange, but it also covers the execution of computing instructions such as storing, querying and sharing.

## IV. CONSENSUS MECHANISMS

***Consensus is the task of getting multi-agent systems with interacting agents to achieve a common goal.***

- Agents must reach an agreement regarding a certain interest (a value or an action, etc.) depending on their state. 