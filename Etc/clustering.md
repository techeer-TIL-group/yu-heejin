## 클러스터링(Clustering) 개념

- 클러스터링이란 **똑같은 구성의 여러대의 서버를 병렬로 연결한 상태**를 말한다.
- **여러 대의 서버 컴퓨터를 마치 하나의 가상 컴퓨터처럼 업무를 수행**하도록 하는 것
- 클러스터링된 서버들 중에서 특정 한 대의 서버에서 문제가 발생하더라도 로드밸런서(Load Balancer)에서 그 서버의 분배를 제거함으로써 전체적인 서비스에는 영향을 주지 않고 제어할 수 있으며, 정상적인 서비스가 지속적으로 이루어지게 한다.

## DB 클러스터링

- 동일한 데이터베이스를 여러 대의 서버가 관리하도록 클러스터를 구축하는 것
- Active-Active 방식과 Active-StandBy 방식이 있다.

## 세션 클러스터링(Session Clustering)

- WAS가 2대 이상 연결되어 있을 경우 연결된 모든 서버의 세션을 동일한 세션으로 관리할 수 있도록 도와주는 것

## 참고 자료

- https://m.blog.naver.com/seek316/221874766712
- https://code-lab1.tistory.com/205
- [https://velog.io/@ragnarok_code/서버클러스터란](https://velog.io/@ragnarok_code/%EC%84%9C%EB%B2%84%ED%81%B4%EB%9F%AC%EC%8A%A4%ED%84%B0%EB%9E%80)