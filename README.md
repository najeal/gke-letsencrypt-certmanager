<img src="media/gke-letsencrypt-certmanager-img.png" alt="wlink logo" width="1000"/>

# gke-letsencrypt-certmanager

Using [cert-manager](https://github.com/jetstack/cert-manager) and [Let's Encrypt](https://letsencrypt.org) on [Google Kubernetes Engine](https://cloud.google.com/kubernetes-engine/).
Before starting, you must have:
- A GKE Cluster
- A Google Domain Name
- helm installed in your cluster (with tls enabled :p)

Steps:
- [configure Cloud DNS](conf_cloud_dns.md) : We will reserve a static ip usable by ingress and redirect traffic from our domain to the static ip
- [deploy sample app](sampleapp.md)
- [deploy Ingress](ingress.md)
- [install cert-manager](conf_cert-manager.md) (the example use cert-manager-v0.6.6)
- [configure CluterIssuer and Certificate](conf_ci_cert.md) ressources of cert-manger
