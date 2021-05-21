# 7장. 블록체인

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
- 규모가 큰 데이터 집합의 완전성을 효율적으로 요약/검증하는데 사용되는 데이터 구조다

![image](https://user-images.githubusercontent.com/20738369/119177191-f0aae080-baa6-11eb-9b22-c9128597fb49.png)

> 머클 트리에서 노드 계산하기
