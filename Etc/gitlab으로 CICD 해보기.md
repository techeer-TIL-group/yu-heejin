## GitLab Runner 설치하기

- 폐쇄망이라 인터넷이 불가능한 관계로 인터넷 환경에서 아래 링크에서 rpm 패키지 설치
    
    https://packages.gitlab.com/runner/gitlab-runner/packages/el/7/gitlab-runner-14.0.0-1.x86_64.rpm
    
- 폐쇄망으로 해당 rpm 패키지 복사 및 아래 명령어 실행
    
    ```bash
    # rpm 파일이 존재하는 경우 yum localinstall
    yum localinstall gitlab-runner-14.0.0-1.x86_64.rpm
    sudo gitlab-runner register
    ```
    

## GitLab pipeline

```yaml
# 작성한 순으로 작업 실행
stages:
 - update
 - install
 - deploy

variables:
	NAME: 'hello'
 
# 모든 job이 끝날 때마다 after_script를 실행한다
after_script:
  - 'net use /delete o:'

update:       # job name
 stage: update
 only:
  - master
 script:
  - cd gitlab-actions-practice
  - git pull origin master

npm:
 stage: install
 cache:
  paths:
   - node_modules
 script:
  - cd request-tester
  - npm install
 
deploy:
 stage: deploy
 script:
  - pwd
  - cd request-tester
  - npm run dev

```

### stages

- 여러 개의 job을 순차적으로 실행할 수 있다.
- 등록한 순서대로 실행한다.
- 같은 stage를 가지는 job은 같이 실행된다.

### cache

- 과거에 수행한 job에서 만든 캐시를 사용할 수 있다.
- npm install처럼 매번 인터넷에서 라이브러리를 다운받아야 한다면 배포 시간을 단축할 수 있다.

### only

- 오직 해당 브랜치에 커밋이나 변경사항이 있을 때만 동작한다.

### after_script

- 모든 job이 끝날 때마다 실행할 명령을 정의한다.

### variables

- 전역변수 선언

## 참고 자료

- https://playit.tistory.com/4
- https://lovemewithoutall.github.io/it/deploy-example-by-gitlab-ci/
- https://bravenamme.github.io/2020/11/09/gitlab-runner/
