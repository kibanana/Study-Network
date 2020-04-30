[[link](https://swalloow.tistory.com/category/Knowledge/Network)]

## 네트워크(Network)

'네트워크 = 인터넷'을 떠올리게 되는데, 네트워크는 **컴퓨터와 컴퓨터 사이의 통신망**이라고 정의할 수 있다.

최근에는 스마트폰, 태플릿 등 다양한 단말기가 네트워크 내에서 함께 연결될 수 있다.

### 네트워크의 역사

네트워크는 그동안 3가지 중점목표를 두고 발전해왔다.

1. 정보를 더 **명확하게 전달**하는 것
2. 투자한 비용에 비해 **정보의 효율성**을 증가시키는 것
3. 정보가 제 3자에게 유출되지 않도록 **보안**을 유지하는 것

유선통신 -> 전화기 개발 -> 전자기파 발견(무선통신 시작) -> 전자기파가 라디오파로 발전(음성 뉴스, 무선 방송) -> 
전화기선을 이용한 컴퓨터 네트워크 기술 -> 웹이 도입되면서 통신에 모뎀 사용 -> 
(현재) 광대역 통신 선로를 이용하여 음성, 화상, 데이터 등 모든 미디어를 통신



## 프로토콜(Protocol, 통신 규약)

프로토콜(= Protocol = 통신 규약)은 컴퓨터 같은 **통신 장비 사이에서 메세지를 주고 받는 양식과 규칙 체계**를 말한다.

이를 이용하여 서버와 클라이언트가 데이터를 주고 받는다.



## 네트워크 전달방식 비교(Packet Switching, Circuit Switching)

이 때 네트워크 전달방식은 '네트워크 상에서 호스트와 호스트 간에 데이터를 주고 받는 방식'을 말한다.

- 회선 교환 방식 (Circuit Switching)

  하나의 회선을 할당받아서 데이터를 주고 받는 방식이다. 
  
  통신을 위한 연결을 우선적으로 해야 하며, 연결이 이루어지고 나면 출발지(Source)로부터 목적지(Destination)에 데이터가 도착하는데, 
  사용되는 회선 전체를 독점(Dedicated)하기 때문에 다른 사람이 끼어들 수 없다. 따라서 속도와 성능이 일정하고, 
  전화와 같은 실시간 통신에 사용된다.
  
  - FDM(Frequency Division Multitasking)
  
    할당된 대역폭을 나누어 사용하는 방식
  
  - TDM(Time Division Multitasking)

    할당된 대역폭을 시간 단위로 나누어서 다수의 사람이 번갈아가며 사용하는 방식


- 패킷 교환 방식 (Packet Switching)

  데이터를 패킷(Packet) 단위로 쪼개서 전송하는 방식이다. 패킷은 다음 노드로 전송하기 전에 저장을 하고 전달하는 
  `store and forward` 방식을 따른다.
  
  패킷의 헤더에는 출발지(Source)와 목적지(Destination) 정보가 있다.
  
  라우팅 알고리즘을 이용하여 경로를 설정하고 중간의 라우터들을 거쳐서 최종 목적지에 도달한다.
  
  패킷이 다음 라우터롤 이동할 때, 패킷은 큐(Queue)에서 대기(Queueing)하는데, 
  이 때 큐(Queue)가 수용할 수 있는 범위를 초과하면 손실(loss)이 발생한다.


둘 중, 패킷 교환 방식이 더 많은 사용자를 수용할 수 있는 방식이다.

- Circuit Switching

  - No store and forward
  - dedicated
  - Bandwidth wastage
  - Reserved
  - Less Delay
  - Telephone line
  - Highly Reliable
  - Call setup

- Packet Switching

  - Store and forward
  - shared
  - No bandwidth wastage
  - Not reserved
  - Higher Delay
  - Internet line
  - Less Reliable
  - No call setup


## OSI 7 계층 모델(Layer Model) & TCP/IP 프로토콜

![image](https://user-images.githubusercontent.com/37951612/80752782-1517e000-8b67-11ea-9642-7d62b2e4f1a9.png)

**네트워크 통신 과정을 7 계층으로 구분한 산업 표준 참조 모델**이다.

> 계층 구조(Layered)를 사용하는 목적은 분할 정복(Divide and Conquer)이다. ex) 네트워크, 운영체제  
어떤 복잡한 문제를 해결할 때, 나눠서 생각하면 쉽게 해결할 수 있다는 거다. 위, 아래로만 이동할 수 있는 게 특징이다.

1. 물리 계층 (Physical Layer)

  상위 계층에서 전송한 데이터를 물리 매체(ex. 허브, 라우터, 케이블 등)를 통해 다른 시스템에 전기적 신호로 전송하는 역할을 한다.
  
  즉, 기계어를 전기적 신호로 바꿔서 와이어에 실어주는 것이다.

2. 데이터 링크 계층 (Data Link Layer)

  네트워크 기기들 사이에서 데이터를 전송하는 역할을 한다. 시스템 간 오류 없는 데이터 전송을 위해 
  패킷을 프레임으로 구성하여 물리계층으로 전송한다.

3. 네트워크 계층 (Network Layer)

  데이터그램(Datagram)이 이동하는 경로를 설정하는 역할을 한다.
  
  라우팅 알고리즘으로 최적의 경로를 선택하고 송신 측에서 수신 측으로 전송한다.
  
  이 때, 전송되는 데이터는 패킷 단위로 분할한 후 전송해서 다시 합친다.

4. 전송 계층 (Transport Layer)

  프로세스 사이의 데이터 이동을 책임진다. 데이터가 오류 없이 도착하게 하기 위해 에러를 체크, 데이터의 순서를 맞추는 작업이 진행된다.

  ex) `TCP`, `UDP` 프로토콜

5. 세션 계층 (Session Layer)

  프로그램 계층 간의 제어 구조를 제공하기 위한 작업을 수행한다. ex) 설정 유지, 동기화, 포인트 체크

6. 표현 계층 (Presentation Layer)

  송신과 수신측 사이에서 데이터의 형식(ex. `png`, `jpg`, `jpeg`)을 정한다. 송신할 데이터를 전달, 암호화, 압축 과정을 통해 올바른 표준 형식으로 변환한다.

7. 응용 프로그램 계층 (Application Layer)

  사용자와 바로 연결되어 있는 계층으로, 응용 S/W를 도와준다.
  
  사용자로부터 정보를 입력받아 하위 계층으로 전달하고, 하위 계층에서 전송한 데이터를 사용자에게 전달한다.
  
  파일 전송, DB, 메일 전송 등 여러가지 응용 프로그램을 네트워크에 연결하는 역할이다.
  
  
### TCP/IP 4계층 모델 (4 Layer Model)

![image](https://user-images.githubusercontent.com/37951612/80757804-b86cf300-8b6f-11ea-9943-c47e5a335bf6.png)

실제 사용되는 프로토콜은 OSI 참조 모델을 완전히 따르지 않는다. TCP/IP 4계층은 인터넷 프로토콜 스택(Internet Protocol Stack)이라고도 하는데, 
이는 대부분 TCP/IP를 따른다.

세션 계층, 표현 계층이 응용프로그램 계층에 포함되어 있다. TCP/IP 4계층에서는 각 계층마다 헤더가 붙어서 전송하데 되며, 
이를 **데이터 캡슐화**라고 한다.


Data(데이터) -> Segment(세그먼트) -> Datagram(데이터그램) -> Frame(프레임)

![image](https://user-images.githubusercontent.com/37951612/80757834-c589e200-8b6f-11ea-9707-4a76e320357f.png)




## 네트워크 전송기기

- 리피터(Repeater)

  전기적 신호를 받아서 이진수로 바꾸고, 다시 전기적 신호로 전해주는 기기다. 신호를 증폭하기 위해 사용한다.

- 라우터(Router)

  두 개의 네트워크 사이에서 데이터를 주고받을 수 있도록 도와주는 기기다. 보안 기능을 제공한다.

- 허브(Hub)

  이더넷 케이블을 통해 연결해서 네트워크 내의 컴퓨터 간에 통신을 도와주는 기기다. 스위치보다 느리다.

- 스위치(Switch)

  허브와 동일하게 작동하지만 대상을 식별할 수 있고, 정보를 동시에 주고 받을 수 있다.

- 브릿지(Bridge)

  여러 네트워크를 연결할 수 있도록 스위치를 업그레이드 시킨 기기다. ex) 에그(4G <--> 3G <--> Wi-fi)

- 게이트웨이(Gateway)

  네트워크 간의 통로 역할을 하는 장치다. 



## TCP vs UDP

![image](https://user-images.githubusercontent.com/37951612/80752475-830fd780-8b66-11ea-934c-f179de338ae9.png)

