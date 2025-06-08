# ==

```python
# 리스트 선언
a = [1, 2, 3]
b = a
c = [1, 2, 3]

# == 비교
a == b  # True (서로 같은 값을 가짐)
a == c  # True (서로 같은 값을 가짐)
```

- 파이썬에서 값이 같은 경우 `True`, 다른 경우 `False`를 반환한다.
- 참조가 같은 다르든 상관없이 오직 ‘값’이 같은지만 확인한다.

# is

```python
# 리스트 선언
a = [1, 2, 3]
b = a
c = [1, 2, 3]

# is 비교
a is b  # True (서로 같은 객체를 가리킴)
a is c  # False (안에 들은 값은 같지만, 서로 다른 객체)
```

- A와 B의 참조가 같은 경우 `True`, 다른 경우 `False`를 반환한다.
- ‘참조가 같다’ = ‘같은 객체를 가리키고 있다’
    - 즉, 변수가 가리키고 있는 객체(주소)가 같은지 확인할 때 사용한다.
    - 변수의 저장 위치(주소)가 같은지를 확인한다.
- 숫자, 글자 같은 고정값에 is 연산자를 사용하면 오류가 발생한다.

# 참고 자료

- https://blockdmask.tistory.com/579
- https://tyoon9781.tistory.com/entry/python-is-operator
- https://sohyunwriter.tistory.com/54