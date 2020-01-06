Nginx
-----

## Issue: Rewrite post data

nginx 에서 http 를 https 로 redirect 할 때, post 요청이 있는 경우 정상적으로 동작하지 않는다.
```nginx
if ( $http_x_forwarded_proto = 'http' ) {
    rewrite ^(.*) https://$host$1 permanent;
}
```

그 이유는 HTTP/1.1 Spec 에 나와 있는데, HTTP/1.0 에이전트가 GET 요청으로 오동작하는 것을 방지하기 위함이다.
 
[10.3.2 301 Moved Permanently](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.3.2)
> If the 301 status code is received in response to a request other than GET or HEAD, the user agent MUST NOT automatically redirect the request unless it can be confirmed by the user, since this might change the conditions under which the request was issued.
>
> When automatically redirecting a POST request after receiving a 301 status code, some existing HTTP/1.0 user agents will erroneously change it into a GET request.

가장 쉬운 해결책은, 요청을 https 로 요청하는 것과 nginx 에 rewrite 응답 코드를 301 -> 307 로 수정하는 것이다. 
curl -L -vvv 옵션을 주고 요청을 하면, 301 일 때에는 `Switch from POST to GET` 란 로그를 보이지만, 307일 때에는 정상 동작한다. 
