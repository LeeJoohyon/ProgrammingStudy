# Django Documentation - Models (번역본)

#### 작성자- 최진수 (hello.python89_gmail.com)
이 번역본은 무단 수정 및 배포를 금합니다. 

## Model (모델)

모델은 당신의 자료에 대한 중요한 정보 소스입니다. 모델은 당신이 저장하는 자료들의 필수적인 필드와 행동(속성)들을 포함합니다. 각각의 모델은 하나의 데이터베이스 테이블에 맵핑합니다(가리킵니다).

기본들:

* 각각의 모델은 django.db.models.Model 을 상속받는 서브클래스인 파이썬의 클래스 입니다.
* 각 모델의 속성은 하나의 데이터베이스 필드를 나타냅니다.
* 상위 두 항목을 포함해서, 장고는 당신에게 자동으로 생성된 데이터베이스-접근 API를 제공합니다. 더 알고싶으시다면 <a href="https://docs.djangoproject.com/en/1.10/topics/db/queries/">쿼리 생성</a>을 보세요.

#### Example  
이 example 모델은 Person을 정의합니다. Person은 first_name(이름) 과 last_name(성)을 가집니다.

```python
from django.db import models

class Person(models.Model):
    first_name = models.CharField(max_length=30)
    last_name = models.CharField(max_length=30)
```

first\_name 과 last\_name 은 모델의 필드입니다. 각각의 필드는 클래스 속성으로 지정되고, 각각의 속성은 데이터베이스 컬럼(column)에 맵핑합니다(가리킵니다).

위 Person 모델은 아래와 같은 데이터베이스 테이블을 생성합니다.

```python
CREATE TABLE myapp_person (
    "id" serial NOT NULL PRIMARY KEY,
    "first_name" varchar(30) NOT NULL,
    "last_name" varchar(30) NOT NULL
);
```

몇가지 기술적 내용들:

* 테이블의 이름인 myapp_person 은 자동적으로 모델의 메타데이터(metadata)로 부터 불려졌고, 이름은 오버라이드 될 수 있습니다. 참고는 여기로 가보세요 <a href="https://docs.djangoproject.com/en/1.10/ref/models/options/#table-names">Table names</a>.
* id 필드는 자동으로 추가됩니다. 하지만, 이것 역시 오버라이드가 가능합니다. 참고는 여기로! <a href="https://docs.djangoproject.com/en/1.10/topics/db/models/#automatic-primary-key-fields">Automatic primary key fields</a>.
* 이 example 안에서의 CREATE TABLE SQL 은 PostgreSQL syntax(문법)으로 포맷되었습니다. 

## Using models (모델 사용하기)

모델을 만든 후에, 장고에게 당신이 만들어낸 모델들을 쓸것이라고 알려야 합니다. 과정은 이와 같습니다, models.py를 포함하고 있는 모듈의 이름을 INSTALLED_APPS 세팅을 통해 추가합니다. 이 세팅은 App 디렉토리 내의 settings.py 안에서 하시면 됩니다.

```python
INSTALLED_APPS = [
    #...
    'myapp',
    #...
]
```
새로운 App을 INSTALLED_APPS에 추가하면, manage.py migrate 명령어를 실행하는걸 잊지마세요, 이 명령어 전에 manage.py makemigrations로 마이그레이션을 생성하는것은 선택적으로 수행이 가능합니다.

## Fields (필드)

필드는 모델안에서 가장 중요하고 또한, 모델 생성시 유일하게 요구되는 부분입니다. 필드는 클래스 속성으로 지정됩니다. 필드 이름을 생성할때 **clean**, **save**, **delete**와 같은 <a href="https://docs.djangoproject.com/en/1.10/ref/models/instances/">models API</a>의 명령어와 충돌하지 않도록 조심하세요.

Example:

```python
from django.db import models

class Musician(models.Model):
    first_name = models.CharField(max_length=50)
    last_name = models.CharField(max_length=50)
    instrument = models.CharField(max_length=100)

class Album(models.Model):
    artist = models.ForeignKey(Musician, on_delete=models.CASCADE)
    name = models.CharField(max_length=100)
    release_date = models.DateField()
    num_stars = models.IntegerField()
```
-

### Field types (필드의 타입)

당신의 모델의 각 필드는 적절한 (Field)필드 클래스(class)의 인스턴스가 되어야 합니다. 장고는 필드 클래스의 타입을 아래 내용을 결정하기 위해 사용합니다.

* Column type (컬럼 타입), 이것은 어떤 형태의 데이터를 저장하는지 데이터베이스에 알려줍니다 (INTEGER, VARCHAR, TEXT)-정수, 문자, 글
* form field를 렌더링할때 사용하는 기본 HTML <a href="https://docs.djangoproject.com/en/1.10/ref/forms/widgets/">widget</a> (widget = 장고 내의 HTML input element)
* Django 관리자 안에서 요구된 최소한의 검증 

장고는 엄청나게 많은 built-in 필드 타입을 가지고 있습니다. 그 내용은 <a href="https://docs.djangoproject.com/en/1.10/ref/models/fields/#model-field-types">model field 참조</a>. 장고 built-in 필드와 충돌하지 않는한, 당신만의 필드를 쉽게 작성할 수 있습니다.

### Field options

각각의 필드는 특정한 필드-지정 arguments의 집합을 가집니다. 예를 들어, CharField(그리고, 그 자식클래스들)은 max_length라는 argument를 요구합니다. max_length는 데이터를 저장하는데 쓰이는 VARCHAR 데이터베이스 필드의 크기를 지정합니다.

모든 필드 타입에서 선택적으로 사용가능한 공동 arguments의 집합이 있습니다. <a href="https://docs.djangoproject.com/en/1.10/ref/models/fields/#common-model-field-options">참조</a>에 자세히 설명 되어있습니다. 가장 많이 사용되는 것들의 짧은 설명이 아래 제공됩니다.

**null**

True일때, 장고는 empty값을 가지는 데이터를 NULL로서 데이터베이스에 저장합니다. 기본설정은 False입니다.

**blank**

True일때, 필드가 blank을 가지는것이 허용됩니다. 기본 설정은 False입니다. 이것은 null과 다릅니다. null은 데이터베이스와 관련된것이고, blank는 검증 관련입니다. 검증을 통해 empty 값의 접근을 허용합니다. 만약 필드가 blank=False를 가진다면, 그 필드는 값이 요구됩니다.

**choices**

순회가능한 2차원 튜플을 선택으로 필드에 사용합니다. 이것이 주어진다면, 기본 form widget은 standard 문자 필드 대신에 (select box)선택 박스가 될것입니다. 그리고 주어진 choice들에 대한 선택을 제한할 것입니다.

```python
YEAR_IN_SCHOOL_CHOICES = (
    ('FR', 'Freshman'),
    ('SO', 'Sophomore'),
    ('JR', 'Junior'),
    ('SR', 'Senior'),
    ('GR', 'Graduate'),
)
```

각 튜플내의 첫번째 요소는 데이터베이스에 저장될 값들입니다. 두번째 요소는 기본 form widget을 통해서나, ModelChoiceField안에서 보여지는 것입니다. 모델 인스턴스가 주어진다면, choices field의 보여지는 값은 get\_FOO\_display() method(메서드)를 사용해 접근이 가능합니다. 예를 들어,

```python
from django.db import models

class Person(models.Model):
    SHIRT_SIZES = (
        ('S', 'Small'),
        ('M', 'Medium'),
        ('L', 'Large'),
    )
    name = models.CharField(max_length=60)
    shirt_size = models.CharField(max_length=1, choices=SHIRT_SIZES)
```
파이썬 명령문으로 실행시

```python
>>> p = Person(name="Fred Flintstone", shirt_size="L")
>>> p.save()
>>> p.shirt_size
'L'
>>> p.get_shirt_size_display()
'Large'
```

**default**

필드를 위한 기본 값입니다. 이것은 값이나 호출가능한 객체일 수 있습니다. 만약 호출가능하다면, 이것은 새로운 객체가 생성될 때마다 호출될 것입니다.

**help_text**

form widget과 함께 보여지기 위한 추가적인 "help" 글입니다. 만약 당신의 필드가 form 에서 사용되지 않을때 유용한 문서입니다.

**primary_key**

만약 True라면, 이 필드는 모델을 위한 primary key입니다. (으잉?)

만약, 당신의 모델 내부의 어느 필드를 위해 primary\_key=True 를 지정해주지 않는다면, 장고는 자동적으로 primary key를 가지기 위해 IntegerField를 추가합니다. 그런 이유로, 기본 primary-key 동작을 오버라이드 하고싶지 않다면, 당신은 primary\_key=True를 필드에서 설정해주지 않아도 됩니다. 

primary key 필드는 read-only(읽기 전용)입니다. 만약 당신이 만들어진 객체의 primary key 값을 변경하고, 저장하고 싶다면 새로운 객체는 이전 객체의 옆에 생성될 것입니다. 예를 들어,

```python
from django.db import models

class Fruit(models.Model):
    name = models.CharField(max_length=100, primary_key=True)
```

파이썬 명령어 실행결과

```python
>>> fruit = Fruit.objects.create(name='Apple')
>>> fruit.name = 'Pear'
>>> fruit.save()
>>> Fruit.objects.values_list('name', flat=True)
['Apple', 'Pear']
```

**unique**

True 라면, 이 필드는 그 테이블 내에서 고유해야 합니다. 

이 내용들은 짧게 설명된 것뿐이기에, 완벽한 디테일을 원하신다면 <a href="common model field option reference">공통 모델 필드 옵션 참조</a>를 참고하세요.


-

### 자동 primary key 필드

기본 설정으로, 장고는 각각의 모델에 다음과 같은 필드를 제공합니다.

```python
id = models.AutoField(primary_key=True)
```

이것은 자동으로 증가하는 primary key입니다.

특정한 방식의 primary key를 지정하고 싶다면, primary_key=True 를 당신의 필드중 하나에 설정해 주세요. 만약 장고가 당신이 외부적으로 Field.primary_key를 설정한 것을 보게되면, 자동으로 id column을 추가하지 않을 것입니다.

-

### Verbose 필드 이름 
(Verbose란, 필요보다 많은 단어로 표현함, by "defines verbose")

ForeignKey, ManyToManyField, 그리고 OneToOneField를 제외한 각각의 필드 타입은 verbose name이라는 선택적인 첫 위치 argument를 가집니다. 만약 verbose name이 주어지지 않는다면, 장고는 필드의 속성을 이용해 자동으로 만들어 냅니다.

아래의 example에서 verbose name은 "person's first name"입니다.

```python
first_name = models.CharField("person's first name", max_length=30)
```

아래의 example에서, verbose name은 "first name"입니다

```python
first_name = models.CharField(max_length=30)
```

ForeignKey(외부키), ManyToManyField(다-대-다 필드), 그리고 OneToOnField는 첫 argument로 모델클래스를 요구합니다. 그래서 verbose_name 키워드 argument를 사용합니다.

```python
poll = models.ForeignKey(
    Poll,
    on_delete=models.CASCADE,
    verbose_name="the related poll",
)
sites = models.ManyToManyField(Site, verbose_name="list of sites")
place = models.OneToOneField(
    Place,
    on_delete=models.CASCADE,
    verbose_name="related place",
)
``` 

verbose_name의 첫 글자를 대문자화 하지 않는것이 규칙입니다. 장고가 필요한 경우에 자동으로 대문자화 합니다.

-

### Relationships (관계)

확실하게, 관계형 데이터베이스의 힘은 테이블을 서로 관계화 하는것에서 비롯됩니다. 장고는  가장 공통적인 세가지 데이터베이스 관계를 정의 하는 방법들을 제공합니다.

#### Many-to-one 관계 (다-대-일)

Many-to-one 관계를 정의하기 위해, django.db.models.ForeignKey를 사용합니다. 이것을 당신의 모델의 클래스 속성처럼 포함시키는 작업을 통해 다른 필드 타입처럼 사용하시면 됩니다.

ForeignKey는 위치 argument(인자)를 요구합니다. 이 위치 argument는 해당 모델이 관계된 클래스 입니다.

예를 들어, Car 모델이 Manufacturer(제작사)를 가지고 있다면, 이것은 Manufacturer가 많은 수의 차를 만들수 있지만 각각의 Car는 하나의 Manufacturer를 가지고 있는 것 입니다.

```python
from django.db import models

class Manufacturer(models.Model):
    # ...
    pass

class Car(models.Model):
    manufacturer = models.ForeignKey(Manufacturer, on_delete=models.CASCADE)
    # ...
```

recursive relationship(재귀적 관계)를 만드는 것도 가능합니다. 이것은 한 객체가 다-대-일 관계를 자기 자신이 가지고 있는것??? <U>***(여기 나중에 다시 편집)***</U>

아 여기는 공부좀 하고 정리해야겠어요 죄송합니다 일루 넘어갑니다

-

### 파일을 가로지르는 모델!

다른 app에 존재하는 모델에 연결하는 작업은 완벽하게 유효합니다. 당신의 모델이 정의된 파일 맨 위에 당신의 모델에 연결하려는 외부 모델을 import(불러오기)합니다. 그 후에, 불러온 모델을 가지고 하려는 작업을 그 파일 안에서 마음껏 하시면 됩니다. 예를 들어,

```python
from django.db import models
from geography.models import ZipCode

class Restaurant(models.Model):
    # ...
    zip_code = models.ForeignKey(
        ZipCode,
        on_delete=models.SET_NULL,
        blank=True,
        null=True,
    )
```

-

### 필드 이름 제약

장고는 모델 필드 이름 설정에 오직 두가지의 제약만을 가집니다.

1. 필드 이름은 파이썬 내부로 지정된 단어를 사용할 수 없습니다. 왜냐하면, 사용할 경우 파이썬 syntax 오류를 가지기 때문이죠. 예를 들어,

```python
class Example(models.Model):
    pass = models.IntegerField() # 'pass' is a reserved word!
```

2. 필드 이름은 두개 이상의 (_)밑줄을 한번에 가질수 없습니다. 장고가 쿼리를 찾는 syntax가 동작하기 위해 설정된 제약입니다. 예를 들어,

```python
class Example(models.Model):
    foo__bar = models.IntegerField() # 'foo__bar' has two underscores!
``` 

당신의 필드 이름이 데이터베이스 column이름에 일치할 필요가 없기 때문에, 이러한 제약들은 예외가 있을수 있습니다. <a href="db_column">DB column 옵션</a>을 참고하세요.

장고가 기본 SQL 쿼리 안에 모든 테이블 이름과 컬럼 이름을 이스케이프 처리하기 때문에, SQL은 join, where, select와 같은 단어사용에 우선권이 있습니다. 

-

### 사용자 지정 필드 타입

어느 모델의 필드 하나가 당신의 목적에 맞지 않는다면, 또는 덜 공통적인 데이터베이스 컬럼 형태를 사용하고 싶다면, 당신만의 필드 클래스를 만들수 있습니다. 이 작업에 대한 모든 것은 <a href="https://docs.djangoproject.com/en/1.10/howto/custom-model-fields/">사용자 지정 모델 필드 만들기</a>에 있습니다.

-

### Meta options 

내부 클래스 Meta 를 사용해서 당신의 모델의 메타데이터를 제공합니다. 

```python
from django.db import models

class Ox(models.Model):
    horn_length = models.IntegerField()

    class Meta:
        ordering = ["horn_length"]
        verbose_name_plural = "oxen"
```

모델의 메타데이터는 순서짓는 옵션, 데이터베이스 테이블 이름, 사용자가 읽을수 있는 단수형, 복수형 이름(verbose_name, verbose_name_plural)와 같은 "필드가 아닌 것" 입니다. 이것들이 요구되진 않습니다, 그리고 메타 클래스 (class Meta)를 추가하는 작업은 완전히 선택적입니다.

사용 가능한 모든 Meta 옵션은 <a href="https://docs.djangoproject.com/en/1.10/ref/models/options/">모델 옵션 참조</a>를 참고하세요.
