---
title: random 모듈
nav_order: 2
parent: 내장 라이브러리
---

# random 모듈

random 모듈은 무작위 수를 생성하는 기능을 제공한다. 

> {: .warning }  
> 이 모듈의 의사 난수 생성기는 보안 목적으로 사용해서는 안 된다.
> 보안이나 암호학적 사용이 필요한 경우에는 `secrets` 모듈을 참고한다.

## 바이트 관련 함수

### `random.randbytes(n)`

n 개의 임의 바이트를 생성한다.

```python
print(random.randbytes(10))
```
```
>>>
b'\xcf\x94\x7f\xe8#\x8d\x92\xdfR\x17'
```

보안 토큰 생성에는 `random.randbytes(n)` 대신 secrets.token_bytes() 를 사용한다.

```python
secrets.token_bytes(10)
```
```
>>>
b'yv\x11\x0e\xacs\x19\xf7\xcc\xdd'
```

## 정수 관련 함수

### `random.randrange(n)`  
### `random.randrange(start, stop[, step])`

주어진 range(start, stop, step) 에서 임의로 선택된 요소를 반환한다.
이는 choice(range(start, stop, step)) 과 유사하지만, 매우 큰 범위에서도 사용할 수 있고 일반적인 경우에 최적화되어 있다.
`range()` 함수와 동일한 패턴을 따르며, 키워드 인자는 예기치 않은 해석이 발생할 수 있으므로 사용하지 않는 것이 좋다.

```python
print(random.randrange(100)) # 0 ~ 99 의 무작위 정수를 생성
print(random.randrange(5, 10)) # 5 ~ 9 의 무작위 정수를 생성
print(random.randrange(1, 99, 2)) # 1, 3, 5, 7, ..., 97 중 무작위 정수를 생성
```
```
>>>
43
5
7
```

### `random.randint(a, b)`

a ≤ N ≤ b 인 임의의 정수 N 를 반환한다.

```python
# 3 ~ 70 사이의 무작위 정수를 생성한다.
# random.randrange(3, 71) 와 정확히 같다. (alias)
print(random.randrange(3, 70))
```
```
>>>
32
```

### `random.getrandbits(k)`

k 개의 임의 비트로 구성된 음이 아닌 파이썬 정수를 반환한다.

```python
# 이진수 000, 001, 010, 011, 100, 101, 110, 111 중 무작위 수를 생성한다. (3 bit 이므로...)
# 십진수로는 0 ~ 7 의 무작위 정수를 생성한다. 
randbits = random.getrandbits(3)

print(randbits)
print(bin(randbits))
```
```
>>>
2
0b10
```

## 시퀀스 관련 함수
(...작성중...)
