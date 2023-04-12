## .nvmrc란?

- nvm의 개별 프로젝트를 위한 설정 파일이다.
- .nvmrc를 통해 특정 프로젝트에 사용되는 버전을 기술할 수 있으면 **nvm을 이용해 프로젝트마다 상이할 수 있는 node version을 빠르게 전환하도록 도와준다.**

## .nvmrc 파일 생성

- 프로젝트 최상위에서 .nvmrc 파일을 생성한다.
    
    ```tsx
    v14.17.6
    ```
    
- 파일 생성 후 .nvmrc 파일이 있는 위치에서 nvm use 명령어를 사용한다.
    
    ```tsx
    nvm use
    ```
    
    - 만약 사용할 node version이 설치되어 있지 않다면 먼저 설치를 진행한다.
    - `nvm install` 명령어를 통해 자동으로 설치된다.

## 참고 자료

- [https://myung-ho.tistory.com/99](https://myung-ho.tistory.com/99)
- [https://taehoonkoo.tistory.com/233](https://taehoonkoo.tistory.com/233)
- [https://hyeok999.github.io/2020/06/02/NVM/](https://hyeok999.github.io/2020/06/02/NVM/)