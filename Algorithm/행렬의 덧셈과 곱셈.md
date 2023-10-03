다음과 같은 2차원 행렬이 있다고 가정하자.

```java
int[][] m1 = {   // 2 x 3
    {1, 2, 3},
    {4, 5, 6}
};
 
int[][] m2 = {   // 3 x 2
    {1, 2},
    {3, 4},
    {5, 6}
};
```

## 행렬의 곱셈

- 두 행렬의 곱셈이 가능하려면 **m1의 열의 길이와 m2의 행의 길이**가 같아야한다.
- 연산의 결과 행렬의 row는 m1의 행의 길이와 같고, col은 m2의 열의 길이와 같다.
    - 즉 위의 행렬 연산 결과의 행렬은 `2 x 2`가 된다.

### 곱셈 연산

```java
class Solution {
    // 행렬은 앞쪽의 열과 뒤쪽의 행이 같아야 곱할 수 있다.
    public int[][] solution(int[][] arr1, int[][] arr2) {
        int[][] answer = new int[arr1.length][arr2[0].length];
        
        for (int i = 0; i < arr1.length; i++) {  // 열
            for (int j = 0; j < arr2[0].length; j++) {  // 행
                for (int k = 0; k < arr1[0].length; k++) {
                    answer[i][j] += arr1[i][k] * arr2[k][j];
                }
            }
        }
        
        return answer;
    }
}
```

- `answer[i][j] += arr1[i][k] * arr2[k][j]`
- 이 때 k는 앞 행렬의 열이자 뒤 행렬의 행이다.

## 행렬의 덧셈

- 행렬의 덧셈은 행과 열의 크기가 같은 두 행렬의 같은 행, 같은 열의 값을 서로 더한 결과가 된다.

### 덧셈 연산

```java
class Solution {
    public int[][] solution(int[][] arr1, int[][] arr2) {
        int[][] answers = new int[arr1.length][arr1[0].length];
        
        for (int i = 0; i < arr1.length; i++) {
            for (int j = 0; j < arr1[i].length; j++) {
                answers[i][j] = arr1[i][j] + arr2[i][j];
            }
        }
        
        return answers;
    }
}
```

## 참고 자료

- [https://yermi.tistory.com/entry/JAVA-다차원-배열의-활용3-행렬의-곱셈-두-행렬을-곱한-결과](https://yermi.tistory.com/entry/JAVA-%EB%8B%A4%EC%B0%A8%EC%9B%90-%EB%B0%B0%EC%97%B4%EC%9D%98-%ED%99%9C%EC%9A%A93-%ED%96%89%EB%A0%AC%EC%9D%98-%EA%B3%B1%EC%85%88-%EB%91%90-%ED%96%89%EB%A0%AC%EC%9D%84-%EA%B3%B1%ED%95%9C-%EA%B2%B0%EA%B3%BC)
- [https://velog.io/@hyeon930/프로그래머스-행렬의-곱셈-Java](https://velog.io/@hyeon930/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%ED%96%89%EB%A0%AC%EC%9D%98-%EA%B3%B1%EC%85%88-Java)
- [https://velog.io/@ujone/프로그래머스-행렬의-덧셈-JAVA](https://velog.io/@ujone/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%ED%96%89%EB%A0%AC%EC%9D%98-%EB%8D%A7%EC%85%88-JAVA)