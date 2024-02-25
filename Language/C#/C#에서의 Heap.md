## 우선순위 큐

- 우선순위의 개념을 큐에 도입한 자료구조
- 데이터들이 우선순위를 가지고 있고, 우선순위가 높은 데이터가 먼저 나간다.
- 우선순위 큐는 배열, 연결리스트, 힙으로 구현이 가능하다.
    - 이 중 힙으로 구현하는 것이 가장 효율적이다.

### 우선순위 큐 이용 사례

- 시뮬레이션 시스템
- 네트워크 트래픽 제어
- 운영체제에서의 작업 스케줄링
- 수치 해석적인 계산

## Heap

- 완전 이진 트리의 일종으로 우선순위 큐를 위하여 만들어진 자료구조
- 여러 개의 값들 중에서 최댓값이나 최솟값을 빠르게 찾아내도록 만들어진 자료구조
- 힙은 일종의 반정렬 상태(느슨한 정렬 상태)를 유지한다.
    - 큰 값이 상위 레벨에 있고 작은 값이 하위 레벨에 있다.
    - 간단히 말하면 부모 노드의 키 값이 자식 노드의 키 값보다 항상 큰(작은) 이진트리이다.
- 이진 탐색 트리는 중복된 값을 허용하지 않지만 힙 트리는 중복된 값을 허용한다.

### Heap의 종류

![Untitled](https://gmlwjd9405.github.io/images/data-structure-heap/types-of-heap.png)

- 최대 힙(max heap)
    - **부모 노드의 키 값이 자식 노드의 키 값보다 크거나 같은** 완전 이진 트리
- 최소 힙(min heap)
    - **부모 노드의 키 값이 자식 노드의 키 값보다 작거나 같은** 완전 이진 트리

## Heap 구현하기

![Untitled](https://gmlwjd9405.github.io/images/data-structure-heap/heap-index-parent-child.png)

- 힙을 저장하는 표준적인 자료구조는 배열이다.
- 구현을 쉽게 하기 위해 배열의 첫번째 인덱스인 0은 사용되지 않는다.
- 특정 위치의 노드 번호는 새로운 노드가 추가되어도 변하지 않는다.
    - 예를 들어, 루트 노드의 오른쪽 노드의 번호는 항상 3이다.

### Heap에서 부모 노드와 자식 노드의 관계

- 왼쪽 자식의 인덱스 = (부모의 인덱스) * 2
- 오른쪽 자식의 인덱스 = (부모의 인덱스) * 2 + 1
- 부모의 인덱스 = (자식의 인덱스) / 2

## 최대 힙에서의 삭제

![https://gmlwjd9405.github.io/2018/05/10/data-structure-heap.html](https://gmlwjd9405.github.io/images/data-structure-heap/maxheap-delete.png)

https://gmlwjd9405.github.io/2018/05/10/data-structure-heap.html

- 최대 힙에서 최댓값은 루트노드이므로 루트 노드가 삭제된다.
    - 최대 힙에서 삭제 연산은 최댓값 요소를 삭제하는 것이다.
- 삭제된 루트 노드에는 힙의 마지막 노드를 가져온다.
- 힙을 재구성한다.

## 최대 힙에서의 삽입

![https://gmlwjd9405.github.io/2018/05/10/data-structure-heap.html](https://gmlwjd9405.github.io/images/data-structure-heap/maxheap-insertion.png)

https://gmlwjd9405.github.io/2018/05/10/data-structure-heap.html

## BinaryHeap 클래스

- 핵심 메서드는 데이터를 새로 추가하는 Add(Insert)와 하나의 최상위 루트노드 데이터를 가져오는 Remove 메서드이다.
- Heap은 한번에 하나씩 혹은 최소의 데이터를 가져오는 기능이 가장 핵심적인 기능이다.

```csharp
public class MaxHeap 
{
    private List<int> A = new List<int>();

    public void Add(int value)
    {
        // add at the end - 일단 마지막 노드에 value를 넣는다.
        A.Add(value);

        // bubble up
        int i = A.Count - 1;
        while (i > 0)
        {
            int parent = (i - 1) / 2;   // 이진 탐색
            if (A[parent] < A[i]) // MinHeap에선 반대, 만약 현재 부모 노드보다 자신이 큰 경우
            {
                Swap(parent, i);   // 부모 노드와 자기 자신을 바꾼다.
                i = parent;
            }
            else
            {
                break;
            }
        }
    }

    public int RemoveOne()
    {
        if (A.Count == 0)  // 배열이 비어있는 경우
            throw new InvalidOperationException();

        int root = A[0];   // 루트 노드의 값을 저장
            
        // move last to first 
        // and remove last one
        A[0] = A[A.Count - 1];   // 마지막 노드의 값을 루트로 이동
        A.RemoveAt(A.Count - 1);  // 마지막 힙 노드 삭제

        // bubble down
        int i = 0;  // 루트 - 부모에는 현재 맨 마지막 노드가 담겨있음
        int last = A.Count - 1;  // 마지막 노드
				// 삽입 노드와 자식 노드를 비교해서 자식 노드 중 더 큰 값이 있으면 바꾼다.
        while (i < last)
        {
            // get left child index
            int child = i*2 + 1;   // 시작은 루트 노드의 왼쪽 자식 노드

            // use right child if it is bigger 
						// 왼쪽 노드 인덱스가 마지막 인덱스보다 작고(인덱스 오류 방지)
						// 왼쪽 노드가 오른쪽 노드보다 작은 경우(오른쪽이 더 큰 경우)
            if (child < last && 
                A[child] < A[child + 1]) // MinHeap에선 반대
                child = child + 1;   // swap 대상을 오른쪽으로 바꾼다.

            // if parent is bigger or equal, stop
						// 만약 자식 노드가 마지막 인덱스보다 크거나(끝까지 탐색 완료)
						// 부모 노드가 현재 자식 노드보다 크거나 같은 경우(바꾸지 않음)
            if (child > last || 
               A[i] >= A[child]) // MinHeap에선 반대
               break;
                
            // swap parent/child - 부모와 자식을 교환
            Swap(i, child);
            i = child;                                    
        }

        return root;
    }

    private void Swap(int i, int j)
    {
        int t = A[i];
        A[i] = A[j];
        A[j] = t;
    }
}
```

- Add(): 데이터를 마지막에 추가한 후, 추가 데이터의 부모, 상위부모 등을 찾아 계속 추가 데이터가 부모보다 크면 부모와 치환한다.
- Remove(): 루트 노드를 꺼내고, 마지막 요소를 루트 노드(첫 배열 요소)에 넣은 후, 루트와 좌우 자식 중 큰 요소와 비교하며, 자식이 크면 치환한다.
- MinHeap 클래스는 위 코멘트에서 부등호를 반대로 해주면 된다.

## [추가] C# 에서 힙 자료구조

- 힙 자료구조에서 배열은 0에서 시작하는 인덱스를 사용하기 때문에, 부모 노드가 i인 경우 왼쪽 자식 노드는 2 * i + 1, 오른쪽 자식 노드의 인덱스는 2 * i + 2이다.
- 이러한 방식을 0-인덱스 기반 배열이라고 한다.
- GPT 답변!
    
    코드에서 본 **`get left child index`** 부분, 즉 **`int child = i*2 + 1;`**이 실제로는 잘못된 것이 아닙니다. 이 표현은 0-인덱스 기반의 배열을 사용하는 힙(Heap) 자료구조에서 부모 노드와 자식 노드 간의 관계를 나타내는 정확한 방식입니다.
    
    힙 자료구조에서, 배열은 0에서 시작하는 인덱스를 사용합니다. 이 경우 부모 노드를 **`i`**라 할 때, 그 왼쪽 자식 노드의 인덱스는 **`2*i + 1`**이 되고, 오른쪽 자식 노드의 인덱스는 **`2*i + 2`**가 됩니다. 이는 배열의 첫 번째 요소(인덱스 0)를 힙의 루트로 사용할 때 유효합니다.
    
    이러한 인덱싱 방법은 1-인덱스 기반에서의 부모 노드와 자식 노드 간의 관계인 **`2*i`** (왼쪽 자식) 및 **`2*i + 1`** (오른쪽 자식)과는 다릅니다. 1-인덱스 기반 시스템에서는 배열의 첫 번째 요소가 인덱스 1로 시작하지만, 대부분의 프로그래밍 언어에서는 0-인덱스 기반 배열을 사용합니다.
    
    따라서, 당신이 언급한 **`i*2 + 1`**이 왼쪽 자식 노드의 인덱스를 계산하는 올바른 방식임을 알 수 있습니다. 이 코드는 C#을 포함한 대부분의 현대 프로그래밍 언어에서 사용되는 0-인덱스 기반 배열을 가정하고 작성되었습니다.
    

## 참고 자료

- https://www.csharpstudy.com/DS/heap.aspx
- https://gmlwjd9405.github.io/2018/05/10/data-structure-heap.html