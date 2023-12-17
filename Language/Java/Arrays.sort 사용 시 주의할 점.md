Arrays.sort 의 경우 정적 배열을 정렬하기 위해 사용한다.

```java
// Array 
int[] intArr = new int[] {1,3,5,2,4};                                //Primitive Type                          
double[] doubletArr = new double[] {1.1, 3.3, 5.5, 2.2, 4.4};        //Primitive Type
String[] stringArr = new String[] {"A","C","F","E","D"};             //Reference type(Wrapper Class)

//Sort
Arrays.sort(intArr);            
Arrays.sort(doubletArr);    
Arrays.sort(stringArr);
```

그러나 아래 코드의 경우 오류가 발생한다.

```java
class Solution {
    public String solution(int[] numbers) {
        Arrays.sort(numbers, (o1, o2) -> {
            String num1 = String.valueOf(o1);
            String num2 = String.valueOf(o2);
            
            int result1 = Integer.parseInt(num1 + num2);
            int result2 = Integer.parseInt(num2 + num1);
            
            return result1 - result2;
        });
        
        System.out.println(numbers);
        return "";
    }
}

// 오류
/Solution.java:5: error: no suitable method found for sort(int[],(o1,o2)->{[...]t2; })
        Arrays.sort(numbers, (o1, o2) -> {
              ^
    method Arrays.<T#1>sort(T#1[],Comparator<? super T#1>) is not applicable
      (inference variable T#1 has incompatible bounds
        equality constraints: int
        lower bounds: Object)
    method Arrays.<T#2>sort(T#2[],int,int,Comparator<? super T#2>) is not applicable
      (cannot infer type-variable(s) T#2
        (actual and formal argument lists differ in length))
  where T#1,T#2 are type-variables:
    T#1 extends Object declared in method <T#1>sort(T#1[],Comparator<? super T#1>)
    T#2 extends Object declared in method <T#2>sort(T#2[],int,int,Comparator<? super T#2>)
Note: Some messages have been simplified; recompile with -Xdiags:verbose to get full output
1 error
```

- 이는 int[]와 같은 Primitive type은 Comparator, Comparable 를 사용할 수 없기 때문이다.
- 따라서 Wrapper 타입으로 변경하여 사용해야 한다.

## 참고 자료

- GPT
- https://ifuwanna.tistory.com/232