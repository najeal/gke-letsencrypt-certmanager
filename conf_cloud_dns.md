# Configure Cloud DNS

Assuming you are in possession of the domain example.com.
You want to access your app running on GKE through app.example.com

Starting by reserving a static ip. Gcloud will reserve a random ip for you. You will can use it with the name you gave to it.
```
gcloud compute addresses create {name-you-give-to-ip} --global
```

Get the reserved ip running the following command
```
gcloud compute addresses list
```
Go to Google CLoud Platform => Cloud DNS service => Create a zone => add a record-set (Type A) redirecting the traffic from app.example.com to you static ip.

Next you will need to update your Domain name servers
You will need the name of the zone created above.
```
gcloud dns managed-zones describe {zone-name}
```

Copy the four name servers at the end of the response.
Go to https://domains.google/ to manage your domain => go to DNS => check 'user custom name servers' and Paste the four name servers you previously copied from the command.

Run

```
host app.example.com
```
It should display you static ip

[>>Install cert-manager](sampleapp.md)
