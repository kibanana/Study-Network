# HTTP/2

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

  달라고 하지 않은 리소스를 서버가 마음대로 보낸다.
  
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
