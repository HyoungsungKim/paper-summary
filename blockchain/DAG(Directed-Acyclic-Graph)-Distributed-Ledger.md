# DAG(Directed Acyclic Graph) Distributed Ledger

https://medium.com/nakamo-to/what-is-dag-distributed-ledger-technology-8b182a858e19

## DAG DLT

When a new transaction is added to the ledger, it is copied to other computers. In that way, multiple, identical copies of the latest version of the ledger are always available.

The Directed Acyclic Graph (DAG) DLT is a more recent solution that offers the benefits of blockchain with better performance.

An example of a DAG DLT is IOTA’s Tangle.

- Each new transaction in IOTA  must validate at least two previous transactions before it can be validated.
- An algorithm in IOTA ensures the random selection of transactions for verification, ***effectively preventing network members from only validating their own transactions.***
  - 새로운 거래를 하기 위해서는 이전의 2개의 거래를 검증해야 함
  - 중요도와 트랜젝션이 생성된 시간을 고려하여 확률적으로 선택
    - 중요도는 직간접적으로 승인한 트랜젝션의 개수로 정함.
- This method of reaching consensus allows multiple transactions to be verified simultaneously. 
- 수수료가 무료임(채굴 기능이 없음)
  - 참여자가 검증도 같이 함.
  - 따라서 참여자들이 증가 할 수록 보안성, 확장성이 증가 함
- Hashgraph is another contemporary entity which utilises DAG. Its protocol, “Gossip”, differs slightly to that of IOTA