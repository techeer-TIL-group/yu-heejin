- Dockerfile과 같은 디렉토리에 들어있는 모든 파일을 컨텍스트(context)라고 한다.
- 이미지를 생성할 때 컨텍스트를 모두 Docker 데몬에 전송하기 때문에 필요없는 파일이 포함될 가능성이 있다.
- 컨텍스트에서 파일이나 디렉터리를 제외하고 싶을 땐 `.dockerignore` 파일을 사용하면 된다.

## 참고 자료

- https://pyrasis.com/jHLsAlwaysUpToDateDocker/Unit07/01