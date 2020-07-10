# Amazon API Gateway Ingress Controller

## Getting Started

Create an ECR repository.

To build and deploy the controller into ECR.

```sh

aws ecr get-login-password --region {aws-region} | docker login --username AWS --password-stdin {accountID}.dkr.ecr.{aws-region}.amazonaws.com

docker build -t apigcontroller .

docker tag apigcontroller:latest {accountID}.dkr.ecr.{aws-region}.amazonaws.com/apigcontroller:latest

docker push {accountID}.dkr.ecr.{aws-region}.amazonaws.com/apigcontroller:latest

```



## Example

```yaml
---
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: foobar-ingress
  annotations:
    kubernetes.io/ingress.class: apigateway
    apigateway.ingress.kubernetes.io/stage-name: prod
    apigateway.ingress.kubernetes.io/client-arns: arn::foo,arn::bar
    apigateway.ingress.kubernetes.io/nginx-replicas: "3"
    apigateway.ingress.kubernetes.io/nginx-image: nginx:latest
    apigateway.ingress.kubernetes.io/nginx-service-port: "9090"
spec:
  rules:
    - http:
        paths:
        - backend:
            serviceName: foo-service
            servicePort: 8080
          path: /api/v1/foo
        - backend:
            serviceName: bar-service
            servicePort: 8080
          path: /api/v1/bar
```
