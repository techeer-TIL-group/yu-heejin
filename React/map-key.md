- 다음과 같이 **컴포넌트를 반복해서 렌더링해야하는 경우 map을 사용한다.**
    
    ![Untitled](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbLQ5Dq%2Fbtq0SkRQkMc%2FOIboEeDcFIg3aQF8IG42H0%2Fimg.png)
    
    - 이 때 **배열 내부의 앨리먼트에 key값을 지정하지 않으면 경고문이 뜬다.**
- React 공식 문서에 따르면, **key는 어떤 아이템이 변화되거나 추가 또는 삭제되었는지를 알아차리기 위해 필요하다.**
- React는 state에서 변경된 부분만 찾아서 리렌더링 해주는데, **이때 가상 dom과 비교하여 바뀐 부분만 리렌더링 해준다.**
    - 만약 배열에 요소가 추가되었다면, **배열 전체가 변경된 것이라 생각하고 전체를 리렌더링하게 된다.**
    - 비록 하나의 요소만 변경되었음에도 불구하고 불필요하게 전체를 리렌더링한다.
    - 따라서 배열 내부 엘리먼트에 key를 지정해줘야 한다.
- **key 값을 지정해주면 리액트는 배열에 추가된 요소에 대해서만 리렌더링하게 된다.**
    - 즉, key는 배열의 어떤 원소에 변동이 있었는지 알아내고자 할 때 사용되는 것!
    - 따라서 데이터가 가진 고유의 값을 key로 설정해야 한다.

## 예시 코드

```tsx
const PostList = ({ posts }) => {
    return(
        posts.map((post) => {
            return(
                <tr key={post.id}>
                    <td>{post.id}</td>
                    <td>{post.nickname}</td>
                    <td>{post.role}</td>
                    <td>{post.phone}</td>
                </tr>
            );
        })
    ); 
}

export default PostList;
```

## 주의할 점

```tsx
// 기존 배열 
key 0, {id: 0, title: 'hello', content: 'olleh'}, 
key 1, {id: 1, title: 'my', content: 'ym'}, 
key 2, {id: 2, title: 'name', content: 'eman'}, 

// 배열 첫 번째 위치에 새로 element를 넣음 
key 0, {id: 3, title: 'is', content: 'si'}, 
key 1, {id: 0, title: 'hello', content: 'olleh'}, 
key 2, {id: 1, title: 'my', content: 'ym'}, 
key 3, {id: 2, title: 'name', content: 'eman'},
```

- **key값으로 배열의 index를 설정하는 경우**도 있는데, 어떤 경우에는 key 설정의 장점을 살리지 못하고 배열 전체가 리렌더링 되는 경우도 있다.
    - 배열에 첫번째 위치에 새로운 element를 넣게 되면, 기존 배열의 index가 하나씩 밀리게 되고, 결국 배열 전체가 변경되었다고 생각해 배열 전체가 리렌더링된다.
- 따라서 **element를 고유하게 식별할 수 있는 unique한 값을 key로 설정해주는 것이 좋다.**
    - 가장 사용하기 쉬운 것이 데이터베이스 id 값 (AUTO_INCREMENT)
    - id가 아니더라도 유니크한 값이면 좋다.
- 다만 **다음과 같은 조건을 만족하는 경우 index를 key로 사용해도 된다.**
    1. 배열과 각 요소가 static이며, computed 되지 않고 변하지 않아야 한다.
    2. 데이터 내부에 id로 쓸만한 unique 값이 없다.
    3. 데이터가 결코 reordered 또는 filtered 되지 않는다.

## 참고 자료

- [https://dolphinsarah.tistory.com/21](https://dolphinsarah.tistory.com/21)