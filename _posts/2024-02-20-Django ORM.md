---
layout: post
title: Djnago ORM
permalink: /posts/Django_ORM/
categories: [Django, Model]
---

## ORM

ORM은 Object-Relational Mapping의 약자로, 여기서 Object는 객체지향프로그래밍의 객체이며 Relational은 관계데이터베이스의 관계를 말한다.  
그러므로 ORM은 객체지향 프로그래밍(OOP)의 객체와 관계 데이터베이스(RDBMS)의 데이터를 연결하는 기술이다.  
ORM을 사용하여 개발자는 SQL을 명시적으로 작성하지 않고도, 애플리케이션의 프로그래밍 언어를 사용하여 데이터베이스를 관리할 수 있다.

ORM은 중간 계층으로 작동하여 DBMS의 종속성을 줄여준다.  
이는 DBMS 제품의 변경이 필요할 때 추가적인 구현 없이도 쉽게 대응할 수 있게 해준다.  
Django의 ORM 외에도 Flask의 SQLAlchemy, Node.js의 Sequelize 등 다양한 ORM 솔루션이 존재한다.

그러나 ORM을 사용할 때 모든 DBMS의 고급 기능을 활용할 수 있는 것은 아니다.  
복잡한 쿼리가 필요한 경우에는 직접 SQL을 작성해야 할 수도 있다.  
또한, ORM의 추상화 과정에서 비효율적인 쿼리가 생성될 위험이 있으며, 각 ORM의 문법이 다르기 때문에 새로운 ORM을 사용할 때마다 학습이 필요하다.  
이는 오히려 공통적인 SQL 쿼리문을 배우는 것이 다양한 플랫폼에서 유용할 수 있음을 시사한다.  
[참고](https://enterprisecraftsmanship.com/posts/do-you-need-an-orm)

# Django-ORM

> [https://docs.djangoproject.com/ko/5.0/topics/db/models](https://docs.djangoproject.com/ko/5.0/topics/db/models)

django manage.py의 startapp으로 생성된 app 폴더에는 기본 구성으로 models.py가 있다.  
가능하면 해당 파일로 모델 구현부를 넣는다.  
models.Model을 상속받아 필드와 관계 속성을 구현할 수 있다.

### 1:N

ForeignKey는 외래키 설정으로 1:N 관계를 사용할 때 이용한다 - ForeignKey를 적은 모델이 N이며 참조되는 쪽이 1이다.

-   참조

모델에서 참조하고 있는 모델을 선택하고 싶으면 인스턴스에서 필드 이름을 직접 사용하면 된다.  
`model.[field]`

-   역참조

반대로 참조되고 있는 모델에서 해당 모델을 참조하는 다른 모델들을 선택할 때는 set manager를 사용할 수 있다.  
이것은 [테이블명_set]으로 모델에 자동으로 생성되어 있다.  
`model.[테이블명_set].all()`  
역참조는 현재 객체를 참조하는 여러 모델들을 선택하기 때문에 복수의 객체를 반환한다.  
또는 참조하는 모델에서 ForeignKey에 related_name 인수를 줘서 [테이블명_set] 대신 접근 속성으로 사용하게 할 수 있다.

### N:M

ManyToManyField는 다대다 관계를 사용할 때 이용한다.
어느 한쪽 모델에서 ManyToManyField를 이용하여 다른 모델과 연결하면 장고 ORM은 두 모델 사이의 관계 테이블을 하나 생성하고 양쪽을 외래키로 설정하여 N:1:M 관계를 설정한다.  
through를 통해 관계 테이블을 직접 구현해 등록할 수도 있다.

```python
class Lecture(models.Model):
    users = models.ManyToManyField(User, through="Enrollment", related_name="lectures")
```

[lecture/models.py L20](https://github.com/Aivle4th-team3/Aplus-EDU/blob/main/lecture/models.py#L20)

User와 Lecture의 관계는 다대다 관계이며 Lecture에서 ManyToManyField로 User를 연결해주었다.  
관계 테이블에 추가 필드를 주기 위해 직접 구현하여 Enrollment 모델을 생성해주었고 through를 통해 연결해주었다.

Enrollment는 (user, lecture)를 유니크키로써 가지려면 Meta에 unique_together로 설정해줄 수 있다.  
이제 이 유니크키를 기본키로 설정하면... 좋겠지만 장고 ORM이 복합키를 생성하지 못한다. 그러므로 기본키는 AutoField로 숫자키를 자동 할당하고 유니크키는 테이블 제한 조건으로 설정되게 된다.

### get_absolute_url

get_absolute_url는 해당 모델 인스턴스의 url을 표현해주는 함수이다. [board/models.py L21-23](https://github.com/Aivle4th-team3/Aplus-EDU/blob/main/board/models.py#L21-23)  
이 함수의 내부 구현을 채우면 뷰나 템플릿에서 객체를 참조할 때 url을 얻을 수 있다.  
뷰에서 `redirect(post)`이나 [board/views.py L62](https://github.com/Aivle4th-team3/Aplus-EDU/blob/main/board/views.py#L62)  
템플릿에서 `<a href="{{ post.get_absolute_url }}"/>` 형태로 사용할 수 있다.
