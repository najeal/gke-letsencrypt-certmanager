# Install sample app

copy the following lines to sampleapp.yaml and deploy it using kubectl.
```
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
name: kuard
spec:
replicas: 1
template:
metadata:
labels:
app: kuard
spec:
containers:
- image: gcr.io/kuar-demo/kuard-amd64:1
imagePullPolicy: Always
name: kuard
ports:
- containerPort: 8080
---
apiVersion: v1
kind: Service
metadata:
name: kuard
spec:
type: NodePort
ports:
- port: 80
targetPort: 8080
protocol: TCP
selector:
app: kuard
```

[>> Deploy Ingress](ingress.md)
