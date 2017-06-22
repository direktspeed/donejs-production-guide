## Ubuntu / Debian Linux
```
# become root if needed
sudo su -

# Install latest stable NodeJS
apt-get update && apt-get install -y npm
npm install -g n
n stable


# Install Nginx via Packagemanager
apt-get install -y nginx

# optional adjust firewall
- iptables
- ufw
sudo ufw app list
sudo ufw allow 'Nginx FULL'


# letsencrypt ssl cert
sudo add-apt-repository ppa:certbot/certbot


sudo nano /etc/nginx/sites-available/default
location ~ /.well-known {
        allow all;
}
location / {
    proxy_pass http://localhost:8080;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection 'upgrade';
    proxy_set_header Host $host;
    proxy_cache_bypass $http_upgrade;
}

sudo certbot certonly --webroot --webroot-path=/var/www/html -d example.com -d www.example.com

sudo nano /etc/nginx/snippets/ssl-example.com.conf
ssl_certificate /etc/letsencrypt/live/example.com/fullchain.pem;
ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem;

# Creating strong dhparam
sudo openssl dhparam -out /etc/ssl/certs/dhparam.pem 2048


sudo nano /etc/nginx/snippets/ssl-params.conf
# from https://cipherli.st/
# and https://raymii.org/s/tutorials/Strong_SSL_Security_On_nginx.html

ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
ssl_prefer_server_ciphers on;
ssl_ciphers "EECDH+AESGCM:EDH+AESGCM:AES256+EECDH:AES256+EDH";
ssl_ecdh_curve secp384r1;
ssl_session_cache shared:SSL:10m;
ssl_session_tickets off;
ssl_stapling on;
ssl_stapling_verify on;
resolver 8.8.8.8 8.8.4.4 valid=300s;
resolver_timeout 5s;
# disable HSTS header for now
#add_header Strict-Transport-Security "max-age=63072000; includeSubDomains; preload";
add_header X-Frame-Options DENY;
add_header X-Content-Type-Options nosniff;

ssl_dhparam /etc/ssl/certs/dhparam.pem;

sudo nano /etc/nginx/sites-available/default
server {
    listen 80 default_server;
    listen [::]:80 default_server;
    server_name example.com www.example.com;
    return 301 https://$server_name$request_uri;
  }

## to
server {
    # SSL configuration

    listen 443 ssl http2 default_server;
    listen [::]:443 ssl http2 default_server;
    include snippets/ssl-example.com.conf;
    include snippets/ssl-params.conf;
}

In a web browser:
https://www.ssllabs.com/ssltest/analyze.html?d=example.com

This SSL setup should report an A+ rating.

sudo crontab -e
15 3 * * * /usr/bin/certbot renew --quiet --renew-hook "/bin/systemctl reload nginx"
```




## CentOs / Redhat Linux


## Docker
