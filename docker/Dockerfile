ARG domain

# Creating strong dhparam
RUN openssl dhparam -out /etc/ssl/certs/dhparam.pem 2048

# letsencrypt ssl cert
RUN add-apt-repository ppa:certbot/certbot

# get Letsencrypt SSL Cert
RUN certbot certonly --webroot --webroot-path=/var/www/html -d $domain -d www.$domain

# Add certbot renew on start
#/usr/bin/certbot renew --quiet --renew-hook "/bin/systemctl reload nginx"


# copy files replace domain.tld with your domain name
RUN sed -i nginx/sites-available/default.conf.example > /etc/nginx/sites-available/default.conf
RUN sed -i nginx/snippets/ssl-domain.tld.conf.example > /etc/nginx/snippets/ssl-$domain.conf
