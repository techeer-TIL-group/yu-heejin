## 파라메트릭 서치

- 파라메트릭 서치(Parametric Search)는 파라메트릭이라는 단어를 보면 알 수 있듯이 **`매개변수`를 이용한 탐색 기법**이라는 것을 알 수 있다.
- 파라메트릭 서치는 **최적화 문제를 결정 문제로 바꾸어 해결**하는 기법이다.
    - 즉, 결정 알고리즘(Decision Algorithm)을 사용한다.
- 결정 알고리즘은 이진 탐색을 사용하는데, 구하고자 하는 답이 원소 배열 범위 내에 존재하는 경우에 사용한다.
- 문제 풀이 아이디어는 구하고자 하는 답(answer)을 반복해서 조정한다.
- 정리하자면 파라메트릭 서치는 매개변수를 이용한 탐색 기법이며, 최적화된 답을 구하기 위해 결정 알고리즘을 사용한다.

## 문제 풀이 아이디어

- 구하고자 하는 답을 반복해서 조정한다.
- **구하고자 하는 답은 반복문 안에서 계속해서 변경되는 중간점(middlePoint)을 의미한다.**
- 즉, **결정 알고리즘에서는 최종적으로 미들포인트가 답이 된다.**

## 단순 이진 탐색 구현과의 차이점

- 시작점은 입력값 n의 시작 범위이며, 끝점은 배열의 마지막 원소 값이다.
- 반복문이 끝난 중간점이 답이 된다.

## 파라메트릭 서치 판단 기준

- 최적화된 값을 요구한다.
- 구하고자 하는 값의 범위가 크다.
- 이진 탐색을 사용하는 경우
- ‘**최댓값 혹은 최솟값을 구하세요’**와 같은 문구가 주어진다.

## 최댓값/최솟값을 구하는 경우

### 최댓값을 구하는 경우

```java
if(decisionToFindTheOptimizedValue(arr, middlePoint) >= m) {
    answer = middlePoint;
    startPoint = middlePoint + 1;
} else {
    endPoint = middlePoint - 1;
}
```

- 조건이 함수의 결과보다 작거나 같으면 시작점을 증가시키면서 answer의 값을 중간점으로 갱신한다.
- 함수의 결과보다 조건이 큰 경우에는 끝점을 감소시킨다.

### 최솟값을 구하는 경우

```java
if(decisionToFindTheOptimizedValue(arr, middlePoint) <= m) {
    answer = middlePoint;
    endPoint = middlePoint - 1;
} else {
    startPoint = middlePoint + 1;
}
```

## 참고 자료

- https://techvu.dev/86
- https://sarah950716.tistory.com/16