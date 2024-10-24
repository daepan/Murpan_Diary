# Iptables
## 개념
리눅스에서 네트워크 트래픽을 관리하는 강력한 도구
방화벽 설정을 통해 패킷을 필터링하고 라우팅 규칙을 제어

Ubuntu에서도 iptables는 기본 방화벽 도구로 사용됨
패킷의 흐름을 제어하기 위해 다양한 규칙을 설정

네트워크 보안을 강화하고, 서버나 클라이언트의 접근을 제어

## 기본 체인
iptables는 체인을 사용하여 패킷을 처리
체인이란 규칙의 모임을 말한다
규칙들은 특정 조건에 맞는 패킷에 대해 수행될 작업을 결정

INPUT  
• 들어오는 패킷을 처리하는 체인  
• 시스템으로 들어오는 트래픽에 대한 규칙을 설정

OUTPUT  
• 나가는 패킷을 처리하는 체인  
• 시스템에서 나가는 트래픽에 대한 규칙을 설정

FORWARD  
• 시스템을 통과해 다른 네트워크로 전달되는 패킷을 처리
• 시스템이 라우터나 게이트웨이처럼 동작할 때, 들어오는 패킷을 다른 네트워크 인터페이스로 전달하는 트래픽을 필터링하는 체인
- FORWARD 체인은 시스템이 트래픽을 라우팅할 때, 즉 다른 네트워크로 패킷을 전달하는 경우 이를 제어하는 체인  
• 라우터, 게이트웨이 또는 NAT를 설정할 때 주로 사용되며, 두 네트워크 간의 트래픽을 허용하거나 차단할 수 있음

ex) 시스템이 라우터로 동작 할때 네트워크 트래픽 전달 허용
만약 시스템이 두 개의 네트워크 인터페이스를 가지고 있고, 이를 통해 네트워크 트래픽을 전달해야 한다면, FORWARD 체인에서 이를 허용하는 규칙을 추가해야한다.

> sudo iptables -A FORWARD -i eth0 -o eth1 -j ACCEPT

- -i eth0: 트래픽이 인터페이스에서 들어옴
- -o eth1: 트래픽이 인터페이스로 나감
- -j ACCEPT: 트래픽을 허용

PREROUTING  
• 패킷이 목적지로 라우팅되기 전에 규칙을 적용

POSTROUTING  
• 패킷이 라우팅된 후 규칙을 적용


## 명령어 형식

sudo iptables [체인] [옵션] [조건] –j [액션]

체인: INPUT, OUTPUT, FORWARD 중 하나  
옵션: 규칙을 추가, 삭제, 보기 등의 옵션 (-A, -D, -L 등)  
-A (append)  
특정 체인에 규칙을 추가
> sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT

SSH(포트 22)에 대해 들어오는 TCP 연결을 허용

-D (delete)  
특정 규칙을 삭제
>sudo iptables -D INPUT -p tcp --dport 22 -j ACCEPT

-L (list)  
현재 설정된 규칙 목록을 확인
>sudo iptables -L

-P (policy)  
체인의 기본 정책을 설정
>sudo iptables -P INPUT DROP

이 명령은 들어오는 모든 트래픽을 기본적으로 차단(DROP)하는 규칙을 설정


조건: 필터링할 패킷의 조건 (예: IP 주소, 포트 번호)  
액션: 패킷에 대해 수행할 작업 (ACCEPT, DROP, REJECT 등)

## iptables 저장 및 복원
- Ubuntu에서 iptables 규칙은 재부팅 시 사라지기에 저장 및 복원 작업이 필요하다.
- iptables 규칙 저장
>sudo apt install iptables-persistent sudo netfilter-persistent save

-  iptables 규칙 복원  
> sudo netfilter-persistent reload

- iptables 규칙 초기화
> sudo iptables -F

