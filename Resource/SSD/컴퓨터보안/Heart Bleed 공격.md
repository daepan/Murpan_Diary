## Heartbleed 공격
- OpenSSl은 전송 계층 보안 및 보안 소켓 계층 프로토콜을 위한 강력하고 상용급의 기능을 갖춘 툴킷
- 또한 일반적인 암호화 라이브러리 역할도 한다.
- OpenSSL의 하트비트 기능은 TLS/DTLS 프로토콜에서 클라이언트와 서버 간의 연결 상태를 확인하기 위한 추가 확장 기능
- TLS 하트비트 확장은 기본적으로 연결을 유지되고 있는지를 학인하거나 연결을 활성 상태로 유지하기 위해 주기적으로 신호를 주고 받음
- Heartbleed는 OpenSSL 암호 라이브러리의 보안 버그
- 이 버그는 TLS 하트 비트 확장 구현에서 입력 검증이 제대로 이루어지지 않아 발생


## Heartbleed 취약점과 Heartbeat의 관계
- Heartbleed 취약점은 openssl heartbeat 기능의 입력 검증 오류에서 비롯됨
- 하트비트 요청을 받을 때 요청된 길이를 검증하지 않아 사용자가 잘못된 길이 값을 지정하여 서버 메모리의 민감한 정보를 읽어올 수 있게 함
- 클라이언트 1바이트의 데이터를 보내면서 길이를 64kb를 요청하면 서버는 추가적인 데이터를 반환하며, 민감한 정보가 노출될 수 있다.

## Heartbleed 공격 예방 방법

### Response
- Heartbleed 취약점이 공개된 후 OpenSSL에서는 하트비트 기능에서 입력 값 검증을 강화하여 메모리 범위를 초과하는 요청을 차단

![](https://i.imgur.com/bJcWxDB.png)



```mermaid
flowchart TD
    A[main() start] --> B[Option parsing and setup check]
    B --> C{Validate arguments}
    C -- No arguments --> D[Print help and exit]
    C -- Valid arguments --> E[Set server and port]
    E --> F[for loop: call check()]

    F --> G[check() start]
    G --> H{STARTTLS option enabled?}
    H -- Enabled --> I[Check STARTTLS with SMTP]
    I --> J{Is STARTTLS supported?}
    J -- Not supported --> K[Print error and exit]
    J -- Supported --> L[Confirm STARTTLS and close SMTP]
    H -- Not enabled --> L
    L --> M[connect() to server]
    M --> N[Set socket timeout]
    N --> O[Call tls() to start TLS handshake]

    O --> P[parseresp() to parse server response]
    P --> Q{Check server version}
    Q -- Error --> R[Print error and exit]
    Q -- Version confirmed --> S[Print server TLS version]

    S --> T{Send heartbeat request by version}
    T -- Version 1 --> U[Send hbv10]
    T -- Version 2 --> V[Send hbv11]
    T -- Version 3 --> W[Send hbv12]
    U --> X[Call hit_hb() to check response]
    V --> X
    W --> X

    X --> Y{Analyze response}
    Y -- Excessive data returned --> Z[Print vulnerability found]
    Y -- Normal data returned --> AA[Print not vulnerable]
    Y -- Warning message received --> AB[Print warning]
    Z --> AC[Close socket and return result]
    AA --> AC
    AB --> AC
    AC --> AD[End program after loop]
```