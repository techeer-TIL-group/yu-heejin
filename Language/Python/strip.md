문자열 **양 끝에 있는** 문자열이나 공백을 제거한다.

## 공백을 제거하는 경우

```python
string = "         abcde         "
string.strip()

print(string.strip())
print(string)      # 여긴 공백 제거 안됨

## 출력 결과
abcde
         abcde         
```

- 문자열 자체를 변환시키진 않기 때문에 새 변수를 만들어 저장해야 한다.

## 문자열을 제거하는 경우

```python
string = "         abcde         "

print(string.strip("c"))

string2 = "cdc"

print(string2.strip("c"))

## 출력 결과
         abcde         
d
```

- 문자열 중간에 있는 문자는 제거하지 않는다.

## 참고 자료

https://106hht.tistory.com/51