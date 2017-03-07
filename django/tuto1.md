#django tutorials1

##프로젝트 만들기
```
*django-admin startproject mysite*
이 명령은 현재 디렉토리에서 mysite 라는 디렉토리를 생성할 것입니다.
```
project 를 생성할 때, Python 이나 Django 에서 사용중인 이름은 피해야 합니다. 특히, django (Django 그 자체와 충돌이 일어납니다) 나, test (Python 패키지의 이름중 하나입니다) 같은 이름은 피해야 한다는 의미입니다.
```
mysite/
    manage.py
    mysite/
        __init__.py
        settings.py
        urls.py
        wsgi.py

```
이 파일들은,

* mysite/ 디렉토리 바깥의 디렉토리는 단순히 프로젝트를 담는 공간입니다. 이 이름은 Django 와 아무 상관이 없으니, 원하는 이름으로 변경하셔도 됩니다.

* manage.py: Django 프로젝트와 다양한 방법으로 상호작용 하는 커맨드라인의 유틸리티 입니다. manage.py 에 대한 자세한 정보는 django-admin and manage.py 에서 확인할 수 있습니다.

* mysite/ 디렉토리 내부에는 project 를 위한 실제 Python 패키지들이 저장됩니다. 이 디렉토리 내의 이름을 이용하여, (mysite.urls 와 같은 식으로) project 어디서나 Python 패키지들을 import 할 수 있습니다.

* mysite/__init__.py: Python 으로 하여금 이 디렉토리를 패키지 처럼 다루라고 알려주는 용도의 단순한 빈 파일입니다. Python 초심자라면, Python 공식 홈페이지의 more about packages 를 읽어보십시요.

* mysite/settings.py: 현재 Django project 의 환경/구성을 저장합니다. Django settings 에서 환경 설정이 어떻게 동작하는지 확인할 수 있습니다.

* mysite/urls.py: 현재 Django project 의 URL 선언을 저장합니다. Django 로 작성된 사이트의 “목차” 라고 할 수 있습니다. URL dispatcher 에서 URL 에 대한 자세한 내용을 읽어보세요.

* mysite/wsgi.py: 현재 project 를 서비스 하기 위한 WSGI 호환 웹 서버의 진입점 입니다. How to deploy with WSGI 를 읽어보세요.

###개발서버 
```
python manage.py runserver
```
Django 개발 서버를 시작했습니다. 개발 서버는 순수 Python 으로 작성된 경량 웹 서버입니다. Django 에 포함되어 있기 때문에 아무 설정없이 바로 개발에 사용할 수 있습니다.

이쯤에서 하나 기억할 것이 있습니다. 절대로 개발 서버를 운영 환경에서 사용하지 마십시요. 개발 서버는 오직 개발 목적으로만 사용하여야 합니다. (우리는 웹 프레임워크를 만들지 웹 서버를 만들지는 않거든요)

이제 서버가 동작하기 시작했으니, 웹 브라우져에서 http://127.0.0.1:8000/ 를 통해 접속할 수있습니다. 파스텔톤의 밝은 파란색의 따뜻한 “Welcome to Django” 환영 페이지를 볼 수 있을 것입니다. 잘 동작 하네요!

####url() 인수: regex
“regex” 는 보통 정규 표현식(“Regular Expression”) 을 짧게 줄여 쓰는 표현입니다. 문자열의 패턴을 일치시키는 문법을 말하며, 이 경우에는 url 의 패턴을 찾아내는데 사용되었습니다. Django 에서는 목록의 첫번째 정규표현식부터 시작해서, 요청된 URL 에 대하여 일치하는 정규 표현식이 발견 될때까지 차례로 비교합니다.

이 정규 표현식 들은 GET 이나 POST 의 매개변수나, 도메인 이름을 뒤지지는 않습니다. 예를 들어, https://www.example.com/myapp/ 가 요청된 경우, URLconf 는 오직 myapp/ 부분만 바라봅니다. https://www.example.com/myapp/?page=3 같은 요청에도, URLconf 는 역시 myapp/ 부분만 신경씁니다.

정규 표현식에 대해 도움이 필요하다면, Wikipedia’s entry 를 참고하시거나 re 모듈의 문서를 참고해 주십시요. 특히 Jeffrey Friedl 가 쓴 오라일리 출판의 “정규표현식 완전 해부와 실습”(“Mastering Regular Expressions”) 는 완벽한 참고서입니다.그러나 현실적으로 이 모든 내용을 전부 알아야 할 필요는 없습니다. 딱 필요한 만큼, 간단한 패턴을 잡아낼 정도만 알면 됩니다. 사실, 복잡한 정규표현식을 사용하면 검색 속도가 아주 느려지므로, 전적으로 정규 표현식에 의존하지 않아야 합니다.

마지막으로 성능에 관해 알아야 할 사실은, 이런 정규 표현식들은 URLconf 모듈이 처음 불러올 때 자동으로 컴파일 되기 때문에 엄청나게 빠르다는 것입니다. 물론 앞서 언급했듯이 복잡한 검색을 쓰지 않는 한 말이죠.

####url() 인수: view
Django 에서 일치하는 정규 표현식을 찾아내면, HttpRequest 객체를 첫번째 인수로 하고, 정규표현식에서 “잡힌” 값들을 나머지 인수로 하여 특정한 view 함수를 호출합니다. 만약 정규표현식이 간단한 형식이라면, 잡힌 값들은 단순히 순서 기반의 인수로서 함수에 넘겨집니다. 만약 이름 기반의 정규표현식이라면, 잡힌 값들은 키워드 인수들로 함수에 넘겨집니다. 나중에 이에 대한 간단한 예제를 살펴보겠습니다.

####url() 인수: kwargs
임의의 키워드 인수들은 목표한 view 에 사전형으로 전달됩니다. 그러나 이 튜토리얼에서는 사용하지 않을겁니다.

####url() 인수: name
URL 에 이름을 지으면, 템플릿을 포함한 Django 어디에서나 명확하게 참조할 수 있습니다. 이 강력한 기능을 이용하여, 단 하나의 파일만 수정해도 project 내의 모든 URL 패턴을 바꿀 수 있도록 도와줍니다.
