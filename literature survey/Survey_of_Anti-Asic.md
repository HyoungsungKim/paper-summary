# Survey of Anti-Asic

13 Best Cryptocurrencies To Mine With GPU In 2019 

https://themoneymongers.com/best-cryptocurrency-coin-to-mine-with-gpu/
https://cryptoslate.com/cryptos/proof-of-work/ : pow 코인 리스트

https://eips.ethereum.org/EIPS/eip-1057 - progpow eip

1. Ethash - 메모리
2. ProgPoW 
3. CryptoNight(monero) - 메모리
4. Equihash(Bitcoin Gold, zcash) - 메모리
5. x계열 11 ~ 17, 16r (verge) - 설계 (x affiliation)
6. Lyra2RE - 메모리, 설계?

## Introduction

1. 작업 증명의 등장
2. 채굴기의 발전
3. 작업 증명의 문제점이 발견되었고 이 문제를 해결해야 함
   - 작업 증명의 문제점
     - specialized hardware(ASIC) 의 등장과 asic이 너무 좋아서 asic이 아니라 보통 방법으로 채굴하는 채굴자는 수익을 얻을 수 없어 채굴을 포기함
   - 왜 해결해야 하는가?
     - 채굴자 수가 줄어들면 소수의 채굴자들에게 독점될 우려가 있음
     - 채굴자가 줄어들면 보안이 약해짐
       - 만약 규제로 중국의 채굴자들이 채굴을 중단하면 비트코인의 해시레이트가 한순간에 주저앉음
   - 따라서 해결해야 함
4. asic을 막기 위한 방법 조사 해볼 것임
   - asic을 막으려면 asic을 제작 했을 때 얻는 이윤아 가능한 작아야 함(as small as possible)
   - asic을 막기 위해서는 내 생각에는 크게 두가지 방법이 있음
     - 메모리 i/o
       - 이더리움 Ethash
       - 크립토 나이트
       - 이퀼해시
     - 설계 자체를 어렵게
       - progPow
       - x계열
5. 그래서 이 논문에서는 각 합의 알고리즘이 어떻게 이 방식을 사용하고 있는지 분석 해 볼 것임



## Ethash

ethash ethwiki : https://github.com/ethereum/wiki/wiki/Ethash

dag-hashmoto ethwiki : https://github.com/ethereum/wiki/wiki/Dagger-Hashimoto

scrypt : https://en.wikipedia.org/wiki/Scrypt

이더리움에서 ethash라는 pow 알고리즘을 사용함. 이 합의의 특징은 dag와 hashmoto의 특징을 결합하여 사용 하고 있음. 

- Hashmoto는 thaddeus dryja라는 사람이 제안한 알고리즘인데 asic구현을 io bound로 막고 있음. 즉 메모리에서 읽는 속도로 채굴 성능은 제한 됨.
- Dagger는 비탈릭 부테린이 제안했는데, 방향이 있는 비순환 그래프를 사용하는데 목적은 메모리 계산은 힘들지만 검증은 쉽게 함을 위해 구현 됨. 핵심 원리는 각각의 개인의 논스는 오직 큰 데이터 트리에서 일부분만 필수적으로 사용함. 그리고 각 논스의 서브트리 재계산은 금지 됨. 그래서 트리를 저장 할 필요가 있음. 하지만 싱글 논스의 가치는 검증 됨. scrypt처럼 메모리가 많이 필여함