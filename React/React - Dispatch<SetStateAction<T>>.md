- useState의 set함수 타입
    
    ```tsx
    Dispatch<SetStateAction<T>>
    ```
    
- 예를 들어, `const [a, setA] = useState(’’);` 코드의 타입은 다음과 같다.
    
    ```tsx
    Dispatch<SetStateAction<string>>
    ```
    

## 참고 자료

- [https://basemenks.tistory.com/241](https://basemenks.tistory.com/241)
