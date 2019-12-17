Ingress
-------

## Nginx Ingress

### How to apply force https behind elb

helm 을 쓰는 경우로, 아래와 같은 설정을 통해 개별 ingress 에 http 에 대한 요청을 https 로 permanent redirect 를 할 수 있다. 
```yaml
ingress:
  enabled: true
  annotations:
    nginx.ingress.kubernetes.io/use-forwarded-headers: "true"
    nginx.ingress.kubernetes.io/configuration-snippet: |
      if ($http_x_forwarded_proto = 'http'){
        return 301 https://$host$request_uri;
      }
```

여기서 주의해야 할 사항으로는 nginx ingress 에서는 annotation 을 통해 backend protocol 을 http 로 지정해야 한다.
만약 TCP 로 되어 있는 경우에는 AWS ELB 인 경우에는 `X-Forwarded-Proto` header 를 넘겨 주지 않는다.
```yaml
service.beta.kubernetes.io/aws-load-balancer-internal: 0.0.0.0/0
service.beta.kubernetes.io/aws-load-balancer-connection-idle-timeout: 600
service.beta.kubernetes.io/aws-load-balancer-backend-protocol: http
service.beta.kubernetes.io/aws-load-balancer-ssl-ports: https
service.beta.kubernetes.io/aws-load-balancer-ssl-cert: arn:aws:acm:xxxxx
service.beta.kubernetes.io/aws-load-balancer-access-log-enabled: true
service.beta.kubernetes.io/aws-load-balancer-access-log-emit-interval: 5
service.beta.kubernetes.io/aws-load-balancer-access-log-s3-bucket-name: gramer.log
service.beta.kubernetes.io/aws-load-balancer-access-log-s3-bucket-prefix: gramer/elb/classic
``` 

관련 설정은 문서를 참고한다. 
- [NGINX Ingress Controller Annotations](https://kubernetes.github.io/ingress-nginx/user-guide/nginx-configuration/annotations/)
- [Kubernetes Concepts Service](https://kubernetes.io/docs/concepts/services-networking/service) 



