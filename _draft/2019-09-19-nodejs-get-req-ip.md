---
layout: post
title:  "nodejs에 접속한 클라이언트 ip 확인하기"
date:   2019-09-19 07:07:07 +0900
categories: nodejs ip
---

웹 서버나 WAS 앞에 L4 같은 Load balancers 나 Proxy server, caching server 등의 장비가 있을 경우에
웹서버는 Proxy server나 장비IP에서 접속한 것으로 인식함. 그래서 실제 클라이언트의 ip가 아닌 proxy서버 ip를 request ip로 인식하고, 
proxy server ip 로 웹로그를 남기게 됨.

이 때, express에서(다른 웹 프레임워크도 마찬가지) http request header 중에서 X-Forwarded-For(XFF)로 실제 요청한 클라이언트 ip를
알 수 있음. 

```req.headers['x-forwarded-for'] || req.connection.remoteAddress```를 사용하면 된다.

그리고 저 코드를 로그에 찍어보면 로컬컴퓨터에서 접속했다는 가정시 '::1' 로 ip가 출력되는데 이건 express 서버가 ipv6를 디폴트로 사용하기 때문임.

```app.listen(process.env.PORT || 3000, '0.0.0.0', listener)```

로 express 의 ip를 ipv4로 바꿔줄 수 있음. 그러면 ip를 로그에 찍었을 때 '127.0.0.1'가 뜨는 것을 확인할 수 있음.

[1]: https://wedul.site/520
[2]: http://blog.plura.io/?p=6597