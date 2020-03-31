## TCP (Transmission Control Protocol)
- 신뢰성 있는 프로토콜이다. 세그먼트 전송이 실패할 경우 재전송을 요청하고, 연결지향적인 프로토콜이기 때문에 신뢰성을 보장한다. 
하지만 에러 확인 및 복구를 위한 정보 확인을 해야하므로 처리속도가 느리다는 단점이 있다.  
- 인터넷 상에서 데이터를 패킷 형태로 보내기 위해 IP와 함께 사용한다.  
- 연결형 서비스로, 가상 회선 방식(연결 시 3-way Handshaking, 해체 시 4-way Handshaking)을 제공한다.
- 흐름 제어: 송신하는 곳에서 감당이 안 될 정도로 많은 데이터를 빠르게 보내서 수신하는 곳에서 문제가 일어나는 것을 막는다
- 혼잡 제어: 네트워크 내의 패킷 수가 과도하게 증가하는 것을 방지한다

![image](https://user-images.githubusercontent.com/37951612/78031748-ead3d680-739e-11ea-8d14-e3ec47d8e978.png)

## 3 way handshaking (연결 과정)

![image](https://user-images.githubusercontent.com/37951612/78031583-b3fdc080-739e-11ea-9327-5efee3284daf.png)

`SYN, SYN/ACK, ACK`  
Client와 Server 또는 P2P Socket 통신 등 네트워크를 사용한 통신 시 TCP 통신을 많이 사용한다. TCP 통신을 위한 네트워크 연결은 **3 way handshake** 방식으로 연결된다.  
쉽게 얘기하면, 서로의 통신을 위한 port를 확인하고 연결하기 위해 3번의 요청/응답 후 연결되는 것이다.

1. (Server의 열린 포트는 `LISTEN`, Client는 `CLOSED` 상태)
2. Client에서 Server에 연결 요청을 하기 위해 `SYN` 데이터를 보낸다.
3. Server에서 해당 포트는 `LISTEN` 상태에서 `SYN` 데이터를 받고 `SYN_RCV` 상태가 된다. 그리고 요청을 정상적으로 받았다는 `ACK` 데이터 + 포트를 열어달라는 `SYN`데이터를 Client에 보낸다.
4. Client는 `SYN + ACK`를 받고 `ESTABLISHED` 상태가 된다. 그리고 Server에 잘 받았다며 `ACK`를 전송한다.
5. `ACK`를 받은 Server는 `ESTABLISHED` 상태로 변경된다.

### Status
- `CLOSED`: 닫힌 상태
- `LISTEN`: 포트가 열린 상태로 연결 요청 대기 중
- `SYN_RCV`: `SYN` 요청을 받고 상대방의 응답을 기다리는 중
- `ESTABLISHED`: 서로의 포트가 연결된 상태

## 2 way data transmission
1. Client가 Server에 데이터를 보낸다
2. Server가 잘 받았다며 Client에게 `ACK`을 전송한다.

> TCP 프로토콜은 데이터 전송 시 ACK 패킷을 통해 흐름제어(Flow Control)를 한다.  
흐름제어(Flow Control): 송신 측과 수신 측의 데이터 처리속도 차이를 해결하기 위한 방법이다. 수신측에서 처리할 수 있는 데이터의 양보다 송신측에서 보내는 데이터의 양이 많으면 처리할 수 없으므로 수신측이 `ACK` 패킷을 통해 처리할 수 있는 양을 Client 측에 알리고, Client는 Server가 처리할 수 있는 데이터만 전송하게 된다.

## 4 way handshaking (종료 과정)

![image](https://user-images.githubusercontent.com/37951612/78031732-e6a7b900-739e-11ea-879a-eb8552f269b4.png)

TCP **연결**을 위해 **3 way handshake**를 통해 `ESTABLISHED`하고, 종료할 때에는 **4 way handshake** 를 수행한다. 아무래도 새로 연결을 하는 것보다 연결을 종료할 때 예기치 못한 상황이 발생할 가능성이 많기 때문에, 확인 과정이 더 까다로운 것 같다.

1. (서로 통신하는 상태 `ESTABLISHED`)
2. 통신을 종료하고자 하는 Client는 Server에게 `FIN` 패킷을 보내고 자신은 `FIN_WAIT_1` 상태로 대기한다.
3. `FIN` 패킷을 받은 Server는 해당 포트를 `CLOSE_WAIT` 상태로 바꾸고 잘 받았다는 `ACK`를 Client에게 전달한다. 그와 동시에 Server는 해당 포트에 연결되어 있는 Application에게 `Close()`를 요청한다.
4. `ACK`를 받은 Client는 상태를 `FIN_WAIT_2`로 변경한다.
5. `Close()` 요청을 받은 Application은 종료 프로세스를 진행시켜 최종적으로 `Close()`되고, Server는 `FIN` 패킷을 Client에 전송하고 자신은 `LAST_ACK`로 상태를 바꾼다.
6. `FIN_WAIT_2` 상태인 Client는 Server가 종료했다는 신호를 기다리다가 Server로부터 `FIN`을 받으면 받았다는 `ACK`를 Server에 전송하고 자신은 `TIME_WAIT`(일정 시간이 지나면 `CLOSED` 된다)로 상태를 바꾼다.
7. 최종 `ACK`를 받은 서버는 자신의 포트도 `CLOSED`로 닫는다.

### 비정상 종료 상황
TCP 연결 종료 시에는 연결보다 더 복잡하고 예기치 못한 상황들이 많이 일어나는데, 이 때 종료를 적절하게 처리하지 못하면, `FIN_WAIT_1`, `FIN_WAIT_2`, `CLOSE_WAIT` 상태로 남아 연결 종료가 완료되지 않고 계속 기다리는 상황이 발생할 수 있다.

- `CLOSE_WAIT`: Application이 `close()`를 적절히 처리해주지 못하면, TCP 포트는 WAIT 상태로 계속 기다리게 된다. 이런 `CLOSE_WAIT` 상태가 statement에 많아지게 되면, Hang이 걸려서 더 이상 연결을 못 할 수도 있다. 따라서 Application 개발 시 여러 상황에 따라 `close()`를 잘 해주어야 한다.
- `FIN_WAIT_1`: 상대방 측에 커넥션 종료 요청을 했는데, 그 뒤로 `ACK`를 받지 못한 채 기다리고 있는 것이다. 서버를 찾을 수 없을 때 발생하며, 네트워크 및 방화벽 문제일 수 있다(일정 시간이 지나 Timeout이 되면 자동으로 닫는다).
- `FIN_WAIT_2`: 클라이언트가 서버에 종료를 요청한 후 서버에서 요청을 접수했다고 `ACK`를 받았지만, 서버에서 종료를 완료했다는 `FIN`을 받지 못한 채 기다리고 있는 것이다. 양방의 통신이 이루어졌기 때문에 네트워크 문제는 아닌 것으로 판단되며(`FIN`을 보내는 순간 순단이 발생했을 수도 있다), 서버 측에서 CLOSE를 처리하지 못하는 경우일 수 있다(일정 시간이 지나 Timeout 되면 스스로 Closed 된다).
- 어떤 이유에서인지 `FIN_WAIT_1`, `FIN_WAIT_2` 상태인 연결이 많이 남았다면, 문제가 발생할 수 있다. 물론 일정 시간이 지나 Timeout이 되면 연결이 자동으로 종료되긴 하지만, 이 Timeout 되는 시간이 길어서 많은 수의 소켓이 늘어나기만 한다면, 메모리 부족으로 더 이상의 소켓을 오픈하지 못하는 상황이 발생한다.  
  이를 해결하기 위해 Timeout 시간을 적절히 조절할 필요가 있다.

## UDP (User Datagram Protocol)
- 신뢰성 없는 프로토콜이다. 에러가 발생한 세그먼트는 폐기시킨다. 이처럼 에러복구 기능이 없는 프로토콜을 신뢰성 없는 프로토콜이라고 한다.
- 데이터그램(독립적인 관계를 가지는 패킷) 단위로 데이터를 처리한다.
- 비연결형 프로토콜로, 연결을 위한 논리적인 경로가 없다. 각각 다른 경로로 패킷을 전송하여 빠른 속도를 보여준다.
- UDP Header의 `CheckSum` 필드를 통해 최소한의 오류만 검출한다.
- 흐름 제어를 하지 않기 때문에 패킷이 제대로 전송됐는지, 오류가 없는지 확인할 수 없다.
`FIN_WAIT_1`
## TCP vs UDP

### TCP
- 연결형
- 신뢰성 있음
- 1:1 통신
- Byte Stream
- 에러 복구
- 재전송 요구
- 경로 확인
- 트래픽 확인
- 수신 확인
- 준비 확인
- 속도 ↓
  
### UDP
- 비연결형
- 신뢰성 없음
- 1:N 통신
- Datagram
- 에러 복구 X
- 재전송 요구 X
- 경로 확인 X
- 트래픽 확인 X
- 수신 확인 X
- 준비 확인 X
- 속도 ↑
