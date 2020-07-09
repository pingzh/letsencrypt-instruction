
### How to Set Up Free SSL Certificates from Let's Encrypt using Docker and Nginx


Follow this doc: https://www.humankode.com/ssl/how-to-set-up-free-ssl-certificates-from-lets-encrypt-using-docker-and-nginx


1. `mkdir -p /docker/letsencrypt-docker-nginx/src/ && cd /docker/letsencrypt-docker-nginx/src/`

2. `git clone https://github.com/pingzh/letsencrypt-instruction.git letsencrypt`

3. Update the `TODO` in the `nginx.conf`


4. Start the `nginx`
```
cd /docker/letsencrypt-docker-nginx/src/letsencrypt
sudo docker-compose up -d
```

5. get a staging cert, TODO: change the `example.com` and/or `api.example.com`
   to your domains

```
sudo docker run -it --rm \
-v /docker-volumes/etc/letsencrypt:/etc/letsencrypt \
-v /docker-volumes/var/lib/letsencrypt:/var/lib/letsencrypt \
-v /docker/letsencrypt-docker-nginx/src/letsencrypt/letsencrypt-site:/data/letsencrypt \
-v "/docker-volumes/var/log/letsencrypt:/var/log/letsencrypt" \
certbot/certbot \
certonly --webroot \
--register-unsafely-without-email --agree-tos \
--webroot-path=/data/letsencrypt \
--staging \
-d example.com \
-d api.example.com
```

6. get a production cert, TODO: update `youremail@domain.com` and change the `example.com` and/or `api.example.com`
   [OR] Renew your cert

```
sudo rm -rf /docker-volumes/

sudo docker run -it --rm \
-v /docker-volumes/etc/letsencrypt:/etc/letsencrypt \
-v /docker-volumes/var/lib/letsencrypt:/var/lib/letsencrypt \
-v /docker/letsencrypt-docker-nginx/src/letsencrypt/letsencrypt-site:/data/letsencrypt \
-v "/docker-volumes/var/log/letsencrypt:/var/log/letsencrypt" \
certbot/certbot \
certonly --webroot \
--email youremail@domain.com --agree-tos --no-eff-email \
--webroot-path=/data/letsencrypt \
-d example.com \
-d api.example.com
```

7. Stop the nginx:

```
cd /docker/letsencrypt-docker-nginx/src/letsencrypt

sudo docker-compose down
```
