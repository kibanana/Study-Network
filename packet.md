## 패킷(packet)이란?
데이터의 전송 단위로, Network Layer에 속한다. `Header + Data + Tail`로 구성되어 있다. Tail 부분은 주로 에러 검출에 사용되며, 종종 생략되기도 한다.  

### 오류제어방식 - 트레일러(Tail)
패킷의 꼬리에 해당하는 트레일러, 또는 Tail은 주로 오류검출코드에 의해 사용되는 영역이다. 그 중 대표적인 것이 `CRC(Cyclic Redundancy Check)`다.  

오류제어방식에는 크게 두 가지가 있다.
- `FEC(Forward Error Correction)`: 순방향 오류 제어. 송신 측에서 오류 확인을 위한 추가적인 정보를 패킷에 붙여 전송하는 방식
- `ARQ(Automatic Repeat reQuest)`: 후방 오류 정정. 수신 측에서 오류가 발생한 것 같다고 판단하면 송신 측에 재전송을 요청하여 확인하는 방식

그 중 `FEC`는 또 두 가지로 나눌 수 있다.
- `FEC`: 전진 에러 수정. 송신자가 제공한 정보를 사용하여 오류를 직접 수정하는 방식
- `BEC`: 후진 에러 수정. 송신자가 제공한 정보로는 오류 검출만 가능하고, 송신자에게 재전송을 요청하는 방식

`FEC`와 `ARQ`는 기능적으로 큰 차이가 있다. `FEC`는 수신과 동시에 에러를 확인하고, 
`ARQ`는 에러가 발생하면 그때 에러를 처리한다. 따라서 에러가 빈번한 경우 `FEC` 방식으로 에러를 확인하고 상위 프로토콜로 넘기는 것이 좋지만, 
안정적인 네트워크라면 에러가 적을 텐데 굳이 매번 에러를 검출하여 자원을 낭비할 필요가 없을 것이다.

`FEC`, `BEC`를 보면, `FEC`는 굳이 재전송을 안해도 되지만, 수신 측에서 에러를 수정하기 위해 송신 측에서 더 많은 정보를 전송해야 한다. 
이 또한 네트워크에서 어떤 서비스를 하느냐에 따라 다르게 선택할 수 있다.

### 네트워크 패킷(packet) 생성 과정

1. Application Layer에서 생성한 데이터를 Transport Layer로 넘긴다
2. Transport Layer에서 port 정보를 헤더에 붙여서 Network Layer에 **세그먼트(data + port)** 를 준다.
3. Network Layer는 ip 정보를 헤더에 붙여서 Data Link Layer에 **데이터그램(segment + ip)** 을 준다.
4. Data Link Layer는 MAC Address를 헤더에 붙여서 **프레임(frame, 데이터그램 + Mac Address)** 을 생성하고, Physical Layer를 통해 목적지로 전송한다.

### 패킷(packet) 분석
- 프레임(Frame)  
  Destination MAC Address(6byte) - Source MAC Address(6byte) - type(0800=IP, 0806=ARP, 2byte) - data(데이터그램) - FCS 뒤에서 4바이트
  
- 데이터그램
  - IP 헤더
  - ARP 헤더
  - ICMP 패킷 구조(`ping`)
