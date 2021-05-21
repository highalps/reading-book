# 6장. 비트코인 네트워크

- 비트코인은 인터넷 상에서 여러 노드들이 분산 네트워크에서 동일한 토폴로지를 가지는 P2P 구조를 이루고 있다.

### 노드의 유형 및 역할

- 모든 노드는 네트워크 내에 라우팅 기능 + @을 보유
- 모든 노드는 거래와 블록을 검증하고 전파하며, 이웃 노드들과의 연결을 유지하는 기능을 수행

![image](https://user-images.githubusercontent.com/20738369/117541031-eae7e080-b04c-11eb-9717-c27d24e4cf84.png)

> 지갑, 채굴자, 풀 블록체인 데이터베이스, 네트워크 라우팅 등 기능에 따른 노드

#### 채굴 노드

- 작업증명 알고리즘을 푸는 전용 하드웨어를 실행, 새로운 블록을 생성하기 위해 경쟁

### 확장 비트코인 네트워크

- 비트코인 네트워크는 기본 클라이언트 (bitcoin core)의 다양한 버전을 구동하는 몇 천개의 리스닝 노드와 그 외 p2p 프로토콜을 구현한 노드 수백개로 구성되어 있다.
- 몇몇 노드는 채굴 노드로, 채굴 과정에서 경쟁을 하고 거래의 유효성으 검증하여 새로운 블록을 생성한다.
- 대기업 중 많은 수가 bitcoin core라는 클라이언트 기반의 풀 노드 클라이언트를 가동해서 비트코인 네트워크에 연결되어 있다. 이런 노드들은 채굴, 지갑 기능은 없고 network edge router 역할을 하여 다양한 서비스 (환전, 블록 탐사, 상거래)가 가능하도록 해준다.
- 확장 비트코인 네트워크 = 비트코인 p2p 프로토콜을 가동하는 네트워크 + 특수한 프로토콜을 가동하는 노드를 포함

![image](https://user-images.githubusercontent.com/20738369/117541690-ef61c880-b04f-11eb-8470-4109556cc3d1.png)

> 확장 비트코인 네트워크 상의 다양한 유형의 노드들

### 네트워크 검색

- 새로운 노드를 작동시킬 때는 반드시 네트워크상에 존재하는 다른 노드를 검색해서 연결 하는 작업이 필요하다

![image](https://user-images.githubusercontent.com/20738369/117542579-2c2fbe80-b054-11eb-962b-88ec043bbc76.png)

> 노드들 간 handshake 과정

### version 메시지의 구성 요소

#### nVersion

The bitcoin P2P protocol version the client "speaks" (e.g., 70002)

#### nLocalServices

A list of local services supported by the node, currently just NODE_NETWORK

#### nTime

The current time

#### addrYou

The IP address of the remote node as seen from this node

#### addrMe

The IP address of the local node, as discovered by the local node

#### subver

A sub-version showing the type of software running on this node (e.g., /Satoshi:0.9.2.1/)

#### BestHeight

The block height of this node’s blockchain

![image](https://user-images.githubusercontent.com/20738369/117542981-ca705400-b055-11eb-8edf-b110f2fcdfac.png)

> 연결 후에 나머지 이웃 노드들에게 주소 전파하는 과정

## 풀 노드 (Full Node, Full Blockchain Node)

- 완전하고 가장 최신의 블록체인 카피를 가지고 있다.
- 외부 참조 없이도 독자적이고 신뢰할 수 있는 방법을 통해 어떠한 거래라도 검증 가능.
- 특정 노드는 블록 체인의 부분 집합으로만 유지 되기도 하고, 단순지불검증(SPV) 라는 방법을 이용하여 거래를 검증하기도 한다. (SPV 노드 / lightweight 노드라고 부른다)

### 인벤토리 교환하기

- 풀 노드가 이웃 노드들에 연결되고 난 후 가장 먼저 하는 작업은 완전한 블록체인을 만드는 일
- 새로 생성된 노드는 블록체인을 보유하고 있지 않다, 즉 단 하나의 블록만 알고 있다. 최초 블록이라고 하고 네트워크와 동기화하는 작업이 필요하다.

#### 동기화 작업 프로세스

        1. version 메시지로 handshake 시작 -> 이 메시지는 현재 블록체인 높이인
        (블록의 개수) BaseHeight을 담고 있다
        2. 노드는 이웃들로부터 전송받은 version 메시지로 이웃 노드 각각이 몇 개의
        블록을 보유하는지 파악
        3. 이웃 노드끼리는 로컬 블록체인상에 있는 getblocks 메시지 교환
        4. 길이가 좀 더 긴 블록체인을 가지고 있는 노드는 그렇지 않은 노드가 이웃
        노드를 따라잡기 위하여 어떤 블록을 필요로 하는지 확인 가능하다
        5. inv (inventory) 메시지를 통해 해시를 공유하고 전송하기 위한 첫 500개 블록을 확인
        6. 해당 블록을 가지고 있지 않은 노드는 getdata 메시지를 전송하여 풀 블록의
        데이터를 이웃 노드에 요구, inv 메시지에 있는 해시를 이용하여 수신 받은 블록
        데이터가 정확한지 검증하며 블록체인을 만들어 나간다

![image](https://user-images.githubusercontent.com/20738369/117547259-b8001580-b069-11eb-8724-b5c77960bde6.png)

> 최초블록이 이웃 노드로부터 블록을 검색해서 블록체인을 동기해 나가는 과정

### 단순지불검증(SPV) 노드

- 많은 수의 비트코인 클라이언트는 스마트폰, 태블릿 등 공간 제약 및 전력 제한이 있는 기기에서 가동 되도록 설계되어 있음.
- 이러한 기기는 SPV 방법을 이용하여 풀 블록체인을 저장하지 않고도 운영이 되도록 한다. (SPV 클라이언트 혹은 라이트웨이트 클라이언트라고 한다)
- SPV 노드는 블록 헤더만 다운로드 하고 각 블록에 들어 있는 거래는 다운로드 X
- 거래 내용이 없는 블록체인은 풀 블록체인보다 1,000배 작다.
- 대신 이웃 노드들에 의지하는 조금 다른 방법을 이용해 거래를 검증한다.
- 단순지불검증은 블록체인 내에서 해당 블록의 height 대신 depth를 참조하여 거래를 검증한다.
- 풀 블록체인은 최초블록까지 연결된 수천 개의 블록과 거래내용을 검증하여 체인을 생성하지만 SPV 노드는 모든 블록의 체인만 검증하고 머클ㄴ 경로 (merkle path)를 이용하여 확인하고자 하는 거래만 체인에 연결하는 방식이다. (지도를 가지고 여행 vs 매번 마주치는 여행객에게 길을 물어보면서 여행)
- SPV 노드는 선택적으로 특정 거래를 검증하기 위해 해당 거래를 검색 해야해서 프라이버시 노출의 위험이 있다 -> 블룸필터 기능의 추가

### 블룸필터

- 확률적 검색 필터. 원하는 패턴이 무엇인지 정확하게 규정할 필요 없이 원하는 패턴을 설명하는 방식
- 예시로 매번 마주치는 여행객에게 길을 물어보면 목적지가 노출되는 꼴과 비슷하다. 그래서 검색의 정확도를 다양하게 변화 시키는 방식 (기흥역 어떻게 가나요? -> ㄱㅎ역 어떻게 가나요?)

![image](https://user-images.githubusercontent.com/20738369/117548186-8b023180-b06e-11eb-91b8-fc47f3e0dfbd.png)

> 단순한 블룸필터 예시 (16bit array bloom filter, 3 hash functions)

![image](https://user-images.githubusercontent.com/20738369/117548194-96edf380-b06e-11eb-8206-4a8cd29fb7a0.png)

> 패턴 A를 단순한 블룸필터에 추가하기

![image](https://user-images.githubusercontent.com/20738369/117548592-c271dd80-b070-11eb-8c84-f4563fec6d1a.png)

> 패턴 B를 단순한 블룸필터에 추가하기

![image](https://user-images.githubusercontent.com/20738369/117548648-07960f80-b071-11eb-91f2-08030f770932.png)

> 모두 1이었다면 이미 블룸필터에 기록되어 있다고 볼 수 있다. 그럴 가능성이라는 확률적 개념에 가깝다. 즉 넣은 적 없는데 있다고 하는 false positive의 가능성이 존재한다 (false negative)는 존재 X
