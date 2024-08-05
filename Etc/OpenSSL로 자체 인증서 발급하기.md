## 개인 SSL/TLS 인증서 발급받기

1. private key 생성 (root key of private key)
    
    ```bash
    openssl genrsa -des3 -out {keyName}.key 2048
    ```
    
    - genrsa : RSA 개인 키 생성
    - -des3 : Triple DES(3DES) 암호화를 사용해 개인키 암호화
    - -out {keyName}.key : 개인 키는 해당 파일명으로 저장된다.
    - 2048 : 2048 bits 키 크기가 일반적으로 개인 키 생성에 사용된다.
2. 개인 키로 root 인증서 생성 (root CA)
    
    ```bash
    openssl req -x509 -new -nodes -key {keyName}.key -sha256 -days 1825 -out {pemName}.pem
    ```
    
    - req : openssl에 인증서 요청을 처리하도록 지시한다.
    - -x509 : 자체 서명된 X.509 인증서 생성
    - -new : 새로운 인증서 요청 생성
    - -nodes : 인증서의 개인키를 암호화하지 않음
    - -key {keyName}.key : 인증서 서명에 사용할 개인 키
    - -sha256 : 인증서 서명을 위해 SHA-256 메시지 알고리즘을 사용한다.
    - -days 1825 : 인증서가 유효한 일수 (약 5년)
    - -out {pemName}.pem : 인증서 출력 파일 이름
3. private CA를 사용해 서버 인증서 발급
    
    ```bash
    openssl genrsa - out {keyName}.key 2048
    ```
    
4. 해당 개인키를 사용해 인증서 서명 요청(CSR) 수행
    
    ```bash
    openssl req -new -key {keyName}.key -out {csrName}.csr
    ```
    
5. Subject Alternative Name (SAN) 생성
    - SAN 인증서를 사용하면 기본 도메인을 보호하고, 대체 도메인을 추가할 수 있다.
    - **단일 인증서를 여러 DNS 이름과 함께 사용할 수 있다.**
    
    ```bash
    # {SANName}.ext
    authorityKeyIdentifier=keyid,issuer
    basicConstraints=CA:FALSE
    keyUsage = digitalSignature, nonRepudiation, keyEncipherment, dataEncipherment
    subjectAltName = @alt_names
    [alt_names]
    DNS.1 = {domainName}
    ```
    
    ```bash
    openssl x509 -req -in {csrName}.csr -CA {pemName}.pem -CAkey {keyName}.key -CAcreateserial -out {crtName}.crt -days 825 -sha256 -extfile {SANName}.ext
    ```
    
6. 인증서 잘 발급됐는지 확인
    
    ```bash
    openssl x509 -text -noout -in {certName}.crt
    ```
    
    ![image](https://github.com/user-attachments/assets/a437cb44-cba6-4859-bbaf-4718354d0933)

    

## Nginx에 연동하기

## Apache에 연동하기

![image](https://github.com/user-attachments/assets/fcb41c81-81e8-4354-81ef-ad5ae8984e8e)


1. httpd, mod_ssl 설치
    
    ```bash
    yum install httpd mod_ssl
    ```
    
2. ssl 인증서 /etc/httpd/ssl 이동
    
    ```bash
    mkdir /etc/httpd/ssl
    cp {certName}.crt /etc/httpd/ssl/
    cp {certName}.key /etc/httpd/ssl/
    cp {certName}.pem /etc/httpd/ssl/
    ```
    
3. ssl.conf 파일 수정
    
    ```bash
    cd /etc/httpd/conf.d/ssl.conf
    
    <VirtualHost *:443>
        ServerName {domainName}
    
        SSLEngine on
        # 아래 파일 경로를 변경
        SSLCertificateFile /etc/httpd/ssl/{crtName}.crt
        SSLCertificateKeyFile /etc/httpd/ssl/{keyName}.key
        SSLCertificateChainFile /etc/httpd/ssl/{pemName}.pem
    
    // ...
    </VirtualHost>
    ```
    
4. Apache 설정 테스트
    
    ```bash
    apachectl configtest
    
    Syntax OK 뜨면 성공
    ```
    
5. Apache 재시작
    
    ```bash
    systemctl restart httpd
    ```
    
6. https 경로로 접속
    - 접속 전 hosts 파일을 수정하면 도메인을 발급받은 것처럼 접속 가능
        
        ![image](https://github.com/user-attachments/assets/4ce605af-16d6-4fba-93e5-9d53de4e02eb)

        

## 참고 자료

- https://amod-kadam.medium.com/how-to-set-up-private-ca-and-use-the-certificates-issued-by-private-ca-da55941c51ee
- https://www.lesstif.com/system-admin/apache-httpd-ssl-https-virtualhost-sni-server-name-indication-19365977.html
