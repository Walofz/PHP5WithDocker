FROM php:5.6-fpm

LABEL name="Doya"

#Set Timezone
ENV TZ=Asia/Bangkok
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update && apt-get install -y \
    git libfreetype6-dev libjpeg62-turbo-dev libmcrypt-dev libssl-dev libpng* \
    libmemcached-dev libz-dev zlib1g-dev libsqlite3-dev zip \
    libxml2-dev libcurl3-dev libedit-dev libpspell-dev libldap2-dev unixodbc-dev libpq-dev

RUN apt-get update && apt-get install -y unixodbc unixodbc-dev freetds-dev freetds-bin tdsodbc
ADD libs/freetds.conf /etc/freetds/freetds.conf
RUN tsql -C

# https://bugs.php.net/bug.php?id=49876
RUN ln -fs /usr/lib/x86_64-linux-gnu/libldap.so /usr/lib/
# https://github.com/phpbrew/phpbrew/issues/643
RUN ln -fs /usr/lib/x86_64-linux-gnu/libsybdb.a /usr/lib/

RUN echo "Installing PHP extensions" \
    && docker-php-ext-install iconv mcrypt gd pdo pdo_mysql pdo_dblib pdo_pgsql mssql pcntl zip bcmath simplexml xmlrpc soap pspell ldap mbstring opcache \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-enable iconv mcrypt gd pdo pdo_mysql pdo_dblib pdo_pgsql mssql pcntl zip bcmath simplexml xmlrpc soap pspell ldap mbstring \
    && apt-get autoremove -y \
    && dpkg -la | awk '{print $2}' | grep '\-dev' | xargs apt-get remove -y \
    && apt-get clean all \
    && rm -rvf /var/lib/apt/lists/* \
    && rm -rvf /usr/share/doc /usr/share/man /usr/share/locale \
    && rm -fv /usr/local/etc/php-fpm.d/*.conf \
    && rm -rvf /usr/src/php

# install composer
RUN curl -sS https://getcomposer.org/installer | php -- --filename=composer --install-dir=/bin
ENV PATH /root/.composer/vendor/bin:$PATH

# install phpunit
RUN curl https://phar.phpunit.de/phpunit-5.phar -L > phpunit.phar \
    && chmod +x phpunit.phar \
    && mv phpunit.phar /usr/local/bin/phpunit \
    && phpunit --version

# Clean up, try to reduce image size (much as you can on Debian..)
RUN apt-get autoremove -y \
    && apt-get clean all \
    && rm -rf /var/lib/apt/lists/* \
    && rm -rf /usr/share/doc /usr/share/man /usr/share/locale \
    && rm -f /usr/local/etc/php-fpm.d/*.conf \
    && rm -rf /usr/src/php

CMD ["php-fpm", "-F"]

EXPOSE 9001