docker compose 가 여러 개의 컨테이너로 구성된 애플리케이션을 관리하기 위한 도구라면, **Docker Stack은 여러 개의 서비스로 구성된 애플리케이션을 관리하기 위한 도구이다.**

서비스를 관리하기 때문에 Docker Stack은 Swarm 모드에서만 사용할 수 있다.

docker compose는 기본 네트워크가 브릿지 네트워크로 생성되지만 Docker Stack은 Overlay 네트워크가 생성된다.

# Docker Compose와 Docker Stack의 차이점

docker compose는 단일 호스트 환경을 위한 도구로, build 지시어를 통해 이미지를 직접 생성할 수 있으나 여러 노드에 걸친 스케줄링이나 셀프 힐링 기능은 제공하지 않는다.

Docker Stack은 도커 스웜 모드의 멀티 노드 환경을 위한 도구다. 목표 상태를 선언하면, 매니저가 클러스터 전체의 노드 상태를 감시하며 이 상태를 강제로 유지한다. 운영 환경의 오케스트레이션에 특화되어 있다.

# 참고 자료

- https://yoo11052.tistory.com/209
- https://nanujahope.tistory.com/131
- https://incodom.kr/Docker_swarm

---

좋아. 이거 **Swarm을 안 써봤으면 당연히 헷갈리는 개념**이야.

너가 지금 `docker node`, `label`, 이런 거 쓰는 거 보면 Swarm 실험 중인 것 같은데 👀

딱 맞춰서 설명해줄게.

---

# 1️⃣ 스택(Stack)이란?

## 👉 한 줄 정의

> **여러 개의 서비스를 하나의 묶음으로 배포하는 단위**
> 

---

# 2️⃣ 서비스(Service)는 뭐냐?

Swarm에서 서비스는:

> "이 컨테이너를 몇 개 띄우고, 어디에 배치할지 관리하는 객체"
> 

예:

```bash
docker service create --replicas 3 nginx
```

이러면:

- nginx 컨테이너 3개 생성
- Swarm이 노드에 자동 배치
- 죽으면 자동 복구

---

# 3️⃣ 그럼 스택은 뭐냐?

서비스 여러 개를 **한 번에 관리**하는 개념이야.

예를 들어:

- web 서비스
- api 서비스
- db 서비스
- redis 서비스

이걸 하나씩 만들면 관리 지옥이 되겠지?

그래서 등장한 게:

```
docker stack deploy
```

---

# 4️⃣ 스택은 docker-compose.yml을 사용한다

예:

```yaml
version: "3.9"

services:
  web:
    image: nginx
    ports:
      - "80:80"

  db:
    image: mysql
    environment:
      MYSQL_ROOT_PASSWORD: 1234
```

이걸 이렇게 배포:

```bash
docker stack deploy -c docker-compose.yml mystack
```

그럼:

- mystack_web 서비스 생성
- mystack_db 서비스 생성
- 네트워크 자동 생성
- 볼륨 자동 생성

---

# 5️⃣ 그림으로 보면

```
Stack (mystack)
│
├── Service: web (replicas 2)
│       ├── container 1
│       └── container 2
│
└── Service: db (replicas 1)
        └── container 1
```

---

# 6️⃣ 왜 스택이 필요하냐?

Swarm은 클러스터야.

- 노드 여러 개
- 서비스 여러 개
- 네트워크 여러 개
- 볼륨 여러 개

이걸 하나의 논리적 단위로 묶어야 관리 가능해.

---

# 7️⃣ docker compose와 차이

| docker compose | docker stack |
| --- | --- |
| 단일 서버 | Swarm 클러스터 |
| 개발용 | 운영용 |
| 로컬 | 분산 환경 |

---

# 8️⃣ 너 상황에 맞게 말해보면

지금 너는:

- `docker node`
- label 붙이고
- 스케줄링 실험

이건 Swarm 클러스터 환경이야.

스택은:

> "이 클러스터에 여러 서비스를 한 번에 배포하는 방법"
> 

---

# 🎯 한 줄 정리

서비스 = 컨테이너 배포 단위

스택 = 여러 서비스를 묶은 배포 단위