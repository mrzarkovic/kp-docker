FROM nyanpass/php5.5:5.5-apache

# XDEBUG
ENV XDEBUGINI_PATH=/usr/local/etc/php/conf.d/xdebug.ini
COPY config/xdebug.ini /tmp/xdebug.ini
RUN yes | pecl install xdebug \
    && echo "zend_extension="`find /usr/local/lib/php/extensions/ -iname 'xdebug.so'` > $XDEBUGINI_PATH \
    && cat /tmp/xdebug.ini >> $XDEBUGINI_PATH \
    && echo "\nxdebug.remote_host="`/sbin/ip route|awk '/default/ { print $3 }'` >> $XDEBUGINI_PATH

# COPY APACHE AND PHP CONFIG
COPY config/001-kp.conf /etc/apache2/sites-enabled/001-kp.conf
COPY config/php.ini /usr/local/etc/php/php.ini

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
	&& docker-php-ext-install pdo pdo_mysql mysql mysqli exif sockets \
	&& docker-php-ext-install -j$(nproc) mcrypt \
	&& docker-php-ext-install -j$(nproc) gd \
	&& a2enmod expires rewrite \
	&& a2enmod ssl \
	&& a2enmod headers \
	&& a2enmod mime \
	&& docker-php-source delete
