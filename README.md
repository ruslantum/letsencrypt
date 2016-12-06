# LetsEncrypt

Create 90 days SSL certificates for given domains.

## How it works

Container creates simple Nginx server listening on port 80 and waiting for letsencrypt validation.

It handles only requests to `mydomain.com/.well-known`, all other requests are redirected to HTTPS. 

```
server {
    listen 80;
    server_name $DOMAINS;

    location ^~ /.well-known/ {
        root /var/www/acme-certs;
    }

    location / {
        return 301 https://$server_name$request_uri;
    }
}
```

## Usage

```sh
docker run \
    -p 80:80 \ 
    -v /srv/certs/mydomain.com:/var/www/certs \
    --name le \
    -e DOMAINS='mydomain.com www.mydomain.com' \
    -e EMAIL='my@email.tld' \
    dockette/letsencrypt:latest
```

You can add `-it` for interactive shell.

After that you will have copies of certificates in your `/srv/certs/mydomain.com/` folder.

