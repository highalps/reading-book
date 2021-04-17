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
