#django 모델
##객체
모델을 만들어 그 모델이 어떤 역할을 가지고 어떻게 행동해야하하는지 정의하여 서로 알아서 상호작용 할 수 있도록 만드는 것입니다.

	객체 = 객체속성(properties) +  메서드(methods)
	
###장고 모델
장고 모델이란 부가적인 메타데이터를 가진 데이터베이스의 구조를 말한다.
###어플리케이션 제작하기
	python manage.py startapp blog
이제 blog 디렉토리가 생성되고 그 안에 여러 파일들도 같이 들어있는 것을 알 수 있어요. 현재 디렉토리와 파일들은 다음과 같을 거에요. 
```
blog
├── mysite
|       __init__.py
|       settings.py
|       urls.py
|       wsgi.py
├── manage.py
└── blog
    ├── migrations
    |       __init__.py
    ├── __init__.py
    ├── admin.py
    ├── models.py
    ├── tests.py
    └── views.py
```
###블로그 글 모델 만들기
모든 Model 객체는 blog/models.py 파일에 선언하여 모델을 만듭니다. 이 파일에 우리의 블로그 글 모델도 정의할 거에요.
blog/models.py 파일을 열어서 안에 모든 내용을 삭제한 후 아래 코드를 추가하세요. 
```
from django.db import models
    from django.utils import timezone


    class Post(models.Model):
        author = models.ForeignKey('auth.User')
        title = models.CharField(max_length=200)
        text = models.TextField()
        created_date = models.DateTimeField(
                default=timezone.now)
        published_date = models.DateTimeField(
                blank=True, null=True)

        def publish(self):
            self.published_date = timezone.now()
            self.save()

        def __str__(self):
            return self.title
```
*from 또는 import로 시작하는 부분은 다른 파일에 있는 것을 추가하라는 뜻입니다. 다시 말해, 매번 다른 파일에 있는 것을 복사&붙여넣기로 해야하는 작업을 `from</0>이 대신 불러와주는 거죠 .</p>

* class Post(models.Model):는 모델을 정의하는 코드입니다. (모델은객체(object)`라고 했죠?).

* class는 특별한 키워드로, 객체를 정의한다는 것을 알려줍니다.
* Post는 모델의 이름입니다. (특수문자와 공백 제외한다면) 다른 이름을 붙일 수도 있습니다. 항상 클래스 이름의 첫 글자는 대문자로 써야 합니다.
models.Model은 Post가 장고 모델임을 의미합니다. 이 코드 때문에 장고는 Post가 데이터베이스에 저장되어야 된다고 알게 됩니다.

이제 속성을 정의하는 것에 대해서 이야기 해볼게요. : title, text, created_date, published_date, author에 대해서 말할 거에요. 속성을 정의하기 위해, 각 필드마다 어떤 종류의 데이터 타입을 가지는지를 정해야해요. 여기서 데이터 타입에는 텍스트, 숫자, 날짜, 유저 같은 다른 객체 참조 등이 있습니다.

* models.CharField - 글자 수가 제한된 텍스트를 정의할 때 사용합니다. 글 제목같이 대부분의 짧은 문자열 정보를 저장할 때 사용합니다.
* models.TextField - 글자 수에 제한이 없는 긴 텍스트를 위한 속성입니다. 블로그 콘텐츠를 담기 좋겠죠?
* models.DateTimeField - 이것은 날짜와 시간을 의미합니다.
* models.ForeignKey - 다른 모델이 대한 링크를 의미합니다.

###django에서 url 은 어떻게 작동할까요?
mysite/urls.py파일
```
from django.conf.urls import include, url
    from django.contrib import admin

    urlpatterns = [
        # Examples:
        # url(r'^$', 'mysite.views.home', name='home'),
        # url(r'^blog/', include('blog.urls')),

        url(r'^admin/', include(admin.site.urls)),
    ]
```
admin/으로 시작하는 모든 URL을 장고가 view와 대조해 찾아낸다는 뜻입니다.

###정규 표현식(Regex)
```
^ 문자열이 시작할 때
$ 문자열이 끝날 때
\d 숫자
+ 바로 앞에 나오는 항목이 계속 나올 때
() 패턴의 부분을 저장할 때
```
* ^post/는 장고에게 url 시작점에 (오른쪽부터) post/가 있다는 것을 말해 줍니다. ^)
* (\d+)는 숫자(한 개 또는 여러개) 가 있다는 뜻입니다. 내가 뽑아내고자 글 번호가 되겠지요.
* /는 장고에게 /뒤에 문자가 있음을 말해 줍니다.
* $는 URL의 끝이 방금 전에 있던 /로 끝나야 매칭될 수 있다는 것을 나타냅니다.


###html
templates 폴더에서 관리가 가능하다 

옛버전에서는 templates폴더를 mysite/blog/templates에서 관리했지만 현재에서는 templates 를 mysite 폴더로 바깥으로 빼주면서 관리하는게 용이하다고한다. 대신에 세팅을 해줘야한다. 
setting.py에서
```
TEMPLATES_DIR = os.path.join(BASE_DIR,'templates')

TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [
            TEMPLATES_DIR, #이것을 수정해주어야한다.
        ],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]

```