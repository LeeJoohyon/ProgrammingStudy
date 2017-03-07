#장고 정리
####unique_together
함께 사용되는 고유 한 필드 이름 세트 :

	unique_together = (("driver", "restaurant"),)

이것은 함께 고려할 때 고유해야하는 튜플의 튜플입니다. Django 관리자에서 사용되며 데이터베이스 수준에서 적용됩니다 (즉, 적절한 UNIQUE 문이 CREATE TABLE 문에 포함됩니다). 편의를 위해 unique_together는 단일 필드 집합을 처리 할 때 단일 튜플이 될 수 있습니다.

	unique_together = ("driver", "restaurant")

ManyToManyField는 unique_together에 포함될 수 없습니다. (그것이 의미하는 바는 분명하지 않습니다!) ManyToManyField와 관련된 고유성의 유효성을 검사해야하는 경우 신호 또는 모델을 통한 명시 적 모델을 사용해보십시오. 제약 조건 위반시 모델 유효성 검사 중에 발생하는 ValidationError는 unique_together 오류 코드를 갖습니다.