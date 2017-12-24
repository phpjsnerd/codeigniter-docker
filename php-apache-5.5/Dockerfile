FROM php:5.5-apache

MAINTAINER Phpjsnerd <phpjsnerd@gmail.com>

ADD ./codeigniter.ini /usr/local/etc/php/conf.d

RUN apt-get update && apt-get install -y \
  libpq-dev \
  libmemcached-dev \
  curl \
  mysql-client \
  postgresql-client \
  libpng12-dev \
  libfreetype6-dev \
  libssl-dev \
  libmcrypt-dev \
  --no-install-recommends && \
  rm -r /var/lib/apt/lists/*

# configure gd library
RUN docker-php-ext-configure gd \
  --enable-gd-native-ttf \
  --with-freetype-dir=/usr/include/freetype2

# Install extensions using the helper script provided by the base image
RUN docker-php-ext-install \
  pdo_mysql \
  pdo_pgsql \
  mysqli \
  gd \
  mcrypt

# Install xdebug
RUN pecl install xdebug && \
  docker-php-ext-enable xdebug
  
# enable apache module rewrite
RUN a2enmod rewrite

RUN usermod -u 1000 www-data


#customized application root
#RUN mkdir -p /var/www/app
#ADD . /var/www/app
#WORKDIR /var/www/app
#ENV APACHE_DOCUMENT_ROOT /var/www/app
#RUN sed -ri -e 's!/var/www/html!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/sites-available/*.conf
#RUN sed -ri -e 's!/var/www/!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/apache2.conf /etc/apache2/conf-available/*.conf

#copy source files
ADD . /var/www/html

WORKDIR /var/www/html

#expose the port
EXPOSE 80
EXPOSE 443

CMD ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]