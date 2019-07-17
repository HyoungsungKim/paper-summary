# An Introduction to Hyperledger

## Purpose of This Paper

The core of this paper presents five compelling uses for enterprise blockchain in different industries. It also describes the open source frameworks and tools that Hyperledger is developing to help enterprises around the world deliver on the promise of blockchain for more secure, more reliable, and more streamlined interactions.

## 1. Introduction

### Many databases today are shared

***By now, the world is so connected that different people often need to access the same data.***

- To meet this need, distributed databases have emerged, where certain chunks of data can be accessed by more than one person at once.

But shared databases raise questions Once you start sharing a database with others, many questions arise:

- Who do you trust to share your data?
- How can you tell that someone is who they say they are online?
- What are they allowed to do to the database?
- What happens if both head office and the sales rep want to sell the same items?
- Who settles any conflicts or disputes?

Clearly, there are many practical issues with sharing a database. Over the years, people have tried many different solutions. ***One exciting new way to share databases that can help solve these problems is through blockchain technology.***

### Blockchain is the technology behind Bitcoin

***What's far more significant for enterprises is the technology behind cryptocurrencies, called blockchain.*** “It's not Bitcoin, the still speculative asset, that should interest you,” write Don and Alan Tapscott in their `book Blockchain Revolution` Instead, they point to “the power and potential of the underlying technological platform” as what will interest most business people.

### Blockchain is a new form of shared database

***A blockchain is a distributed database with no central authority and no point of trust.*** When you want to share a database, but you don't have a lot of trust in the other people who might use it, a blockchain can be very helpful. In this context, “trust” could mean many things. ***Trust could mean trusting others to perform actions on the database properly.*** 

Discussing trust brings up the two main kinds of blockchain.

- Most cryptocurrencies use permissionless blockchains where anyone can join and have full rights to use it. For example, anyone can buy Bitcoin or Ether because those use wide-open, permissionless blockchains.
- ***On the other hand, business blockchains tend to be permissioned.*** This means a person needs to meet certain requirements to perform certain actions on the blockchain. Some permissioned blockchains restrict access to pre-verified users who have already proven they are who they say they are.
- Others allow anyone to join, but only let trusted identities verify transactions on the blockchain.

### Blockchain permissions and consensus

Blockchains use consensus systems to make sure the information in the database is always correct. For example, a consensus system would use pre-established rules to determine which field rep gets the limited items in stock. Consensus systems take many different forms with different names. 

***Many permissioned blockchains use something called Byzantine Fault-Tolerant consensus algorithms.*** No matter how they're built, all blockchains rely on cryptography, the art and science of encoding information so that it's difficult to decode. Basic identity management—proving you are who you say you are—usually involves digital signatures and a certificate authority. 

## 3. The Greenhouse for Enterprise Blockchain

Hyperledger serves as a “greenhouse” that brings together users, developers, and vendors from many different sectors and market spaces. All these participants have one thing in common: All are interested in learning about, developing, and using enterprise blockchains.

***While blockchain is a powerful technology, it is not one-size-fits-all.***

Every enterprise needs special features and modifications to help a blockchain achieve its intended purpose. Since different organizations have different needs, there will never be one single, standard blockchain. Instead, we expect to see many blockchains with different features that provide a wide range of solutions across many industries.

Hyperledger provides a greenhouse structure that can incubate new ideas, support each one with essential resources, and distribute the results widely. ***A greenhouse structure can support many different varieties while consuming far fewer resources.***

As the greenhouse organization for open source blockchain development, Hyperledger provides these benefits:

- Help keeping up with developments
- Better productivity through specialization
- Collaboration to avoid duplicate efforts
- Better quality control of code
- Easier handling of intellectual property

## 4. Hyperledger Design Philosophy

Distributed ledgers can have vastly different requirements for different use cases. For instance, when participants share high levels of trust—such as between financial institutions with legal agreements—blockchains can add blocks to the chain with shorter confirmation times by using a more rapid consensus algorithm. On the other hand, when there is minimal trust between participants, they must tolerate slower processing for added security.

***Hyperledger embraces the full spectrum of use cases.*** We recognize that different enterprise scenarios have different requirements for confirmation times, decentralization, trust, and other issues, and that each issue represents a potential “optimization point” for the technology. To address this diversity, all Hyperledger projects follow the same design philosophy. All our projects must be: 

- Modular
- Highly secure
- Interoperable
- Cryptocurrency-agnostic(암화화폐의 지식 없이 수행 가능한 기술)
- Complete with APIs

#### Modular

Hyperledger is developing modular, extensible frameworks with common building blocks that can be reused. This modular approach enables developers to experiment with different types of components as they evolve, and to change individual components without affecting the rest of the system. This helps developers to create components that can be combined to build distributed ledger solutions well-suited to different requirements.

This modular approach also means that a diverse community of developers can work independently on different modules, and re-use common modules across multiple projects. The Hyperledger Architecture Working Group defines functional modules and interfaces for issues such as communication, consensus, cryptography, identity, ledger storage, smart contracts, and policy.

#### Highly secure

Security is a key consideration for distributed ledgers, especially since many use cases involve high-value transactions or sensitive data. With large codebases, many networked nodes, and valuable data flows, distributed ledgers have become prime targets for online attackers. ***Securing a blockchain is quite a difficult task:*** Distributed ledgers must provide a large set of features and functions, while resisting persistent adversaries.

Security and robustness are the keys to enable enterprise-class blockchains to evolve, and provide the critical infrastructure for next-generation business networks. 

#### Interoperable

In the future, many different blockchain networks will need to communicate and exchange data to form more complex and powerful networks. ***At Hyperledger, we believe that most smart contracts and applications should be portable across many different blockchain networks.*** This high degree of interoperability will help meet the increased adoption of blockchain and distributed ledger technologies.

#### Cryptocurrency-agnostic

Hyperledger projects are independent and agnostic of all alt-coins, cryptocurrencies, and tokens. ***Hyperledger will never issue its own cryptocurrency;*** this is decidedly not our purpose. Hyperledger exists to create blockchain software for enterprises, not to administer any cryptocurrency. ***However, the design philosophy includes the capability to create a token*** used to manage digital objects, which may represent currencies, although this is not required for the network to operate. 

#### Complemte with APIs

All Hyperledger projects provide rich and easy-to-use APIs that support interoperability with other systems. A well-defined set of APIs enable external clients and applications to interface quickly and easily with Hyperledger's core distributed ledger infrastructure. These APIs support the growth of a rich developer ecosystem, and help blockchain and distributed ledger technologies proliferate across a wide range of industries and use cases.

## 5. Some Compelling Use Cases

This section describes five concrete examples where blockchain has a clear and compelling use case. These use cases are drawn from different domains and arranged in alphabetical order:

- Banking—applying for a loan
- Financial services—post-trade processing
- Healthcare—credentialing physicians
- IT—managing portable identities
- Supply chain management—tracking fish from ocean to table

In each case, Hyperledger has useful tools available; in some cases, a proof-of-concept has already been developed.

## 6. Current Project: Frameworks

Hyperledger incubates and promotes a range of business blockchain technologies, including:

- Distributed ledger frameworks
- Smart contract engines
- Client libraries
- Graphical interfaces
- Utility libraries
- Sample applications

### Hyperledger Frameworks

- HYPERLEDGER BURROW : A modular blockchain client with a permissioned smart contract interpreter developed in part to the specifications of the Ethereum Virtual Machine (EVM).
- HYPERLEDGER FABRIC : Aplatform for building distributed ledger solutions with a modular architecture that delivers a high degree of confidentiality, flexibility, resiliency, and scalability. This enables solutions developed with Fabric to be adapted for any industry.
- HYPERLEDGER INDY : A distributed ledger that provides tools, libraries, and reusable components purpose-built for decentralized identity.
- HYPERLEDGER IROHA : A blockchain framework designed to be simple and easy to incorporate into enterprise infrastructure projects.
- HYPERLEDGER SAWTOOTH : A modular platform for building, deploying, and running distributed ledgers. Sawtooth features a new type of consensus, proof of elapsed time (PoET) which consumes far fewer resources than proof of work (PoW)

## 7. Current Project: Tools

Hyperledger incubates and promotes a range of business blockchain technologies, including tools and utility libraries.  The Hyperledger strategy encourages the re-use of common building blocks, enables rapid innovation of components, and promotes interoperability between projects. 

### Hyperledger Tools

- HYPERLEDGER CALIPER : A blockchain benchmark tool that measures the performance of any blockchain by using a set of predefined use cases.
- HYPERLEDGER CELLO  : A set of tools to bring the on-demand deployment model to the blockchain ecosystem with automated ways to provision and manage blockchain operations that reduce effort.
- HYPERLEDGER COMPOSER : An open development toolset and framework to make developing blockchain applications easier.
- HYPERLEDGER EXPLORER : A dashboard for viewing information on the network, including blocks, node logs, statistics, smart contracts, and transactions.
- HYPERLEDGER QUILT : A set of tools that offer interoperability by implementing ILP, which is primarily a payments protocol designed to transfer value across distributed and non-distributed ledgers.

## 8. Long-Term Vision

We live in a highly interconnected world. In the future, the world will no doubt become even more closely tied together. In both our business and personal lives, more data, more digital content, more communication, and more sharing will be the norm. All this will require careful management of our security, privacy, and trust.

### A common problem, and a sensible solution

We expect to see a common problem: ***Many people will want to share data in a distributed database, but no single owner will be trusted by every user.*** The solution is distributed ledger technology (DLT). As data sharing increases, we expect blockchain technology and DLT to become more and more common. But reaching widespread use of distributed ledgers will not be simple. For instance, gaining security and privacy with a blockchain often means sacrificing performance. This suggests that we'll need a variety of different blockchains that can all communicate and interact seamlessly. No one blockchain will work best for all applications. ***The long-term vision for Hyperledger is driven by two main concerns:***

- The architecture must be modular
- The architecture must be interoperable.

### Interchangeable modules

We hope that eventually Hyperledger consists of many modules that can be assembled into a cohesive, functional, and secure distributed ledger. All these modules will ideally be interchangeable with other modules of the same type. All these modules will ideally be able to communicate with other modules of the same or different types. And ideally, even a non-expert will be able to use them to set up a secure, interoperable blockchain quickly, easily, and efficiently.

### Many blockchains that work together

We want to specifically point out that we do not believe any Hyperledger blockchain should be the “one distributed ledger to rule them all.’’ The Hyperledger community sees merit in many different blockchains. We hope that other developers consider interoperability with Hyperledger projects.

### Not a single stack, but a collection of tools

***The goal for Hyperledger is not to become a single software stack.*** Instead, we want to create a collection of tools built with modularity and interoperability in mind. Then, any individual can use one, some, or all of the Hyperledger projects to create a distributed ledger to suit their needs.