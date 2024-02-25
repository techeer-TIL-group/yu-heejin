## 해시(Hash)

![https://kang-james.tistory.com/136](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbcluEM%2FbtryF769jdT%2FtldrkmqK9arIia8rGKH5e0%2Fimg.png)

https://kang-james.tistory.com/136

- 해시는 **입력 데이터를 고정된 길이의 데이터로 변환된 값**을 말한다.
    - 다른 말로는 해시 값(Hash value), 해시 코드, 체크섬이라고도 한다.
- 이러한 해시는 해시 함수에 의해서 얻게 된다.
    - 간단히 말해 데이터의 Key 값이 해시 함수를 통해 변환된 간단한 정수이다.
    - key를 해시함수라는 함수에 input으로 넣어서 Output으로 나오는 것이 Hash라고 생각하면 되고, 이 Hash가 저장 위치가 된다고 생각하면 된다.
    - 결국 Hash Function은 Key로 해시를 만들어내는 함수이다.
- 이렇게 정수로 변환된 해시는 배열의 인덱스, 위치, 데이터 값을 저장하거나 검색할 때 활용된다.

## 특징

- Key에 Value를 매핑할 수 있는 데이터 구조
- 해시 함수를 통해 키의 데이터를 배열에 저장할 수 있는 주소(인덱스 번호)를 계산
- 키를 통해 저장된 데이터를 빠르게 찾고, 저장 및 탐색 속도가 획기적으로 빨라진다.

## 해시 함수

- 임의의 데이터를 고정된 길이의 값으로 리턴해주는 함수
- 입력받은 데이터를 해시 값으로 출력시키는 알고리즘이다.
    - 출력된 해시 값은 알고리즘에 따라 다양한 결과를 보인다.
    - 따라서 함수는 목적에 맞게 다양하게 설계된다.

## 해시 테이블(Hash Table)

- 키 값의 연산에 의해 직접 접근이 가능한 데이터 구조
- 키와 값을 함께 저장해둔 데이터 구조이다.
    - 이는 데이터가 행과 열로 구성된 표에 저장되는 것과 유사하다.
- 테이블에 데이터를 저장할 때 위치는 무작위로 지정되어 작성되기 때문에 중간에 빈 공간이 발생할 수 있다.

## 해싱(Hasing)

![Untitled](hhttps://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb8A2pM%2FbtryI5m2piK%2Fcv3C7kfDq5xsaQuv4inKWK%2Fimg.png)

- 해싱은 해시 함수에서 해시를 출력하고 해시 테이블에 저장하는 과정까지의 행위를 말한다.

## 해시에서의 충돌(Collision)

- 만약 다른 키가 있는데 값이 동일하게 추출된 경우, 배열에서 같은 장소에 저장되고 이전에 저장된 정보는 사라진다.
    - 이를 충돌, Collision이라고 한다.
    - 즉, 서로 다른 key가 hashing 후 같은 Hash값이 나오는 경우 충돌이라고 한다.

### 충돌이 발생하는 경우

1. 함수 알고리즘의 성능이 좋지 못한 경우
2. 저장되는 데이터 양이 해시 테이블의 크기보다 클 때

### 충돌 해결하기

1. Chaining 기법
    
    ![Untitled](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FwBocM%2FbtryJjFpTos%2FHM3Ap9sSUwDGxT6KawqHJ0%2Fimg.png)
    
    - 개방 해싱 또는 Open Hasing 기법 중 하나
    - 해시 테이블 저장공간 외의 공간을 활용하는 기법
    - 충돌이 발생했을 때 연결 리스트 자료구조를 사용해서 해결하는 방법
2. Linear Probing 기법
    - 폐쇄 해싱 또는 Close Hashing 기법 중 하나
    - 해시 테이블 저장공간 안에서 충돌 문제를 해결하는 기법
    - 충돌이 발생했을 때 해당 해시 주소의 다음 주소부터 맨 처음까지 순회하며 빈 공간을 찾는 방식
    - 저장 공간 활용도를 높이기 위한 기법
    - Linear Probing 기법은 해시 함수를 통해서 얻은 주소에 이미 데이터가 있다면 다음 주소를 체크한다.
        - 만약 체크했을 때 데이터가 없다면 해당 자리에 저장된다.

## 해시의 시간 복잡도

- 해시 자료구조는 충돌이 없는 일반적인 경우 O(1)이 발생한다.
    - 키를 통해 바로 저장과 검색을 하여 값을 구하기 때문이다.
- 최악의 경우 모든 인덱스에서 충돌이 발생할 경우 O(n)이 발생한다.

## C#에서의 HashTable

- C#의 Hashtable은 key-value 구조를 가지는 컬렉션이다.
- key는 데이터를 식별하기 위해 필요한 정보를 가지며, Hashtable에서 키는 모든 데이터 타입이 될 수 있다.
- value는 키에 매핑되는 데이터이다.
- HashTable은 Key-Value 구조로 키를 사용하여 데이터에 접근할 수 있다.
    - Hashtable은 각 키에 대해 해시 코드를 계산하므로 데이터 접근 성능을 향상시킨다.

```csharp
Hashtable ht = new Hashtable();
ht.Add("irina", "Irina SP");
ht.Add("tom", "Tom Cr");

if (ht.Contains("tom"))
{
	Console.WriteLine(ht["tom"]);   // 값이 존재하는 경우 해당 값 출력
}

class Program
{
    static void Main(string[] args)
    {
        Hashtable userInfo = new Hashtable();

        // 첫 번째 인자는 Key
        // 두 번째 인자는 Value
        userInfo.Add("name", "홍길동");
        userInfo.Add("age", 20);
        userInfo.Add("address", "서울특별시 강서구...");
        userInfo.Add("email", "Hong@Test.com");

        // 이름을 알고 싶은 경우
        Console.WriteLine("이름 : " + userInfo["name"]);

        // 이메일을 알고 싶은 경우
        Console.WriteLine("이메일 : " + userInfo["email"]);
    }
}
출처: https://developer-talk.tistory.com/321 [DevStory:티스토리]
```

- Hashtable은 Double Hashing 방식을 사용하여 Collision Resolution을 하게 된다.
    - 즉, 해시 함수를 사용하여 key에서 충돌이 발생하면, 다른 해시 함수를 계속 사용하여 빈 버켓을 찾게 된다.
- 이 자료구조는 추가, 삭제, 검색에서 O(1)의 시간이 소요된다.
- 만약 동일한 키를 중복해서 설정하는 경우 예외가 발생한다.
    - 키는 값을 식별하기 위해 사용되는 고유한 값이므로 Hashtable의 객체는 동일한 키를 가질 수 없다.
- Hashtable 객체를 반복문을 이용해 값을 접근하는 경우 foreach()문을 사용할 수 있다.
    
    ```csharp
    class Program
    {
        static void Main(string[] args)
        {
            Hashtable userInfo = new Hashtable();
    
            userInfo.Add("name", "홍길동");
            userInfo.Add("age", 20);
            userInfo.Add("address", "서울특별시 강서구...");
            userInfo.Add("email", "Hong@Test.com");
    		
            foreach(object key in userInfo.Keys)
            {
                Console.WriteLine("Value : " + userInfo[key]);
            }
        }
    }
    출처: https://developer-talk.tistory.com/321 [DevStory:티스토리]
    ```
    
- 특정 키나 값이 존재하는지 확인하려면 `ContainsKey()`나 `ContainsValue()` 함수를 사용한다.
    
    ```csharp
    class Program
    {
        static void Main(string[] args)
        {
            Hashtable userInfo = new Hashtable();
    
            userInfo.Add("name", "홍길동");
            userInfo.Add("age", 20);
            userInfo.Add("address", "서울특별시 강서구...");
            userInfo.Add("email", "Hong@Test.com");
    
            // ContainsKey() 함수 사용
            Console.WriteLine("name이라는 키가 존재하는가? : " + userInfo.ContainsKey("name"));
            Console.WriteLine("gender라는 키가 존재하는가? : " + userInfo.ContainsKey("gender"));
            
            // ContainsValue() 함수 사용
            Console.WriteLine("20이라는 값이 존재하는가? : " + userInfo.ContainsValue(20));
            Console.WriteLine("man이라는 값이 존재하는가? : " + userInfo.ContainsValue("man"));
        }
    }
    출처: https://developer-talk.tistory.com/321 [DevStory:티스토리]
    ```
    

## 참고 자료

- https://kang-james.tistory.com/136
- https://go-coding.tistory.com/30
- https://developer-talk.tistory.com/321