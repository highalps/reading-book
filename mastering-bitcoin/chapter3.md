# 3장. 비트코인 클라이언트

비트코인 입문을 위한 클라이언트 사용 방법 소개


- bitcoing.org에서 기본 클라이언트 중 하나인 비트코인 코어를 다운로드
- 소스코드에서 비트코인 코어 컴파일 실행하기

```bash
$ git clone https://github.com/bitcoin/bitcoin.git
Cloning into 'bitcoin'...
remote: Counting objects: 102071, done.
remote: Compressing objects: 100% (10/10), done.
Receiving objects: 100% (102071/102071), 86.38 MiB | 730.00 KiB/s, done.
remote: Total 102071 (delta 4), reused 5 (delta 1), pack-reused 102060
Resolving deltas: 100% (76168/76168), done.
Checking connectivity... done.
```

..... 쭉 설치과정 설명
<br />
지갑 설정 및 암호화, 백업, 덤프, 복원, 지갑 주소 생성 설명

<br />

# Bitcoin Core Application Programming Interface (API)

```bash
$ bitcoin-cli help
addmultisigaddress nrequired ["key",...] ( "account" )
addnode "node" "add|remove|onetry"
backupwallet "destination"
createmultisig nrequired ["key",...]
createrawtransaction [{"txid":"id","vout":n},...] {"address":amount,...}
decoderawtransaction "hexstring"
...
...
verifymessage "bitcoinaddress" "signature" "message"
walletlock
walletpassphrase "passphrase" timeout
walletpassphrasechange "oldpassphrase" "newpassphrase"
```

# Getting Information on the Bitcoin Core Client Status
: 비트코인 네트워크 노드, 지갑,블록체인 데이터베이스 등의 상태의 기본적인 정보
```bash
$ bitcoin-cli getnetworkinfo

{
  "version": 150000,
  "subversion": "/Satoshi:0.15.0/",
  "protocolversion": 70015,
  "localservices": "000000000000000d",
  "localrelay": true,
  "timeoffset": 0,
  "networkactive": true,
  "connections": 8,
  "networks": [
    ...
    detailed information about all networks (ipv4, ipv6 or onion)
    ...
  ],
  "relayfee": 0.00001000,
  "incrementalfee": 0.00001000,
  "localaddresses": [
  ],
  "warnings": ""
}
```


# Alternative Clients, Libraries, and Toolkits

The following sections list some of the best libraries, clients, and toolkits, organized by programming languages.

## C/C++
### Bitcoin Core
The reference implementation of bitcoin

### libbitcoin
Cross-platform C++ development toolkit, node, and consensus library

### bitcoin explorer
Libbitcoin’s command-line tool

### picocoin
A C language lightweight client library for bitcoin by Jeff Garzik

## JavaScript
### bcoin
A modular and scalable full-node implementation with API

### Bitcore
Full node, API, and library by Bitpay

### BitcoinJS
A pure JavaScript Bitcoin library for node.js and browsers

## Java
### bitcoinj
A Java full-node client library

## PHP
### bitwasp/bitcoin
A PHP bitcoin library, and related projects

## Python
### python-bitcoinlib
A Python bitcoin library, consensus library, and node by Peter Todd

### pycoin
A Python bitcoin library by Richard Kiss

### pybitcointools
An archived fork of Python bitcoin library by Vitalik Buterin

## Ruby
### bitcoin-client
A Ru by library wrapper for the JSON-RPC API

## Go
## btcd
A G o language full-node bitcoin client

## Rust
### rust-bitcoin
Rust bitcoin library for serialization, parsing, and API calls

## C#
### NBitcoin
Comprehensive bitcoin library for the .NET framework

## Objective-C
### CoreBitcoin
Bitcoin toolkit for ObjC and Swift