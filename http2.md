# HTTP/2

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
    - (PNG 파일 수만큼 3개 단계 반복)
  
  - `HTTP/1.0+` (KEEP-ALIVE, **22RTT**)
  
    - TCP 커넥션 수립 (3-way handshake)
    - HTML 파일 1개 가져옴
    - PNG 파일 1개 가져옴(20번 반복)
  
  - PARALLEL CONNECTIONS (**6RTT**)
  
    - TCP 커넥션 수립 (3-way handshake)
    - HTML 파일 1개 가져옴
    - TCP 커넥션 7개 수립 (3-way handshake)
      - PNG 파일 8개, PNG 파일 8개, PNG 파일 4개씩 나누어서 가져옴
  
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
- `HTTP/2`의 헤더 압축: `HPACK`

### 현재(2014.10) `HTTP/2` 진행상황
- Working Group Last Call
- 2015 2월 RFC 출판 예정 => [실제 예정대로 출판됨](https://tools.ietf.org/html/rfc7540)
