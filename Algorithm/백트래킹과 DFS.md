## 백트래킹(Backtracking)

- 탐색 과정에서 유명하지 않은 후보해에 대해 빠르게 포기하고 이전 단계로 되돌아가 다른 후보해를 찾는 알고리즘 기법
    - 노드의 유망성을 판단하고, **불필요한 경로는 끝까지 탐색하지 않는다.**
- ‘해가 아니라면 다시 돌아온다’ → 더 이상 탐색할 필요가 없는 상태를 제외하는 것을 가지치기라고 한다.
- 현재 상태에서 가능한 모든 경로를 따라 탐색하다, **원하는 값과 불일치하는 부분이 발생하면 더 이상 탐색을 진행하지 않고 전 단계로 돌아가는 알고리즘**
    - 이후 해당 노드의 부모 노드로 되돌아간 후 다른 자손 노드를 검색한다.
- 백트래킹은 일반적으로 재귀의 형태로 작성되며, 아래의 3가지 내용을 작성해야 한다.
    
    ```java
    private static void dfs(int depth/*(깊이)*/, int start, char[] numbers) {
      if (depth == 0) {      // 재귀가 종료되는 시점에서 수행해야할 내용
          int number = Integer.parseInt(new String(numbers));
          
          if (answer < number) {
              answer = number;
          }
          
          return;
      }
      
    	// 가지치기 할 내용 확인
      for (int i = start; i < numbers.length; i++) {
          for (int j = i + 1; j < numbers.length; j++) {
              char temp = numbers[i];
              numbers[i] = numbers[j];
              numbers[j] = temp;
             
              dfs(depth - 1, i, numbers);
              
    					// 가지치기 시 왔던 길을 되돌아감
              temp = numbers[i];
              numbers[i] = numbers[j];
              numbers[j] = temp;
          }
    	 }
    }
    ```
    
    1. 재귀를 진행하는 동안 사용될 깊이를 매개변수로 넣기
    2. 재귀가 종료되는 시점에서 수행해야 할 내용
    3. 재귀가 진행중이면 가지치기(백트래킹)할 내용

## 백트래킹과 DFS

![https://sojeong-lululala.tistory.com/184?category=1015520](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FBbqtt%2FbtrviIWvF8L%2FKzsKgqsOGKCx9UrPaUlfhK%2Fimg.png)

https://sojeong-lululala.tistory.com/184?category=1015520

- DFS는 완전 탐색을 기본으로 하는 그래프 순회 기법으로, 모든 노드를 방문하는 것을 목표로 한다.
- 백트래킹은 불필요한 탐색을 하지 않고 이전 단계로 돌아와 다른 후보해를 탐색해 나가는 기법이다.
- 예를 들어 `[132, 234, 123]` 배열에서 `123`이라는 값을 찾고 있다고 가정하자.
    - 백트래킹은 `132`라는 값에 접근할 때, 십의 자리 수가 3, 2로 각각 다르기 때문에 더 이상 탐색을 진행하지 않고 다음 수로 넘어간다.
    - 반면 DFS는 원하는 숫자가 아님에도 일의 자리 수까지 탐색을 계속한다.

## 백트래킹 과정

1. 재귀를 호출하면서 DFS를 수행한다.
2. 유망한 노드면 서브트리로 이동하고, 그렇지 않으면 백트래킹을 수행한다.
3. 방문한 노드의 하위 노드로 이동하여 다시 재귀를 통해 DFS를 수행
4. 더이상 유효한 노드라고 생각되지 않으면 상위 노드로 백하여 백트래킹 수행

## 참고 자료

- https://veggie-garden.tistory.com/24
- https://kwanik.tistory.com/34
- https://sojeong-lululala.tistory.com/184
- https://devyoseph.tistory.com/161
- https://didu-story.tistory.com/270
- https://gamedevlog.tistory.com/49