# CORS

이번 주 CS 교육은 다다음주 해커톤을 맞아 백과 프론트를 연결할 때 발생할 수 있는 문제인 CORS를 주제로 선정했다.

해당 글을 통해 CORS 정책이 무엇인지와 Origin에 대한 개념을 정리하겠다.

### **API 요청 예시**

우선, 아래와 같이 프론트엔드에서 백엔드로 API 요청을 보내는 경우를 생각해보자.

요청은 브라우저를 통해 백엔드 서버로 전달될 것이다.

![https://blog.kakaocdn.net/dn/b9nzCL/btsCLo3099Q/66Lg9kkWpnUZeiuQkDEAB1/img.png](https://blog.kakaocdn.net/dn/b9nzCL/btsCLo3099Q/66Lg9kkWpnUZeiuQkDEAB1/img.png)

아무런 설정 없이 요청을 보내면 아래와 같이 **CORS 정책에 의해 요청이 차단되었다.** 는 에러가 발생한다.

![https://blog.kakaocdn.net/dn/bIAA3a/btsCEJH8was/M5iaEZcyX0l9QdYktKqpY0/img.png](https://blog.kakaocdn.net/dn/bIAA3a/btsCEJH8was/M5iaEZcyX0l9QdYktKqpY0/img.png)

CORS가 무엇이길래 다음과 같은 에러가 발생할까?

CORS를 이해하기 위해선 SOP와 Origin의 개념을 먼저 이해해야한다.

### **출처 (Origin)**

웹 개념상에서 출처는 **프로토콜 + 도메인 + 포트번호** 를 합친 개념을 의미한다. 아래 일반적인 URL을 보자.

![https://blog.kakaocdn.net/dn/MX3Ig/btsCRiBvtUo/qjcCmx6mbtkqMh8QMOl3qK/img.png](https://blog.kakaocdn.net/dn/MX3Ig/btsCRiBvtUo/qjcCmx6mbtkqMh8QMOl3qK/img.png)

차례대로 프로토콜, 도메인, 포트번호, 경로, 쿼리, 프래그먼트로 분리할 수 있는데, 이 중 앞에 3가지 프로토콜 + 도메인 + 포트번호를 출처, Origin이라고 지칭한다.

예를 들어, `http://localhost:8080/api/join`과 `http://localhost:8080/api/login` 둘은 같은 출처이다. 앞에 프로토콜, 도메인, 포트번호가 동일하기 때문이다.

그러나, `https://localhost:8080/api/join`와 `http://localhost:8080/api/join`는 다른 출처이다. **프로토콜이 http와 https로 다르기 때문이다.**

따라서, 프로토콜 도메인, 포트번호 세 가지 모두 같으면 같은 출처, 하나라도 다르면 다른 출처가 된다.

## **SOP**

SOP는 Same Origin Policy의 약자로 같은 출처 정책을 의미하는 같은 출처끼리만 자원 공유를 허용하는 브라우저 정책이다. 왜 다른 출처간의 자원 공유를 허용하지 않는가?라는 생각이 들 수 있는데 다른 출처간의 자원 공유를 허용하면 악의적인 JavaScript 조작을 통해 CSRF, XSS와 같은 공격이 가능해진다.

브라우저의 JS 코드 악의적으로 수정해서 서버로 요청을 보내고 개인정보 등이 탈취될 수 있기에 브라우저는 기본적으로 r같은 출처 정책을 사용한다.

## **CORS**

Cross Origin Resourse Sharing의 약자로 `다른 출처 자원 공유`이다. 다른 출처간의 자원 공유를 **허용**하는 브라우저 정책을 의미한다.

과거 Monolithic 방식과 다르게 웹 개발의 흐름이 변화하며 백엔드와 프론트엔드를 구분하는 방식으로 개발을 하게 되었고 다른 출처간의 자원 공유가 필요해졌다. 그래서 등장한 개념이 CORS이다.

CORS 설정을 통해 자원 공유가 가능한 다른 출처를 등록하고, 해당 출처에겐 자원 공유를 허용할 수 있다.

### **CORS 동작원리**

일반적인 Http 통신과정은 아래와 같은 흐름으로 진행된다.

1. JS에서 요청을 보낸다.

2. 브라우저가 이를 서버로 전달한다.

3. 서버가 브라우저에 응답한다.

4. 브라우저가 JS에 결과를 응답한다.

![https://blog.kakaocdn.net/dn/bJuWom/btsCDdWSEvK/6TT4lTknKEVBVp0DVGNch1/img.png](https://blog.kakaocdn.net/dn/bJuWom/btsCDdWSEvK/6TT4lTknKEVBVp0DVGNch1/img.png)

이 과정으로 통신이 진행되는데, JS에서 브라우저에 fetch 요청을 보낼 때 Request Header의 Origin 필드에 자신의 출처를 적어서 요청을 보낸다.

![https://blog.kakaocdn.net/dn/cGRzZd/btsCF6QJlPR/rZoDKK5UVT7jAfpMXfImbk/img.png](https://blog.kakaocdn.net/dn/cGRzZd/btsCF6QJlPR/rZoDKK5UVT7jAfpMXfImbk/img.png)

브라우저는 해당 요청을 서버에 전달하고 서버는 브라우저에 응답으로 Response Header에 자신의 자원 출처를 허용하는 `Access-Control-Allow-Origin`를 반환한다.

![https://blog.kakaocdn.net/dn/byre5B/btsCMyk4vPJ/U2rjmv27q8U5gwSApqzos1/img.png](https://blog.kakaocdn.net/dn/byre5B/btsCMyk4vPJ/U2rjmv27q8U5gwSApqzos1/img.png)

이후 브라우저는 JS의 Origin과 서버가 요청한 Access-Control-Allow-Origin을 비교하고 CORS 정책에 맞게 같으면 정상적인 요청 처리를, 다르면 CORS 에러를 반환하게 된다.

위 경우엔 `Access-Control-Allow-Origin`이 와일드카드로 설정되었기에 CORS 에러가 발생하지 않는다.

### **CORS 시나리오**

CORS 시나리오는 아래와 같이 3가지가 존재하는데 이 글에선 Simple Request를 제외한 2가지 시나리오에 대해 설명하겠다.

1. Simple Request

2. Preflight Request

3. Credentialed Request

### Preflight Request

이름에서 Pre가 붙었듯 미리 요청을 보내는 시나리오이다. 예비요청을 먼저 보낸 후 그 이후에 본 요청을 보내는 시나리오이다.

JS에서 서버에 Http GET 메서드로 요청을 보내고 싶어한다.

브라우저는 해당 요청을 바로 서버로 전송하지 않고 `OPTION`메서드를 활용한 예비요청을 먼저 보낸다.

요청을 보내면 서버는 Response Header에 `Access-Control-Allow-Origin`을 적어 반환하게 되고, 브라우저는 이 값과 JS의 Origin을 비교해 CORS 정책에 맞게 처리를 하는 것이다.

![https://blog.kakaocdn.net/dn/bxIAu7/btsCKqVtCUV/L7uCFtL0kmNBFoLi7k9GN0/img.png](https://blog.kakaocdn.net/dn/bxIAu7/btsCKqVtCUV/L7uCFtL0kmNBFoLi7k9GN0/img.png)

두 Origin이 동일하다고 판단되면 그때 본 요청을 보내고 동일하지 않으면 CORS에러를 발생시킨다.

**본요청을 보내면 되지 예비요청을 왜 보내는가?** 의문이 들 수 있다.

이는 불필요한 리소스를 낭비하지 않기 위해서인데 가령 **JS에서 서버로 리소스가 큰 요청을 보내는 상황을 가정해보자.**

해당 요청이 Origin이 다른 요청이라면 결국 CORS 에러가 발생하게 될 요청이다. 예비요청을 보내지 않고 바로 본 요청을 보내게 되면 큰 리소스를 전달했지만 결국 처리되지 않고 에러가 발생하게 되는데 불필요한 리소스를 전달하게 되고 이는 자원 낭비를 초래하게 되기 때문이다.

또한 Preflight Request에선 Access-Control-Max-Age를 통해 초단위로 응답에 대한 캐시를 설정할 수 있다.

```
Access-Control-Max-Age: 86400
```

위와 같이 86400 초를 설정하면 24시간동안 브라우저에선 응답된 값을 캐시로 가지고 있을 수 있고, 응답 헤더의 Access-Control-Allow-Origin도 가지고 있을 수 있기에 이후의 요청 처리에 시간을 아낄 수 있다.

### Credentialed Request

Credentialed Request는 기본적으로 Preflight Request와 예비 요청을 보내는 플로우가 동일하다. 다만, 여기에 자격증명이 필요한 인증정보를 요구하는 요청인 점이 큰 차이이다.

서버 입장에선, 특정 출처에 자원 공유를 허용하지만, 인증된 사용자에게만 자원 공유를 허용하고 싶다면 Client에게 인증 정보를 요구해야한다.

이러한 요청을 하기 위해선 서버에서는 Access-Control-Allow-Credential을 True로 아래와 같이 설정할 수 있다.

```
        CorsConfiguration corsConfiguration = new CorsConfiguration();
        corsConfiguration.setAllowCredentials(true);// credentials 허용 설정
```

다음과 같이 설정을 완료하면 이제 서버에 보내는 요청에는 자격증명을 필요로한다.

따라서, JS에서 요청을 보낼때 Request Header에는 Origin뿐만 아니라 자격 증명을 위한 인증 정보로 **쿠키**를 담아서 보낸다.

![https://blog.kakaocdn.net/dn/bmHLXK/btsCOFjJPk2/r1J9AO9OqVW56VgQCz9ncK/img.png](https://blog.kakaocdn.net/dn/bmHLXK/btsCOFjJPk2/r1J9AO9OqVW56VgQCz9ncK/img.png)

이후 Preflight와 동일하게 예비요청을 보내게 되고 서버는 브라우저에 응답로 **`Access-Control-Allow-Origin`**에 허용된 출처를, **`Access-Control-Allow-Credential`**에 true라는 값을 응답헤더에 반환하게 된다.

이 때 Access-Control-Allow-Origin에 와일드카드가 아닌 명시적인 url이 지정된 것을 볼 수 있는데, Credential Request에서는 **Access-Control-Allow-Origin**에 보안상의 이유로 와일드카드가 아닌 반드시 명시적인 url을 지정해줘야한다.

또한 Client에서 요청을 보낼때에도 인증 정보를 담기위한 설정을 해줘야한다.

fetch요청의 경우: credentials를 include로 설정

![https://blog.kakaocdn.net/dn/mj1Cj/btsCMobKDbJ/sejwK6Zrls6K8XQ44NPIl1/img.png](https://blog.kakaocdn.net/dn/mj1Cj/btsCMobKDbJ/sejwK6Zrls6K8XQ44NPIl1/img.png)

axios 요청의 경우: withCredentials를 true로 설정

![https://blog.kakaocdn.net/dn/1qSV9/btsCEJalvhj/eOAn44pj5QCW3j03k3jet0/img.png](https://blog.kakaocdn.net/dn/1qSV9/btsCEJalvhj/eOAn44pj5QCW3j03k3jet0/img.png)

위와 같은 방법외에도 서버에서 해결책을 제시하지 못한다면 클라이언트쪽 해결책으로 프록시란 개념이 존재하는데 이 글에선 다루지 않도록 하겠다.

이렇게 CORS에 대해 알아봤다.

CORS는 다른 출처 간의 자원 공유를 **허용하는 브라우저 정책**이고, 출처는 **프로토콜 + 도메인 + 포트번호** 를 합친 개념이라는 것을 기억하며 글을 마무리한다.

참고자료

[https://core-research-team.github.io/2021-04-01/Easy-to-understand-Web-security-model-story-1(SOP,-CORS)](https://core-research-team.github.io/2021-04-01/Easy-to-understand-Web-security-model-story-1(SOP,-CORS))[https://developer.mozilla.org/ko/docs/Web/HTTP/CORS](https://developer.mozilla.org/ko/docs/Web/HTTP/CORS)

[https://www.youtube.com/watch?v=4iXWkBi2hCA&t=1s](https://www.youtube.com/watch?v=4iXWkBi2hCA&t=1s)