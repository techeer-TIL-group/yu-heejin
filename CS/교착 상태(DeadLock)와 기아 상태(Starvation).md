- **두 개 이상의 프로세스나 스레드가 서로 자원을 얻지 못해 다음을 처리하지 못하는 상태**
- 무한히 다음 자원을 기다리게 되는 상태를 말한다.

## 주로 발생하는 경우

- 멀티 프로그래밍 환경에서 한정된 자원을 얻기 위해 서로 경쟁하는 상황 발생
- 한 프로세스가 자원을 요청했을 때 동시에 그 자원을 사용할 수 없는 상황이 발생할 수 있다.
    - 이 때 프로세스는 대기 상태로 돌아간다.
- 대기 상태로 들어간 프로세스들이 실행 상태로 변경될 수 없을 때 교착 상태가 발생한다.

## DeadLock 발생 조건

> 아래 4가지 모두 성립해야 데드락이 발생한다.
이 말은 즉 **하나라도 성립하지 않으면 데드락 문제가 해결된다.**
> 

### 상호 배제(Mutual Exclusion)

- 자원은 한 번에 한 프로세스만 사용할 수 있다.
- 사용중인 자원을 다른 프로세스가 사용하려면 요청한 자원이 해제될 때까지 기다려야 한다.

### 점유 대기(Hold and Wait)

- 최소한 하나의 자원을 점유하고 있으면서 다른 프로세스에 할당되어 사용하고 있는 자원을 추가로 점유하기 위해 대기하는 프로세스가 존재해야 한다.

### 비선점(No Preemption)

- 다른 프로세스에 할당된 자원은 사용이 끝날 때까지 강제로 빼앗을 수 없다.

### 순환 대기(Circular Wait)

- 프로세스의 집합에서 순환 형태로 자원을 대기하고 있어야 한다.

## DeadLock 처리

### 예방(prevention)

> 교착 상태 발생 조건중 하나를 제거하면서 해결한다. (자원 낭비가 심하다)
> 
- 상호배제 부정: 여러 프로세스가 공유 자원을 사용할 수 있도록 설정
    - 단, 추후 동기화 문제가 발생할 수 있다.
- 점유대기 부정: 프로세스 실행 전 모든 자원을 한꺼번에 요구하고 허용할 때까지 작업을 보류해서 나중에 또 다른 자원을 점유하기 위한 대기 조건을 성립하지 않도록 한다.
- 비선점 부정: 자원 점유 중인 프로세스가 다른 자원을 요구할 때 가진 자원 반납
    - 이미 다른 프로세스에게 할당된 자원이 선점권이 없다고 가정할 때, 높은 우선순위의 프로세스가 해당 자원을 선점할 수 있도록 한다.
- 순환대기 부정: 자원에 고유번호 할당 후 순서대로 자원 요구
    - 자원을 순환 형태로 대기하지 않도록 일정한 한 쪽 방향으로만 자원을 요구할 수 있도록 한다.

### 회피(avoidance)

> 교착 상태 발생 시 피해나가는 방법
> 
- 시스템의 프로세스들이 요청하는 **모든 자원을, 데드락을 발생시키지 않으면서도 차례로 모두에게 할당해줄 수 있다면 안정 상태(safe state)**에 있다고 말한다.
- 그리고 이처럼 특정한 순서로 프로세스들에게 자원을 할당, 실행 및 종료 등의 작업을 할 때 **데드락이 발생하지 않는 순서를 찾을 수 있다면, 그것을 안전 순서(safe sequence)라고 부른다.**
- 반대로 불안정 상태는 안정 상태가 아닌 상황을 의미하는데, 데드락 발생 가능성이 있는 상황이며, 교착상태는 불안정 상태일 때 발생한다.
- **회피 알고리즘은 자원을 할당한 후에도 시스템이 항상 safe state에 있을 수 있도록 할당을 허용하는 것이 기본 특징이다.**
    - 이러한 특징을 살린 알고리즘으로 유명한 것이 은행원 알고리즘이다.
- 은행원 알고리즘(Banker’s Algorithm)
    - 은행에서 모든 고객의 요구가 충족되도록 현금을 할당하는데서 유래한다.
    - 프로세스가 자원을 요구할 때 **시스템은 자원을 할당한 후에도 안정 상태로 남아있게 되는지 사전에 검사하여 교착 상태 회피**
    - 안정 상태면 자원 할당, 아니면 다른 프로세스들이 자원해지까지 대기

### 탐지(Detection)

> 시스템이 데드락 예방이나 회피법을 사용하지 않았을 경우 데드락이 발생할 위험이 있기 때문에 회복을 위해 데드락을 탐지하고 회복하는 알고리즘을 사용한다.
> 
- 자원 할당 그래프 혹은 Allocation, Request, Available 등으로 시스템에 데드락이 발생했는지 여부를 탐지한다.
    
    [Deadlock 개념이란? 그에 대한 해결책/회피책](https://jwprogramming.tistory.com/12)
    
- 자원 요청 시, 탐지 알고리즘을 실행시켜 그에 대한 오버헤드 발생

### 회복(Recovery)

- 데드락 탐지 기법을 통해 발견한 경우, 데드락으로부터 회복하기 위한 방법을 사용한다.
- 교착 상태를 일으킨 프로세스를 종료하거나 할당된 자원을 해제시켜 회복시키는 방법
1. 단순히 프로세스를 한 개 이상 중단시키는 방법
    - 교착 상태에 빠진 모든 프로세스를 중단시키는 방법
        - 계속 연산중이던 프로세스들도 모두 일시에 중단되어 부분 결과가 폐기될 수 있는 부작용이 발생할 수 있다.
    - 프로세스를 하나씩 중단시킬 때마다 탐지 알고리즘으로 데드락을 탐지하면서 회복시키는 방법
        - 매번 탐지 알고리즘을 호출 및 수행해야하므로 부담이 되는 작업일 수 있다.
2. 자원 선점하기
    - 프로세스에 할당된 자원을 선점해서 **교착 상태를 해결할 때까지 그 자원을 다른 프로세스에 할당해주는 방법**
    - 우선 순위가 낮은 프로세스, 수행된 횟수가 적은 프로세스 등을 위주로 프로세스의 자원을 선점한다.

## 은행원 알고리즘과 식사하는 철학자들

### 은행원 알고리즘

[[운영체제] 데드락(Deadlock, 교착 상태)이란?](https://chanhuiseok.github.io/posts/cs-2/)

- 다익스트라가 제안한 기법으로, 어떤 자원의 할당을 허용하는지에 관한 여부를 결정하기 전에 **미리 결정된 모든 자원들의 최대 가능한 할당량을 가지고 시뮬레이션 해서 safe state에 들 수 있는지 여부를 검사한다.**
    - 즉, 대기중이 다른 프로세스들의 활동에 대한 교착 상태 가능성을 미리 조사하는 것

### 식사하는 철학자들

[[운영체제] 식사하는 철학자들 문제 (Dining philosophers problem) - Deadlock, Starvation](https://itstory1592.tistory.com/95)

## 기아 상태(Starvation)

- 특정 프로세스의 우선 순위가 낮아서 원하는 자원을 계속 할당받지 못하는 상태

### 교착 상태와의 차이점

- 교착 상태는 프로세스가 자원을 얻지 못해 다음 처리를 하지 못하는 상태를 말하고, 기아 상태는 프로세스가 원하는 자원을 계속 할당 받지 못하는 상태이다.
- 즉, 교착 상태는 여러 프로세스가 동일한 자원 점유를 원할 때 발생하고, 기아 상태는 여러 프로세스가 자원을 점유하기 위해 경쟁할 때 특정 프로세스는 영원히 자원을 할당받지 못하는 것이다.

## 참고 자료

- https://gyoogle.dev/blog/computer-science/operating-system/DeadLock.html
- https://chanhuiseok.github.io/posts/cs-2/
- https://jwprogramming.tistory.com/12
- https://excited-hyun.tistory.com/121
- https://arainablog.tistory.com/276