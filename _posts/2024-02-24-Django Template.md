---
layout: post
title: Django Template
permalink: /posts/Django_Template/
categories: [Django, Template]
---

{% raw %}

## 서버 사이드 렌더링

장고 템플릿은 서버 사이드 렌더링 (Server-Side Rendering, SSR) 방식을 사용한다.

SSR은 서버에서 HTTP 요청을 받은 후 로직을 처리하여 유동적인 HTML을 생성하고 이를 클라이언트로 전송하는 방식이다.  
SSR 방식은 페이지가 서버에서 완성되어 전송되므로 검색 엔진 최적화(SEO)에 유리하며, 초기 페이지 로딩 시간을 단축시킬 수 있다.
장고 템플릿을 사용하면, 서버에서 HTML 컨텐츠를 동적으로 수정하고 클라이언트에게 전송하기 전까지 수정할 수 있다.

반면, React, Vue, Angular와 같은 클라이언트 사이드 렌더링 (Client-Side Rendering, CSR)은 HTML과 JS 파일을 클라이언트에 전송하고, 클라이언트에서 JavaScript 프레임워크가 실행되어 동적인 작업을 수행한다.  
이러한 인터렉티브한 CSR 방식은 사용자와의 상호작용을 자연스럽고 반응성을 높인다.

SSR은 CSR로 발전해오다, 최근에는 Next.js와 같은 SSG(Static Site Generation) 방식이 재조명받고 있다.

다만, SSR 방식의 장고 템플릿을 진행하고 보니 Github page와 같은 정적 사이트 호스팅에서 프론트엔드 부분을 호스팅하고, 백엔드 서버에서 데이터 페칭을 통해 페이지를 구성하는 전략이 불가능해져버렸다...

## 문법

view에서 render함수를 이용해 정적인 html과 컨텍스트를 보내 템플릿을 구성할 수 있다.  
컨텍스트 변수들은 장고 템플릿 문법으로 구성된 템플릿 태그에 사용할 수 있다.  
기본 문법은 {{ 값 }}으로 변수 값을 채울 수 있고, {% for | if | extends | include %} 등의 구문을 통해 로직을 수행할 수 있다.

### {% load static %}

load 태그는 템플릿 최상단에 위치해야 하며 탬플릿 태그나 자원들을 가져오기 사용된다.  
load static은 django.contrib.staticfiles 앱으로 작동한다. [settings.py L56](https://github.com/Aivle4th-team3/Aplus-EDU/blob/main/config/settings.py#L56)  
지정된 STATIC_URL의 경로에 이어 정적 파일들을 url로 생성하게 된다. [L269](https://github.com/Aivle4th-team3/Aplus-EDU/blob/main/config/settings.py#L269)  
정적 파일들은 배포 시에는 CDN등을 통해 따로 제공되는 경우가 생기는데 이 때 static 태그를 통해 자동으로 처리하게 된다.

### {% extends %}

템플릿을 상속하게 만들어 주는 태그를 통해 HTML 파일을 계층화할 수 있다.

자식 템플릿에서는 extends 태그를 통해 부모 템플릿을 지정하게 되며, [home/layout.html L1](https://github.com/Aivle4th-team3/Aplus-EDU/blob/main/templates/home/layout.html#L1)  
부모 템플릿에서는 block 태그를 통해 자식 템플릿에서 재정의 할 수 있는 공간을 지정해 둔다. [base.html L55-57](https://github.com/Aivle4th-team3/Aplus-EDU/blob/main/templates/home/base.py#L55-L57)

```html
<!-- 부모 템플릿-->
{% block 컨텐츠 %}{% endblock %}

<!-- 자식 템플릿 -->
{% extends "부모 템플릿.html" %} {% block 컨텐츠 %}컨텐츠{% endblock %}
```

이러한 구조로 block간 영역을 재정의 할 수 있다.

### {% include %}

include 태그는 다른 템플릿을 포함하게 된다. [layout.html L16, L23](https://github.com/Aivle4th-team3/Aplus-EDU/blob/main/templates/home/layout.html#L16)  
이 때 호출되는 템플릿은 호출하는 템플릿의 컨텍스트를 받게 된다.

### 커스텀 태그

커스텀 태그는 템플릿 태그 기능의 확장이다.  
앱 내에서 templatetags 폴더를 만들고 태그를 포함할 모듈 파일을 생성한다. [templatetags/url_utils.py](https://github.com/Aivle4th-team3/Aplus-EDU/blob/main/board/templatetags/url_utils.py)  
django.template.Library의 인스턴스를 가지고 simple_tag 데코레이터로 사용하여 구현한 함수를 태그로 등록한다. [L4-6](https://github.com/Aivle4th-team3/Aplus-EDU/blob/main/board/templatetags/url_utils.py#L4-L6)  
입력값과 현재 템플릿의 컨텍스트를 이용할 수 있어 추가 기능을 사용할 수 있게 된다. [L7](https://github.com/Aivle4th-team3/Aplus-EDU/blob/main/board/templatetags/url_utils.py#L7)

템플릿에서는 load 태그를 통해 태그 모듈을 호출한다. [board.html L3](https://github.com/Aivle4th-team3/Aplus-EDU/blob/main/templates/board/board.html#L3)  
사용시에는 모듈 내 구현된 태그명을 이용한다. [L39](https://github.com/Aivle4th-team3/Aplus-EDU/blob/main/templates/board/board.html#L39)  
함수 호출 표현식을 사용하여 첫번째 값이 태그이며 다음 값들이 인수가 된다.  
`{% current_url 'create' %}`

{% endraw %}
