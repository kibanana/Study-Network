# HTTP/2

## 나만 모르고 있던 HTTP/2 ([link]())

> `HTTP/1.1`은 약 15년간(1996년 1.0 release, 1999년 ) 웹 통신 프로토콜을 대표하는 프로토콜이었으며, 그 불편함과 느림을 대체하기 위해 `HTTP/2.0`이 등장하게 되었다. 현재의 웹은 다량의 멀티미디어 리소스를 처리해야 하고, 웹페이지 하나를 구성하기 위해 다수의 비동기 요청이 발생하는데 이를 처리하기에 `HTTP/1.1` 스펙이 너무 느리고 비효율적이기 때문이다. 모바일 환경에선 더욱 그렇다.

### `HTTP/1.1` 동작방식
- 기본적으로 커넥션 당 하나의 요청을 처리하도록 설계되어 있다. 따라서 동시전송이 불가능하고 요청과 응답은 순차적으로 이루어진다. 따라서 HTTP 문서 내에 포함된 다수의 리소스(ex. image, css, script)를 처리하려면 요청할 리소스 개수에 비례해서 대기 시간(Latency)이 길어지게 된다.

### `HTTP/1.1` 단점
- HOL(Head Of Line) Blocking : 특정 응답의 지연

  - HTTP의 HOL Blocking
  - TCP의 HOL Blocking
  
    커넥션 당 하나의 요청처리를 개선하는 기법 중 **Pipelining**이 존재한다. 이것은 **하나의 커넥션을 통해 다수의 파일을 요청/응답받을 수 있는 기법**을 말하며, 이 기법을 통해 어느 정도의 성능 향상이 가능하다.
    
    하지만 여러 개의 **요청 - 응답** 사이클 중 첫번째 리소스가 지연되면 두번째, 세번째 리소스 또한 지연되게 된다. 이렇게 앞의 응답이 지연됨에 따라 뒤의 응답들도 따라서 지연되는 것을 **HTTP의 Head of Line Blocking**이라고 하며, 파이프 라이닝 기법의 큰 문제점 중 하나다.
    
- RTT(Round Trip Time) 증가
  
  매 요청별로 커넥션을 만들면 TCP 위에서 동작하는 HTTP 특성상 **3-way-handshake**가 반복적으로 발생하고 불필요한 RTT가 증가함으로써 네트워크 지연을 초래하며 성능을 저하시킨다.

- 무거운 Header 구조 (특히 Cookie)

  `HTTP/1.1` 헤더에는 많은 메타정보가 저장되어 있다. HTTP 요청이 발생할 때마다 매번 중복된 헤더값을 전송하게 되며, 해당 도메인에 설정된 쿠키 정보도 요청할 때마다 헤더에 포함되어 전송된다(별도의 Domain Sharding을 하지 않았을 경우). 때론 요청에 대한 응답 리소스보다 헤더 값이 더 큰 경우도 있다(User-Agent 정보: 대략 120byte).

### `HTTP/1.1`의 문제점과 비효율성을 극복하고자 한 노력
- Image Spriting

  웹 페이지를 구성하는 다양한 아이콘 이미지 파일의 요청 횟수를 줄이기 위해 모든 아이콘을 합쳐서 하나의 큰 이미지로 만든 다음 CSS에서 해당 이미지의 좌표 값을 지정하여 표시한다.

- Domain Sharding

  `HTTP/1.1`의 단점을 극복하기 위해 요즘 브라우저들은 다수의 커넥션을 생성해서 병렬로 요청을 보낸다. 하지만 브라우저 별로 Domain당 커넥션 개수 제한이 존재하고, 근본적인 해결책이 되지 못한다.

- Minify CSS/Javascript

  HTTP를 통해 전송되는 데이터 용량을 줄이기 위해 CSS, Javascript 코드를 축소하여 적용한다. ex) Webpack

- Data URI Scheme

  HTML 문서 내의 이미지 리소스를 `Base64`로 인코딩된 이미지 데이터(쉽게 말하자면 그냥 문자열)로 직접 기술하여 이미지가 요청되는 횟수를 줄인다.

- Load Faster

  StyleSheet를 HTML 문서 상단에, Script를 HTML 문서 하단에 배치한다.
  
하지만 이런 노력도 `HTTP/1.1`의 단점을 근복적으로 해결할 수 없었고, 구글은 더 빠른 웹을 실현하기 위해 **Throughput 관점이 아닌 Latency 관점에서 HTTP를 고속화**한 `SPDY(스피디)`라는 새로운 프로토콜을 구현했다. 다만 이 프로토콜은 HTTP를 대체할 수 없으며 HTTP를 이용한 전송을 재정의하는 동작을 한다. 이 프로토콜은 상당한 성능 향상과 효율성을 보여줬고 이는 `HTTP/2` 초안에 참고된다.

### 어서오세요~ `HTTP/2`

> "HTTP/2 is a replacement for how HTTP is expressed “on the wire.” It is not a ground-up rewrite of the protocol; HTTP methods, status codes and semantics are the same, and it should be possible to use the same APIs as HTTP/1.x (possibly with some small additions) to represent the protocol. The focus of the protocol is on performance; specifically, end-user perceived latency, network and server resource usage. One major goal is to allow the use of a single connection from browsers to a Web site."  
"HTTP/2는 HTTP가 유선상에서 표현 방법을 대치 하는것이다. 이것은 프로토콜을 완전히 다시 작성하는게 아니라 HTTP 메소드, 상태 코드 및 의미는 동일하며 프로토콜을 나타 내기 위해 HTTP/ 1.x와 동일한 API (일부 작은 추가 기능 포함)를 사용 할 수 있어야 한다. HTTP/2의 초점은 성능에 있다. 특히 최종 사용자가 대기 시간, 네트워크 및 서버 리소스 사용을 인식한다. 주요 목표 중 하나는 브라우저에서 웹 사이트로의 단일 연결을 허용하는 것입니다." 라고 소개되어 있다.  즉 완전히 새로운 프로토콜을 만들었기 보단 성능향상에 초점을 맞춘 프로토콜 이라는것이다.

`HTTP/2`는 이런 방식으로 성능을 향상시킨다.
- Multiplexed Streams

  한 커넥션으로 동시에 여러 메시지를 주고받을 수 있으며 응답은 순서에 관계없이 스트림으로 주고 받는다. `HTTP/1.1`의 `Connection Keep-Alive, Pipelining`이 개선된 형태라고 볼 수 있다.
  
- Stream Prioritization

  리소스간 의존관계(우선순위)를 설정하여 *(클라이언트가 요청한 HTML 문서 내의 CSS 파일, Image 파일 여러개 중 Image 파일보다 CSS 파일의 수신이 늦어지는 경우 브라우저 렌더링 문제 발생)* 등의 문제를 없앤다

- Server Push

  서버는 클라이언트의 요청에 대해 요청하지 않은 리소스를 마음대로 보낼 수도 있다. (중간 생략) 이를 `PUSH_PROMISE`라고 부르며, 클라이언트는 `PUSH_PROMISE`를 통해 서버가 전송한 리소스에 대해 요청하지 않는다.

- Header Compression

  `HTTP/2`는 Header 정보를 압축하기 위해 **Header Table**, **Huffman Encoding** 기법을 사용하며, 이를 **[HPACK](https://http2.github.io/http2-spec/compression.html)** 압축방식이라 부른다.
  
  > 두 번째 요청 중 많은 부분이 첫 번째 요청을 반복하고 있음을 볼 수 있다. 첫 번째 요청은 약 220 bytes며, 두 번째는 약 230 bytes다. 그러나 단 20 bytes만 차이가 난다. 그 20bytes만 전송한다면 전송 바이트 수를 90% 정도 줄일 수 있을 것이다.
  
  `HTTP/2`에서는 연속된 요청들의 Header에 중복값이 존재하는 경우 **Static/Dynamic Header Table 개념을 사용하여 중복 Header를 검출하고 중복된 Header는 index 값만 전송**하며 **중복되지 않은 Header 정보값**은 **Huffman Encoding 기법으로 인코딩하여 전송**한다.
  
  > Huffman Encoding 기법  
    무손실 압축 알고리즘으로, 심볼의 출현횟수(출현빈도)를 기준으로 코드를 부여하는데 자주 출현하는 심볼에는 짧은 코드를 부여하고, 상대적으로 적게 출현하는 심볼에는 더 긴 코드를 부여한다.  
    Huffman coding을 이해하는데 중요한 키워드는 내림차순 정렬, 이진트리 정도이다. "dessert"에 대해서 심볼, 출현 횟수를 "(심볼,횟수)"로 표현하고 출현 횟수를 기준으로 내림차순 정렬한다.
    
    ![image](https://user-images.githubusercontent.com/37951612/78396303-1cac9d80-762a-11ea-9dbc-ad3066951a46.png)
    
    심볼, 출현 횟수, 코드

    - e, 2, 00
    - s, 2, 01
    - r, 1, 100
    - t, 1, 101
    - d, 1, 11

    "dessert"를 Huffman coding으로 encoding하면 `1100010100100101`라는 결과가 나온다.

> 우리가 일반적으로 사용하는 모바일 Device에서 구동되는 대 부분 브라우저는 HTTP/2를 지원하고 있고 이는 항상 데이터 부족에 시달리는 많은 사용자들에게 보다 더 적은량으로 동일한 화면을 볼 수 있는 이득을 줄 수 있고 또한 유선망에 비해 상대적으로 느린 LET, 3G망의 사용자가에게 조금 더 나은 서비스 이용 환경을 제공 해 줄 수 있다. 그저 **우리의 웹서버에 HTTP/2 지원 모듈을 설치하거나 HTTP/2를 지원하는 웹서버로의 변경을 통해서 말이다.**  물론 HTTP/2 도입을 위해 새로운 웹서버로의 변경 작업은 자칫 서비스 장애로 이어질 수 있기에 신중히 판단해서 처리할 작업이겠지만, 이는 변경할 만한 충분한 가치를 가진 작업이라 생각된다.

## `HTTP/2`와 `HTTP/1.1`의 주요 차이점
- HTTP Body는 `1.1`에서 text로 전송되었었다. 하지만 `2.0`에서는 **binary로 전송**된다.
- 기존에는 HTTP Header와 Body가 `\r\n`으로 구분되었으나, `HTTP/2.0`부터는 header와 body가 layer로 구분된다.

  이를 **binary framing layer**라고 부른다.

- 기존에는 header-body를 묶은 **http message**가 최소 전송 단위였다면, `2.0`에서는 최소 전송 단위가 **binary frame**이 된다.

  ![image](https://user-images.githubusercontent.com/37951612/78388319-bf115480-761b-11ea-9ce6-041aeeb7be78.png)
  
  ![image](https://user-images.githubusercontent.com/37951612/78388347-cafd1680-761b-11ea-8392-4c6859797bd6.png)

  `HTTP/1.x`는 다음과 같은 단점을 가졌기에 `HTTP/2.0`은 **binary**를 전송 형식으로 지정하게 되었다.
  
    - 본문은 압축되지만 헤더는 압축되지 않는다.
    - 연속된 메시지들은 보통 비슷한 헤더 구조를 가졌는데, 메시지는 매번 전송되어야 한다.
    - 다중전송(multiplexing)이 불가능하다.
    
      > 다중 전송은 하나의 커넥션에 여러 요청/응답을 병렬적으로 실어 보내는 것을 의미한다. `HTTP/1.x`에서는 이게 불가능하니 여러 개의 커넥션을 열어야 한다. 이 때 **병렬적**이란 요청을 보내고 응답을 기다리지 않고 또다른 요청을 보내는 수준의 병렬성(= multiplexing)을 말한다.
      
      > `HTTP/1.x`에서도 **Keep-Alive**로 Socket 커넥션을 유지하고 TCP 연결을 재사용할 수 있지 않나? 라는 의문이 들 수 있는데, 
          이 때에는 **요청-응답**으로 이루어진 한 HTTP 연결이 끝난 다음, 그 다음의 HTTP 연결이 가능한 식이다. 따라서 HTTP Layer에서는 **요청-응답**이 반복된다. 그러므로 **요청-요청-요청 응답-응답-응답**을 뜻하는 (병렬적인)다중 전송은 불가능하다. 이를 지원하기 위해서는 결국 커넥션을 여러 개 열어야 하기 때문이다.
          
      한마디로 `HTTP/1.x`는 **multi-connection** 기반, `HTTP/2`는 **multiplexing** 기반이라고 할 수 있다. 

### `HTTP/2`의 multiplexing
하나의 커넥션을 여러 스트림이 이용하며 다중 전송을 할 수 있다. 한 칸의 단위는 **binary frame**이다. 

새로운 TCP 커넥션을 여는 것이 **cold request**, 기존의 커넥션을 이용하는 것이 **warm request**인데, 대체로 커넥션을 생성하는 데에 드는 비용을 아낄 수 있는 후자가 더 효율적이다.


## HTTP/2 : 더 빠른 웹을 위해 ([link](https://www.slideshare.net/eungjun/http2-40582114))

## 왜 만들었나?
- `HTTP/1` 이 너무 느려서

  `HTTP/1`은 클라이언트가 서버에 요청 - 서버가 클라이언트에 응답 하는 동작으로 이루어져있는데, 
  대역폭과 상관없이 round trip(왕복, 왕복 시간은 RTT) 때문에 느려진다. 거리가 멀어질수록 더 오래걸린다.

  - 태초의 HTTP (PNG 파일 20개 가정, **42RTT**)
    
    - TCP 커넥션 수립 (3-way handshake)
    - HTML 파일 1개 가져옴
    - TCP 커넥션 닫음
    - TCP 커넥션 수립 (3-way handshake)
    - PNG 파일 1개 가져옴
    - TCP 커넥션 닫음
    - (PNG 파일 수만큼 반복)
  
  - `HTTP/1.0+` (KEEP-ALIVE, **22RTT**)
  
    - TCP 커넥션 수립 (3-way handshake)
    - HTML 파일 1개 가져옴
    - PNG 파일 1개 가져옴(20번 반복)
  
  - PARALLEL CONNECTIONS (**6RTT**)
  
    - TCP 커넥션 수립 (3-way handshake)
    - HTML 파일 1개 가져옴
    - TCP 커넥션 7개 수립 (3-way handshake)
    - PNG 파일 8개, PNG 파일 8개, PNG 파일 4개씩 나누어서 가져옴 => 3RTT
  
  - PIPELINING (**3RTT**, 하지만 구현이 어려워서 잘 안쓴다)
  
    - TCP 커넥션 수립 (3-way handshake)
    - HTML 파일 1개 가져옴
    - PNG 파일 20개 가져옴
  
- 헤더가 너무 크다 (특히 쿠키)

### 성능 개선을 위한 노력

대표적으로 **구글이 2009년 발표한 SPDY**가 있다.

### HTTP/2

HTTP 작업그룹이 SPDY를 기반으로 `HTTP/2` 작업 시작(2012년 10월)

## 왜 빠른가?

- Header Compression

  헤더 필드를 Static Table, Dynamic Table의 인덱스로 대체하여 길이를 줄인다.

  - Huffman Coding
  - Header Tables

- Multiplexed Streams

  한 커넥션으로 동시에 여러 메시지를 주고받을 수 있다.
  
  |HTTP/1.1|HTTP/2|
  |------|------|
  |TCP 커넥션 1개를 연다|TCP 커넥션 1개를 연다|
  |HTML 문서 1개를 요청해서 받는다|HTML 문서 1개를 요청해서 받는다|
  |TCP 커넥션 8개를 요청해서 받는다|PNG 파일 20개를 요청해서 받는다|
  |PNG 파일 8개를 요청해서 받는다| |
  |PNG 파일 8개를 더 요청해서 받는다| |
  |PNG 파일 4개를 더 요청해서 받는다| |
  
  Stream내의 Frame 하나당 하나의 데이터를 담아서 주고받는다.
  
- Server Push

  달라고 하지 않은 리소스를 서버가 마음대로 보낸다. => HTML 문서 상 필요한 리소스, 즉 HTML 문서를 요청한 클라이언트에 어떻게든 전달되어야 하는 
  
  (HTML 1개, PNG 2개)
  
  - Server Push를 안하면?
    
    TCP 커넥션을 1개 열고, HTML 문서 1개를 요청해서 받고, 그림 파일 2개를 요청해서 받는다.
  
  - Server Push를 하면?
  
    TCP 커넥션을 열고, HTML 문서 1개를 요청해서 그림 파일 2개와 함께 받는다.
    
    ![image](https://user-images.githubusercontent.com/37951612/78350023-7176f680-75df-11ea-8fc2-0be879678d30.png)

- Stream Priority

  요청에 의존성 관계를 지정할 수 있다.
  
  (HTML 1개, CSS 1개, PNG 2개)
  
  - 의존성 지정을 안하면?
  
    TCP 커넥션을 1개 열고, HTML 문서 1개를 요청해서 받고, CSS 문서 1개와 그림 파일 2개를 요청해서 받는다.
    
    => CSS 문서가 늦게 와서 렌더링이 늦어진다.
  
  - 의존성 지정을 하면?
  
    TCP 커넥션을 1개 열고, HTML 문서 1개를 요청해서 받고, CSS 문서 1개와 그림 파일 2개를 요청해서 받되 
    그림 파일이 CSS에 의존성이 있다고 알려준다.
   
    => CSS 문서가 가장 먼저 순조롭게 렌더링

    ![image](https://user-images.githubusercontent.com/37951612/78350620-74261b80-75e0-11ea-87e1-634cd7ac02d8.png)

## `HTTP/1`과 `HTTP/2` 비교

### `HTTP/1`에서 변하는 것
- HTTP 메시지 포맷
- HTTP 메시지 전송방법
  - Connection 헤더 사라짐
  - Chunk 인코딩 사용 금지

### `HTTP/1`에서 변하지 않는 것
> HTTP's existing semantics remain unchanged.

### `SPDY`와 다른 점
- `SPDY`의 헤더 압축: `zlib` => CRIME 취약점

  > CRIME(Compression Ratio Info-leak Made Easy)은 데이터 압축을 사용하는 HTTPS와 SPDY 프로토콜을 사용하는 연결을 통해 기밀 HTTP 쿠키에 대항한 취약점이다.[1] 기밀 HTTP 쿠키의 내용 복원을 사용할 때 공격자는 인증된 웹 세션에 대한 세션 하이재킹을 수행할 수 있게 함으로써 추가 공격의 수행을 허용한다.

- `HTTP/2`의 헤더 압축: `HPACK`

### 현재(2014.10) `HTTP/2` 진행상황
- Working Group Last Call
- 2015 2월 RFC 출판 예정 => [실제 예정대로 출판됨](https://tools.ietf.org/html/rfc7540)
