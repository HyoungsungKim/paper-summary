

# Majority Is Not Enough: Bitcoin Mining Is Vulnerable

# Abstract

We present an attack with which colluding miners’ revenue is larger than their fair share. The attack can have significant consequences for Bitcoin: Rational miners will prefer to join the attackers, and the colluding group will increase in size until it becomes a majority. At this point, the Bitcoin system ceases to be a decentralized currency.

We propose a practical modification to the Bitcoin protocol that protects Bitcoin in the general case. It prohibits selfish mining by a coalition that command less than 1/4 of the resources. 

## Introduction

In this paper, we show that the conventional wisdom is wrong: the Bitcoin mining protocol, as prescribed and implemented, is not incentive-compatible. ***We describe a strategy that can be used by a minority pool to obtain more revenue than the pool’s fair share, that is, more than its ratio of the total mining power.***

> 보통 전체 마이닝 파워에서 차지하는 비율 만큼 보상을 얻지만 더 큰 수익을 얻을 수 있는 방법을 제시

The key idea behind this strategy, called `Selfish Mining`, is for a pool to keep its discovered blocks private, thereby intentionally forking the chain.

Our analysis demonstrates that, while both honest and selfish parties waste some resources, the honest miners waste proportionally more, and the selfish pool’s rewards exceed its share of the network’s mining power, conferring it a competitive advantage and incentivizing rational miners to join the selfish mining pool.

> selfish mining이 발생하면 정직한 노드의 손해가 매우 큼(컴퓨팅 파워를 낭비함) 따라서 selfish mining pool이 특정 사이즈(threshold)를 넘으면 정직한 노드에서 공격자 노드로 전향하는 노드들이 발생할 것임

We show that, above a certain threshold size, the revenue of a selfish pool rises superlinearly with pool size above its revenue with the honest strategy. Once a selfish mining pool reaches the threshold, rational miners will preferentially join selfish miners to reap the higher revenues compared to other pools. Such a selfish mining pool can quickly grow towards a majority. 

We show that, for a mining pool with high connectivity and good control on information flow, the threshold is close to zero. This implies that, ***if less than 100% of the miners are honest, the system may not be incentive compatible:*** The first selfish miner will earn proportionally higher revenues than its honest counterparts, and the revenue of the selfish mining pool will increase superlinearly with pool size.

> 일단 한번 selfish minor가 수익 차지하면 superlinearly하게 증가 할 것임

## 3. The Selfish-mine startegy

The mining power of a pool is the sum of mining power of its members, and its revenue is divided among its members according to their relative mining power.

### 3.2 Selfish-mine

Selfish-Mine allows a pool of sufficient size to obtain a revenue larger than its ratio of mining power. The key insight behind the selfish mining strategy is to force the honest miners into performing wasted computations on the stale public branch. Specifically, selfish mining forces the honest miners to spend their cycles on blocks that are destined to not be part of the blockchain.

the honest miners will switch to the recently revealed blocks, abandoning the shorter public branch.

> 정직한 노드는 이게 공격인 줄 모르고 selfish minor가 만든 블록에 붙음

## 4. Analysis

We can now analyze the expected rewards for a system where the selfish pool has mining power of α and the others of (1 − α).

### 4.1 Reveneu

For a pool running the Selfish-Mine strategy, the revenue of each pool member increases with pool size for pools larger than the threshold.

### 6.1 Problem

The Bitcoin protocol prescribes that when a miner knows of multiple branches of the same length, it mines and propagates only the first branch it received. 

Because selfish mining is reactive, and it springs into action only after the honest nodes have discovered a block X, it may seem to be at a disadvantage. But a savvy pool operator can perform a sybil attack on honest miners by adding a significant number of zero-power miners to the Bitcoin miner network.

The virtual miners are managed by the pool, and once they hear of block X, they ignore it and start propagating block P. 

### 6.2 Solution

***Specifically, when a miner learns of competing branches of the same length, it should propagate all of them, and choose which one to mine on uniformly at random.*** In the case of two branches of length 1, as discussed in Section 4, this would result in half the nodes (in expectancy) mining on the pool’s branch and the other half mining on the other branch. This yields γ = 1/2, which in turn yields a threshold of 1/4