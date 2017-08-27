# docker-jenkins-nginx-letsencrypt

Dockerised Jenkins with SSL support using Nginx and Let's Encrypt

# Requirements

Docker and docker-compose:
* https://docs.docker.com/engine/installation/
* https://docs.docker.com/compose/install/

# Usage

Your domain `example.com` needs to be publicly resolvable and accessible from the internet.

Modify the domains inside the `docker-compose.yml`:

```
# Used by `nginx-proxy` to automatically proxy the traffic to the `nginx` docker
VIRTUAL_HOST: example.com
# Used by `letsencrypt-nginx-proxy-companion` to generate SSL certificates
LETSENCRYPT_HOST: example.com
LETSENCRYPT_EMAIL: youremail@example.com
```

Bring up the services:

```
docker-compose up
```


# Local testing

For development purposes, you could run `boulder`, the CA server behind Let's Encrypt: https://letsencrypt.readthedocs.io/en/latest/contributing.html#integration-testing-with-the-boulder-ca

Alternatively you can use ngrok.io. Their free service is sufficient to test this example.

* Register with https://ngrok.io and download `ngrok` app
* Run locally `ngrok http 80` and note `yoursubdomain.grok.io`
* Replace `example.com` inside `docker-compose.yml` with `yoursubdomain.ngrok.io` and `youremail@example.com` with your e-mail address
* Add to your `/etc/hosts` the mapping to the ngrok domain: `127.0.0.1 yoursubdomain.ngrok.io`. You need this step, otherwise your call to this domain will be routed through ngrok.io.
* Run `docker-compose up`

What happens after running this example is the following:
* Let's encrypt will generate new certificate
* It will call `yoursubdomain.ngrok.io/.well-known/acme-challenge` which will be redirected to our localhost, courtesy of ngrok.io You can confirm this behaviour when checking: http://localhost:4040/inspect/http
* Navigate to `https://yoursubdomain.ngrok.io` and setup your Jenkins. 

References:
* https://github.com/JrCs/docker-letsencrypt-nginx-proxy-companion
* https://github.com/jwilder/nginx-proxy
* https://hub.docker.com/_/nginx/
* https://hub.docker.com/r/jenkins/jenkins/
