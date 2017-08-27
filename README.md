# docker-jenkins-nginx-letsencrypt
Dockerised Jenkins with SSL support using nginx and letsencrypt

# Requirements

Docker and docker-compose:
* https://docs.docker.com/engine/installation/
* https://docs.docker.com/compose/install/

# Usage

Modify the domains inside the docker-compose:

```
# Used by nginx-proxy to automatically proxy the traffix to nginx for the following domain
VIRTUAL_HOST: example.com
VIRTUAL_PORT: 8088
# Used by letsencrypt-nginx-proxy-companion to generate SSL certificates
LETSENCRYPT_HOST: example.com
LETSENCRYPT_EMAIL: youremail@example.com
```

Bring up the services:

```
docker-compose up
```


# Local testing

To test this your service `example.com` needs to be accessible from the internet on a given DNS.
Alternatively you can use ngrok.io. Their free service is sufficient for this example.

* Register with ngrok.io
* Run: `ngrok http 80` and note `yoursubdomain.grok.io`, for example `cebfc0a7.ngrok.io`
* Replace `example.com` inside docker-compose.yml with `yoursubdomain.ngrok.io` and `youremail@example.com` with your e-mail address
* Add to your `/etc/hosts` the domain: `127.0.0.1 yoursubdomain.ngrok.io`
* Run `docker-compose up`

What happens after running this example is the following:
* Let's encrypt will generate the new certificate
* It will call `yoursubdomain.ngrok.io/.well-known/acme-challenge` which will be redirected to your localhost port 80, curtosy of ngrok.io 
* Then just navigate to `https://yoursubdomain.ngrok.io` and setup your Jenkins secured with the SSL certificates automatically generates with Let's Encrypt.
