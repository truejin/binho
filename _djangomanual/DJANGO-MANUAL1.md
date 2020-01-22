---
layout: manualpost
title: Django EZ Manual
author: 설명 및 프로젝트 구조
icon: fa-lightbulb
icon-style: regular
layout: manualpost
order: 2
---

<h1>Django EZ Manual</h1>

<h4>Django 란?:</h4>

1. Python으로 작성된 Open Source 웹 애플리케이션 프레임워크이다.
2. 장고는 MVC(모델-뷰-컨트롤러) 패턴을 따르고 있다. 하지만 컨트롤러의 기능을 프레임워크 자체에서 하기 때문에
모델, 템플릿, 뷰로 분류해 MTV 프레임워크라고 보기도 한다.
3. 장고는 데이터베이스 기반의 웹사이트를 작성하는데 있어서 수고를 더는 것이 목표이다.
4. 장고는 컴포넌트의 재사용성, 플러그인화 가능성, 빠른 개발 등을 강조하고 있다.
5. 장고는 "DRY(Don't repeat yourself: 중복배제)" 원칙을 따른다.

<hr/>

<h3>목차</h3>

0. 들어가기 전...

1. 간단한 설명 및 프로젝트 구조

2. URL 요청과 응답

3. 모델과 관리자페이지

4. 뷰와 템플릿

5. 정적 파일

6. 테스팅

7. 재사용 가능한 앱 패키징

<hr/>

<h2>0. 들어가기 전...</h2>

이제 부터 대괄호(대괄호는 왼쪽에 있는 기호가 대괄호다. [] )로 묶인 것은 자신이 마음대로 이름을 지정하거나, 혹은 자신의 설정에 맞는 값을 의미한다.
예를 들어:
`$ django-admin startproject [프로젝트 이름]` 을 쓰라고 하면
자기 마음대로
`$ django-admin startproject jinho` 라고 자기 마음대로 적을 수 있고,
`/home/[자신의 하위 디렉토리 명]` 이면,
자신의 기존 디렉토리 이름에 맞게 받아들여야 한다.

마지막으로..
무엇이 자신이 마음대로 바꿔도 되는 이름인지 자세한 설명은 하지 않을 것이다. (귀찮다..)

<hr/>

<h2>1. 간단한 설명 및 프로젝트 구조</h2>

우선 Django가 잘 설치되어 있는지 버전을 확인해보자
`$ python -m django --version`

확인되었다면 이제 Django프로젝트를 만들어 보자

<h4>하지만 그 전에!!</h4>

<strong>
`PHP` 를 작성해본 경험이 있는 사람은 아마 코드 전체를 `/var/www/html/`과 같은
DocumentRoot 에 두려는 경향이 있는데
Django 에서는 되도록이면 `/var/www/html/` 과 같은 곳에 만들지 말자
보통은 `/home/[유저]/` 과 같은 DocumentRoot 의 바깥에 두는 것을 권한다.
외부 사람들이 웹을 통해 Python 코드를 직접 열어볼 수 있는 위험이 있기 때문이다.
</strong>

<br>
<br>

확인 했다면 이제 만들어보자
`$ django-admin startproject [프로젝트 이름]`


만들었다면 무엇이 생성되었는지 확인해 보자.
```text
[프로젝트 이름]/
    manage.py
    [프로젝트 이름]/
        __init__.py
        settings.py
        urls.py
        wsgi.py
        asgi.py
```

<h6>각각의 파일 기능과 의미는 이번 단원의 마지막에 상세하게 나온다.</h6>

같은가??? 아마 asgi.py는 생성이 안되었을 수도 있다. (하지만 괜찮다.)

프로젝트 디렉토리로 이동한 뒤 프로젝트를 동작해보자.
`$ python manage.py runserver`

자, 이제 https://127.0.0.1:8000/ 을 통해 접속해보자.  
그러면 로켓이 발사되는 모습을 볼 수 있다.  

만약, 포트를 변경하여 접속하고 싶다면: `python manage.py runserver 0:[포트번호]` 로 실행하면 된다.

그럼 이제 프로젝트에 필요한 앱을 만들어 보자
앱 생성: `python manage.py startapp [앱 이름]`

만들어진 앱의 디렉토리를 보자

```text
[앱 이름]/
    __init__.py
    admin.py
    apps.py
    migrations/
        __init__.py
    models.py
    tests.py
    views.py
    urls.py
```

<h6>각각의 파일 기능과 의미는 바로 아래 상세하게 나온다.</h2>

<hr/>

만약 이 문서의 모든 단원을 모두 읽는다면 당신은 다음과 같은 프로젝트 구조를 가지게 될 것이다.

```text
[프로젝트 이름]/
    manage.py   # 프로젝트와 상호작용하는 커맨드라인 유틸리티
    [프로젝트 이름]/   # 프로젝트를 위한 실제 py 패키지 저장
        __init__.py   # 이 디렉토리를 패키지로 다루라고 말하는 단순한 빈 파일
        settings.py   # 프로젝트의 환경 및 구성 저장
        urls.py   # 이 프로젝트의 URL 선언 저장 (어떻게 보면 "목차 ?", "필터 ?"의 기능을 한다고 봐도 됨)
        asgi.py   # ASGI 호환 웹 서버가 프로젝트를 처리할 수 있는 진입점
        wsgi.py   # 현재 프로젝트를 서비스하기 위한 WSGI 호환 웹 서버의 진입점
    [앱 이름]/   # 앱 패키지
        __init__.py   # 이 디렉토리를 패키지로 다루라고 말하는 단순한 빈 파일
        admin.py   # 앱의 관리자 사이트에 대한 설정
        apps.py   # 앱에 대한 기본 설정 정보를 담음
        migrations/   # 데이터베이스 설정 파이를 담고 있는 디렉토리
            __init__.py   # 이 디렉토리를 패키지로 다루라고 말하는 단순한 빈 파일
            0001_initial.py   # 데이터베이스 정보를 담고있는 파일
        models.py   # 데이터 모델에 대한 정보를 정의하고 저장하는 파일, 테이블을 생성하고 테이블의 필드가 정의된 파일
        static/   # 정적 파일의 정보를 담은 파일 ex) 이미지, CSS, json 파일 등등
            [앱 이름]/   # Django가 구분하기 위해 만든 디렉토리
                images/   # 이미지 파일을 담는 디렉토리 (이미지 파일을 쓸 일이 없다면 없어도 된다.)
                    [사진].jpg   # 설명 생략
                    ...
                [css 파일이름].css   # 설명 생략
                ...
        templates/   # 프론트 작업, 페이지의 디자인이 이루어질 디렉토리
            [앱 이름]/   # Django 가 구분하기 위해 만든 디렉토리
                [html 이름1].html   # 설명 생략
                [html 이름2].html   # 설명 생략
        tests.py   # 앱을 테스트 해보기 위한 파일(QA는 중요하니깐)
        urls.py   # views 에 대한 url 설정
        views.py   # 특정 기능과 templates 를 제공하는 파일
    templates/   # 프로젝트의 템플릿 커스터마이징 디렉토리
        admin/   # Django 가 구분하기 위해 만든 디렉토리
            [...].html   # 설명 생략
```

이미 웹 프레임워크에 익숙하다면 감을 잡았을 수도 있지만 그렇지 않다면,
아직 감이 잘 오지 않을 것이다.
이제 본격적으로 알아보며 감을 잡아보자

<hr/>