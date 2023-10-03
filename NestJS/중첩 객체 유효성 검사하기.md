```tsx
export class Test {
  @ValidateNested({ each: true })
  @Type(() => Foo)
  input: Foo[];
}
```

- dto 내 연관 객체에 ValidateNested 데코레이터와 Type 데코레이터를 사용
- 만약 연관 객체가 배열 객체인 경우 ValidateNested 내에 `{ each: true }` 옵션을 부여하면 된다.
    - `{ each: true }`는 요소별로 검사하는 옵션으로, 배열에서 사용한다.
- 중첩된 객체를 검증할 땐 `@Type`을 통해 리터럴 객체를 해당 클래스의 인스턴스로 변환을 해주어야 한다.

## 참고 자료

- [https://www.zodaland.com/tip/8](https://www.zodaland.com/tip/8)
- [https://sub.isthislee.com/validate-nested-dto/](https://sub.isthislee.com/validate-nested-dto/)
- [https://dev.to/avantar/validating-nested-objects-with-class-validator-in-nestjs-1gn8](https://dev.to/avantar/validating-nested-objects-with-class-validator-in-nestjs-1gn8)