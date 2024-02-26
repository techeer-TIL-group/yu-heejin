닷넷 7 이상부터 우선순위 큐가 추가되었지만, 이전 버전에서는 사용할 수 없다.

[PriorityQueue<TElement,TPriority> 클래스 (System.Collections.Generic)](https://learn.microsoft.com/ko-kr/dotnet/api/system.collections.generic.priorityqueue-2?view=net-8.0)

## SortedSet을 이용한 우선순위 큐 구현하기

- SortedSet은 내부 요소가 자동적으로 정렬되지만 중복 요소는 허용되지 않는다.
- 원시 타입은 정렬 기준을 만들지 않으면 자동으로 오름차순으로 정렬되지면, 컬렉션이나 배열은 IComparable, IComparer를 통해 정렬 기준을 만들어야한다.

```csharp
public class Program
{
	// 정렬 기준
    public class Mycompare : IComparer<string>
    {	
    // 문자열의 길이에 따라 오름차순
        public int Compare(string x, string y)
        {
            return x.Length - y.Length;
        }
    }
    public static void Main(String[] args)
    {
        // 우선순위 큐 선언
        SortedSet<string> priority_queue = new SortedSet<string>(new Mycompare());

        priority_queue.Add("ABC"); // pq.Push
        priority_queue.Add("ABCDDF");
        priority_queue.Add("AB");
        priority_queue.Add("CDDD");
        priority_queue.Add("C");

        // 큐 안의 원소가 몇개있는지 확인
        while (priority_queue.Count > 0)
        {
            string item = priority_queue.First(); // peek
            priority_queue.Remove(item); // pop
            Console.WriteLine(item);
        }
    }
}
```

## 참고 자료

- https://howudong.tistory.com/335
- [https://velog.io/@wowns226/C-우선순위-큐-사용하기](https://velog.io/@wowns226/C-%EC%9A%B0%EC%84%A0%EC%88%9C%EC%9C%84-%ED%81%90-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0)