## type alias와 interface의 정의

- 둘 모두 타입에 이름을 부여해주는 것이지만, **type alias는 모든 타입(any)에 이름을 달아줄 수 있으나 interface는 오직 객체 타입에만 가능하다.**

## interface vs type alias

- interface의 대부분의 기능들은 type에서도 가능하다.
- **type alias는 새로운 프로퍼티에 열려있지 않으며, interface는 항상 열려있다.**
- 다음 예시를 살펴보자.
    
    ![Untitled](https://tecoble.techcourse.co.kr/static/33901ee3f6eecc05ab8b35de2871a072/0e821/2022-11-07-interfacetype-interfacevstype.png)
    
    - 왼쪽은 interface, 오른쪽은 type
    - 왼쪽 코드를 보면, interface 키워드를 통해 Window 타입을 선언한 후, 바로 아래에서 다시 다른 프로퍼티를 가진 Window 타입을 선언했다.
        - 위에 선언된 타입의 이름과 같은 이름으로 새로운 타입을 선언했으나 에러가 나지 않고 **나중에 선언된 타입의 프로퍼티를 가진다.**
        - 따라서 interface는 확장 가능하다. (선언 병합)
    - 반면에 오른쪽 코드는 중복 오류가 발생한다.
        - 따라서 **type은 새로운 프로퍼티를 추가할 수 없다.**

### 확장 가능한 방법과 가능하지 않은 방법?

- 확장 가능하지 않은 타입이 존재하는 이유는 타입의 속성들이 추후 추가될 수도 있다면, 개발자들이 타입이 변할 수도 있다고 생각하며 개발하기 때문이다.
    - 이는 마치 let보다는 const를 활용하여 상수로 선언해주는 것이 좋은 것과 비슷한 맥락이다.
- 확장 가능한 방법이 필요한 상황은 다음과 같다.
    - interface를 활용하면 **선언 병합(Declaration merging)**이라는 것이 가능하다.
    - 아래 코드처럼 **기존에 선언된 타입을 확장해서 다른 속성을 추가로 선언**할 수 있는 것이다.
        
        ```tsx
        interface Person {
          name: string;
        }
        
        interface Person {
          age: number;
        }
        
        const user: Person = {
          name: 'al-bur',
          age: 21,
        };
        ```
        
    - 이는 **라이브러리를 사용하는 상황에서 추가적으로 타입의 속성들을 선언할 때 유용**하다.
        
        ```tsx
        // @emotion/react/types
        export interface Theme {}
        
        // emotion.d.ts
        import '@emotion/react';
        
        declare module '@emotion/react' {
          export interface Theme {
            colors: typeof Colors;
          }
        }
        ```
        
        - emotion의 types를 보면, interface 키워드를 통해 Theme라는 타입을 제공한다.
        - emotion 라이브러리를 사용하는 개발자는 **해당 타입에 선언 병합을 활용해 본인들이 원하는 속성들을 선언해 사용할 수 있다.**

## 정리

- type-alias는 모든 타입을 선언할 때 사용할 수 있고, interface는 객체에 대한 타입을 선언할 때 사용될 수 있다.
- 둘 모두 객체에 대한 타입을 선언하는데 사용될 수 있으나 확장 측면에서 사용 용도가 달라진다.
    - 확장 불가능한 타입을 선언하려면 type-alias, 확장 가능한 타입을 선언하려면 interface를 사용하면 된다.
- 회사에서 진행하는 과제에서 interface를 무지성(?)으로 사용했었는데, type으로 바꿀 필요성을 느꼈다!

## 참고 자료

- [https://tecoble.techcourse.co.kr/post/2022-11-07-typeAlias-interface/](https://tecoble.techcourse.co.kr/post/2022-11-07-typeAlias-interface/)