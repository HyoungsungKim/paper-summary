# Understanding Ethash using Ethereum yellow paper

## Size of dataset and cache

The set of all byte sequences is $$\mathbb{B}$$

Scalars are non-negative integers and thus belong to the set $$\mathbb{N}$$.

- The size for Ethash’s cache $$c \in \mathbb{B}$$
  - Cache(c) is byte sequence
- Dataset $$d \in \mathbb{B}$$ depend on the epoch, which in turn depends on the block number.
  - Dataset(d) is byte sequence

### Definition

- $$H_i$$ : A scalar value equal to the number of ancestor blocks. The genesis block has a number of
  zero; formally 
- $$J_{datasetinit}$$ : Bytes in dataset at genesis. value is $$2^{30}$$
- $$J_{epoch}$$ : Blocks per epoch. value is 3000
- $$J_{datasetgrowth}$$ : Dataset growth per epoch. value is $$2^{33}$$ byte
- $$J_{cachegrowth}$$ : Cache growth per epoch. value is $$2^{17}$$
- $$J_{mixbyte}$$ : mix length in bytes. value is $$2^7$$
- $$J_{hashbyte}$$ : Hash length in bytes. value is $$2^6$$

$$
E_{epoch}(H_i) = \left \lfloor \frac{H_i}{J_{epoch}} \right \rfloor
$$

3000번째 블록마다 $$E_{epoch}(H_i)$$가 증가

- The size of the dataset growth by $$J_{datasetgrowth}$$ bytes
- The size of the cache by $$J_{cachegrowth}$$ bytes, every epoch.
- In order to avoid regularity leading to cyclic behavior, ***the size must be a prime number.***
  - Therefore the size is reduced by a multiple of $$J_{mixbytes}$$, for the dataset
  - and $$J_{hashbytes}$$ for the cache.

$$
d_{size} = E_{prime}(J_{datasetinit} + J_{datasetgrowth}·E_{epoch} − J_{mixbytes}, J_{mixbytes} )
$$

- Dataset은 $$2^{33}$$ bytes 만큼 증가 하지만 cyclic behavior을 제거하기 위해 $$J_{mixbytes}$$(128 bytes) 만큼 감소

$$
c_{size} = E_{prime}(J_{cacheinit} + J_{cachegrowth} · E_{epoch} − J_{hashbytes}, J_{hashbytes} )
$$

- Cache도 dataset과 유사하게 증가 및 감소

$$
E_{prime}(x, y) = \begin{cases} x & \text{if x / y } \in \mathbb{N} \\ 
E_{prime}(x - 2*y, y) & \text{otherwise}
\end{cases}
$$

## Dataset generation

In order to generate the dataset we need the cache c, which is an array of bytes.

- It depends on the cache size c size and the seed hash $$s \in \mathbb{B_{32}}$$ 

### Seed hash

***The seed hash is different for every epoch.***

- For the first epoch it is the Keccak-256 hash of a series of 32 bytes of zeros.
- For every other epoch it is always the ***Keccak-256 hash of the previous seed hash:***

seed hash : $$s = C_{seedhash}(H_i)$$
$$
C_{seedhash}(H_i) = \begin{cases} 0_{32} & \text{if } E_{epoche}(H_i) = 0 \\ KEC(C_{seedhash}(H_i - J_{epoche})) & otherwise \end{cases}
$$

### Cache

- $$J_{cacherounds}$$ : Number of rounds in cache production. Value is 3

The cache production process involves using the seed hash to first sequentially filling up $$c_{size}$$ bytes of
memory, then performing $$J_{cacherounds}$$ passes of the RandMemoHash algorithm created by Lerner [2014]. The initial cache $$c`$$ , being an array of arrays of single bytes, will be constructed as follows.

## Proof-of-work function

Essentially, we maintain a “mix” $$J_{mixbytes}$$ bytes wide, and repeatedly sequentially fetch $$J_{mixbytes}$$ bytes from the full dataset and use the $$E_{FNV}$$ function to combine it with the mix.

- $$J_{mixbytes}$$ bytes of sequential access are used so that each round of the algorithm always fetches a full page from RAM, minimizing translation lookaside buffer misses which ASICs would theoretically be able to avoid.

If the output of this algorithm is below the desired target, then the nonce is valid.

- Note that the extra application of KEC at the end ensures that there exists an intermediate nonce which can be provided to prove that at least a small amount of work was done;
  - this quick outer PoW verification can be used for anti-DDoS purposes.
  - It also serves to provide statistical assurance that the result is an unbiased, 256 bit number.

https://github.com/ethereum/wiki/wiki/Ethash

https://medium.com/tomak/%EC%9D%B4%EB%8D%94%EB%A6%AC%EC%9B%80-ethash-%EC%97%B0%EA%B5%AC-16677ed7da50

캐시 사이즈 계산 -> 캐시 생성 ( [RandMemoHash](http://www.hashcash.org/papers/memohash.pdf) 사용) -> dataset 생성 -> hashmoto(mix 생성 할 때 데이터 셋에서 128byte읽어 옴 - 데이터 셋 사이즈는 30000블록마다 변함)