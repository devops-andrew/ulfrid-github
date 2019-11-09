---
title: '장고 그놈 참...'
date: '2019-07-10T19:54:32.169Z'
category: 'python'
---

![](https://onsil-thegreenhouse.github.io/assets/images//programming/django/python-django-logo.jpg)

## 장고를 쓰다보니 점점 익숙해지는 한편 자주 쓰게 되는 것들이 있다.

# 오늘은 그것을 정리!

- 장고 ORM에 익숙해지려고 해도 아직은 경험 부족 및 많은 부분에서 자유롭지 않다.

- 정답이 있는 테이블을 보고 짜는데도, 원하는대로 결과를 얻지 못하기가 부지기수...

- 그리하여 내가 최근에 겪은 것들을 정리해보기로 했다.

### 1. 모델을 불러오는 다양한 방법 중 습관화 된 것.

```python
class Category(View):
 13
 14     def get(self, request):
 15         category_list = list(Categories.objects.values())
 16         return JsonResponse(category_list, safe=False)
```

- 위와 같은 오브젝트의 값을 불러올때 쿼리셋 형태가 되면 바로 호출하기 까다롭다.
  그래서 리스트안에 넣고, 바로 JsonResponse에 넘길때가 많다. safe=False 안해주면 에러다...

### 2. 조인된 데이터 불러오기.

```python
31
32     User.objects.filter(id=request.user.id).select_related('provider')
33     User.objects.filter(id=request.user.id).prefetch_related('provider')
34
```

- 1대1, 1대다 관계에서 주로 사용하는 select_related
- 다대다혹은 다대1 관계에 쓰는 prefetch_related 효율적으로 관계표현을 하고 쿼리 수행도 한다.

### 3. Nested된 관계.

- 이를 테면 기준 테이블에 포런키로 연결된 테이블의 포런키로 연결된 테이블이 있을경우.
  위의 프리패치나 셀렉트로는 예쁘게 데이터를 정렬해서 뽑기가 어렵다.

```python
33     item_list = [
34         {
35               'id'      : item["id"],
36               'data'    : item["data"],
37               'answer'  : [{
38                           'id'     : answer["id"],
39                           'value'  : answer["value"]}
40                           for answer in Answer.objects.filter(....)
41          }for item in Items.objects.filter(...)
42     ]
```

- 위와 같이 리스트 컴프리헨션을 사용해서 네스티드된관계에 따른 예쁜 데이터 모양을
  뿌려서 JsonResponse에 전달할수 있다! (오늘 터득해서 완전 신남!!!!)

### 4. 트랜잭션을 활용하자.

- 업데이트를 한다던지, 가입시에 절차적으로 선행되는 작업이 있다던지 할때.
  트랜잭션에 담아두면, 해당 되는 작업 들이 모두 완료되지 않으면 롤백하는
  기능이 있다. 심지어 쉽다.

```python
 41 from django.db import transaction
 42
 43 def get(self,requset):
 44
 45      with transaction.atomic():
 46           a.delete()
 47
 48           b.save()
 49
 50
```

- 위와 같이 삭제하고 저장하는 경우, 삭제 작업이 제대로 이루어져야만 저장할수 있다.
  트랜잭션에 대한 자료는
  [https://docs.djangoproject.com/en/2.2/topics/db/transactions/#tying-transactions-to-http-requests](https://docs.djangoproject.com/en/2.2/topics/db/transactions/#tying-transactions-to-http-requests)

### 5. httpie로 테스트하는 법

```bash
http -v localhost:8000/user/auth email=test@test.net password=12345678
#로그인 테스트
http -v localhost:8000/user/aaaa Access_token:"1asde1jiop1230124$#21....."
#인증 테스트
echo '{"title":"A","user_id":111,"answer":[{"title": "BBB", "service_list":"1,2,3,4,5,6,7,8"},{"title":"CCC","service_list":"2,4,6,8,7,10"}]}' | http -v localhost:8000/aaa/bbb/6 Access_token:"eyJ0eXAiOiJKiJIUzI1NiJ9......"
# 글작성 테스트위한 Json 넘기기
```

- 다양한 테스트를 httpie로 해볼 수 있다.
