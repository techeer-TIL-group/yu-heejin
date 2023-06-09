## @ValidateIf

```jsx
import { ValidateIf, IsNotEmpty } from 'class-validator';

export class Post {
  otherProperty: string;

  @ValidateIf(o => o.otherProperty === 'value')
  @IsNotEmpty()
  example: string;
}
```

- 해당 필드에 대해 validation을 진행할지 여부를 별도로 설정한다.
- 제공된 조건 함수가 `false`를 반환할 때 속성의 validate를 무시할 수 있다.
    - 조건 함수는 `boolean`값을 반환한다.
- 위 코드에서 `otherProperty`가 `‘value’`가 아니면 유효성 검사를 하지 않는다.
- **조건이 거짓인 경우 모든 유효성 검사가 무시된다.**

## 참고 자료

- https://idenrai.tistory.com/247
- https://malgogi-developer.tistory.com/61