# 4장. 키, 주소, 지갑

개인키, 공개키, 주소, 스크립트 주소, 다양한 인코딩 형식에 대해 설명


- 디지털 키, 비트코인 주소, 디지털 서명 등을 통해 비트코인의 소유권이 성립
- 디지컬 키는 실제로 네트워크상에 저장되지 않고 사용자의 파일 속, 즉 간단한 DB인 지갑 속에 생성해서 저장
- 디지털 키는 비트코인 프로토콜과는 완전히 독립, 블록체인에 대한 참조나 인터넷 접근 없이 사용자의 지갑 소프트웨어가 생성/관리 가능
- 모든 비트코인 거래에는 블록체인에 포함되기 위해 유효한 서명이 필요하고 서명은 디지털 키가 있어야 한다.
- 키는 private key과 public key 두개로 구성되어 있다 (비대칭 암호화 방식)
- public key는 비트코인을 전송 받을때, private key는 비트코인 거래에 서명을 할때 사용

![image](https://user-images.githubusercontent.com/20738369/115123268-2d427280-9ff7-11eb-8dce-df26cdfe4e3f.png)


<br />

private key는 숫자로 구성되어 있고 타원곡선 곱셈함수를 이용하여 public key를 생성한다.
그다음 public key에 해시함수 사용 하여 비트코인 주소(address)를 생성한다.

![image](https://user-images.githubusercontent.com/20738369/115123322-6ed31d80-9ff7-11eb-9843-8123d607cff2.png)

<br />
<br />

# 타원곡선 암호법 (ECC; Elliptic Curve Cryptography)


![image](https://user-images.githubusercontent.com/20738369/115123976-b4451a00-9ffa-11eb-8cfd-51cb1f9fdf39.png)

![image](https://user-images.githubusercontent.com/20738369/115124026-fa01e280-9ffa-11eb-8d1d-ce6ed6a304bb.png)

OR 

![image](https://user-images.githubusercontent.com/20738369/115124036-09812b80-9ffb-11eb-9957-4afcd4fc6b01.png)


# 공개 키 (Public key)

- 공개키를 해싱해서 비트코인 주소를 만드는데, 이때 사용 되는 해싱 알고리즘은 SHA와 RIPEMD이다. 예를 들어 SHA256, RIPEMD160 등이 있다.

> 비트코인 주소 = RIPEMD160(SHA256(public_key))


- 비트코인 주소는 항상 Base58Check encoding으로 제공된다.

![image](https://user-images.githubusercontent.com/20738369/115970129-76587080-a57b-11eb-9e98-7b6912445214.png)


## Base58, Base58Check Encoding

- Base58: 텍스트에 기반을 둔 2진법 인코딩 포맷, 비트코인이나 기타 여러 암호화폐에서 사용하기 위해 개발.
- Base64에서 0, O, l, I, +, / 등을 쓰지 않기 때문에 Base64의 부분집합이라 볼 수 있다.
- Base64Check은 비트코인에 주로 사용, 내장된 오류 검사 코드를 가지고 있다.

### 특정 데이터를 Base64Check으로 전환하기

1. 데이터에 version byte (버전 바이트)라 하는 prefix를 붙인다. (비트코인은 0, public key를 인코딩 할때는 128, 16진법으로는 0x80 등)
2. 다음으로 double-SHA checksum을 만든다. 
<br /> 
: checksum = SHA256(SHA256(prefix + data))
3. 결과로 나온 32bit hash에서 첫 4btye가 오류 검사코드 역할을 한다.

## Compressed Public key (압축 공개키)

- 거래 크기를 줄이고 비트코인 블록체인 노드의 디스크 공간을 줄이기 위해 비트코인에 도입
- 대부분의 트랜잭션에는 비트코인 소유주의 자격을 검증하고 거래하는데 필요한 공개키가 포함되어 있는데, 각각의 공개키는 520bit의 space가 필요하다.
- 매일 수만 개의 거래가 생겨나는 경우 블록체인에 어마어마한 데이터들이 append 되는 셈이다.
- public key는 앞서 말한듯이 타원곡선상의 (x,y) 좌표 이므로, x좌표만 알면 y도 알 수 있기 때문에 x좌표만 저장한다.
- 다만 y값은 제곱근 이기 때문에 y제곱 값의 부호는 저장해야 한다.

![image](https://user-images.githubusercontent.com/20738369/115971489-b7548300-a583-11eb-8aaf-224e605ba72c.png)



