FROM php:5.6-apache
MAINTAINER Eric Fehr ricofehr@nextdeploy.io
LABEL description="Nextdeploy apache-php container"

# apache vhost
COPY 000-default.conf /etc/apache2/sites-available/

# Upgrade system
RUN /bin/sed -i "s;jessie main;jessie main contrib non-free;" /etc/apt/sources.list
RUN DEBIAN_FRONTEND=noninteractive apt-get update -y --force-yes
RUN DEBIAN_FRONTEND=noninteractive apt-get dist-upgrade -y --force-yes

# Install prerequis
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y wget curl git sudo libxrender-dev libfontconfig1-dev libxext-dev mysql-client php-pear

# Install libraries for php extensions
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y libpng12-dev libbz2-dev libjpeg62-turbo-dev  libjpeg-dev libpq-dev libcurl4-openssl-dev libldap2-dev libicu-dev libxml2-dev libmcrypt-dev libxslt-dev libtidy-dev libfreetype6-dev

# Install php extensions
RUN DEBIAN_FRONTEND=noninteractive ln -fs /usr/lib/x86_64-linux-gnu/libldap.so /usr/lib/ \
&& docker-php-ext-configure gd --with-png-dir=/usr --with-jpeg-dir=/usr \
&& docker-php-ext-install gd mbstring pdo pdo_mysql \
&& docker-php-ext-install curl ldap mcrypt xmlrpc intl xsl tidy \
&& rm -r /var/lib/apt/lists/*

# Enable rewrite module
RUN a2enmod rewrite alias autoindex deflate expires headers include mime

# Suppose current host user has uid 1000, it keep well permission on volume share
RUN usermod -u 1000 www-data

WORKDIR /var/www/html
VOLUME /var/www/html
