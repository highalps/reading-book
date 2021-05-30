# 7장. 블록체인

![image](https://user-images.githubusercontent.com/20738369/120070824-51e63b80-c0c7-11eb-97c2-4135d2fda332.png)


## 블록 구조

| Size                      | Field               | Description                  |
| ------------------------- | ------------------- | ---------------------------- |
| 4 bytes                   | Block Size          | 블록의 크기                  |
| 80 bytes                  | Block Header        | 여러 필드로 형성된 블록 헤더 |
| 1&#x2013;9 bytes (VarInt) | Transaction Counter | 거래의 개수                  |
| Variable                  | Transactions        | 블록에 기록된 거래           |

### Block Header (블록 헤더)

블록 메타데이터 3가지 집합으로 구성되어 있다.

1. **이전 블록 해시값** - 현재 블록이 블록체인에 있는 이전 블록과 연결되어 있음을 나타냄
2. **난이도**, **타임스탬프**, **nonce** - 채굴 경쟁과 연관
3. **머클 트리 루트** - 블록 내에 거래 전부를 효율적으로 요약하는데 사용.

| Size     | Field               | Description                                                                                                   |
| -------- | ------------------- | ------------------------------------------------------------------------------------------------------------- |
| 4 bytes  | Version             | A version number to track software/protocol upgrades                                                          |
| 32 bytes | Previous Block Hash | A reference to the hash of the previous (parent) block in the chain 체인 내 이전블록 (= 부모블록) 해시 참조값 |
| 32 bytes | Merkle Root         | A hash of the root of the merkle tree of this block's transactions                                            |
| 4 bytes  | Timestamp           | The approximate creation time of this block (in seconds elapsed since Unix Epoch)                             |
| 4 bytes  | Difficulty Target   | The Proof-of-Work algorithm difficulty target for this block 난이도 목표                                      |
| 4 bytes  | Nonce               | A counter used for the Proof-of-Work algorithm 작업증명 알고리즘에 사용되는 카운터                            |

<br />

### Block Identifiers (블록 식별자): Block Header Hash and Block Height

### 블록 해시 (= 블록 헤더 해시)

- 디지털 지문 역할을 하는 암호화 해시
- 블록 헤더를 SHA256 알고리즘을 2번 적용해서 얻어진다 (32바이트 크기의 블록 헤더 해시가 나온다)
- 실제 블록 데이터 구조에는 들어가 있지 않고, 네트워크 내에서 각 노드에 의해 계산된다
- 별도 데이터베이스 테이블에 저장해서 인덱싱으로 퍼포먼스를 높일 수 있다

### 최초 블록

- 블록체인 내의 첫번째 블록
- 신뢰받는 블록체인을 만드는 기반이 되는 루트
- 모든 노드는 적어도 하나의 블록으로 구성된 블록체인으로 시작한다
- 이 최초블록은 변경이 불가능하다

## Merkle Trees (머클 트리)

비트코인 블록체인 내에 있는 블록 각각은 머클 트리를 이용하여 해당 블록에 들어 있는 모든 거래의 요약본을 가지고 있다

- binary hash tree (이진 해시 트리) 라고도 한다.
- 규모가 큰 데이터 집합의 완전성을 효율적으로 요약/검증하는데 사용되는 데이터 구조다.
- 머클 트리에 사용되는 암호 해시 알고리즘은 더블 SHA256 이다.
- N개의 데이터 요소가 해시되고 요약되면, 특정 데이터 요소가 트리에 포함되는지 찾는데는 최대 2*log2(N)의 시간 복잡도가 필요하다 .

![image](https://user-images.githubusercontent.com/20738369/119177191-f0aae080-baa6-11eb-9b22-c9128597fb49.png)

> 머클 트리에서 노드 계산하기

그림 설명)
- H_A = SHA256(SHA256(Transaction A))
- H_AB = SHA256(SHA256(H_A + H_B))
- ..... root node까지 반복적으로 수행 (bottom-up 방식)
- H_ABCD에는 블록 헤더에 저장되어 4건의 거래 데이터 전체를 요약한다 (32byte의 크기)

<br />

![image](https://user-images.githubusercontent.com/20738369/120069932-7213fb80-c0c3-11eb-882f-846530c524be.png)

> 홀수개인 경우엔 마지막 거래의 해시를 복사해서 balanced tree를 만든다.

<br />

![image](https://user-images.githubusercontent.com/20738369/120070353-0894ec80-c0c5-11eb-8bb6-ea868865845d.png)


## Reference

- https://www.banksalad.com/contents/%EC%89%BD%EA%B2%8C-%EC%84%A4%EB%AA%85%ED%95%98%EB%8A%94-%EB%B8%94%EB%A1%9D%EC%B2%B4%EC%9D%B8-%EB%A8%B8%ED%81%B4%ED%8A%B8%EB%A6%AC-Merkle-Trees-%EB%9E%80-ilULl#:~:text=%EC%9D%B4%20%EA%B3%BC%EC%A0%95%EC%9D%80%20%EA%B7%B8%EB%A6%BC%EC%B2%98%EB%9F%BC,%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%A1%9C%20%EB%A7%8C%EB%93%A4%EC%96%B4%EC%A3%BC%EB%8A%94%20%EA%B2%83%EC%9D%B4%EB%8B%A4.