- 응답으로 넘어온 JSON 객체는 리터럴 객체이지 **클래스의 인스턴스가 아니다.**
- Axios를 비롯해 NodeJS & Typescript 환경에서 자주 사용하는 HTTP API 중 어느 것도 클래스의 인스턴스를 응답으로 넘겨주진 않는다.
- 종종 개발을 하다보면 외부에서 들어온 JSON 응답을 내부 로직에 사용하거나 내부에 있는 데이터를 외부로 보내줘야할 때가 있는데, 그럴 때마다 들어온 JSON을 그대로 사용하거나 Plain Objet를 만들어 반환해야 한다.
    - 그러나 이러한 경우 **외부의 세부 사항이 그대로 드러나기 때문에 내부 로직을 더럽히는 경우가 발생한다.**
    - 즉, 캡슐화가 제대로 되지 않는다!

## 문제

- 외부 API를 통해 다음과 같은 결과를 받았다고 가정하자.
    
    ```json
    [
      {
        "id": 1,
        "firstName": "Johny",
        "lastName": "Cage",
        "age": 27
      },
      {
        "id": 2,
        "firstName": "Ismoil",
        "lastName": "Somoni",
        "age": 50
      },
      {
        "id": 3,
        "firstName": "Luke",
        "lastName": "Dacascos",
        "age": 12
      }
    ]
    ```
    
    - 보통 해당 값을 가공하거나 추가하는 등의 비즈니스 로직이 동반된다.
- 위 JSON처럼 값만 있는 리터럴 객체라면 추가 가공은 별도의 함수에서 처리해야 한다.
    
    ```tsx
    const users = api.getUsers();
    return users.map(u => toFullName(u)); // 값 user와 toFullName 함수가 별도로 존재한다
    
    export function toFullName (user) {
      return `${user.firstName} ${user.lastName}`;
    }
    ```
    
    - 따라서 상태와 행위가 따로 노는 응집력이 떨어지는 코드가 된다.
    - 만약 위 코드에서 `isAdult` 같은 추가 가공 로직이 하나 더 있다면 응집력이 더욱 떨어진다.
- 반면, **받은 값 가공 로직을 클래스 내부에 둔다면 상태와 행위가 한 곳에 있는 응집력있는 코드**가 된다.
    
    ```tsx
    export class User {
      id: number
      firstName: string
      lastName: string
      age: number
    
      getFullName() {
        return this.firstName + ' ' + this.lastName
      }
    
      isAdult() {
        return this.age > 36 && this.age < 60
      }
    }
    ```
    
    - 위처럼 작성된 코드에서는 아래와 같이 User 클래스에 모든 책임을 위임할 수 있다.
        
        ```tsx
        const users: User[] = api.getUsers();
        return users.map(u => u.getFullName());
        ```
        
- 반드시 외부 API로 받은 값에서만 발생하지 않고, **프론트엔드에서 넘겨준 Request Body에서도 값과 행위가 함께 응집력있는 코드가 필요한 경우가 대부분**이다.
- 흔히 말하는 OOP, 도메인 기반의 Entity 설계 등을 고려했을 때 **어떤 객체에 어떤 책임을 줄 것인가는 매우 중요**하다.

## 해결

- 위처럼 외부와 연동하는 상황에서 요청/응답 값을 리터럴 객체로만 다루는 것은 한계가 있다.
- 리터럴 객체 ↔ 클래스 인스턴스 변환은 필요한 작업 중 하나인데, 이 문제를 `class-transformer`가 해결해준다.
- NestJS에서는 `class-transformer`를 **글로벌 설정으로 Controller에서 받는 Request Dto로 변환**할 수 있도록 지원한다.
- 다음과 같은 정보를 API를 통해 받아온다고 가정하자.
    
    ```json
    {
      "by": "norvig",
      "id": 2921983,
      "kids": [
        2922097,
        2922429,
        2924562,
        2922709,
        2922573,
        2922140,
        2922141
      ],
      "parent": 2921506,
      "text": "Aw shucks, guys ... you make me blush with your compliments.<p>Tell you what, Ill make a deal: I'll keep writing if you keep reading. K?",
      "time": 1314211127,
      "type": "comment"
    }
    ```
    
    - 다음과 같은 비즈니스 로직이 필요하다고 가정하자.
        1. `time`의 ms 타임을 `LocalDateTime`으로 변환한 값이 필요하다.
        2. `parent`가 없는 경우엔 최상위 `item`임을 알 수 있어야 한다.
- 위 데이터를 단일 클래스로 표현하면 다음과 같다.
    
    ```tsx
    export class HackerNewsItem {
      by: string;
      descendants: number;
      id: number;
      kids: number[];
      parent: number;
      score: number;
      time: number;
      title: string;
      type: string;
      url: string;
    
      constructor() {}
    
      get createTime(): LocalDateTime {
        const milliTime = this.time * 1000;
        return LocalDateTime.ofInstant(Instant.ofEpochMilli(milliTime));
      }
    
      get isFirstItem(): boolean {
        return !this.parent;
      }
    }
    ```
    
    - 이를 Http API와 함께 쓴다면 다음과 같다.
        
        ```tsx
        const result = api.get();
        const hackerNewsItem = plainToClass(HackerNewsItem, result.data);
        ```
        
        - 이처럼 `plainToClass`와 함께 사용한다면 리터럴 객체를 다룰 필요 없이 **값과 행위가 한 곳에 모여있는 클래스 인스턴스 단위로 다룰 수 있게 된다.**

### 제네릭을 활용한 HTTP API 함수

- 매번 plainToClass를 호출하는 것이 번거롭다면, 다음과 같이 별도의 함수를 만들어 사용할 수도 있다.
    
    ```tsx
    export const instance: AxiosInstance = axios.create({
      responseType: 'json',
      validateStatus(status) {
        return [200].includes(status);
      },
    });
    
    export async function request<T>(
      config: AxiosRequestConfig,
      classType: any,
    ): Promise<T> {
      const response = await instance.request<T>(config);
      return plainToClass<T, AxiosResponse['data']>(classType, response.data);
    }
    ```
    
- `request<T>` 제네릭을 사용하면 편리하게 HTTP API를 호출할 수 있다.
    
    ```tsx
    it('HackerNews를 통해서 가져온다', async () => {
      const data: HackerNewsItem = await request<HackerNewsItem>(
        {
          url: 'https://hacker-news.firebaseio.com/v0/item/2921983.json',
          method: 'get',
        },
        HackerNewsItem,
      );
    
      expect(data.type).toBe('comment');
      expect(data.createTime.toString()).toBe(
        LocalDateTime.of(2011, 8, 25, 3, 38, 47).toString(),
      );
      expect(data.isFirstItem).toBe(false);
    });
    ```
    

### 케멀 케이스 ↔ 스네이크 케이스

- class-transformer는 일반적으로 `@Expose()` 데코레이터를 기반으로 변환 기준을 잡는다.
    - 따라서 모든 클래스의 속성에 `@Expose()`를 선언해야하는게 아닐까 싶지만, 기본적으로 클래스 속성으로 선언되어 있으면 변환 대상으로 자동 인지된다.
    - 따라서 위 예제에서는 별도로 데코레이터 지정 없이도 가능하다.
- `@Expose()`는 서로 이름, 컨벤션이 다른 경우 유용하게 사용할 수 있다.
    - 대표적으로 오픈 API가 있는데, 오픈 API 대부분이 스네이크 케이스로 결과를 전송한다.
        
        ```
        {
            email: 'test@test.com',
            phone_number: '+82 10-1234-1234',
        };
        ```
        
    - 특별한 조작이 없다면 클래스 내에서도 스네이크 케이스를 사용해야만 하지만, `class-transformer`를 사용하면 스네이크 케이스를 클래스에 맞게 케멀 케이스를 사용할 수 있다.
        
        ```tsx
        export class KakaoAccountDto {
          @Expose({ name: 'email' })
          public _email: string;
        
          @Expose({ name: 'phone_number' })
          public phoneNumber: string;
        
        }
        ```
        
        - `@Expose({ name: })` 을 지정하면 name에 일치하는 값으로 매핑된다.

### 특정 필드 제외

- 특정 상황에서는 클래스의 일부 필드는 비즈니스 로직에서는 사용하되 HTTP 응답에서는 제외하는 등의 로직이 필요하다.
    - 대표적으로 `getter` 패턴 DTO를 들 수 있다.
- `getter` 패턴과 같은 경우 `@Exclude()`를 통해 **private 필드는 변환 대상에서 제외**하면 `getter`만 변환 대상에 포함되기 때문에 의도한대로 응답 결과를 내려줄 수 있다.
    
    ```tsx
    export class ResponseEntity<T> {
    
      // private 필드들은 모두 @Exclude()로 제외
      @Exclude() private readonly _statusCode: string;
      @Exclude() private readonly _message: string;
      @Exclude() private readonly _data: T;
    
      public constructor(status: ResponseStatus, message: string, data: T) {
        this._statusCode = ResponseStatus[status];
        this._message = message;
        this._data = data;
      }
    
      // getter 는 모두 @Expose()로 공개
      @Expose()
      get statusCode(): string {
        return this._statusCode;
      }
    
      @Expose()
      get message(): string {
        return this._message;
      }
    
      @Expose()
      get data(): T {
        return this._data;
      }
    }
    ```
    

### 중첩 객체 변환

- 클래스 안의 클래스 (중첩 객체)가 있는 인스턴스를 변환하려는 경우 중첩 객체의 타입을 알아야 한다.
- TypeScript는 좋은 리플렉션 기능이 없기 때문에 **각 속성에 설정된 객체 타입을 명시적으로 지정해야 한다.**
- 다음과 같이 `Album` 클래스 내부에 `Photo` 클래스 타입의 변수가 있을 경우, `@Type()` 데코레이터를 사용해서 변환이 가능하다.
    
    ```tsx
    export class Photo {
      id: number
      filename: string
    }
    ```
    
    ```tsx
    import { Type, plainToClass } from 'class-transformer'
    
    export class Album {
      id: number
    
      name: string
    
      @Type(() => Photo)
      photos: Photo[]
    }
    ```
    
    - 위처럼 `@Type()`을 사용하면 어떤 클래스 타입인지 명시할 수 있기 때문에 `class-transformer`가 변환할 수 있다.
    - 이 경우 다음과 같이 `plainToClass`로 편하게 리터럴 객체에서 클래스 인스턴스로 변환이 가능하다.
        
        ```tsx
        const album = plainToClass(Album, albumJson);
        ```
        

## plainToClass

- `plainToClass`는 deprecated 되었기 때문에 `plainToInstance`를 사용해야 한다.
- 외부에서 API를 호출해 다음과 같은 데이터를 얻었다고 가정하자.
    
    ```
    {
        username: "junha park",
        birthDate: "1994-09-02"
    }
    ```
    
    - 이 경우 유저의 나이를 알기 위해서는 매번 다음과 같은 로직을 작성해줘야 한다.
        
        ```tsx
        function calculateAge(){
            var today = new Date();
            var birthDate = new Date(dateString);
            var age = today.getFullYear() - birthDate.getFullYear();
            var m = today.getMonth() - birthDate.getMonth();
            if (m < 0 || (m === 0 && today.getDate() < birthDate.getDate())) {
                age--;
            }
            return age;
         }
         
         
        calculateAge(apiResult.birthDate)
        ```
        
        - 이러한 방법이 기능상 문제가 되는 것은 아니지만, 나이를 계산하는 코드를 따로 분리해서 보관한다는 점이 지저분한 코드를 유발한다.
    - 위와 같은 경우 `plainToClass`를 활용하면 된다.
- 먼저 유저 API를 호출해 가져온 정보로 만들 클래스를 작성한다.
    
    ```tsx
    class User{
        username: string
        birthDate: string
        
        // 나이를 계산하는 로직 
        get age() {
            var today = new Date();
            var birthDate = new Date(this.birthDate);
            var age = today.getFullYear() - birthDate.getFullYear();
            var m = today.getMonth() - birthDate.getMonth();
            if (m < 0 || (m === 0 && today.getDate() < birthDate.getDate())) {
               age--;
            }
            return age;
        }
    }
    ```
    
- plainToClass를 이용해 클래스로 만들어주면 다음과 같다.
    
    ```tsx
    import { plainToClass } from "class-transformer";
    
    const user = plainToClass(User, apiResult);
    console.log(user);
    //User { username: 'junha park', birthDate: '1994-09-02' }
    
    console.log(user.age);
    //27
    ```
    
    - 이로써 캡슐화가 가능해진다.

## 참고 자료

- [https://jojoldu.tistory.com/617](https://jojoldu.tistory.com/617)
- [https://cocook.tistory.com/203](https://cocook.tistory.com/203)
