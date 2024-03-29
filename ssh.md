## SSH (Secure Shell Protocol) 란?
네트워크 프로토콜 중 하나로, **컴퓨터와 컴퓨터가 인터넷과 같은 Public Network를 통해 
서로 통신을 할 때 안전성, 보안성 있게 통신을 하기 위해 사용하는 프로토콜**이다.

### SSH의 기능
1. 데이터 전송  
  ex) 원격 저장소 Github: 소스 코드를 원격 저장소에 푸쉬할 때 SSH을 활용하여 파일을 전송한다
2. 원격 제어  
  ex) AWS 같은 클라우드 서비스: 인스턴스 서버에 접속하여 명령어를 입력한다

### 여타 프로토콜과 SSH의 차이점
**보안** 이다.  
FTP, Telnet 같은 프로토콜도 다른 컴퓨터와 통신하기 위해 사용되지만, 
이 프로토콜로 보안에 민감한 정보(ex. 로그인 정보)를 주고받을 때 직접 네트워크를 통해 정보를 넘기기 때문에 
누구나 해당 정보에 접근할 수 있어서 보안에 취약하다.  
SSH는 일단 보안적으로 안전한 채널을 구성한 뒤 정보를 교환한다.

### Private Key & Public Key
SSH는 다른 컴퓨터와 통신하려고 접속할 때, **한 쌍의 Key**를 통해 인증한다.
- Private Key  
  절대로 외부에 노출되면 안되는 Key로, 사용자 컴퓨터 내부에 저장하게 되어있다.  
  암호화된 메시지를 복호화하는 역할을 한다.
- Public Key  
  공개되어도 비교적 안전한 Key로, 메시지를 전송하기 전 암호화하는 역할을 한다.  
  암호화만 가능하며 복호화는 불가능하다.

### SSH의 인증 과정
1. Public Key를 통신하고자 하는 컴퓨터에 복사하여 저장한다.
2. 요청을 보내는 클라이언트 사이드 컴퓨터에서
접속 요청을 할 때 응답하는 서버 사이드 컴퓨터에 복사한 Public Key와 그와 쌍을 이루는 클라이언트 사이드의 Private Key와 비교하여 서로 한 쌍의 Key인지 검사한다
3. 서로 관계를 맺은 Key라는 것이 증명되면 비로소 두 컴퓨터 사이에 암호화된 채널이 형성되어 Key를 활용해 메시지를 암호화하고 복호화하며 데이터를 주고받을 수 있다.
