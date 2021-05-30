# 8장. 채굴과 협의

채굴에 대해서, 그리고 중앙 통제 기관 없이 어떤 과정에 의해 코인 네트워크가 전 세계적으로 합의에 이를 수 있는지 살펴본다.

## 채굴

- 새로운 코인이 통화 공급량에 추가되는 과정
- 사기 거래나 1회 이상 동일한 코인이 소비되는 거래 (이중지불)로부터 코인 시스템을 안전하게 **보호**
- 채굴자들은 코인 네트워크에 처리 능력을 제공하고 **댓가로 코인을 받는다**
- 채굴 작업에 대해서 새로운 코인을 받는것과 해당 블록 내 들어있는 거래 전부로부터 거래 수수료를 받는 **두가지 종류의 보상을 받는다** (bitcoin 기준)
- 보상을 받기 위해 채굴자들은 해시 알고리즘 기반으로 한 어려운 수학 문제를 풀기 위해 경쟁한다. (이 문제에 대한 해답을 **작업증명** ㅡ 중요한 계산을 하려고 노력했다는 사실을 증명하는 역할을 한다 ㅡ 이라고 한다.)
- bitcoin 기준으로 **2140년 후에는 채굴량이 고갈된다.**

![image](https://user-images.githubusercontent.com/20738369/120071937-447f8000-c0cc-11eb-9f98-46571cc14175.png)

> 비트코인 통화의 공급량 (채굴 제한)

## 분산화된 합의

비트코인의 분산화된 합의는 네트워크상 노드의 4가지 프로세스가 서로 상호작용 하면서 이뤄진다.

1. 포괄적인 판단 기준에 근거, 모든 풀 노드가 각 트랜잭션마다 **독립된 검증** 실시
2. 작업증명 알고리즘을 통해 증명된 계산법을 사용하여 **채굴 노드**들이 검증된 거래를 새로운 블록에 추가
3. 모든 노드들이 새 블록을 독립적으로 검증한 후 **체인에 블록 연결**
4. 모든 노드가 작업증명을 통해 이루어진 최고 누적 연산 체인을 독립적으로 선택

### 거래의 독립된 검증

- 거래가 발생하면 거래는 네트워크 내의 이웃 노드들에게 전송 된 후에 네트워크 전체로 전파된다.
- 전송 받은 거래는 검증되어야 하는데, 이때 유효하지 않은 경우엔 전송 받은 첫 노드에서 폐기한다.
- 다음과 같은 체크리스트를 바탕으로 모든 거래를 검증한다.

1. 거래의 구문(syntax)와 데이터 구조가 정확해야 한다.
2. 입력/출력값의 목록이 비어있지 않아야 한다.
3. byte 단위의 거래 크기가 MAX_BLOCK_SIZE보다 작아야 한다.
4. 출력값 금액과 노드의 총 금액이 허용된 범위 (0 ~ 2,100만 비트코인) 내에 있어야 한다.
5. ... 그외 여러가지는 [원문의 체크리스트](https://github.com/bitcoinbook/bitcoinbook/blob/059311e44cef07560cd177c66fba187dafddc182/ch10.asciidoc#independent-verification-of-transactions)를 참고한다.
6. 이러한 조건들은 다양한 공격 패턴 방어 및 다양한 거래 유형을 포함 시키기 위해서 시간이 흐르면서 변경 될 수 있다.


### 블록에 거래 추가하기

- 코인 노드는 검증된 거래들을 **메모리 풀(memory pool)** 또는 **거래 풀**에 추가한다. 거래 풀은 거래들이 블록 내에 포함될 수 있을때까지 기다리는 장소다.
- 메모리 풀 중에서 우선순위 메트릭을 적용하여 우선순위 높은 거래를 먼저 선택한다 (후보 블록 - candidate block)

> Priority = Sum(UTXO의 입력 값 * UTXO의 나이) / Transaction size
<br />
*UTXO의 나이는 블록체인에 기록된 이후 지나간 시간이다

- 이 경우 우선 순위는 1비트코인이 250btye 크기의 거래에서 하루 동안 나이를 먹는 (144 block) 수치인 57,600,000 보다 커야한다.

> High Priority > 100,000,000 (satoshi) * 144 / 250 = 57,600,000

- 블록이 채워진 후 메모리 풀에 남아있는 거래들은 다음 블록에 포함되기 위해 여전히 기다린다. (기다릴 수록 나이가 들게되면서 우선순위가 높아진다)


### 생성 거래 (generation transaction)

- 채굴 작업에 대한 보상으로 블록에 첫번째로 추가되는 거래.
- coinbase transaction이라고 부르기도 하고 특수한 거래 형태를 띤다.


## 작업증명 알고리즘

- 무작위로 입력값을 대입해서 원하는 해시값을 얻어내는 과정이다.

```python
Python 3.7.3
>>> import hashlib
>>> hashlib.sha256(b"I am Satoshi Nakamoto").hexdigest()
'5d7c7ba21cbbcd75d14800b100252d5b428e5b1213d27c385bc141ca6b47989e'
```
> 256bit 크기의 hash (= digest) 예시

```python
I am Satoshi Nakamoto0 => a80a81401765c8eddee25df36728d732...
I am Satoshi Nakamoto1 => f7bc9a6304a4647bb41241a677b5345f...
I am Satoshi Nakamoto2 => ea758a8134b115298a1583ffb80ae629...
I am Satoshi Nakamoto3 => bfa9779618ff072c903d773de30c99bd...
I am Satoshi Nakamoto4 => bce8564de9a83c18c31944a66bde992f...
I am Satoshi Nakamoto5 => eb362c3cf3479be0a97a20163589038e...
I am Satoshi Nakamoto6 => 4a2fd48e3be420d0d28e202360cfbaba...
I am Satoshi Nakamoto7 => 790b5a1349a5f2b909bf74d0d166b17a...
I am Satoshi Nakamoto8 => 702c45e5b15aa54b625d68dd947f1597...
I am Satoshi Nakamoto9 => 7007cf7dd40f5e933cd89fff5b791ff0...
I am Satoshi Nakamoto10 => c2f38c81992f4614206a21537bd634a...
I am Satoshi Nakamoto11 => 7045da6ed8a914690f087690e1e8d66...
I am Satoshi Nakamoto12 => 60f01db30c1a0d4cbce2b4b22e88b9b...
I am Satoshi Nakamoto13 => 0ebc56d59a34f5082aaef3d66b37a66...
I am Satoshi Nakamoto14 => 27ead1ca85da66981fd9da01a8c6816...
I am Satoshi Nakamoto15 => 394809fb809c5f83ce97ab554a2812c...
I am Satoshi Nakamoto16 => 8fa4992219df33f50834465d3047429...
I am Satoshi Nakamoto17 => dca9b8b4f8d8e1521fa4eaa46f4f0cd...
I am Satoshi Nakamoto18 => 9989a401b2a3a318b01e9ca9a22b0f3...
I am Satoshi Nakamoto19 => cda56022ecb5b67b2bc93a2d764e75f...
```

> 0, 1, 2, .... n 처럼 사용된 변수를 nonce라고 한다.