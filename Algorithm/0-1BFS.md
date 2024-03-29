## Deque(Double-Ended Queue)

![https://soft.plusblog.co.kr/24](https://t1.daumcdn.net/cfile/tistory/9955354C5C4723F11C?original)

https://soft.plusblog.co.kr/24

- 큐의 양쪽으로 엘리먼트를 삽입, 삭제할 수 있는 자료구조이다.
- 어떤 쪽으로 입력하고 출력하느냐에 따라 스택이나 큐로 사용할 수 있다.

## 0-1 BFS

- **가중치가 0, 1로 주어진 경로**에서 최단 경로를 찾아낼 수 있는 알고리즘
- 최단 경로 알고리즘인 다익스트라의 경우 시간 복잡도가 `O(ElogE)`지만, 0-1 BFS는 `O(V+E)`의 시간 복잡도로 문제를 해결할 수 있다.
- 일반적인 BFS 탐색과 동일하지만, 가중치가 0인 경우, 즉 **가중치가 더 낮은 경우를 고려해야 한다.**
- 일반적인 DFS와의 차이점은 **가중치가 낮은 정점으로 이동을 높은 우선순위로 해야하기 때문에 deque에 가장 앞에 삽입한다는 것이다.**

## 0-1 BFS 문제

[13549번: 숨바꼭질 3](https://www.acmicpc.net/problem/13549)

```java
import java.io.*;
import java.util.*;

public class Main {
    static int[] visited;
    
    public static void main(String args[]) throws Exception {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        
        String[] input = br.readLine().split(" ");
        int n = Integer.parseInt(input[0]);
        int k = Integer.parseInt(input[1]);
        
        if (n == k) System.out.println(0);
        else bfs(n, k);
    }
    
    private static void bfs(int n, int k) {
        Deque<Integer> q = new ArrayDeque<>();   // deque
        visited = new int[100001];
        
        Arrays.fill(visited, -1);
        
        q.add(n);
        visited[n] = 0;      // 가중치 == 시간
        
        while (!q.isEmpty()) {
            int curr = q.poll();
            
            if (curr == k) {
                System.out.println(visited[curr]);
                break;
            }
            
            // 2 * x, 해당 부분을 먼저 놔야 우선으로 들어간다.
            if (curr * 2 <= 100000 && visited[curr * 2] == -1) {
                visited[curr * 2] = visited[curr];      // 0초 후
                q.addFirst(curr * 2);
            }
            
            // x - 1로 이동했을 때 아직 방문하지 않은 곳인 경우
            if (curr - 1 >= 0 && visited[curr - 1] == -1) {
                visited[curr - 1] = visited[curr] + 1;   // 1초 후
                q.addLast(curr - 1);
            }
            
            // x + 1
            if (curr + 1 <= 100000 && visited[curr + 1] == -1) {
                visited[curr + 1] = visited[curr] + 1;
                q.addLast(curr + 1);
            }
        }
    }
}
```

- 순간 이동하는 경우 시간은 0초, 걷는 경우 시간은 1초이다.
- 따라서 가중치가 0인 순간 이동을 먼저 `addFirst`하고, 1인 순간을 `addLast`한다.
- 이 때, 순간 이동을 하는 경우를 먼저 앞단에 세워 놓아야(?) 정답이 된다.
    - 이 부분은 `addFirst`와 `addLast`를 바꾸면 순서에 상관없이 정답이다. (사실 왜인진 아직도 모르겠다,,, `addFirst`를 하면 앞쪽에 삽입되는게 맞는 것 같은데,,,)
        
        ```java
        import java.io.*;
        import java.util.*;
        
        public class Main {
            static int[] visited;
            
            public static void main(String args[]) throws Exception {
                BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
                
                String[] input = br.readLine().split(" ");
                int n = Integer.parseInt(input[0]);
                int k = Integer.parseInt(input[1]);
                
                if (n == k) System.out.println(0);
                else bfs(n, k);
            }
            
            private static void bfs(int n, int k) {
                Deque<Integer> q = new ArrayDeque<>();   // deque
                visited = new int[100001];
                
                Arrays.fill(visited, -1);
                
                q.add(n);
                visited[n] = 0;      // 가중치 == 시간
                
                while (!q.isEmpty()) {
                    System.out.println(q);
                    int curr = q.poll();
                    
                    if (curr == k) {
                        System.out.println(visited[curr]);
                        break;
                    }
                    
                    // x - 1로 이동했을 때 아직 방문하지 않은 곳인 경우
                    if (curr - 1 >= 0 && visited[curr - 1] == -1) {
                        visited[curr - 1] = visited[curr] + 1;   // 1초 후
                        q.addFirst(curr - 1);
                    }
                    
                    // x + 1
                    if (curr + 1 <= 100000 && visited[curr + 1] == -1) {
                        visited[curr + 1] = visited[curr] + 1;
                        q.addFirst(curr + 1);
                    }
                    
                    // 2 * x, 해당 부분을 먼저 놔야 우선으로 들어간다.
                    if (curr * 2 <= 100000 && visited[curr * 2] == -1) {
                        visited[curr * 2] = visited[curr];      // 0초 후
                        q.addLast(curr * 2);
                    }
                }
            }
        ```
        

## 참고 자료

- https://soft.plusblog.co.kr/24
- https://velog.io/@nmrhtn7898/ps-0-1-BFS