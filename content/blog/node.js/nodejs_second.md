---
title: 'node.js 2회차'
date: '2019-12-08T22:15:21.169Z'
category: 'node.js'
---

![](./images/nodejs.png)

# node.js

## 지난시간에는...

node.js를 실행하고, 모듈로 임포트/익스포트하고 내장 객체를 이용하는 방법등을 공부해왔다. 오늘도 이어서 진행한다.

## 노드?

- 내장 모듈

  - os

    > os 모듈은 운영체제의 정보를 가져올 수 있는 모듈이다. python의 os와 같다.
    > os.arch() 아키텍쳐 정보
    > os.platform() 운영체제 정보
    > os.type() 운영체제의 종류
    > os.uptime() 운영체제의 실행 시간
    > os.hostname() 운영체제의 이름
    > os.release() 운영체제의 버전
    > os.homedir() 홈 디렉토리 경로
    > os.tmpdir() 임시파일 저장경로
    > os.cpus() 컴퓨터의 코어 정보
    > os.cpus().length 코어 개수를 카운트
    > os.freemem() 사용가능한 메모리
    > os.totalmem() 전체 메모리 용량

  - path

    > 폴더와 파일의 경로를 쉽게 조작하도록 도와주는 모듈
    > 운영체제별로 경로 구분자가 다르기 때문에 필요.
    > Windows 타입: ₩, POSIX 타입(macOS, linux 등): /
    > path.sep 경로 구분자
    > path.delimiter 환경변수의 구분자
    > path.dirname(경로) 파일이 위치한 폴더 경로를 보여준다.
    > path.extname(경로) 파일의 확장자를 보여준다.
    > path.basename(경로, 확장자) 파일의 이름 및 확장자를 보여줌
    > path.parse(경로) 파일경로를 root, dir, base, ext, name으로 분류
    > path.format(객체) path.parse()한 객체를 파일경로로 합침
    > path.normalize(경로) \ 나 /를 실수로 여러 번 사용했거나 혼용했을때 정상적인 경로로 변환
    > path.isAbsolute(경로) 절대 경로 여부를 boolean으로 리턴
    > path.relative(기준경로, 비교경로) 경로를 두 개 넣으면 첫 번째 경로에서 두번째 경로로 가는 방법을 알려줌
    > path.join(경로, ...) 여러인자를 넣으면 하나의 경로로 합쳐줌. 상대경로도 알아서 처리해줌
    > path.resolve(경로, ...) 조인과 같은 일을 하지만, 절대경로로 인식하고 앞의 경로를 무시한다.
    > \\의 경우 윈도우에서 \로 경로 구분을 하는데 자바스크립트에서는 \ 가 특수문자이므로 수식처리해야한다.

  - url

    > 인터넷 주소를 쉽게 조작하도록 도와주는 모듈
    > WHATWG 방식과 기존 노드 url구분 방식
    > URL() 생성자로 처리하는 방법과 기존의 url.parse()
    > 두가지를 다 지원하는 url.format()
    > WHATWG 방식 username, password, origin, searchParams
    > node 방식 auth, query
    > pathname만 오는 경우 기존 node방식을 써야만 읽어올 수 있다.
    > URL() searchParams는 쿼리스트링 방식이 간편해지는 장점이 있다.
    > node는 querystring을 쓰려면 querystring모듈을 사용해야 한다.

  - querystring

    > querystring.parse(쿼리) url 쿼리를 자바스크립트 객체로 분해함.
    > querystring.stringify(객체) 분해된 쿼리 객체를 문자열로 다시 조립함.

  - crypto
    > 암호화를 도와주는 모듈

  이후에는 뭔가 만들면서 정리 해볼 생각이다. 오늘은 여기까지 화이팅!
