FROM nyanpass/php5.5:5.5-apache

#RUN apt-get update && apt-get install -y \
#        libfreetype6-dev \
#        libjpeg62-turbo-dev \
#        libmcrypt-dev \
#        libpng12-dev \
#    && docker-php-ext-install -j$(nproc) iconv mcrypt \
#    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
#    && docker-php-ext-install -j$(nproc) gd

#RUN apt-get update && apt-get install -y libmemcached-dev \
#    && pecl install memcached-2.2.0 \
#    && docker-php-ext-enable memcached

RUN docker-php-source extract \
	&& apt-get update \
	&& apt-get install -y \
	   libmemcached-dev \
	   libfreetype6-dev \
       libjpeg62-turbo-dev \
       libmcrypt-dev \
       libpng12-dev \
    && pecl install memcached-2.2.0 \
    && docker-php-ext-enable memcached \
	&& docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
	&& docker-php-ext-install pdo pdo_mysql mysql mysqli \
	&& docker-php-ext-install -j$(nproc) mcrypt \
	&& docker-php-ext-install -j$(nproc) gd \
	&& a2enmod expires rewrite \
	&& a2enmod ssl \
	&& a2enmod headers \
	&& a2enmod mime \
	&& docker-php-source delete

#COPY config/001-kp.conf /etc/apache2/sites-enabled/000-default.conf
#RUN chmod 755 /var/www/html/