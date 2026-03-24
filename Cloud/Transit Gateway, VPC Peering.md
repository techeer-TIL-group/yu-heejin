# VPC Peering

<img width="1280" height="944" alt="image" src="https://github.com/user-attachments/assets/8bddbdfd-cfd6-4480-85d6-d99a6e53246a" />


다른 VPC간에 Private IP로 통신 가능하도록 연결하는 것을 말한다. 보통의 VPC 간 통신은 Public IP를 통해 이루어진다.

Public IP를 통해 통신할 경우, 데이터의 안전성에 대한 문제가 발생할 수 있는데, VPC Peering을 사용하면 다른 VPC 간에 Private IP 로 통신하기 때문에 데이터의 안정성을 보장받을 수 있다.

## 특징

- **1:1 VPC 연결**만 지원한다.
- 다른 리전의 VPC 및 다른 AWS 계정의 VPC와도 연결이 가능하다.
- 연결된 VPC는 CIDR가 겹치면 안된다.
- 단방향 통신과 양방향 통신 모두 지원한다.
    - 라우팅 테이블을 한쪽만 정의한 경우 단방향 통신 지원
    - 라우팅 테이블을 양쪽 다 정의할 경우 양방향 통신을 지원한다.

# Transit Gateway

VPC Peering과 마찬가지로 서로 다른 VPC 간에 통신이 가능하게 하는 서비스이다.

VPC Peering은 1:1 VPC 연결만 지원해 직접적으로 연결되지 않은 VPC에 바로 접근할 수 없었다면, Transit Gateway는 중앙 허브를 통해 여러 VPC 간 연결 정책을 중앙에서 관리할 수 있고, VPN을 통해 VPC와 온프레미스 네트워크를 연결할 수 있다.

<img width="1280" height="945" alt="image" src="https://github.com/user-attachments/assets/47304f10-f177-4acc-bbe0-362fb47565a4" />


VPC를 Transit Gateway에 연결해주기만 하면 Transit Gateway에 연결된 모든 다른 VPC와 통신이 가능하게 된다.

또한, CGW(Customer Gateway)와 Transit Gateway를 연결하게 되면 Transit Gateway에 연결된 VPC는 VPN을 통해 온프레미스 네트워크와도 통신이 가능하다.

## 특징

- 중앙 허브와 VPN을 통해 VPC와 온프레미스 네트워크를 연결할 수 있다.
- 복잡한 피어링 관계를 제거해 네트워크를 간소화시킬 수 있다.
- 클라우드 라우터 역할을 하므로 새로운 연결을 한 번만 추가하면 된다.
- 다른 리전간의 Transit Gateway와 피어링 연결이 가능하다.
- VPC Peering을 사용하는 것 보다 약 1.5배 저렴하다.

# 참고 자료

- https://yoo11052.tistory.com/147
- https://yoo11052.tistory.com/170
