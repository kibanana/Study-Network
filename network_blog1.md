# [[link](https://real-dongsoo7.tistory.com/category/Computer%20Network)]

# OSI 7 Layer Model
## OSI 7 Layer Model

> 국제 표준화 기구가 발표했다. 통신이 일어나는 과정을 7단계로 구분하여 한눈에 볼 수 있으며,  
컴퓨터 통신 구조의 모델과 앞으로 개발될 프로토콜의 표준적인 뼈대를 제공하기 위해 개발된 참조 모델이다.

## TCP/IP Model

> 미국에서 개발한 인터넷 기본 통신 트로토콜이다. DOD Model을 기반으로 개발되었다.  
현재 실질적으로 사용되고 있는 모델이다. OSI 7 계층은 장비 개발자들이 어떻게 표준을 잡을지 결정하기 위해 많이 사용한다.

- TCP: 연결지향형 프로토콜. 세션의 연결과 종료, 흐름제어, 패킷의 분할 및 재조립에 사용
- IP: 비연결 지향형 프로토콜. 데이터 전송에 사용

5~7 계층에서 데이터가 생성된다.

1. Physical Layer(물리 계층)

  네트워크 통신을 위한 물리적인 표준을 정의하는 계층이다.
  
  두 컴퓨터 간의 전기적, 기계적, 절차적인 연결을 정의한다.
  
  ex) 케이블의 종류, 송수신 속도, 전기, 전압
  
  `Hub`, `Repeater`

2. Data Link Layer(데이터 링크 계층)

  물리적 계층을 통한 데이터 전송에 신뢰성을 더해주는 계층이다.
  
  Local Network에서 Frame을 안전하게 전송하는 것을 목적으로 한다.
  
  직접 연결되어 있지 않은 네트워크에 대해서는 상위 계층에서 오류 제어를 담당해야 한다.
  
  `Switch`, `Bridge`

3. Network Layer(네트워크 계층)

  Logical Address(IP Address)를 담당하고 Packet의 이동경로를 결정하는 계층이다.
  
  경로 선택(Routing), 논리적인 주소 정의
  
  Routing Protocol을 이용하여 Best Path를 선택한다.
  
  `Router`

4. Transport Layer(전송 계층)

  정보를 분할(`Segment`)하고 상대편에 도달하기 전에 다시 합치는 과정을 담당하는 계층이다.
  
  목적지에서 발신지 간의 통신에서 에러제어와 흐름제어를 담당한다.
  
  `TCP`, `UDP`

5. Session Layer(세션 계층)

  네트워크 상에서 통신할 경우, 양쪽 Host 간에 최초 연결이 이루어지게 하고, 통신 중 연결이 끊어지지 않도록 유지하는 역할을 하는 계층이다.
  
  -> 통신을 하는 Host 사이에서 세션을 열고, 닫고, 관리하는 기능을 담당한다.

6. Presentation Layer(표현 계층)

  전송하는 데이터의 Format(구성 방식)을 결정하는 계층이다.
  
  다양한 데이터 Format을 일관되게 상호변환하고 압축, 암호화, 복호화 기능을 수행한다.

7. Application Layer(응용프로그램 계층)

  사용자 인터페이스의 역할을 담당한다.
  
  `HTTP`, `FTP`, `Telnet`, `SMTP`, `DNS`, `TFTP`


# HTTP (HyterText Transfer Protocol)
네트워크 상에서 정보를 주고받을 수 있는 프로토콜이다. 포트는 `80`번을 사용한다. 현재 `HTTP/2`까지 나와있다.

HTTP는 TCP/IP 위에서 동작한다. HTTP는 클라이언트와 서버 사이에 이루어지는 요청·응답 프로토콜이다. 
웹브라우저가 HTTP를 통하여 서버에 요청하면 서버는 요청에 맞는 응답을 클라이언트에게 전달한다.

## HTTP 동작 방식
1. ISP(Internet Service Provider)를 통해 DNS 서버로부터 IP를 취득한다.
2. **3-way-handshake**를 통해 서버와 클라이언트 간에 세션을 생성한다. 일종의 초기화 과정을 거친다.
3. 통신을 마칠 때가 되면 **4-way-handshake**를 통해 세션을 종료한다.

![image](https://user-images.githubusercontent.com/37951612/80857968-2957fc00-8c91-11ea-8666-b8c22b82a4a3.png)

## 3-way-handshake

TCP 장치들 사이에서 **논리적인 연결을 정립**하는 과정이다.

TCP/IP 프로토콜을 이용하여 통신할 때, 응용프로그램과의 통신이 이루어지기 전에 정확한 전송을 보장하기 위해 
사전에 **세션을 시작**하는 과정이다.

![image](https://user-images.githubusercontent.com/37951612/80858037-979cbe80-8c91-11ea-918b-10b15fe5a9c4.png)

1. TCP Client는 TCP Server에 `SYN(n)` 패킷을 전송해서 접속을 요청한다.
2. TCP Server는 `SYN` 메시지를 받고 `SYN(n+1)` 메시지와 함께 메시지를 확인했다는 `ACK(m)` 패킷을 전송한다.
3. TCP Client는 `SYN`과 `ACK`를 수락하고 다시 `ACK(m+1)`(응답)를 전송하면 두 노드 간에 연결이 성립(Establish Connection)된다.

- SYN(Synchronize sequence number)

  연결을 요청하고 세션을 성립하는 데에 이용한다.

- ACK(Acknowledgement)

  받은 시퀀스 번호에 TCP 계층의 길이 또는 양을 더한 것과 같은 값을 담는다.

## 4-way-handshake

TCP 간의 **세션을 종료**하기 위한 과정이다.

![image](https://user-images.githubusercontent.com/37951612/80858024-88b60c00-8c91-11ea-87a1-3362bf97ac7c.png)

1. Client가 종료를 알리는 `FIN FLAG`를 Server에 전송한다.
2. Server는 `FIN` 패킷을 정상적으로 받았다는 `ACK`를 Client에 전송한다. 그 후 Server는 `CLOSE-WAIT` 상태가 된다.
3. 연결을 종료한 후 Server는 Client에게 `FIN FLAG`를 전송한다.
4. Client는 Server가 전송한 `FIN FLAG`를 받고 확인을 알리는 `ACK`를 Server에 전송한다. 그 후 Client는 일정시간동안 `TIME-WAIT` 상태가 된다.
5. Server는 Client로부터 `ACK`를 받고 소켓을 Close한다. 두 TCP 간의 세션이 종료된다.
6. Client는 `TIME-WAIT`에 빠져서 Server로부터 `FIN`을 수신해도, 일정시간동안 세션을 유지하며 도착하지 않은 패킷을 기다린다.

- FIN(Finish)
  
  세션의 종료를 알리며, 더이상 보낸 데이터가 없음을 표시한다.













