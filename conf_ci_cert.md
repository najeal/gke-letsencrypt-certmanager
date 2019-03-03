# Configure CluterIssuer and Certificate

Create an ClusterIssuer.yaml file with the line below and deploy it with kubectl

```
apiVersion: certmanager.k8s.io/v1alpha1
kind: ClusterIssuer
metadata:
name: letsencrypt-staging
spec:
acme:
# You must replace this email address with your own.
# Let's Encrypt will use this to contact you about expiring
# certificates, and issues related to your account.
email: {YOUR-EMAIL}
server: https://acme-staging-v02.api.letsencrypt.org/directory
privateKeySecretRef:
# Secret resource used to store the account's private key.
name: example-issuer-account-key
# Enable the HTTP01 challenge mechanism for this Issuer
http01: {}
```
Note that you should write your email in the ClusterIssuer.

Create an Certificate.yaml file with the line below and deploy it with kubectl
```
kind: Certificate
apiVersion: certmanager.k8s.io/v1alpha1
metadata:
name: example-com
spec:
secretName: example-com-tls
issuerRef:
name: letsencrypt-staging
kind: ClusterIssuer
commonName: app.example.com
dnsNames:
- app.example.com
acme:
config:
- http01:
ingress: basic-ingress
domains:
- app.example.com
```

In the Certificate Ressource, we reference the Issuer previously created. Note that the **secretName example-com-tls is the secret used by the Ingress**.

If you run
```
kubectl describe ing basic-ingress
```
You should see the path **/** and the path **http://app.example.com/.well-known/acme-challenge/XXXXXXXXXXX**
The endpoint will be used to validate you own the domain.
**Note**: It can take several minutes to validate. **About 10 minutes**

During the 10 minutes, if you check the logs of your cert-manager
```
kubectl logs -n cert-manager cert-manager-xxxxxxxxx
```
You could see a lot of log like :
**could not reach 'http://app.example.com/.well-known/acme-challenge/XXXXXXXXXXXXXXXXXXXXXXX': wrong status code '404', expected '200'**

When the certifiacte is obtain and valid, running the command
```
kubectl describe certificate example-com
```
You should see **Certificate issued Successfully** in the events

Note: If you try to access your app throw SSL you will get and error because you used the staging api of Let's Encrypt
You will need to replace the api in the ClusterIssuer before it works: https://acme-v02.api.letsencrypt.org/directory

At this point your certificate need to be regenerate by their prod server. You may need to wait again.
When you get one more time the  **Certificate issued Successfully** in the events, it should work.

If you check in chrome or with curl command https://app.example.com you should not have any problem or warning.
Note: You may have to restart your Ingress

I hoped it helped !
