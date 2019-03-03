# Deploy Ingress

Create an Ingress.yaml file with the line below and deploy it with kubectl

```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
name: basic-ingress
annotations:
kubernetes.io/ingress.class: gce
kubernetes.io/ingress.global-static-ip-name: {you-static-ip-name}
spec:
rules:
- host: app.example.com
http:
paths:
- path: /
backend:
serviceName: kuard
servicePort: 80
tls:
- hosts:
- app.example.com
secretName: example-com-tls
```

after one minute. Run
```
kubectl get ing
```

You should see your static ip in the ADDRESS column.

try to access you app through chrome or curl command curl http://app.example.com without HTTPS. You should succeed.

[>> Configure CluterIssuer and Certificate](conf_cert-manager.md)
