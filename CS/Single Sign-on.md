## SSO(Single Sign-On)

![https://aws.amazon.com/ko/what-is/sso/](https://d1.awsstatic.com/AM_AWSSSOUseCases_120717a(1).6fddf94767ba21cae19bbd26106cc97ecb491929.png)

https://aws.amazon.com/ko/what-is/sso/

- **1회 사용자 인증으로 다수의 애플리케이션 및 웹 사이트에 대한 사용자 로그인을 허용**하는 인증 솔루션
    - 여러 개의 사이트에서 한 번의 로그인으로 여러가지 다른 사이트들을 자동적으로 접속하여 이용하는 방법이다.
- 사용자들은 브라우저에서 직접 애플리케이션에 자주 액세스하기 때문에 **조직은 보안 및 사용자 경험 모두를 개선하는 액세스 관리 전략에 우선 순위를 둔다.**
- SSO는 한 번 자격 증명이 검증된 사용자에게는 반복되는 로그인 없이 모든 암호 보호 리소스에 액세스하도록 하여 보안과 사용자 경험을 모두 충족할 수 있다.

## SSO의 장점

### 암호 보안 강화

- SSO를 사용하지 않을 때에는 여러 웹 사이트에 대한 암호를 기억해야 한다.
    - 이로 인해 서로 다른 계정에 대해 단순하거나 반복적인 암호를 사용하는 등 권장되지 않는 보안 관행이 발생할 수 있다.
    - 또한, 사용자가 서비스에 로그인할 때 보안 인증 정보를 잊어버리거나 잘못 입력할 수 있다.
- SSO는 암호로 인한 번거로움을 방지하고 사용자가 여러 웹 사이트에서 사용할 수 있는 강력한 비밀번호를 만들 수 있도록 권장한다.

### 생산성 향상

- 직원들은 경우에 따라 별도의 인증이 필요한 둘 이상의 엔터프라이즈 애플리케이션을 사용한다.
- 모든 애플리케이션에 대해 사용자 이름과 암호를 수동으로 입력하는 것은 시간이 많이 걸리고 생산성이 저하되는데, SSO는 엔터프라이즈 애플리케이션의 사용자 검증 프로세스를 간소화하고 보호된 리소스에 더 쉽게 엑세스 할 수 있도록 지원한다.

### 비용 절감

- 기업 사용자는 수많은 암호를 기억하려고 할 때 로그인 보안 인증 정보를 잊어버릴 수 있다.
- 따라서 암호를 검색하거나 재설정하라는 요청이 빈번하게 발생하여 사내 IT 팀의 작업 부하가 증가한다.
- SSO를 구현하면 암호를 잊어버리는 일이 줄어들어 암호 재설정 요청을 처리할 때 지원 리소스를 최소화할 수 있다.

### 비용 절감

- 기업 사용자는 수많은 암호를 기억하려고 할 때 로그인 보안 인증 정보를 잊어버릴 수 있다.
    - 따라서 암호를 검색하거나 재설정하라는 요청이 빈번하게 발생하여 사내 IT 팀의 작업 부하가 증가한다.
- SSO를 구현하면 암호를 잊어버리는 일이 줄어들어 암호 재설정 요청을 처리할 때 지원 리소스를 최소화할 수 있다.

### 보안 태세 개선

- SSO는 사용자 당 비밀번호 수를 최소화하여 사용자 액세스 감사를 용이하게 하고 모든 유형의 데이터에 대한 강력한 액세스 제어를 제공한다.
- 이렇게 하면 암호를 대상으로 하는 보안 이벤트의 위험을 줄이는 동시에 조직이 데이터 보안 규정을 준수할 수 있다.

### 보다 나은 고객 경험 제공

- 클라우드 애플리케이션 제공업체는 SSO를 사용하여 최종 사용자에게 원활한 로그인 경험과 보안 인증 관리를 제공한다.
- 사용자는 더 적은 수의 암호를 관리하고 일상적인 작업을 완료하는 데 필요한 정보와 앱에 안전하게 엑세스할 수 있다.

## SSO는 어떻게 작동하는가?

- SSO는 애플리케이션 또는 서비스와 ID 제공업체(IdP)라고도 하는 외부 서비스 공급자 간의 신뢰를 설정한다.
- 이 문제는 애플리케이션과 중앙 집중식 SSO 서비스 간에 수행되는 일련의 인증, 검증 및 통신 단계를 통해 발생한다.

## SSO 구성 요소

### SSO 서비스

- 사용자가 로그인할 때 애플리케이션이 사용하는 중앙 서비스
- 인증되지 않은 사용자가 애플리케이션에 대한 액세스를 요청하는 경우 애플리케이션은 해당 사용자를 SSO 서비스로 리디렉션한다.
- 그 후 서비스가 사용자를 인증하고 원래 애플리케이션으로 리디렉션 한다.
- 이러한 서비스들은 일반적으로 전용 SSO 정책 서버에서 실행된다.

### SSO 토큰

- SSO 토큰은 사용자 이름 또는 전자 메일 주소와 같은 사용자 식별 정보를 포함하는 디지털 파일이다.
- 사용자가 애플리케이션에 대한 액세스를 요청하면 애플리케이션은 SSO 토큰을 SSO 서비스와 교환하여 사용자를 인증한다.

## SSO 동작 과정

1. 사용자가 애플리케이션에 로그인하면 앱은 SSO 토큰을 생성하고 인증 요청을 SSO 서비스로 보낸다.
2. 이 서비스는 사용자가 이전에 시스템에서 인증되었는지 확인한다.
    - 인증된 경우, 애플리케이션에 인증 확인 응답을 전송하여 사용자에게 액세스 권한을 부여한다.
3. 사용자에게 유효한 보안 인증 정보가 없는 경우 SSO 서비스는 사용자를 중앙 로그인 시스템으로 리디렉션하고, 사용자에게 사용자 이름과 암호를 제출하라는 메세지를 표시한다.
4. 사용자가 정보를 제출하면 이 서비스는 사용자 보안 인증 정보를 확인하고 긍정적인 응답을 애플리케이션으로 보낸다.
    - 그렇지 않으면 오류 메시지가 표시되고 인증 정보를 다시 입력해야 한다.
    - 로그인 시도가 여러번 실패하면 서비스에서 사용자가 일정 기간 동안 더 이상 시도하지 못하도록 차단할 수 있다.

## 참고 자료

- https://aws.amazon.com/ko/what-is/sso/
- https://toma0912.tistory.com/75