## Disjoint Set(서로소 집합)

![Untitled](https://velog.velcdn.com/images%2Fkimdukbae%2Fpost%2F7a95018f-cea9-43d1-967b-56f54bfb1ed4%2Fimage.png)

- **서로 중복되지 않는 부분 집합들로 나눠진 원소들에 대한 정보를 저장하고 조작**하는 자료구조
    - 공통 원소가 없는 상호 배타적인 부분 집합들로 나눠진 원소들에 대한 자료구조이다.
- 집합을 표현하는데 가장 효율적인 트리 구조를 사용하여 구현한다.
- 공통 원소가 없는 상호 배타적인 부분 집합들로 나눠진 원소들에 대한 자료구조이다.

## Union Find 알고리즘

![Untitled](https://gmlwjd9405.github.io/images/algorithm-union-find/union-find-example.png)

- Disjoint Set을 표현할 때 사용하는 알고리즘
- 여러 노드가 있을 때 두 노드가 같은 그래프에 속해있는지(같은 집합인지) 알 수 있음
- union 연산과 find 연산으로 나뉘어진다.
    - `union(x, y)`: x와 y그래프를 합친다.
        - x가 속한 집합과 y가 속한 집합을 합친다.
        - x와 y가 속한 두 집합을 합치는 연산
    - `find(x)`: x가 어느 그래프에 속하는지 연산한다.
        - x가 속한 집합의 대표값(루트 노드 값)을 반환한다.
        - x가 어떤 집합에 속해 있는지 찾는 연산

## Union 연산 코드

```java
x = find(x);    // x의 부모 노드 찾기
y = find(y);    // y의 부모 노드 찾기

if (x == y) {     // 이미 같은 그래프에 속한 경우(같은 집합인 경우) false
	return false:
}

// 더 작은 값을 부모 노드로 설정한다.
if (x <= y) {
	parent[y] = x;
} else {
	parent[x] = y;
}

return true;
```

## find 연산 코드

```java
if (parent[x] == x) {
	return x;   // 부모노드가 자기 자신인 경우 종료
}

return find(parent[x]);
```

## 참고 자료

- [https://velog.io/@suk13574/알고리즘Java-유니온-파인드Union-Find-알고리즘](https://velog.io/@suk13574/%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98Java-%EC%9C%A0%EB%8B%88%EC%98%A8-%ED%8C%8C%EC%9D%B8%EB%93%9CUnion-Find-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98)
- [https://velog.io/@kimdukbae/Union-Find-알고리즘](https://velog.io/@kimdukbae/Union-Find-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98)
- https://gmlwjd9405.github.io/2018/08/31/algorithm-union-find.html