## 최대 공약수 구하기(GCD) - 유클리드 호제법

- 2개의 자연수의 최대 공약수를 구하는 알고리즘
- 비교 대상인 두 개의 자연수 `a`와 `b`에서 (a > b) `a`를 `b`로 나눈 나머지를 `r`이라고 할 때, `a`와 `b`의 최대 공약수는 `b`와 `r`의 최대공약수와 같다.
    - 이 성질을 이용해 `a`를 `b`로 나눈 나머지 `r`을 구하고, `b`를 `r`로 나눈 나머지 `r’`을 구한다.
    - **나머지가 0이 되는 수가 바로 a, b의 최대 공약수가 된다.**

### 반복문으로 구하기

```java
int gcd(int a, int b) { //최대공약수
    while (b != 0) {
      int r = a % b;
      a = b;
      b = r;
    }

    return a;
  }
```

### 재귀함수로 구하기

```java
//재귀함수 사용 - 구현이 간단, 코드 간결, 시간 복잡도 O(logN)
int GCD(int a, int b) { //최대공약수 - 재귀함수 사용
	if(a % b == 0) {
		return b;
	}

	return GCD(b, a % b);
}
```

## 최소 공배수(LCM) 구하기

- 유클리드 호제법으로 최대 공약수를 구하면 다음과 같이 최소 공배수를 구할 수 있다.
    
    ```java
    a * b / GCD(a, b)
    ```
    

### 여러 숫자의 최소 공배수 구하기 - 프로그래머스 N개의 최소공배수

```java
class Solution {
    public int solution(int[] arr) {
        int answer = (arr[0] * arr[1]) / getGCD(arr[0], arr[1]);
        
        // 이전 최소 공배수 * 다음 수 / GCD(이전 최소 공배수, 다음 수)
        // 최소 공배수 : (a x b) / (최대 공약수)
        for (int i = 2; i < arr.length; i++) {
            answer = (answer * arr[i]) / getGCD(answer, arr[i]);
        }
        
        return answer;
    }
    
    // 최대 공약수 구하기
    private int getGCD(int a, int b) {
        if (a % b == 0) {
            return b;
        }
        
        return getGCD(b, a % b);
    }
}
```

## 참고 자료

- https://programmer-chocho.tistory.com/9
- https://bbinya.tistory.com/45
- https://small-stap.tistory.com/31