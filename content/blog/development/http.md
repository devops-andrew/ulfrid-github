---
title: 'Http 정리하기'
date: '2019-06-29T18:30:25.169Z'
category: 'development'
---

![](./images/http.png)

- Http를 정리하자

# HTTP 구조 및 핵심 요소

## HTTP

- HyperText Transfer Protocol
- 하이퍼텍스트(HTML) 문서를 교환하기 위해 만들어진 protocol(통신 규약).
  - 즉 웹상에서 네트워크로 서버끼리 통신을 할때 어떠한 형식으로 서로 통신을 하자고 규정해 놓은 "통신 형식" 혹은 "통신 구조" 라고 보면 된다.
  - 프론트앤드 서버와 클라이언트간의 통신에 사용된다.
  - 또한 백앤드와 프론트앤드 서버간에의 통신에도 사용된다.
- HTTP는 TCP/IP 기반으로 되어있다.

## HTTP 핵심 요소

HTTP 통신 방식

- HTTP 기본적으로 요청/응답 (request/response) 구조로 되어있다.
  - 클라이언트가 HTTP request를 서버에 보내면 서버는 HTTP response를 보내는 구조.
  - 클라이언트와 서버의 모든 통신이 요청과 응답으로 이루어 진다.
  - HTTP는 Stateless 이다.
- Stateless 란 말그대로 state(상태)를 저장하지 않는 다는 뜻.
- 즉, 요청이 오면 그에 응답을 할뿐, 여러 요청/응답 끼리 연결되어 있지 않다는 뜻이다. 즉 각각의 요청/응답은 독립적인 요청/응답 이다.
- 예를 들어, 클라이언트가 요청을 보내고 응답을 받은후, 조금 있다 다시 요청을 보낼때, 전에 보낸 요청/응답에 대해 알지 못한다는 뜻이다.
- 그래서 만일 여러 요청과응답 의 진행과정이나 데이터가 필요할때는 쿠키나 세션 등등을 사용하게 된다.

HTTP Request 구조

- HTTP request 메세지는 크게 3부분으로 구성된다:
  - status line
  - headers
  - body

HTTP Response 구조

- Response도 request와 마찬가지로 크게 3부분으로 구성되어 있다.
  - Status line
  - Headers
  - Body
