---
layout: post
title: CORS 란?
subtitle: "cross origin resource sharing"
type: "WEB"
createDate: 2021-02-22
til: true
text: true
author: "codeFabian"
post-header: false
header-img: ""
order: 15
---

# CORS 와 SOP

## 개요

---

**CORS** (Cross-Origin Resource Sharing), "**교차 출처 리소스 공유**" 개념을 이해하기에 앞서
**SOP** (Same-Origin Policy) "**동일 출처 정책**" 의 개념부터 이해하고 넘어가는 것이, CORS가 등장한 배경에 대해 이해하는것 것에 도움이 되었습니다.

<br>

## SOP (Same-Origin Policy) 과 CORS 등장배경

---

동일 출처 정책이란, 어떤 출처에서 불러온 문서 또는 스크립트가 다른 **출처**에서 가져온 리소스와 상호작용하는 것을 제한하는 보안 방식입니다. 이를 통해 외부로부터의 공격과 잠재적으로 위험이 있는 자원들을 사전에 차단하기 위해 다른 출처에서 가져온 자원을 사용하지 못하게 제한 하는 것 입니다.

그렇다면 "출처" 라는 것은 어떤 것인지를 알아야 할 필요가 있습니다.

출처란, 서로 상호작용을 하고자 하는 두 URL의 **프로토콜**, **포트**, **호스트가** 모두 같아야 동일한 출처를 뜻합니다.

```jsx
프로토콜(Protocol)://호스트[:포트]/디렉토리명/파일명

URL: http://store.company.com/dir/page.html

URL                                     | 결과 | 이유
-----------------------------------------------------------
http://store.company.com/dir/page.html  | 성공 | 경로만 다름
-----------------------------------------------------------
https://store.company.com/secure.html   | 실패 | 프로토콜 다름
-----------------------------------------------------------
http://store.company.com:81/dir/etc.html| 실패 | 포트가 다름 (default: 80)
-----------------------------------------------------------
http://news.company.com/dir/other.html  | 실패 | 호스트가 다름
```

따라서 이렇게 출처들이 같지 않다면, 리소스 요청을 거절하는 방식이였는데,
**AJAX가 널리 사용되면서 <script></script> 로 둘려 쌓여있는 스크립트에서 생성되는 XMLHttpRequest 에 대해서도 Cross-Site HTTP Request 가 가능 해야한다는 요구가 늘어나자 W3C 에서 CORS 라는 이름의 권고안이 등장한 것 입니다.**

(이를 우회할 수 있는 방법으로는 **JSONP** 라는 방법이 있었다는 것만 알고 넘어가도록 하였습니다)

<br>

## 정리하기

---

**HTTP 요청은 기본적으로 Cross-Site HTTP Request 가 가능합니다.**
<Img> 태그로 다른 도메인의 이미지 파일을 가져오거나,

<Link> 태그로 다른 도메인의 CSS를 가져오거나,
<script> 태그로 다른 도메인의 Javascript 라이브러리를 가져오는 것이 가능하다는 뜻 입니다.

**하지만 <script></script>로 둘려쌓인 스크립트에서 생성된 Cross-Site HTTP Request 는
Same-Origin Policy 를 적용받기 때문에 Cross-Site HTTP Request 이 불가능했고, 이를 위해
CORS가 등장한 것 입니다.**

<br>

## CORS 란?

---

**CORS**(Cross-Origin Resource Sharing)란 교차 출처 리소스 공유라는 뜻으로,

**서로 다른 Origin 간의 HTTP Request 가 가능하도록 해주는 표준입니다.**

이는, `<img src="test-domain.com">`, `<script src="test-domain.com/script.js">`와 같이 다른 도메인에 존재하는 리소스(이미지, 스크립트 등)를 참조 할 수 있음을 뜻 합니다.`

---

<br>

## CORS 요청의 종류

---

CORS 는 다음과 같은 4가지가 존재합니다.
브라우저가 요청 내용을 분석하여 4가지 방식 중 해당하는 방식으로 서버에 요청을 날리므로, 개발자가 목적에 맞는 방식을 선택하고 그 조건에 맞게 코드를 작성 해야합니다.

- **Simple Requests**
- **Preflighted Requests**
- **Requests with Credentials**
- **Request without Credentials**

<br>

### Simple Requests

---

아래의 3가지 조건을 만족하면 간단한 요청에 해당합니다.

- GET, POST, HEAD 중 한 가지 방식을 사용해야 한다.
- POST 방식일 경우, Content-type이 아래 중 하나여야 한다.
  - application/x-www-form-urlencoded
  - multipart/form-data
  - text/plain
- 커스텀 헤더를 전송하지 말아야 한다.

Simple Requests는 서버에 1번 요청하고, 서버도 1번 회신하는 것으로 종료됩니다.

<br>

### **Preflighted Requests**

---

**Simple Request** 조건에 해당하지 않으면 브라우저는 **Prefliget Request** 방식으로 요청합니다.

따라서, Preflight Request는

- GET, HEAD, POST 외의 다른 방식으로도 요청을 보낼 수 있고,
- application/xml 처럼 다른 Content-Type으로도 요청을 보낼 수 있습니다.
- 커스텀 헤더도 사용이 가능합니다.

**Preflighted Requests** 는 OPTIONS 메서드를 통해 다른 도메인의 리소스로 사전에 HTTP 요청을 보내 안전한지 확인합니다.

즉, 먼저 서버에 사전요청 (Preflighted Requests) 을 보내고 서버는 사전요청에 대해 응답하고,

그 다음 본 요청을 서버에 보내고, 서버도 본 요청에 응답하는 방식입니다.

하지만, 사전요청과 본 요청은 개발자가 직접 구현하는 것이 아니라 Access-Control 계열의 Response Header만 적절히 정해주면 OPTIONS 요청으로 오는 사전요청과 GET, POST, HEAD, PUT, DELETE 등으로 오는 본 요청의 처리는 서버가 알아서 처리합니다.

<br>

### **Requests with Credentials**

---

HTTP Cookie와 HTTP Authentication 정보를 인식할 수 있게 해주는 요청입니다.

클라이언트는 요청 생성 시 반드시 withCredentials = true;로 지정하여 credential 요청을 보낼 수 있고,

서버에서는 Access-Control-Allow-Credentials: true로 지정해야 합니다.

또한, Access-Control-Allow-Origin 속성을 요청한 도메인으로 지정해야합니다.

- 와 같은 와일드 카드는 사용 할 수 없습니다, http://test.origin 같은 구체적인 도메인이 와야 합니다.

<br>

### **Request without Credential**

---

CORS 요청은 기본적으로 Non-Credential 요청이므로, withCredentials = true 를 지정하지 않으면 Non-Credential 요청입니다.

<br>

## HTTP 응답헤더

---

- Access-Control-Allow-Origin
  - 브라우저가 해당 origin이 자원에 접근할 수 있도록 허용합니다. "\*" 와일드 카드는 브라우저의 origin에 상관없이 모든 리소스에 접근하도록 허용합니다.
- Access-Control-Expose-Headers
  - 브라우저가 액세스할 수있는 서버 화이트리스트 헤더를 허용합니다.
- Access-Control-Max-Age
  - 얼마나 오랫동안 `preflight`요청이 캐싱 될 수 있는지를 나타낸다.
- Access-Control-Allow-Credentials
  - `Credentials`가 true 일 때 요청에 대한 응답이 노출될 수 있는지를 나타냅니다.
  - `preflight`요청에 대한 응답의 일부로 사용되는 경우 실제 자격 증명을 사용하여 실제 요청을 수행 할 수 있는지를 나타냅니다.
  - 간단한 GET 요청은 `preflight`되지 않으므로 자격 증명이 있는 리소스를 요청하면 헤더가 리소스와 함께 반환되지 않으면 브라우저에서 응답을 무시하고 웹 콘텐츠로 반환하지 않습니다.
- Access-Control-Allow-Methods
  - preflight`요청에 대한 대한 응답으로 허용되는 메서드들을 나타냅니다.
- Access-Control-Allow-Headers
  - `preflight`요청에 대한 대한 응답으로 실제 요청 시 사용할 수 있는 HTTP 헤더를 나타냅니다.

<br>

## HTTP 요청헤더

---

- Origin
- Access-Control-Request-Method
  - `preflight`요청을 할 때 실제 요청에서 어떤 메서드를 사용할 것인지 서버에게 알리기 위해 사용됩니다.
- Access-Control-Request-Headers
  - `preflight`요청을 할 때 실제 요청에서 어떤 header를 사용할 것인지 서버에게 알리기 위해 사용됩니다.
