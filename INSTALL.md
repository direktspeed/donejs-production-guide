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


# Creating strong dhparam
sudo openssl dhparam -out /etc/ssl/certs/dhparam.pem 2048

# letsencrypt ssl cert
sudo add-apt-repository ppa:certbot/certbot

# export MY_DOMAIN environment variable to use it later in commands.
export MY_DOMAIN=domain.tld

# get Letsencrypt SSL Cert
sudo certbot certonly --webroot --webroot-path=/var/www/html -d ${MY_DOMAIN} -d www.${MY_DOMAIN}

# Add certbot renew to crontab
sudo crontab -e
15 3 * * * /usr/bin/certbot renew --quiet --renew-hook "/bin/systemctl reload nginx"


# copy files replace domain.tld with your domain name
sed -i nginx/sites-available/default.conf.example > /etc/nginx/sites-available/default.conf
sed -i nginx/snippets/ssl-domain.tld.conf.example > /etc/nginx/snippets/ssl-${MY_DOMAIN}.conf




## Optional config if you want to force ssl only default is http + https
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
    ....
}


## Testing Verification of setip
In a web browser:
https://www.ssllabs.com/ssltest/analyze.html?d=example.com

This SSL setup should report an A+ rating.


```




## CentOs / Redhat Linux


## Docker
