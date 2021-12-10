---
title:  'Django app 만들기'
excerpt:  'Djnago를 이용해서 app을 만들어보자'
categories: 
 -Django
tags:
  - [Django,WEB]
toc: true
toc_sticky: true
date: 2021-12-11
last_modified_at: 2021-12-11
---


# Django 앱 작성

## 프로젝트 생성

> Django를 처음 사용하는 경우 초기 설정을 처리해야 합니다. 즉, 데이터베이스 구성, Django 관련 옵션 및 응용 프로그램별 설정을 포함하여 Django 인스턴스에 대한 설정 모음인 Django 프로젝트를 설정하는 일부 코드를 자동 생성해야합니다.
>
> 명령줄에서 cd코드를 저장하려는 디렉터리로 이동한 후 다음 명령을 실행합니다.
>
> ~~~django
> $ django-admin startproject mysite
> ~~~
>
> 위 코드는 mysite현재 디렉토리에 디렉토리를 생성합니다.
>
> ## 메모
>
> ~~~
> 내장 Python 또는 Django 구성 요소 이후에 프로젝트 이름을 지정하는 것을 피해야합니다. 특히, 이것은 django(Django 자체와 충돌함) 또는 test(내장된Python 패키지와 충돌함) 과 같은 이름을 사용하지 않아야 함을 의미합니다.
> ~~~
>
> ## 이 코드는 어디에 있어야 합니까?
>
> ~~~
> 배경은 일반 PHP(현대 프레임워크를 사용하지 않음)인 경우 웹 서버의 문서 루투 아래(예: /var/wwww)에 코드를 넣는 데 익숙할 것 입니다.
> Django에서는 그렇게 하지 않습니다. 이 Python 코드를 웹 서버의 문서 루트에 넣는 것은 좋은 생각이 아닙니다. 사람들이 웹을 통해 코드를 볼 수 있는 위험이 있기 때문입니다. 보안에 좋지 않습니다.
> 코드를 문서 루트 외부의 디렉터리(예: /home/mycode)
> ~~~
>
> **startproject**생성된 내용을 살펴보겠습니다.
>
> ~~~
> mysite/
> 		manage.py
> 		mysite/
> 				__init__.py
> 				settings.py
> 				urls.py
> 				wsgi.py
> ~~~
>
> 이러한 파일은 다음과 같습니다.
>
> - 외부 **mysite/**루트 디렉토리는 프로젝트의 컨테이너일 뿐입니다. 그 이름은 Django에게 중요하지 않습니다. 원하는 이름으로 변경할 수 있습니다.
> - **manage.py**: 이 Django 프로젝트와 다양한 방식으로 상호 작용할 수 있게 해주는 명령줄 유틸리티입니다. django-admin 및 manage.py에서 모든 세부 정보를 읽을 수 있습니다.
> - 내부**mysite/**디렉터리는 프로젝트의 실제 Python 패키지입니다. 그 이름은 그 안에 있는 모든 것을 가져오기 위해 사용해야 하는 Python 패키지 이름입니다.(예:**mysite.urls**)
> - **mysite/__init__.py**: 이 디렉토리가 Python 패키지로 간주되어야 함을 Python에 알리는 빈 파일입니다. Python 초보자인 경우 공식 Python 문서에서 [패키지](https://docs.python.org/3/tutorial/modules.html#tut-packages)에 대해 자세히 읽어 보세요.
> - **mysite/settings.py**: 이 Django 프로젝트의 설정/구성입니다. Django[설정](https://docs.djangoproject.com/en/2.2/topics/settings/)은 설정 작동 방식에 대한 모든 것을 알려줍니다.
> - **mysite/urls.py**: 이 Django 프로젝트에 대한 URL 선언; Django 기반 사이트의"목차"입니다. [URL 디스패처](https://docs.djangoproject.com/en/2.2/topics/http/urls/)에서 URL에 대한 자세한 내용을 읽을 수 있습니다.
> - **mysite/wsgi.py**: 프로젝트를 제공하기 위한 WSGI 호환 웹 서버의 진입점입니다. 자세한 내용은 [WSGI로 배포하는 방법을](https://docs.djangoproject.com/en/2.2/howto/deployment/wsgi/) 참조 하세요.

## 개발 서버

> - Django 프로젝트가 작동하는지 확인합시다. mysite아직 변경 하지 않은 경우 외부 디렉토리 로 변경 하고 다음 명령을 실행합니다.
>
> ~~~python
> $ python manage.py runserver
> ~~~
>
> - 명령줄에 다음 출력이 표시됩니다.
>
> ~~~
> 시스템 검사를 수행하는 중...
> 
> 시스템 검사에서 문제가 확인되지 않았습니다(0 무음).
> 
> 적용되지 않은 마이그레이션이 있습니다. 적용할 때까지 앱이 제대로 작동하지 않을 수 있습니다.
> 'python manage.py migrate'를 실행하여 적용합니다.
> 
> 2021년 7월 30일 - 15:50:53
> Django 버전 2.2, 'mysite.settings' 설정 사용http://127.0.0.1:8000/ 
> 에서 개발 서버 시작
> CONTROL-C로 서버를 종료합니다.
> ~~~
>
> 메모
>
> ~~~
> 지금은 적용되지 않은 데이터베이스 마이그레이션에 대한 경고를 무시하십니다. 우리는 곧 데이터베이스를 다룰 것입니다.
> ~~~
>
> - 순수하게 Python으로 작성된 경량 웹 서버인 Django 개발 서버를 시작했습니다. 이것을 Django에 포함시켰으므로 프로덕션 준비가 될 때가지 Apache와 같은 프로덕션 서버를 구성할 필요 없이 빠르게 개발할 수 있습니다.
>
> - 이제 주의할 좋은 시간 입니다. 프로덕션 환경과 유사한 어떤 곳에서도 이 서버를 사용 하지 마십시오. 개발하는 동안에만 사용하기 위한 것입니다.( 우리는 웹 서버가 아니라 웹 프레임워크를 만드는 사업을 하고 있습니다.)
>
> 