# Double-spend Attack Models with Time Advantange for Bitcoin

## Abstract

Bitcoin is based on the concept of ‘proof of work’ allowing users to execute payments by digitally signing their transactions. ***This paper is about attack models that can assign possible time advantage to attacker agents in the Bitcoin network.***

In particular, this paper presents:

1. two attack models in which partial advancement towards ***block production can be influenced by time*** and not only by the hashpower used to produce blocks of hashes
2. algorithmic experimentation comparing these models against existing well-known hashrate-based attack models that do not consider time advantage.

***As a conclusion, this paper presents evidence on the fact that advantages are not negligible for cases in which an attacker has had enough time for secretly mining fraudulent blocks or significant control over the network.*** Also, the models presented in this paper help in supporting previous claims in the literature about how to correctly model and detect double-spend attacks in the Bitcoin network.

> negligible : 무시해도 될 정도의

# 1. Introduction

Bitcoin does not have a trusted third-party that can verify whether a digital coin has been spent, fraudulent transactions with users spending the same money at least twice can happen.

In this setting, ***hashrate means that the model considers how fast a block header can be hashed, while the number of confirmations refers to the number of blocks of transactions claiming a transaction to be valid.***

two question

1. What is the probability of a double-spend attack with K confirmations given that an attacker has mined n blocks?

One important aspect of the models of S. Nakamoto and M. Rosenfeld - ***These models do not consider blocks to be partially built, neither by honest nor attacker nodes.***

2. What is the probability of a double-spend attack with K confirmations given that an attacker has mined *n* blocks and, in addition, ***has been mining for t time units?***

this paper presents **two double-spend attack models*** for Bitcoin in which an attacker can be assigned ***some advantage in terms of time units.***

- The first model presented in this paper results from generalizing the model of M. Rosenfeld ***by adding an extra parameter denoting some time advantage assigned to the attacker,*** assumed to be used in producing and mining fraudulent blocks
- The second model, following a completely different approach, is a ***purely time-based model that takes into account the times at which both honest and attacker nodes last mined a block, in addition to their relative progress in terms of the length of chains already mined.***

> take into account : ~을 고려하다.

In this paper, the former model is called the ***generalized model*** and the latter is called the ***time-based model***.

Both the generalized and the time-based models are hash rate-based attack models in which partial advancement towards block production is ***influenced by time***, and not only by the hash power used to produce blocks of hashes. The equations governing these models use an ***Erlang probability distribution,*** which is specially suited for modeling waiting times in queuing systems [12].

The choice of this distribution can be justified because ***waiting times between occurrences of the event of producing a block can be naturally considered*** to be Erlang distributed. Furthermore, since partial block production is directly tied to the amount of time spent on mining a block, ***it is more of a continuous process than a discrete one.*** Because of these reasons, the Erlang probability distribution seems well-suited for both models.

To the best of the authors’ knowledge, double-spend attack models for Bitcoin that incorporate time have never been reported before in the literature.

- Section 2 presents a technical overview of the Bitcoin digital currency network.
- Section 3 briefly explains what a double-spend attack is.
- Section 4 presents an overview of the double-spend attack models of S. Nakamoto and M. Rosenfeld.
- ***Section 5*** presents the main contribution of this paper, namely, the generalized and time-based attack models proposed by the authors.
- ***Section 6*** gathers some information obtained from experimentation on the double-spend attack models and establishes some properties about these models. 
- ***Section 7*** gathers some concluding remarks and future work.

## 5. Two New Attack Models

### 5.1 The Generalized Model

This is the first double-spend attack model proposed by the authors. This model results from generalizing the model of M. Rosenfeld ***by adding an extra parameter denoting some time advantage assigned to the attacker.***

#### 5.1.1 Attacker’s potential progress function

$$
P(q, m, n, t)
$$

> 정직한 노드가 m번째 노드 채굴 하고 있을때 공격자가 n개의 블록 채굴할 확률. 공격자는 시간 t만큼의 어드벤티지를 가짐

Function `P` ***denotes the probability of an attacker mining exactly `n` blocks once the honest nodes mine the m’th block,*** assuming that the attacker has been mining secretly during `tτ` seconds. ***The extra parameter `t` represents the time advantage to be assigned to the attacker nodes towards fraudulent block production.*** 

In order to define the progress function P for this model, ***it is necessary to consider the possible number of blocks that could have been mined during `tτ` seconds.***

> 사전에 `tτ` 시간 동안 공격자가 얼마나 많은 블록 생성 할 수 있는지 정의 필요 함

Let `a(q, t, n)` denote the probability of mining exactly `n` blocks if constantly mining during `tτ` seconds with a proportion of `q` hash power respect to the total power in the network.

> a(q, t, n)의 정의 : `tτ` 시간 동안 q해시 파워로 n개 채굴 할 확률

$$
a(q, t, n)=\frac{(qt)^n}{n!}e^{-qt}
$$

Note that a(q, t, 0), which is the probability of mining 0 blocks in `tτ` seconds, represents the probability of mining the first block in a time greater than `tτ` seconds.

> a(q,t,0) 인 경우 0개의 블록 생성했고 블록 1개 생성에 `tτ`  시간보다 더 걸림 의미

The goal is to show
$$
a(q, t, n +1) = \frac{(qt)^{n+1}}{(n +1)!}e^{−qt}
$$
where
$$
a(q,t,n) = \begin{cases}
1 & {, if } {\ t = n = 0 } \\
0 & {, if } {\ t <= 0 }	\\
\frac{(qt)^{n+1}}{(n +1)!}e^{−qt} & {, otherwise}
\end{cases}
$$

$$
P_N(q, m,n) ≈ P_R(q, m,n)= P(q,m, n, 0)
$$

### 5.1.3 Double-spend attack probability

$$
DS(q,K, n, t)
$$

The probability of an attacker successfully performing a double-spend attack given an ***initial advantage of `n` blocks and `tτ` seconds over the honest nodes.***

K : honest just mined `K`th block
$$
DS_N(q,K) ≈ DS_R(q,K)= DS(q,K, 1, 0)
$$

## 5.2 The Time-Based Model

On the other hand, it uses a completely different approach to compute the probability of an attacker successfully performing a double-spend attack.

In the time-based model, states are identified by the length of the valid and fraudulent branches, which are assumed to have the same length, and the time difference at which both the honest and attacker nodes mined their last block. 

> time-based model에서 state는 유효한 가지와 사기치는 가지는 길이로 확인 됨. 가지들은 같은 길이를 가지고 있고 가장 마지막 블록 부분에서 시간 차이를 가지고 있음

Namely, a state in the time-based model consists of two values `t` and `n` denoting the time difference `t` at which both the honest and attacker nodes mined their `n`’th block.

It is key to note that the states in the time-based model differ from the previous attack-models in that the latter represents the states in terms of the length of the branches where the attacker and the honest nodes are mining, and they can be different.

In the time-based model, sometimes it is enough to focus on the time parameter `t`, leaving the length parameter `n` to represent the maximum such length so that both the attacker and the honest nodes have mined their `n`’th block.

#### 5.2.1 Attacker's potential progress function in time

$$
P_T(q, m, n, t)
$$

denoting the probability of the time at which the `n`’thblock mined by an attacker node is exactly (in the sense of a probability density function) `tτ` seconds after the time at which the `m`’th block is mined by a honest node.

$P_T$ is the probability density function of the random variable $X = T_{q,n} − T_{p,m}$ with independent $T_{q,n}$ and $T_{p,m}$.

#### 5.2.2 Catch-up function

$$
C_T(q, t)
$$

denoting ***the probability of an attacker’s branch ever becoming longer than the valid one*** given that the honest nodes mined their last block `tτ` seconds before the attacker mined its last block.

#### 5.2.3 Double-spend attack probability

$$
D_{ST}(q,K, n, t)
$$

denoting, similar to DS in the generalized model, the probability of an attacker successfully performing a double-spend attack given an initial advantage of n blocks and `tτ` seconds over the honest nodes.

## 6. Model Comparison

 `τ` is the average time between blocks

