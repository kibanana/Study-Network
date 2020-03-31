## Mac 헤더

**MAC 주소(Media Access Control Address**는 데이터 링크 계층에서의 네트워크 세그먼트 통신을 위한 네트워크 인터페이스에 할당된 **고유 식별자**다.

![image](https://user-images.githubusercontent.com/37951612/77874699-c2f24f00-7288-11ea-8fda-2180061a170c.png)

- **수신처 MAC 주소** (48bit)
  
  해당 패킷을 수신하는 측 MAC주소. LAN의 패킷 송신은 이 주소를 바탕으로 이루어진다.
  
- **송신처 MAC 주소** (48bit)

  해당 패킷을 송신하는 측 MAC주소. 패킷을 수신하였을 때 이 값을 가지고 어디서 송신했는지 판별한다.

- **Ether Type** (16bit)

  사용하는 프로토콜의 종류. TCP/IP에서 실질적으로 사용되는 값은 `0080`(IP 프로토콜), `0806`(ARP 프로토콜)이다.
 
> **MAC 헤더의 끝부분은 IP 헤더의 시작 부분과 인접해있다.** 이 계층에서는 **IP 헤더부터의 데이터를 MAC 헤더의 데이터로 취급**한다. 
전송하고자 하는 호스트에서 OSI 7계층의 7계층 => 1계층 쪽으로 내려가면서 각 계층의 헤더가 붙으며, 
**목적지 호스트로 패킷이 전송되는 과정에서 라우터를 거칠 때마다 2(데이터 링크) 계층의 헤더가 갱신되면서 목적 호스트에 패킷이 전송된다.**

> **MAC Address**  
`6byte = 48bit`. **상위 3byte는 제조회사 고유 번호**를, **하위 3byte는 제품(하드웨어)의 고유 번호**를 가져서 6바이트를 구성한다. 
하드웨어마다 다른 MAC Address를 가지고 있기 때문에 논리적인 주소인 **IP주소로 전송할 호스트를 찾고, 물리적인 주소인 MAC Address로 실제 패킷을 송수신**한다.

## ARP 헤더

**ARP(Address Resolution Protocol)** 는 네트워크 상에서 **IP주소를** 물리적 네트워크 주소인 **MAC주소로 대응(bind)시키기 위해 사용**되는 프로토콜이다.

![image](https://user-images.githubusercontent.com/37951612/77874859-490e9580-7289-11ea-98a4-70e17b3a3500.png)

- 하드웨어 주소 타입 (2byte)
- 프로토콜 타입 (2byte)
- H/W 주소 길이 (1byte)
- 프로토콜 주소 길이 (1byte)
- Operation (OpCode, 2byte)

  패킷의 유형이 요청(ARP Request)인지, 응답(ARP Reply)인지 정의한다. 이 때 요청일 경우에는 1, 응답일 경우에는 2가 들어간다.

- 출발지 MAC 주소 (6byte)
- 출발지 IP 주소 (4byte)
- 목적지 MAC 주소 (6byte)
- 목적지 IP 주소 (4byte)

## IP 헤더

![image](https://user-images.githubusercontent.com/37951612/77874928-7fe4ab80-7289-11ea-9bda-118e8dc9c974.png)

- Version (4bit)
  
  IP 프로토콜의 버전 정보로, 보통 IPv4 주소체계를 사용하므로 보통 값이 4다.
  
- IP Header Length, IHL (4bit)

  프로토콜의 마지막 메타 데이터인 옵션의 유무로 헤더 길이가 달라지므로 정확한 헤더 길이를 알기 위한 정보다. 
  4bit로 표현할 수 있는 최대값은 15라서 실제 IP 헤더의 최소 길이인 20을 커버하지 못한다. 그래서 이 값에 4(2^2)를 곱해서 정확한 길이를 구하며, 보통 0101, 즉 5를 값으로 가진다.

- Type of Service, ToS (8bit)

  패킷을 운반할 때 우선 순위를 나타낸다.

- Total Length (16bit)

  IP 메시지의 전체 길이 정보를 가진다.
  
- Identification (16bit)

  각 패킷을 식별하는 번호. 보통 패킷의 참조 번호를 나타낸다. IP 클라이언트에 따라 분할된 패킷 값이 모두 동일하다.

- Flags (3bit)

  실제 사용되는 건 2bit다. 이 중 한 개의 비트로 Fragmentation이 가능한지, 나머지 한 개의 비트로 해당 패킷이 조각으로 나누어진 것인지 확인한다.

- Fragment Offset (13bit)

  해당 패킷이 IP 메시지의 맨 앞 부분부터 몇 번째 바이트에 위치했는지 기록되어 있다.
  
- TTL, Time to Live (8bit)

  해당 패킷이 전송 목적지로 가지 못하고 네트워크에서 루프를 돌며 순환되지 않도록, 패킷의 생존 기간을 지정한 데이터다. 라우터를 경유할 때마다 이 값이 1씩 감소하고 0이 되면 소멸한다.

- Protocol (8bit)

  프로토콜 번호 ex) `TCP`: `0x06`, `UDP`: `0x11`, `ICMP`: `0x01`

- Header Checksum (16bit)

  오류 검사용 데이터지만 현재는 사용되지 않는다.

- Source Address (32bit)

  송신지 IP 정보

- Destination Address (32bit)

  수신지의 IP 정보

- Options and Padding (multiples of 32bit)

  위 헤더 정보 이외의 제어 정보를 기록할 때 옵션 필드를 추가할 수 있는 영역이다. 하지만 거의 안 사용한다.

## TCP 헤더

- Source Port (16bit)

  포트 번호는 멀티프로세싱 환경에서 해당 패킷이 어떤 어플리케이션에서 사용되는지 구분하기 위해 사용된다.  
  ex) IE같은 웹 브라우저: 주로 `80(HTTP)`번 사용

- Destination Port (16bit)

- Sequence Number (32bit)
  
  송신 데이터의 일련 번호. 해당 패킷의 맨 앞 데이터가 송신 데이터의 몇 번째 바이트에 위치하는지 수신 측에 전달하기 위한 데이터다. ACK 번호와 함께 데이터가 제대로 전달되었는지 확인하는 용도로 쓰인다.

- ACK Number, Acknowledgment Number (32bit)

  수신 데이터의 일련 번호. 송신측에서 보낸 데이터가 몇 바이트까지 수신측에 도착했는지 송신 측에 전달하기 위한 데이터다. 송신 측의 데이터가 수신 측에 제대로 전달되었는지 확인하여 송신 측에서 재전송 여부를 판별한다.

- Offset (4bit)

  데이터가 어느 부분부터 시작하는지 나타낸다. 데이터의 시작이 헤더의 끝이 되므로 헤더의 길이로 해석할 수도 있다. 4byte 단위이므로 값에 4를 곱해야하며, 보통 `5(0101)`가 값이다.

- Reserved (4bit)
  
  예약. 차후의 사용을 위해 예약된 필드다.

- Flags (8bit)

  8개의 서로 다른 제어 비트 또는 플래그를 나타낸다. 동시에 여러 개의 비트가 1로 설정될 수 있다.

  - CWR(Congestion Window Reduced): 혼잡 윈도우 크기 감소

  - ECN(Explicit Congestion Notification): 혼잡을 알림

  - URG(Urgent) : Urgent Pointer 필드가 가리키는 세그먼트 번호까지 긴급 데이터를 포함되어 있다는 것을 뜻한다. 이 플래그가 설정되지 않았다면 Uregent Pointer 필드는 무시되어야 한다.

  - `ACK`(Acknowledgment) : 확인 응답 메시지

  - PSH(Push) : 데이터를 포함한다는 것을 뜻한다.

  - RST(Reset) : 수신 거부를 하고자 할때 사용

  - `SYN`(Synchronize) : 가상 회선이 처음 개설될 때 두 시스템의 TCP 소프트웨어는 의미 있는 확인 메시지를 전송하기 위해 일련 번호를 서로 동기화해야 한다.

  - `FIN`(Finish) : 작업이 끝나고 가상 회선을 종결하고자 할 때 사용

- Window size (16bit)

  송신 시스템의 가용 수신 버퍼 크기(수신 확인을 기다리지 않고 묶어서 송신할 수 있는 단위)를 byte 단위로 나타낸다.

- Checksum (16bit)

  TCP 세그먼트 내용이 유효한지 검증, 손상 여부 검사할 수 있다.

- Urgent Pointer (16bit)
- Option (0~40 byte)

![image](https://user-images.githubusercontent.com/37951612/77874959-9c80e380-7289-11ea-8504-5c3824a680ed.png)

↑   TCP 헤더 중 Option 필드 제외하면 20 byte

![image](https://user-images.githubusercontent.com/37951612/77874966-a30f5b00-7289-11ea-846a-803b14d0e1d5.png)

## UDP 헤더

- Source Port

  포트 번호는 멀티프로세싱 환경에서 해당 패킷이 어떤 어플리케이션에서 사용되는지 구분하기 위해 사용된다.  
  ex) IE같은 웹 브라우저: 주로 `80(HTTP)`번 사용

- Destination Port

- Total Length

  UDP 헤더 이후의 데이터 길이 정보

- Checksum

  오류 유뮤를 검사하기 위해 존재


![image](https://user-images.githubusercontent.com/37951612/77875050-e9fd5080-7289-11ea-97f7-fa0a6ed6e855.png)

