## Queue 클래스

- Queue 클래스는 내부적으로 순환 배열로 구현되어 있는데, 배열의 마지막 요소에 다다른 경우 다시 배열 처음 요소로 순환하는 구조를 가지고 있다.
- Queue는 내부적으로 head와 tail 포인터를 가지고 있는데, tail에 데이터를 추가하고(Enqueue) head에서 데이터를 읽고 제거(Dequeue)한다.
- 만약 데이터 양이 많아 순환 배열이 모두 찰 경우. Queue는 Capacity를 2배로 증가시킨 후, 모든 배열 요소를 새 순환 배열에 자동으로 복사하여 큐를 확장한다.

## C#에서의 Queue

```csharp
Queue<int> q = new Queue<int>();

q.Enqueue(120);
q.Enqueue(130);
q.Enqueue(150);

int next = q.Dequeue();   // 120
next = q.Dequeue();   // 130

// 큐에 저장된 요소의 수 반환
int count = q.Count;

// 큐에 특정 요소가 있는지 여부 확인
bool contains = q.Contains(1);

// 큐의 모든 요소를 배열로 반환
string[] elements = q.ToArray();
```

## Stack 클래스

- Queue와 마찬가지로 내부적으로 순환 배열로 구현되어 있으며, 스택이 가득 차면 자동으로 배열을 동적으로 확장하게 된다.

## C#에서의 Stack

```csharp
// 스택에 요소 추가
stack.Push(1);

// 스택 맨 위의 요소 제거
stack.Pop();

// 맨 위의 요소 반환
int data = stack.Peek();

// 스택 요소 개수 반환
int count = stack.Count;

// 스택에 모든 요소 제거
stack.Clear();

// 스택의 요소들을 배열로 반환
int[] elements = stack.ToArray();
```

## 참고 자료

- https://www.csharpstudy.com/DS/queue.aspx
- [https://code-piggy.tistory.com/entry/C-큐Queue](https://code-piggy.tistory.com/entry/C-%ED%81%90Queue)
- https://code-piggy.tistory.com/304
- https://www.csharpstudy.com/DS/stack.aspx