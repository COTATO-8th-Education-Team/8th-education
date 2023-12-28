# CORS

## CORS 개념

### CORS란?

추가 HTTP 헤더를 사용하여, 한 출처에서 실행 중인 웹 애플리케이션이 다른 출처의 자원에 접근할 수 있는 권한을 부여하도록 브라우저에 알려주는 체제
<br>
-> HTTP 헤더에 추가적인 정보를 담아, 서버가 브라우저에게 다른 출처에서 자원을 접근할 수 있는 권한이 있음을 알려줌

> 출처 : 프로토콜, 도메인, 포트 번호

CORS 정책은 **브라우저**에 의해 실행, 서버 간의 통신에는 적용되지 않음 <br>
CORS 정책 자체는 브라우저에서 **브라우저 사용자를 보호**하기 위한 장치

### CORS가 필요한 이유

#### SOP가 생긴 이유

웹에서 실행되는 클라이언트는 **사용자 공격에 매우 취약** (개발자 도구를 사용해 DOM, JS코드를 까볼수 있음) <br>
-> 해커가 악의적으로 다른 사이트를 모방하여 사용자 정보를 탈취 가능 (ex: bank.com을 모방한 bad-bank.com가 bank.com의 서버랑 통신하면, **사용자의 정보를 열람**할 수 있고 심지어 **악의적인 요청**도 가능해짐) <br>
이러한 문제점을 해결하기 위해 **동일한 출처의 서버로만 리소스를 주고 받도록 하는 SOP**를 적용

#### SOP를 위반하는 CORS

**클라이언트와 서버가 다른 출처에서 실행**되는 환경이면, 클라이언트와 서버는 통신이 불가능 <br>
CORS를 사용해 **서버가 지정한 출처**에서 실행되는 클라이언트에게는 리소스 요청을 허용

<br>

## 동작 원리

### 기본적인 동작 방식

- 브라우저는 요청 헤더에 있는 **Origin 필드**에 요청을 보내는 출처를 담아 제출
- 서버는 클라이언트 요청에 대해서 응답 헤더에 있는 **Access-Control-Allow-Origin**이라는 값에 자원 요청 권한을 허용하는 출처를 담아 보내줌
- 브라우저는 자신이 보낸 요청의 Origin과 서버로 부터 받은 응답의 Aceess-Control-Allow_origin을 비교하고, 유효한 응답인지 결정

### Preflight Request

- 일반적으로 웹 개발시 가장 많이 발생하는 시나리오
- 브라우저는 요청을 **예비요청**(preflight)과 **본요청**으로 나누어 서버에 전송 -본요청을 보내기 이전 브라우저 스스로 이 요쳥을 보내는것이 안전한지 확인
- HTTP 메소드중 OPTIONS 메소드를 예비요청으로 사용
- 예비요청을 보내야 하면 실제 요청에 걸리는 시간이 걸리게 됨
  - 서버가 예비요청을 응답할때 Access-Control-Max-Age 헤더에 캐시될 시간을 명시해주면, 예비요청을 캐싱시켜 최적화를 가능하게 함

### Simple Request

- 대부분 preflight 방식을 사용하지만, **예비요청없이 본요청만으로 CORS 위반 여부를 검사**
- 예비요청없이 서버에게 바로 본요청을 보낸 후, 서버가 전송하는 응답헤더에 Access-Control-Allow-Origin과 같은 값을 보내주면 브라우저가 CORS 위반 여부를 검사
- 다음 조건을 충족하면 예비요청 생략 가능
  - 요청의 메소드는 GET, HEAD, POST 중 하나
  - User Agent가 자동으로 설정한 헤더를 제외하면, 아래와 같은 헤더들만 사용
    - Accept
    - Accept-Language
    - Content-Language
    - Content-Type
    - DPR
    - Downlink
    - Save-Data
    - Viewport-Width
    - Width
  - content-Type 헤더에는 아래와 같은 값들만 설정
    - application/x-www-form-urlencode
    - multipart/form-data
    - text/plain
- 현실적으로 조건을 충족하기 어려운 시나리오

### Credentialed Request

- 자바스크립트로 cross origin 요청을 보내느 경우 **기본적으로 쿠키나 HTTP 인증 같은 자격 증명(credential)이 함께 전송되지 않음** (cross origin 요청 시 작겨 증명을 함께 전송할 수 있으면 사용자 동의 없이 자바스크립트로 민감한 정보에 접근 가능)
- 서버에게 자격 증명을 전송하고 싶으면 헤더에 **명시적으로 자격 증명을 담겠다고 허용**해주어야 함
  - 클라이언트 : credentials: 'include' 또는 withCredeintals: true
  - 서버 : Access-Control-Allow-Origin : true
- 자격 증명이 함께 전송되는 요청을 보내는 경우 **Access-Control-Allow-Origin에 와일드카드(\*)사용 불가** (정확한 출처 필요)

<br>

## CORS에러 해결책

### 프론트엔드

- credentials를 포함

```JavaScript
fetch('https://example.com', {
  method: 'GET',
  // credentials: 'omit', credentials를 요청에 포함하지 않음
  // credentials: 'same-site', 기본값으로 같은 사이트에 대해서만 credentials를 포함
  credentails: 'include', // 다른 사이트라도 credentials를 포함
})
asxios.get('https://example.com', {
  withCredentials: true,
})
```

- 이거 외에는 백엔드가 해줘야 하는건데, 백엔드가 설정을 해주지 못하는 상황에서 해볼수 있는 방법
  - 프록시 서버를통해 요청
  - http-proxy-middleware 사용

### 백엔드

- Access-Control-Allow-Origin: 허용할 url
  - 헤더에 작성된 출처만 브라우저가 리소스를 접근할 수 있도록 허용
- Access-Control-Request-Methods: GET, POST, PUT, DELETE
  - 리소스 접근을 허용하는 HTTP 메소드를 지정해주는 헤더
- Access-Control-Allow-Header: Origin,Accept,X-Requested-With,Content-Type,Access-Control-Request-Method,Access-Control-Request-Headers,Authorization
  - 요청을 허용하는 헤더
- Access-Control-Max-Age: 60
  - preflight의 캐싱 기간
- Access-Control-Allow-Credentials: true
  - 자격 증명을 해야하는경우
- Access-Control-Expose-Headers: Content-Length
  - 브라우저측에서 접근할 수 있게 허용해주는 헤더를 지정
