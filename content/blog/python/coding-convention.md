---
title: '코드 컨벤션의 중요성'
date: '2019-11-07T19:54:32.169Z'
category: 'python'
---


![](https://onsil-thegreenhouse.github.io/assets/images//programming/django/python-django-logo.jpg)

## 요즘 프로젝트를 진행하면서 PR을 날리고 가장 두근두근하는건 바로

# 코드 컨벤션

- 아직까지 많은 코딩 경험이 없다보니, 그때 그때 코드를 쓰는 방법이 바뀐다.

- 규칙을 만들고 변하지 않도록 지속적으로 유지하려고 노력해야한다.(심지어 코드카타를 하더라도)

- 그리하여 내가 가장 잘 실수 하는 것들을 요약하는 시간을 가져보기로 했다.

### 1. 클래스 아래 함수의 시작시 한칸 띄우기(new line)

```python
class Category(View):
 13
 14     def get(self, request):
 15         category_list = list(Categories.objects.values())
```

### 2. 클래스 아래 함수에 데코레이터가 존재하면 붙이기

```python
class AdminList(View):
31     @login_required
32     def get(self, request):
33         admin = User.objects.get(id=request.user.id)
```

### 3. if 나 else 가 시작되면 (new line)

```python
33   admin = User.objects.get(id=request.user.id)
34
35         if admin.is_admin:
```

### 4. return ()에 장문이 올경우.

```python
 41             return JsonResponse(
 42                 {"message" : "관리자 외에는 접근할 수 없습니다.", "error_code" : "NOT_ADMIN"},
 43                 status=401
 44             )
```

### 5. indent 잘지키기.

```python
1 return aaaaa(
      abcd,
          efgh,
      ijkl,
  )
```

위와 같이해도 문제는 없을 테지만, 보기에 좋지 않다.

### 6. 불린형(boolean)의 값을 조건문에서 ==를 통해 비교하지 마세요.

### 7. 기타 찾아본 규칙

- 들여쓰기는 공백 4칸을 권장합니다.
- 한 줄은 최대 79자까지
- 최상위(top-level) 함수와 클래스 정의는 2줄씩 띄어 씁니다.
- 클래스 내의 메소드 정의는 1줄씩 띄어 씁니다.

- 대괄호([])와 소괄호(())안
- 쉼표(,), 쌍점(:)과 쌍반점(;) 앞
- 키워드 인자(keyword argument)와 인자의 기본값(default parameter value)의 = 는 붙여 씁니다.

- 코드와 모순되는 주석은 없느니만 못합니다. 항상 코드에 따라 갱신해야 합니다.
- 불필요한 주석은 달지 마세요.
- 한 줄 주석은 신중히 다세요.
- 문서화 문자열(Docstring)에 대한 컨벤션은 PEP 257을 참고하세요.

- 언뜻 보면 잘 이해가 안 가는 규칙은 이런 것도 있습니다.

```python
  Yes: import os
       import sys
  No:  import sys, os
```

굳이 한 줄씩 내려쓰면 길어지기만 하고 보기 안 좋지 않을까요? 하지만 이 역시 대부분의 변경 추적 도구가 행 기반임을 고려하면 그렇지 않습니다.

### 마지막으로 네이밍 규칙

- 변수명에서 \_(밑줄)은 위치에 따라 다음과 같은 의미가 있습니다.

  _single_leading_underscore: 내부적으로 사용되는 변수를 일컫습니다.  
  single_trailing_underscore_: 파이썬 기본 키워드와 충돌을 피하려고 사용합니다.  
  **double_leading_underscore: 클래스 속성으로 사용되면 그 이름을 변경합니다. (ex. FooBar에 정의된 **boo는 \_FooBar**boo로
  바뀝니다.)  
  **double_leading_and_trailing_underscore\_\_: 마술(magic)을 부리는 용도로 사용되거나 사용자가 조정할 수 있는 네임스페이스 안의 속성을 뜻합니다. 이런 이름을 새로 만들지 마시고 오직 문서대로만 사용하세요.

- 소문자 L, 대문자 O, 대문자 I는 변수명으로 사용하지 마세요. 어떤 폰트에서는 가독성이 굉장히 안 좋습니다.
- 모듈(Module) 명은 짧은 소문자로 구성되며 필요하다면 밑줄로 나눕니다.
- 모듈은 파이썬 파일(.py)에 대응하기 때문에 파일 시스템의 영향을 받으니 주의하세요.
  C/C++ 확장 모듈은 밑줄로 시작합니다.
- 클래스 명은 카멜케이스(CamelCase)로 작성합니다.
  내부적으로 쓰이면 밑줄을 앞에 붙입니다.
  예외(Exception)는 실제로 에러인 경우엔 “Error”를 뒤에 붙입니다.
- 함수명은 소문자로 구성하되 필요하면 밑줄로 나눕니다.
  대소문자 혼용은 이미 흔하게 사용되는 부분에 대해서만 하위호환을 위해 허용합니다.
- 인스턴스 메소드의 첫 번째 인자는 언제나 self입니다.
- 클래스 메소드의 첫 번째 인자는 언제나 cls입니다.
- 메소드명은 함수명과 같으나 비공개(non-public) 메소드, 혹은 변수면 밑줄을 앞에 붙입니다.
- 서브 클래스(sub-class)의 이름충돌을 막기 위해서는 밑줄 2개를 앞에 붙입니다.
- 상수(Constant)는 모듈 단위에서만 정의하며 모두 대문자에 필요하다면 밑줄로 나눕니다.

참고내용
스포카 기술블로그 [https://spoqa.github.io/2012/08/03/about-python-coding-convention.html](https://spoqa.github.io/2012/08/03/about-python-coding-convention.html)
