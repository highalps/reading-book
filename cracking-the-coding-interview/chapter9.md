# Chapter.9 면접 문제

자료구조에 널리 사용되는 보편적인 기법들을 설명

## Hash Table (해시 테이블)
- 효율적인 탐색을 위한 자료구조로서 해시 함수를 사용하여 변환한 값을 index로 하여금 key, value를 저장하는 형태다.
- 구현 방법은 여러가지, 흔한 방법은 linked list와 hash code function을 이용한다.

<br />

![image](https://user-images.githubusercontent.com/20738369/110660710-2a3db080-8207-11eb-9e65-8438646f8a94.png)


![image](https://user-images.githubusercontent.com/20738369/110660886-5c4f1280-8207-11eb-9559-b312051583ba.png)

1. key의 hash code를 계산
2. hash(key) % array_length로 배열의 index를 구한다
3. 배열의 요소에는 value가 존재.
4. 충돌이 잘 소화 된 경우 탐색 시간은 O(1), 최악엔 O(N)

<br />

### Collision(충돌)을 해소하는 방법

충돌이 일어나는 이유는 Load factor (적재율)의 한계가 존재하기 때문이다.
키의 개수를 K, 해시 테이블의 크기가 N이라면 적재율은 K/N이 된다. 이 값이 1을 초과 한다는 것은 무조건 충돌이 발생함을 의미한다.

1. Channing - linked list를 이용한다. key값과 linked list를 위한 자료구조를 넣는다. <br />(최악은 여전히 O(N) 이다)

![image](https://user-images.githubusercontent.com/20738369/110661413-e39c8600-8207-11eb-9812-44d579118c34.png)


2. Open addressing 기법. 충돌 시 다른 주소를 찾아본다 <br />
선형 탐사, 제곱 탐사, 이중 해싱등의 방법이 있다. 

3. binary search tree로 구성한다. <br />
- Binary tree는 각 node의 자식이 2개 이하인 경우이다.
- 단, Binary search tree의 경우에는 왼쪽 자신은 부모보다 작고, 오른쪽 자식은 부모보다 크게 구성한다.
- 여전히 선형으로 구성될 위험이 있으므로 balanced tree인 AVL 트리나 Red-Black 트리로 구성한다.


![image](https://user-images.githubusercontent.com/20738369/110662575-fc596b80-8208-11eb-9f08-1736f1d74b91.png)

<br />
<br />

## Bit Manipulation (비트 조작)
