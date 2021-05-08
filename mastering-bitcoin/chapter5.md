# 5장. 거래

![image](https://user-images.githubusercontent.com/20738369/115994134-ab62d280-a610-11eb-85ab-56e6dd848330.png)


거래, 거래에 포함된 내용, 거래 생성 방법, 거래 승인 방법, 하나의 거래가 영구적인 기록으로 남는 방법 등을 소개

### 거래란?
비트코인 시스템 내에 있는 노드들 간 가치를 전송하는 행위를 인코딩한 데이터 구조

## 거래의 수명 주기

1. 생성된 거래에서 해당 거래가 참조하는 금액을 소비하기 위해서 승인을 의미하는 서명을 한 건이상 받음.
2. 비트코인 네트워크 상에 해당 거래를 알리면 각 노드(참가자)가 해당 거래를 검증하고 네트워크 내 노드 전부에게 거래가 도착할 때까지 거래를 계속 전파.
3. 채굴 노드에 의해 해당 거래가 검증되면 블록체인에 기록되어 있는 "거래들로 구성된 블록내"에 포함된다.
4. 이후 비트코인 장부에 영구적으로 기록되며 모든 노드에 의해 유효하다고 인정 받는다.

## 거래 출력값 & 입력값

- 비트코인 거래 블록을 구성하는 기본 요소는 *소비되지 않은 거래 출력값 (unspent transaction output)* 줄여서 UTXO다.
- UTXO는 특정 소유주에 대해 암호로 잠겨있고 블록체인상에 기록되어 있다.
- UTXO는 네트워크에 의해 통화 단위로 인정받은 불가분의 비트코인 통화 덩어리다.
- 특정 노드가 비트코인을 받게 되면 해당 금액은 블록체인 내에 UXTO로 기록된다.
- 비트코인 주소/계좌 자체에 잔액이 보관되는 개념이 아니고 특정 소유주에 대해 잠겨있는 UTXO가 네트워크 여지거지 산재되어 있는 형태이다. (즉, 코인 잔액은 실물이 아니라 지갑 어플리케이션이 만들어낸 파생개념)
- 잔액을 계산하는 방법은 블록체인을 조사해서 소유주가 보유한 UTXO 전부를 합산하는 것

### Transaction Outputs (거래 출력값)

거래 출력값은 두 부분으로 구성된다

1. 사토시로 표현되는 최소 단위의 비트코인 금액
2. cryptographic puzzle. locking script(잠금 스크립트) 라고도하고 witness script, 혹은 scriptPubKey로 불리기도 한다. 출력값을 소비하기 위해 충족되어야 하는 조건을 명시한다.

|Size| Field | Description |
|------|---|---|
| 8 bytes (little-endian) | Amount  | Bitcoin value in satoshis (10^-8^ bitcoin)
| 1&#x2013;9 bytes (VarInt) | Locking-Script Size | Locking-Script length in bytes, to follow
| Variable | Locking-Script | A script defining the conditions needed to spend the output

<br />

> blockchain.info API에서 UTXO 검색하는 script 예시

```python
# get unspent outputs from blockchain API

import json
import requests

# example address
address = '1Dorian4RoXcnBv9hnQ4Y2C1an6NJ4UrjX'

# The API URL is https://blockchain.info/unspent?active=<address>
# It returns a JSON object with a list "unspent_outputs", containing UTXO, like this:
# {	"unspent_outputs":[
#   {
#     "tx_hash":"ebadfaa92f1fd29e2fe296eda702c48bd11ffd52313e986e99ddad9084062167",
#     "tx_index":51919767,
#     "tx_output_n": 1,
#     "script":"76a9148c7e252f8d64b0b6e313985915110fcfefcf4a2d88ac",
#     "value": 8000000,
#     "value_hex": "7a1200",
#     "confirmations":28691
#   },
# ...
# ]}

resp = requests.get('https://blockchain.info/unspent?active=%s' % address)
utxo_set = json.loads(resp.text)["unspent_outputs"]

for utxo in utxo_set:
    print("%s:%d - %ld Satoshis" % (utxo['tx_hash'], utxo['tx_output_n'],
                                    utxo['value']))
    # Or try...
    # print("{tx_hash}:{tx_output_n} - {value} Satoshis".format(**utxo))
```

## Transaction Inputs (거래 입력값)

- 거래 입력값은 UTXO에 대한 pointer 역할을 한다. UTXO가 블록체인 내에 기록되는 경우 해당 트랜잭션의 hash와 일련번호를 참조하여 특정한 UTXO를 지정하게 된다.
- UTXO를 소비하기 위해서 거래 입력값에는 UTXO에 설정된 해제 스크립트(unlocking scrit), 즉 소비 조건을 만족 시키기 위한 스크립트도 포함되어야 한다.
- 비트코인 지불을 하려는 경우 지갑은 사용 가능한 UTXO 중에서 하나를 선택하여 트랜잭션을 구성한다.
- 그 다음 지갑은 UTXO 각각을 위한 서명을 담고 있는 해제 스크립트(unlocking script)를 생성하여 UTXO가 잠금 스크립트 조건을 만족시켜 소비 될 수 있게 해준다.


|Size| Field | Description |
|------|---|---|
| 32 bytes | Transaction Hash | Pointer to the transaction containing the UTXO to be spent
| 4 bytes | Output Index | The index number of the UTXO to be spent; first one is 0
| 1&#x2013;9 bytes (VarInt) | Unlocking-Script Size | Unlocking-Script length in bytes, to follow
| Variable | Unlocking-Script | A script that fulfills the conditions of the UTXO locking script
| 4 bytes | Sequence Number | Used for locktime or disabled (0xFFFFFFFF)

## Transaction Fees (거래 수수료)

- 대부분의 거래는 수수료를 포함 (채굴자에게 제공하는 일종의 보상금)
- 모든 거래에 수수료를 부과해서 다음 블록에 거래를 포함 시키는 (채굴하는)데 대한 동기부여 역할을 한다.

### 거래에 수수료 추가하기

- 거래의 데이터 구조 (data structure)에는 수수료 필드가 없다. 대신 입력값 총합과 출력값 총합의 차이로 알 수 있다.

> Fees = Sum(Inputs) = Sum(Outputs)

- 이는 중요한 개념인데, 트랜잭션을 구성할때 입력값을 덜 소비함으로써 의도하지 않게 많은 수수료를 거래에 포함시킬 수도 있기 때문이다. (채굴자에게 큰 금액의 팁을 주게 된다)
- 예를 들어 1비트코인을 지불하기 위해 20비트코인 UTXO를 사용한다고 가정하자. <br />
그러면 19비트코인의 잔액 출력값이 생성되어 지갑속에 되돌아오도록 해야 한다. <br />
그렇지 않은 경우 19비트코인은 거래 수수료로 계산되어 채굴자가 수집하게 된다.

## 거래 사슬과 고아 거래(Orphan transaction)

- 거래는 사슬을 형성하고 있다
- 사슬에 따라 이전 거래 (부모 거래라고도 함)의 출력값을 소비하여 다음 거래 (자식 거래라고도 함)를 위한 출력값을 생성한다
- 사슬 전체 구성 요소가 동시에 생성될 수도 있다.
- 거래 사슬이 네트워크상에 전송될 때, 항상 차례대로 도착하는 것은 아니다
- 먼저 자식 거래가 온 경우 부모거래가 도착하기를 기다린다. 이런 거래들로 이루어진 풀을 고아거래 풀 (orphan trasction pool)이라고 한다.

## 거래 스크립트 구성 (잠금과 해제)

- 비트코인 거래 유효화 엔진은 거래를 유효화 하는 두가지 스크립트에 의존한다 (잠금 스크립트, 해제 스크립트)

> 잠금 스크립트: 추후 출력값을 소비하기 위해 충족되어야 하는 요건

> 해제 스크립트: 잠금 스크립트가 출력값에 놓아 둔 조건을 해결하거나 충족시켜 출력값이 소비 될 수 있도록 하는 스크립트

## 스크립팅 언어

- 스크립트라고 불리는 비트코인 거래 스크립트 언어는 포스 언어처럼 구성되고 역폴란드식 표기법을 따르는 스택 기반의 실행 언어다. (push, pop)

> 2 + 7 - 3 + 1

> 2 7 OP_ADD 3 OP_SUB 1 OP_ADD 7 OP_EQUAL

![image](https://user-images.githubusercontent.com/20738369/117540131-96426680-b048-11eb-9517-317143c35a2f.png)

## Pay-to-Public-Key-Hash (P2PKH)

- 비트코인 네트워크상에서 진행되는 거래 대부분은 P2PKH이다.


....스크립트 패스