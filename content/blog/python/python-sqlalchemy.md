---
title: 'SQLAlchemy 그것이 알고싶다'
date: '2019-06-26T11:19:55.169Z'
category: 'python'
---

![](./images/sqlalchemy.jpg)

#sqlalchemy는?

- **python**에서 사용가능한 _ORM_(Object-relational maping)이다.
- ORM은 말그대로 객체(Object)와 관계(Relation)를 연결해주는것이다.
- 데이터베이스의 데이터를 <--매핑--> Object필드
- 장점
  1. 객체 지향적인 코드로 비즈니스 로직에 집중가능
  2. 재사용 및 유지보수 편리성이 증가
  3. DBMS에 대한 종속성이 줄어듬.
- 단점

  1. ORM 만으로 서비스를 구현하기 어려움.
  2. 프로시저가 많은 시스템에서는 장점을 가져가기 어려움.

- ORM이 더 궁금하시다면, [ORM이란 - 권희정님 블로그](https://gmlwjd9405.github.io/2019/02/01/orm.html)

- sqlalchemy 에 대해 잘 파악하려면 역시 공식 기술문서를 보는게 정석!

  공식문서[sql alchemy 공식사이트](https://www.sqlalchemy.org/)

  그 외 이해도를 높이기 위해 블로그 검색을 통해 진행했다.

- 인스톨은 pip로 심플하게~

```zsh
  pip install sqlalchemy
```

- DB를 사용하려면 파이썬 파일 혹은 인터프리터 환경에서 해볼 수 있다.

- DB는 SQLite3 버전을 사용했다.

- 처음은 당연히 임포트로 시작하고, create_engine이라는 메소드로 DB를 정의한다.

- SQLite라서 매우 간단하지만, 다른 DB라면 별도의 접속정보를 명기해야한다.

```zsh
  from sqlalchemy import create_engine

  engine = create_engine('sqlite://"/지정하고 싶은 파일명.db"', echo=True)
  # 위 명령이 DB에 바로 연결시키는건 아니고, 이제 메모리에 인식 시키는 상황이다.
  # echo=True는 찍히는 쿼리를 볼 수 있다.
```

위와 같이 DB에 정의를 했다면,

이제 테이블을 생성한다.

```python
Base = declarative_base()

class Movie(Base):
    __tablename__ = 'movies'
    id = Column(Integer, primary_key=True)
    date = Column(Integer)
    rank = Column(Integer)
    movieNm = Column(String(30))
    movieCd = Column(Integer)
    salesAmt = Column(Integer)
    audiCnt = Column(Integer)

Movie.__table__.create(bind=engine, checkfirst=True)
```

테이블 생성은 위에서와 같이 선언형을 베이스로 하고,
클래스가 테이블을 의미하는게 아니라, 클래스에 넣고
`__tablename__`에 정의해야 원하는 테이블 명으로 맵핑이 이루어진다.

코드 마지막 줄에 'Movie.**table**.create()'가 있어야 실질적으로 생성을 한다.

그래서 다음에 든 생각은 이제 테이블을 만들었으니, 그냥 무언가를 테이블에 맞춰서 add()하면 되지 않을까 했지만,
그것은 경기도 오산 이었다.(죄송합니다;;)

생성한 DB에 데이터 처리를 하려면, sessionmaker를 이용해야 하는것이었다.

세션이라는 단어로인해, 마치 네트웍적인 접근이라 생각하고 처음에 무시했는데, DB와 대화하려면
생성해야 하는 절차였다.

```python
from sqlalchemy.orm import sessionmaker
Session = sessionmaker(bind=engine)
session = Session()
```

다음과 같은 코드를 통해 세션을 생성하고, 생성해둔 엔진(DB)을 연결한다.

이후에 데이터를 인서트 하기위해 데이터를 꾸리고,

```python

movie_list=Movie(date=20190625, rank=1, movieNm='토이 스토리4', movieCd=12345, salesAmt=1234545123,audiCnt=342)

session.add(movie_list)
session.commit()
```

커밋까지 진행해야 실질적으로 데이터 인서트가 이루어진다.

```bash
$ python db.py
2019-06-26 10:21:45,563 INFO sqlalchemy.engine.base.Engine SELECT CAST('test plain returns' AS VARCHAR(60)) AS anon_1
2019-06-26 10:21:45,564 INFO sqlalchemy.engine.base.Engine ()
2019-06-26 10:21:45,564 INFO sqlalchemy.engine.base.Engine SELECT CAST('test unicode returns' AS VARCHAR(60)) AS anon_1
2019-06-26 10:21:45,564 INFO sqlalchemy.engine.base.Engine ()
2019-06-26 10:21:45,564 INFO sqlalchemy.engine.base.Engine PRAGMA table_info("movies")
2019-06-26 10:21:45,564 INFO sqlalchemy.engine.base.Engine ()
2019-06-26 10:21:45,565 INFO sqlalchemy.engine.base.Engine BEGIN (implicit)
2019-06-26 10:21:45,568 INFO sqlalchemy.engine.base.Engine INSERT INTO movies (date, rank, "movieNm", "movieCd", "salesAmt", "audiCnt") VALUES (?, ?, ?, ?, ?, ?)
2019-06-26 10:21:45,568 INFO sqlalchemy.engine.base.Engine (20190625, 1, '토이 스토리4', 12345, 1234545123, 342)
2019-06-26 10:21:45,573 INFO sqlalchemy.engine.base.Engine COMMIT
```

코드가 실행되고 echo=True로 인해 진행 사항이 찍힌 화면.

만약 잘 입력이 됐는지 확인하고 싶다면, session.query(Movie).all()과 같이 불러온 뒤, 반복문으로 돌면서 프린트 해보면 된다.

```vim
result = session.query(Movie).all()
for row in result:
  print(row.date,row.rank,row.movieNm,row.movieCd,row.salesAmt,row.audiCnt)
```

결과는 생략한다. 위와 같이 수행해본 결과들을 정리했다.

아직 모든 기능을 파악한 것은 아니지만, 확실히 장고 ORM과 비교해보면 좀 더 수동적인 절차가 있다고 느껴진다.
마치 오토 차를 타다 수동차로 바꿔타는 느낌~

일단 이번 회차의 파악은 여기서 종료 한다. 아마도 다음에 글을 쓰게 되면 더 능숙해졌거나 더 삽질을 하게 되서 정리하게 될것 같다.

기본적인 인서트와 셀렉트를 다루었고, 아직 조인을 해야할 상황을 마주한적은 없어서 현재는 동기부여가 없지만, 조만간 학습해서 정리해 보도록 해야겠다.

이상 정리 끄읏~~~

-- 참고자료 --

위지원님 블로그 [https://weejw.tistory.com/37](https://weejw.tistory.com/37)

김용균님 블로그 [https://edykim.com/ko/post/getting-started-with-sqlalchemy-part-2/](https://edykim.com/ko/post/getting-started-with-sqlalchemy-part-2/)

공식문서는 위에 링크 참고.
