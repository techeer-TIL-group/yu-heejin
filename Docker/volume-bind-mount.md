- Docker 컨테이너에 쓰인 데이터는 **기본적으로 컨테이너가 삭제될 때 함께 사라진다.**
- 그러나 Docker에서 돌아가는 많은 애플리케이션이 컨테이너의 생명 주기와 관계없이 데이터를 영속적으로 저장해야 한다.
- 또한, **여러 개의 컨테이너가 하나의 저장 공간을 공유**해서 데이터를 읽거나 써야한다.
- Docker 컨테이너의 생명 주기와 관계없이 데이터를 영속적으로 저장할 수 있도록 Docker에서는 **볼륨(volume)과 바인드 마운트(bind mount) 두 가지 옵션을 제공한다.**

## 볼륨(Volume)

![Untitled](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FRWsTY%2Fbtr3ogrIuIE%2FFpG7S8ukHvqMTW4Tv2pfB0%2Fimg.png)
![Untitled](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdSmcHC%2FbtrrXpGR9aI%2F76Lycv31t9smhnrMWorolk%2Fimg.png)

- 도커 공식 문서에서 권장하는 방식으로, **도커 엔진이 관리하는 도커 스토리지 디렉토리에 새 디렉토리를 생성하여 컨테이너 내부의 볼륨 데이터를 저장하는 방식**이다.
- 생성된 볼륨은 자동으로 호스트의 도커 스토리지 디렉토리인 `/var/lib/docker/volumes/~`에 저장된다.
- 볼륨을 컨테이너에 탑재하면 이 디렉터리가 컨테이너에 탑재되며, 도커에 의해 관리되고 호스트 시스템의 핵심 기능과 분리된다.
- 바인드 마운트가 호스트 머신의 디렉토리 구조나 OS에 의존적인 반면, 볼륨은 도커에 의해 완전히 관리된다.

### 특징

- Docker API를 사용하여 볼륨 관리
- Linux 및 Windows 컨테이너 모두에서 작동
- 여러 컨테이너 간에 볼륨을 안전하게 공유 가능

### 장점

- 바인드 마운트보다 백업하거나 마이그레이트하기 좋다.
- Docker CLI 커맨드나 Docker API를 활용해 관리할 수 있다.
- 리눅스, 윈도우 컨테이너 모두에서 작동한다.
- 여러 컨테이너 간 공유할 때 더 안전하다.
- 사용 시 컨테이너의 용량을 늘리지 않는다.
    - 컨테이너 생명주기 밖에서 볼륨의 컨텐츠가 존재하기 때문이다.

## 바인드 마운트(Bind mount)

![Untitled](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbkMA3O%2Fbtr3dbr3Mjl%2FVh7kmypNQhsD9ff0W0che1%2Fimg.png)
![Untitled](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbfE8uA%2Fbtrr0AABfhh%2FYsfQkHevGnplanjg8B1KG1%2Fimg.png)

- 도커가 관리하는 디렉토리가 아닌, **호스트 시스템의 파일이나 디렉터리가 컨테이너에 마운트**되며, **호스트 시스템의 절대 경로가 참조**되는 방식이다.
- 도커의 관리 없이 호스트 디렉터리와 마운트하기 때문에 컨테이너에서 호스트의 파일 시스템에 접근하여 **컨테이너에 지정된 파일이 아닌 다른 파일을 삭제하거나 수정할 수 있게 된다.**
- 호스트 시스템의 비 Docker 프로세스에 영향을 줄 수 있으며, 보안에도 영향을 미칠 수 있다.

### 특징

- 파일 또는 디렉터리가 Docker호스트에 존재하지 않는 경우 요청시 생성된다.
- 성능이 뛰어나지만 특정 디렉터리 구조를 사용하는 호스트의 파일 시스템에 의존적이다.
- Docker CLI 명령어를 사용하여 바인드 마운트를 직접 관리할 수 없다.

## Volume vs Bind mount

- 기능적인 측면에서는 Host directory 위치에 파일을 생성, 공유, 수정할 수 있다는 점은 동일하다.
- Docker volume으로 만들면 Docker가 관리하는 디렉토리 내에 생성된다.
- Docker cli or api를 통해서 볼륨 관리가 가능하다.
- mount는 사용자의 소유자와 권한 때문에 실행 및 수정에 문제가 있을 수 있다.
    - 즉, 보안에 취약하다.
    - 반면 볼륨은 여러가지 형태로 볼륨을 만들 수 있고, 관리할 수 있다.

## 참고 자료

- https://yechoi.tistory.com/83
- https://www.daleseo.com/docker-volumes-bind-mounts/
- https://hwan-shell.tistory.com/369
- https://bentist.tistory.com/79