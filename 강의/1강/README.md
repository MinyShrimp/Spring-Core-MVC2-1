# 타임리프 - 기본 기능
## 프로젝트 생성
`src/main/resources/static/index.html`
```html
<html>
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<ul>
    <li>텍스트
        <ul>
            <li><a href="/basic/text-basic">텍스트 출력 기본</a></li>
            <li><a href="/basic/text-unescaped">텍스트 text, utext</a></li>
        </ul>
    </li>
    <li>표준 표현식 구문
        <ul>
            <li><a href="/basic/variable">변수 - SpringEL</a></li>
            <li><a href="/basic/basic-objects?paramData=HelloParam">기본 객체들</
                a></li>
            <li><a href="/basic/date">유틸리티 객체와 날짜</a></li>
            <li><a href="/basic/link">링크 URL</a></li>
            <li><a href="/basic/literal">리터럴</a></li>
            <li><a href="/basic/operation">연산</a></li>
        </ul>
    </li>
    <li>속성 값 설정
        <ul>
            <li><a href="/basic/attribute">속성 값 설정</a></li>
        </ul>
    </li>
    <li>반복
        <ul>
            <li><a href="/basic/each">반복</a></li>
        </ul>
    </li>
    <li>조건부 평가
        <ul>
            <li><a href="/basic/condition">조건부 평가</a></li>
        </ul>
    </li>
    <li>주석 및 블록
        <ul>
            <li><a href="/basic/comments">주석</a></li>
            <li><a href="/basic/block">블록</a></li>
        </ul>
    </li>
    <li>자바스크립트 인라인
        <ul>
            <li><a href="/basic/javascript">자바스크립트 인라인</a></li>
        </ul>
    </li>
    <li>템플릿 레이아웃
        <ul>
            <li><a href="/template/fragment">템플릿 조각</a></li>
            <li><a href="/template/layout">유연한 레이아웃</a></li>
            <li><a href="/template/layoutExtend">레이아웃 상속</a></li>
        </ul>
    </li>
</ul>
</body>
</html>
```

## 타임리프 소개
* 공식 사이트: https://www.thymeleaf.org/
* 공식 메뉴얼 - 기본 기능: https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html
* 공식 메뉴얼 - 스프링 통합: https://www.thymeleaf.org/doc/tutorials/3.0/thymeleafspring.html

타임 리프의 개념은 간단히 소개하고, 실제 동작하는 기본 기능 위주로 알아보겠다.

### 타임리프 특징
* 서버 사이드 HTML 렌더링 (SSR)
* 네튜얼 템플릿
* 스프링 통합 지원

### 서버 사이드 HTML 렌더링 (SSR)
타임리프는 백엔드 서버에서 HTML을 동적으로 렌더링하는 용도로 사용된다.

### 네츄럴 템플릿
타임리프는 순수 HTML을 최대한 유지하는 특징이 있다.
타임리프로 작성한 파일은 HTML을 유지하기 때문에 웹 브라우저에서 파일을 직접 열어도 내용을 확인할 수 있고,
서버를 통해 뷰 템플릿을 거치면 동적으로 변경된 결과를 확인할 수 있다.

### 스프링 통합 지원
타임리프는 스프링과 자연스럽게 통합되고, 스프링의 다양한 기능을 편리하게 사용할 수 있게 지원한다.

## 텍스트 - text, utext
### 타임리프 사용 선언
`<html xmlns:th="http://www.thymeleaf.org">`

타임리프는 기본적으로 HTML 태그의 속성에 기능을 정의해서 동작한다.
`<span th:text="${data}>`

### BasicController
```java
@Controller
@RequestMapping("/basic")
public class BasicController {
    @GetMapping("text-basic")
    public String textBasic(Model model) {
        model.addAttribute("data", "Hello Spring");
        return "basic/text-basic";
    }
}
```

### text-basic.html
```html
<!DOCTYPE html>
<html lang="ko" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <h1>컨텐츠에 데이터 출력하기</h1>
    <ul>
        <li>th:text 사용 <span th:text="${data}"></span> </li>
        <li>컨텐츠 안에서 직접 출력하기 = [[${data}]]</li>
    </ul>
</body>
</html>
```

![img_1.png](img_1.png)

### Escape
HTML 문서는 `<>` 같은 특수 문자를 기반으로 정의된다.
따라서 뷰 템플릿으로 HTML 화면을 생성할 때는 출력하는 데이터에 이러한 특수 문자가 있는 것을 주의해서 사용해야 한다.

웹 브라우저는 `<` 를 HTML 태그의 시작으로 인식한다.
따라서 `<`를 문자로 표현하기 위해서, 별개의 특수 문자를 HTML 엔티티로 변경하는 작업이 필요하다.

타임리프가 제공하는 `th:text`, `[[...]]`는 기본적으로 Escape로 동작한다.

### Unescape
타임리프가 Escape 기능을 사용하지 않게 하려면 아래의 코드를 작성하면 된다.
* `th:text` -> `th:utext`
* `[[...]]` -> `[(...)]`

### BasicController
```java
@Controller
@RequestMapping("/basic")
public class BasicController {
    @GetMapping("text-unescaped")
    public String textUnescaped(Model model) {
        model.addAttribute("data", "Hello <b>Spring</b>");
        return "basic/text-unescape";
    }
}
```

### text-unescape.html
```html
<!DOCTYPE html>
<html lang="ko" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1>text vs utext</h1>
<ul>
    <li>th:text = <span th:text="${data}"></span></li>
    <li>th:utext = <span th:utext="${data}"></span></li>
</ul>

<h1><span th:inline="none"> [[...]] vs [(...)] </span></h1>
<ul>
    <li><span th:inline="none">[[...]] = </span> [[${data}]]</li>
    <li><span th:inline="none">[(...)] = </span> [(${data})] </li>
</ul>
</body>
</html>
```
* `th:inline="none"`: 해당 태그 안의 글자는 타임리프가 해석하지 말라는 뜻이다. 

![img.png](img.png)

> 주의!<br>
> 실제 서비스를 개발하다 보면 escape를 사용하지 않아서 HTML이 정상 렌더링 되지 않는 수 많은 문제가 발생한다.
> escape를 기본으로 하고, 꼭 필요할 때만 unescape를 사용하자.


## 변수 - SpringEL

## 기본 객체들

## 유틸리티 객체와 날짜

## URL 링크

## 리터럴

## 연산

## 속성 값 설정

## 반복

## 조건부 평가

## 주석

## 블록

## 자바스크립트 인라인

## 템플릿 조각

## 템플릿 레이아웃 1

## 템플릿 레이아웃 2

## 정리

