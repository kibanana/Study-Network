## Load Balancing이란?
부하 분산을 위해 가상 IP를 통해 클러스터링 된 서버에 트래픽을 분배하는 기능

### Load Balancing의 종류
- L2
    - **Mac주소**를 바탕으로 Load Balancing 진행
- L3
    - **IP주소**를 바탕으로 Load Balancing 진행
- L4
    - **Transport Layer**(IP와 Port) Level에서 Load Balancing 진행
    - **TCP, UDP**
     ![load_balancing1](https://user-images.githubusercontent.com/37951612/77849517-2340ac80-7207-11ea-9ef3-490e973747f2.png)
- L7
    - **Application Layer**(사용자의 Request) Level에서 Load Balancing 진행
    - **HTTP, HTTPS, FTP**
    ![load_balancing2](https://user-images.githubusercontent.com/37951612/77849519-250a7000-7207-11ea-96b4-d96781c54b31.png)

### HTTP
- X-Forwarded-For : HTTP or HTTPS 로드 밸런서를 사용할 때 **클라이언트의 IP 주소를 식별**하는 데 도움을 준다.
- X-Forwarded-Proto : 클라이언트가 로드 밸런서 연결에 사용한 **프로토콜(HTTP or HTTPS)을 식별**하는 데 도움을 준다.
- X-Forwarded-Port : 클라이언트가 로드 밸런서 연결에 사용한 **포트**를 식별하는 데 도움을 준다.

### 장애 대비 & Fail Over
기본적으로 서버의 이중화 구성 ⇒ Load Balancer 두 개 구성  

이중화된 Load Balancer들은 **서로 Health Check**를 하는데, 
**Master와 Standby 서버를 구성하고 Master 서버가 Fail 시 Standby 서버는 자동으로 Master 서버의 역할을 한다.**
(평소, 장애가 발생하지 않았을 때에는 Standby 서버는 대기상태로만 있는다).

### 단점
세션 데이터를 관리해야 하는데, 클라이언트 연결 정보를 저장하는 세션이 로드밸런싱을 통해 하나의 서버에 저장되는 경우,
추후 **다른 서버로 접속했을 때 세션이 유지되지 않는다.** 이런 문제를 해결하기 위해 **세션을 고정 (session sticky)** 한다.
이 방법으로 특정 사용자의 요청이 전달될 노드를 고정시킬 수 있다.  
**하지만 세션이 고정된 노드에 장애가 발생하면 고정한 의미가 없어지므로 장애가 발생하여 비활성화된 노드에 대한 고려가 필요**하다.

### 주요 기술
- **NAT (Network Address Translation)**  
    Private IP를 Public IP로 바꾸는 데에 사용하는, 통신망의 주소 변조기

- **DSR (Dynamic Source Routing Protocol)**  
    로드밸런서 사용 시 서버에서 클라이언트로 돌아가는 경우, 목적지 주소를 Switch의 IP주소가 아닌, 
    클라이언트의 IP로 전달해서 네트워크 Switch를 거치지 않고 바로 클라이언트에 응답하는 개념

- **IP Tunneling**  
    데이터를 캡슐화하여 연결된 상호 간에만 캡슐화된 패킷을 구별하여 캡슐화를 해제할 수 있음. 
    운영체제 단에서 지원만 한다면 확장이 쉬운 시스템을 설계할 수 있음

### 대용량 서비스를 위한 부하 분산
- 대용량 트래픽을 장애 없이 처리하려면 여러 대의 서버에 적절히 트래픽을 분배해야 한다. 
단지 몇 개의 노드만 있다면, 로드 밸런서 자체의 비용이 높고 불필요한 복잡함을 증가시킬 수 있기 때문에 
**라운드 로빈 DNS**와 같이 방식이 합리적이다.
- **DNS에서 하나의 도메인 네임을 라운드 로빈 방식으로 여러 개의 IP 주소로 변환**한다면, 이것만으로도 쉽게 부하 분산이 가능하다
- 단점
    - 대부분의 클라이언트에서 DNS 서버의 부하를 줄이고 성능을 향상시키기 위해 일정 시간동안 **캐싱**된 데이터를 사용한다. 
    이때문에 부하 분산이 균등하게 발생하지 않는다.
    - 특정 서버에 장애가 발생했을 때 **장애 여부가 감지되지 않는다.** 따라서 해당 서버를 서비스에서 제외할 수 없다.  
        => 대규모 시스템에서는 다양한 알고리즘과 스케줄링을 사용하여 네트워크 트래픽과 분산 요청을 제외하면서 신뢰성 관련 기능(ex. 이상 노드 제거)을 제공한다.
