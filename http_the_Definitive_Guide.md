# HTTP 완벽 가이드

## ch1. Overview of HTTP ([link](https://www.slideshare.net/HyeonSeokChoi/http-1-59184615))

### HTTP (Hyper Text Transfer Protocol)
인터넷의 멀티미디어 배달부

신뢰성 있는 데이터 전송 프로토콜

### 웹 클라이언트 & 서버
웹 클라이언트: 서버에게 HTTP 객체 요청 (ex. 브라우저)
웹 서버: 리소스를 저장하고, 클라이언트가 요청한 데이터 제공

### 리소스
웹 서버는 리소스를 관리하는데, 이 때 리소스는 어떤 종류의 콘텐츠 소스이든 상관 없다.

### 미디어 타입
다양한 리소스의 데이터 타입을 다루기 위한 방법으로, `MEMI(MIME: Multipurpose Internet Mail Extensions)` 타입을 이용하여 나타낸다. HTTP에서 멀티미디어 콘텐츠를 기술하기 위해 채택되었다.

> MIME 타입은 클라이언트에 전송된 문서의 형식을 알려주기 위한 메커니즘입니다.  
  각기 다른 전자 메일 시스템 사이에서 메시지가 오갈 때 겪는 문제점을 해결하기 위해 설계되었으나 이메일에서 너무나 잘 동작해 HTTP에서도 멀티미디어 콘텐츠를 기술하고 라벨을 붙이기 위해 채택했습니다.

### URI (Uniform Resource Identifier)
웹에서 각각의 리소스를 식별하기 위한 방법으로, URL(특정 서버의 한 리소스에 대한 구체적인 위치 기술)과 URN(리소스의 위치에 영향받지 않은 유일무이한 이름 사용. `urn:ietf:rfc:2141`)으로 나뉜다.

### HTTP 트랜잭션
요청 명령(Request) + 요청 응답(Reponse). 각 과정(요청과 응답)은 HTTP 메시지로 이루어진다.

### HTTP 메서드 (Method)
서버에게 어떤 동작이 발생해야 하는지 전달한다

### HTTP 상태 코드 (Status Code)
클라이언트 요청에 대한 처리 결과

- `1xx` : 정보 제공
- `2xx` : 성공
- `3xx` : 리다이렉션
- `4xx` : 클라이언트 요청 오류
- `5xx` : 서버 오류

### HTTP 메시지 (Message)
줄단위의 문자열로, 일반 텍스트다.

- 시작줄: 요청 / 응답의 종류
- 헤더: 이름과 값으로 구성된 추가정보, 헤더는 빈줄로 끝난다(헤더와 본문 사이 빈 줄)
- 본문: 실질적인 콘텐츠 데이터

### TCP/IP
HTTP는 Application Layer 프로토콜이고 네트워크 전송은 신뢰성 있는 TCP/IP를 사용한다.

HTTP 메시지를 전송하기 위해서는 클라이언트가 IP, Port를 사용하여 서버와 연결을 맺어야 한다.

### HTTP 프로토콜 버전
- `HTTP/0.9`

  1991년의 **HTTP 프로토타입**으로, **심각한 결함**을 가졌다.
  
  `GET` 메소드만 지원하며, `MIME` 타입(마임 타입)을 지원하지 않는다.
  
- `HTTP/1.0`

  처음으로 HTTP가 널리 쓰이기 시작한 버전으로, **메소드와 멀티미디어 객체 처리가 추가**되었다.
  
- `HTTP/1.0+`

  1.0의 확장판으로, `keep-alive` **커넥션**과 **가상 호스팅**과 **프록시 연결**을 지원한다.

- `HTTP/1.1`

  현재의 보편적인 HTTP 버전으로, **구조적인 결함을 교정**하고 **성능을 최적화**했으며 **잘못된 기능**을 제거했다.

- `HTTP/2.0`

  구글의 SPDY 프로토콜을 기반으로 한 성능 개선 버전이다.

### 웹의 구성요소
- 인터넷과 상호작용할 수 있는 웹 어플리케이션
- 프록시

  클라이언트의 모든 HTTP 요청을 받아서 서버에 전달한다. 주로 **보안** 목적으로 사용하며 **요청과 응답을 필터링**한다.

- 캐시

  클라이언트와 서버의 중간에서 **문서를 저장해두는 특별한 종류의 프록시**로, HTTP는 캐시를 효율적으로 동작하게 하는 많은 기능을 정의한다.

- 게이트웨이

  다른 서버의 중계자로 동작하는 특별한 서버로, 보통 HTTP 트래픽을 다른 프로토콜(ex. `FTP`)로 변환하기 위해 사용한다.
  
  게이트웨이는 **리소스를 갖고 있는 진짜 서버인 것처럼 요청을 다룬다.**

- 터널

  두 연결 사이에서 raw 데이터를 **전달**해주는 HTTP 애플리케이션. ex) SSL 트래픽을 HTTP 연결로 전송함으로써 웹 트래픽만을 허용하는 사내 방화벽 통과

- 에이전트

  **HTTP 요청을 만드는 클라이언트 프로그램** ex) Browser, Web Robot


## ch2. URL과 Resource ([link](https://www.slideshare.net/choong83/http-2-url))

### 리소스는 표준화된다
- Phone Number `010-1234-5678`
- Address `서울특별시 도봉구 방학동 ~`
- Book `ISBN 81-7525-766-0`
- Bank Account `110-2345-0933`

### 웹에서의 리소스 위치를 표준화한 URL (Uniform Resource Locator)

- 브라우저가 정보를 찾을 때 필요한 리소스 위치를 가리킨다.
- `HTTP` 이외에 프로토콜을 통해서도 접근 가능
- URI (Uniform Resource Identifier) = URL

  HTTP 명세에서는 URI를 더 일반화된 개념의 리소스 식별자로 이용한다.

- `Scheme`(How) + `Host`(Where) + `Path`(What) 으로 이루어진다.
- URL은 일반적으로 9개 부분으로 나누어지지만, 이 모든 컴포넌트를 가지는 URL은 없다.

  `<scheme>://<user-name>:<password>@<host>:<port>/<path>;<params>?<query>#<fragment>`

  - scheme (protocol)
  
    `HTTP, MAILTO, FTP, RTSP` 등
    
    주어진 리소스에 어떻게 접근하는지 알려주는 정보로, 알파벳으로 시작하며 `:`로 schema와 URL의 나머지 부분을 구성한다. 
    대소문자를 구분하지 않는다.
  
  - user name, password
  
    `FTP`인 경우 기본 user name은 `anonymous`, 비밀번호는 브라우저마다 가지고 있는 기본값을 사용한다.
  
  - host, port
  
    Host는 접근하려고 하는 리소스를 가지고 있는 인터넷상의 호스트 장비로, Host명이나 IP주소 형태로 제공한다.
  
    Port는 서버가 열어 놓은 Network Port를 말한다. HTTP Default Port: `80`
  
  - path
  
    리소스가 서버 내의 어느 위치에 있는지 알려준다. 계층적 파일 시스템 경로와 유사한 구조로, `/` 문자를 기준으로 나누어진다.
  
  - params
  
    Host, Path만으로는 리소스를 찾지 못할 수 없는데, 이 때 Application에 정확한 요청을 하기 위한 입력 param을 넘길 때 `params`를 사용한다. `;` 로 구분하여 기술한다.
  
  - query
  
    요청할 리소스 형식의 범위를 좁히기 위해 query param을 받을 수 있다. `?`로 시작하고 `&`로 각 쿼리스트링을 구분한다.
  
  - fragment

    HTML 같은 리소스 형식들은 본래의 수준보다 더 작게 나뉠 수 있다. 
    용량이 큰 텍스트 문서의 경우 URL은 문서 전체를 가리키지만 이상적으로 특정 문단을 가리킬 수 있다. 
    `#` 뒤에 fragment id를 작성한다.

### 상대 URL

`https://search.shopping.naver.com/search/category.nhn?cat_id=50000806` 중 전체는 절대URL, `/search/category.nhn?cat_id=50000806`은 상대URL이다. 

절대 URL은 모든 정보를 가지고 있고, 상대 URL은 모든 정보를 가지고 있지 않기 때문에 기저 URL을 사용해야 한다.

기저 URL은 Scheme + Host로, 위의 예시에서는 `https://search.shopping.naver.com`를 기저 URL과 상대 URL을 결합하여 새로운 절대 URL을 만들 수 있다.

### 기저 URL
- 리소스에서 명시적으로 제공

  HTML에서는 `<base>` 태그로 기술 가능
  
  `<base href="" target="">`

- 리소스를 포함하고 있는 기저 URL

  리소스의 URL을 기저 URL로 사용할 수 있다.

- 기저 URL이 없는 경우

  불완전하거나 깨진 URL일 수 있다.

### URL 문자 집합
컴퓨터 시스템의 기본 문자 집합은 영어(`US-ASCII`)를 중심으로 구성되어 있다. 
유럽언어나 기타 다른 언어들은 `US-ASCII`에서 지원하지 않는다.

URL은 특정 이진 데이터를 포함해야하기 때문에 이스케이스 문자열을 쓸 수 있게 설계한다. 
이스케이스 문자열은 `US-ASCII`에서 금지된 문자로, 특정 문자나 데이터를 인코딩할 수 있으므로 이동성과 완성도가 높아진다.

### 인코딩 체계
안전한 문자 집합의 한계를 넘기 위해 인코딩 방식을 고안했다. 
안전하지 않은 문자를 `%`로 시작해서 ASCII 코드로 표현되는 16진수 2개로 이루어진 이스케이스 문자로 바꾼다.

ex)
- `~`: ASCII 코드 0x7E, %7E
- `빈 문자`: ASCII 코드 0x20, %20
- `%`: ASCII 코드 0x25, %25

### 문자제한
몇몇 문자는 특별한 의미로 예약되어 있다. US-ASCII 출력 가능한 문자 집합에 포함되지 않은 문자로, 
인터넷 게이트와 프로토콜에서 이미 선점한 문자다. 이 예약된 문자를 사용하려면 인코딩하여 사용해야 한다.

### PURL (지속 통합 자원 지시자, Persistent Uniform Resource Locators)
이것을 사용하면 URL로 URN 기능을 제공받을 수 있다.

1. Resource Resolver에게 특정 목적지의 URL이 무엇인지 묻는다.
2. Resolver는 리소스의 현재 위치를 보내준다.
3. 실제 URL로 리소스를 가져온다.


## ch3. HTTP Message ([link](https://www.slideshare.net/choong83/http-3-http))

### HTTP 메시지의 각 부분
- 시작줄 : `HTTP/1.0 200 OK`
- Header : `Content-type: text/plain`
- Body : `Hello Word!`

시작줄과 Header는 `CRLF`로 구분한다. Body는 선택적인 데이터 덩어리, 이진데이터를 포함할 수 있으며 비어있는 것도 가능하다.

### HTTP 메시지 문법
- 모든 HTTP 메시지는 요청, 응답 메시지로 구분된다.

#### 요청 메시지의 형식

```
<method> <request URL> <version>
<header>

<body>
```

#### 응답 메시지의 형식

```
<version> <HTTP Status Code> <HTTP Status Message>
<header>

<body>
```

### Start-line (request-line)
`<HTTP Method> <URL> <HTTP Version>`

어떤 동작이 일어나야하는지 설명하는 `HTTP Method`, 동작의 대상을 지칭하는 요청 `URL`, `HTTP Version`으로 구성되며 모든 필드는 공백으로 구분한다.

### Start-line (status-line)
`<HTTP Version> <HTTP Status code> <HTTP Status message>`

### method

- `Method-GET` : 리소스 요청 용도, Body를 가질 수 없다.
- `Method-HEAD` : GET 처럼 동작하지만 서버는 Header 값만 돌려준다. 서버 개발자는 GET과 동일하게 개발하되 Body는 반환되지 않게 처리해야 한다.
- `Method-PUT` : 서버가 요청의 본문을 가지고 요청 URL의 이름대로 새 문서를 만들거나, 이미 URL이 존재하면 본문을 교체하는 것이다.
- `Method-POST` : 서버에 입력 데이터를 전송하기 위해 설계했다.
- `Method-OPTIONS` : 특정 리소스에 어떤 method가 지원하는지 확인 가능하다.
- `Method-DELETE` : 리소스 삭제를 요청, Body를 가질 수 없다.

### status code ([link](https://velog.io/@honeysuckle/HTTP-%EC%83%81%ED%83%9C-%EC%BD%94%EB%93%9C-HTTP-status-code-))

상태코드는 응답의 시작줄에 위치하며, 3자리 숫자로 구성된다.

![image](https://user-images.githubusercontent.com/37951612/78338634-14267980-75ce-11ea-8b1d-2c225337ab8d.png)

정보성 상태 코드로, `HTTP/1.1`에 도입되어 비교적 새롭지만 복잡함을 감수할만큼 가치가 있는지에 대한 논란이 있다.

- `100` : 요청의 시작 부분 일부가 받아들여졌으며, 클라이언트는 나머지를 계속 이어보내야 함을 의미한다
- `101` : 요청자가 서버에 프로토콜 전환을 요청했으며, 서버에서 이를 승인하는 중임을 의미한다

![image](https://user-images.githubusercontent.com/37951612/78338645-1a1c5a80-75ce-11ea-9f5d-efb026d3d243.png)

- `200` : 요청은 **정상**이고, 본문은 요청된 리소스를 포함하고 있다.
- `201` : 생성 작업을 요청받았고, **생성 작업**에 성공했다.
- `202` : 요청은 받아들여졌으나 아직 **동작은 수행하지 않았고, 요청이 적절**하다.
- `203` : 요청에 성공했지만 **요청에 대한 검증이 되지 않았다.**
- `204` : 요청에 성공했지만 **제공할 내용이 없다.**
- `205` : `204`와 동일하지만 새로고침 등을 통해 **새로운 내용을 추가적으로 확인할 것**을 의미한다.
- `206` : **요청의 일부분만 성공**했다.

![image](https://user-images.githubusercontent.com/37951612/78338723-41732780-75ce-11ea-999a-d678e54a624b.png)

- `300` : 클라이언트가 동시에 여러 응답을 가리키는 URL을 요청한 경우 응답 목록과 함께 반환된다. ex) HTML 문서를 영문, 불어 페이지 두 개 버전으로 제공
- `301` : 요청한 URL이 옮겨졌을 때 사용하며, **옮겨진 URL 정보와 함께 응답**되어야 한다.
- `302` : `301` 과 동일하나 클라이언트는 **여전히 옮겨지기 전의 URL로 요청해야 한다**는 것을 의미한다.
- `303` : 요청받은 행동 수행을 위해 **다른 URL로 요청해야한다**는 것을 의미한다.
- `304` : **이전의 동일한 요청과 비교하여 변화**가 없음을 의미한다(단시간에 반복된 동일 요청에 대해).
- `305` : 반드시 **프록시(우회경로)**를 통해 요청되어야함을 의미한다(직접적 X).
- `307` : `302`와 동일하며, **HTTP Method도 변경 없이 요청해야 한**다는 것을 의미한다.

![image](https://user-images.githubusercontent.com/37951612/78338851-797a6a80-75ce-11ea-97de-38297bcd4776.png)

- `400` : 클라이언트가 **올바르지 않은 요청**을 보냈다.
- `401` : 요청을 위해 권한 **인증** 등이 요구한다.
- `403` : 요청이 **서버에 의해 거부되었다.** 서버는 거부 이유를 응답할 수 있지만 보통 숨긴다.
- `404` : **요청한 URL을 찾을 수 없다.**
- `405` : 요청한 URL이 **Method를 지원하지 않음**을 의미한다.
- `406` : **클라이언트의 요청에 대한 적절한 컨텐츠가 없다.**
- `407` : `401`과 동일하지만 **프록시(우회 경로)** 를 통해 **인증**할 것을 요구한다.
- `408` : 요청에 **응답하는 시간이 너무 길다.**
- `409` : 클라이언트 요청에 대해 **서버 충돌이 발생할 요소가 있다.**
- `410` : 요청한 URL이 더이상 사용되지 않고 사라졌다.
- `411` : 클라이언트 요청에 `Content-Length` 헤더가 포함되어야 한다.
- `412` : 클라이언트가 조건부 요청을 했는데 그 중 하나가 실패했다.
- `413` : **요청이 너무 커서** 서버가 처리할 수 없다.
- `414` : **요청 URL이 너무 길어서** 서버가 처리할 수 없다.
- `415` : **서버가 이해하지 못하는 유형의 컨텐츠**를 요청했다.
- `416` : 클라이언트의 **요청 범위**가 잘못됐다.
- `417` : 클라이언트 요청 헤더의 `Expect`에 대해 서버가 만족하지 않았다.

![image](https://user-images.githubusercontent.com/37951612/78338894-8bf4a400-75ce-11ea-9cb1-2f8c61c1a4a2.png)

- `500` : **서버에 오류가 발생**하여 응답할 수 없다.
- `501` : 클라이언트 요청에 대한 **서버의 응답 수행 기능이 없다.**
- `502` : 프록시나 게이트웨이 등의 서버에서 응답하며, 서버의 **상위 서버에서 오류가 발생하였음을 의미**한다.
- `503` : 현재 서버가 유지보수 등의 이유로 일시적으로 사용이 불가함을 의미한다.
- `504` : 서버에서 다른 서버로 요청을 보냈으나 **응답 지연이 발생하여 처리가 불가**하다.
- `505` : 서버가 지원할 수 없거나 올바르지 못한 프로토콜로 요청을 받았다.

### 사유구절 (Reason-Phrase)

응답 시작줄의 마지막 구성요소로, 상태코드에 대한 설명 글이기때문에 상태코드와 1:1로 대응한다.

### 버전 번호

요청, 응답 메시지 양쪽에 모두 기술된다. `HTTP/1.1`은 꼭 해당 메시지가 1.1 메시지라는 의미가 아니다. 보낸 측에서 1.1 version까지 지원할수 있다는 의미다. version은 `<major>.<minor>`으로 구분된다.

### Header

요청, 응답에 대한 추가정보다. `<name>: <value>` 쌍으로 이루어지며, 일반 Header(요청과 응답 양쪽에서 사용 가능), Request Header, Response Header, Entity Header(본문 크기와 contents, resource를 서술), Entension Header(명세에 정의되지 않은 새로운 Header)로 구분된다.

### 일반 Header
메시지에 대한 기본 정보를 제공

- Connection
- Date
- MEMI-version
- Trailer chunked transfer
- Transfer-Encoding
- Upgrade
- Via

### 일반 Cache Header
매번 서버로부터 객체를 가져오는 대신 로컬 복사본으로 캐시할 수 있다.

- cache-control

  메시지와 함께 캐시 지시자를 전달하기 위해 사용한다.

- Pragma

  메시지와 함께 지시자를 전달하는 방법으로, 캐시에 국한되지 않는다.

### Request Header
- Client-IP
- From
- Host
- Referer
- User-Agent
- UA-Color
- UA-CPU
- UA-Disp
- UA-OS
- UA-Pixels

### Accept Header
클라이언트가 원하는 것, 원하지 않는 것을 알려줄 수 있다.

- Accept
- Accept-Charset
- Accept-Encoding
- Accept-Language
- TE

### 조건부 Request Header
요청에 대한 제약

- Except
- If-Match
- If-Modified-Since
- If-None-Match
- If-Range
- If-Unmodified-Since
- Range

### 요청 보안 헤더
요청을 위한 간단 인증 요구 / 응답 체계를 가지고 있다. 이 헤더를 이용하여 진행되는 절차는 트랜잭션을 더 안전하게 만들어준다.

- Authorization
- Cookie
- Cookie2

### Proxy Request Header
Proxy를 돕기 위한 헤더다.

- Max-Forwards

  요청이 원 서버로 향하는 과정 중 다른 프록시나 게이트로 전달될 수 있는 최대 횟수다. `TRACE Method`와 함께 사용된다.

- Proxy-Authorization

  `Authorization` 헤더와 같으나 프록시에서 인증 시 사용한다.

- Proxy-Connection

  `Connection` 헤더와 같으니 프록시에 연결을 맺을 때 사용한다.

### Response Header
응답에 대한 부가정보를 제공한다.

- Age
- Public
- Retry-After
- Server
- Title
- Warming

### Entity Header
요청, 응답 모두 이 헤더를 가질 수 있다.

- Allow

  수행될 수 있는 요청 메소드를 나열한다.

- Location

  Client에게 엔티티가 실제로 어디 위치하는지 설명한다. 수신자에게 리소스에 대한 새로운 위치를 알려준다. 

### Content Header
엔티티의 컨텐츠에 대한 구체적인 정보를 제공한다

`Content-*`


### Entity Caching Header
엔티티 캐싱에 대한 정보를 제공한다. 리소스에 대해 캐시된 사본이 아직 유효한지에 대한 정보다.
- ETag

  해당 엔티티에 대한 엔티티 태그

- Expires
- Last-Modified
  
  가장 최근 변경 일시


