FROM ubuntu:20.04

LABEL description="Bootstrap image of Ubuntu 20.04 with php7.4-fpm and Phalcon 4" \
    maintainer="Rudi Servo <rudi.servo@lavaimagem.com" \
    vendor=Lavaimagem \
    name="net.lavaimagem.images.ubuntu-20.04"

ENV TIMEZONE=UTC \
    LANGUAGE=en_US.UTF-8 \
    LANG=en_US.UTF-8 \
    LC_ALL=en_US.UTF-8 \
    DEBIAN_FRONTEND=noninteractive \
    TERM=xterm

ENV APACHE_RUN_USER=www-data \
    APACHE_RUN_GROUP=www-data \
    APACHE_LOG_DIR=/var/log/apache2 \
    APACHE_LOCK_DIR=/var/lock/apache2 \
    APACHE_PID_FILE=/var/run/apache2.pid \
    APACHE_RUN_DIR=/var/run/apache2 \
    APACHE_DOCUMENT_ROOT=/var/www/html


RUN apt clean -y \
    && apt update -y \
    && apt dist-upgrade -y \
    && apt install -y locales gpg-agent curl wget netcat unzip p7zip-full git \
    && export LANGUAGE=${LANGUAGE} \
    && export LANG=${LANG} \
    && export LC_ALL=${LC_ALL} \
    && locale-gen ${LANG} \
    && dpkg-reconfigure --frontend noninteractive locales \
    && apt install --no-install-recommends -yq \
        apt-utils \
        software-properties-common \
        lsb-release \
        ca-certificates \
        apt-transport-https \
        tzdata \
    && echo "#!/bin/sh\nexit 0" > /usr/sbin/policy-rc.d \
    && ln -sf /usr/share/zoneinfo/${TIMEZONE} /etc/localtime \
    && echo "${TIMEZONE}" | tee /etc/timezone \
    && dpkg-reconfigure --frontend noninteractive tzdata \
    && LC_ALL=en_US.UTF-8 apt-add-repository -y ppa:ondrej/php \
    && apt-add-repository -y multiverse \
    && apt update -y \
    && apt dist-upgrade -y \
    && apt-get update -y \
    && apt-get install --no-install-recommends -yq \
        apache2 \
        libapache2-mod-php7.4 \
        php7.4-common \
        php7.4-cli \
        php7.4-fpm \
        php7.4-phalcon\
        php7.4-bcmath \
        php7.4-bz2 \
        php7.4-curl \
        php7.4-dba \
        php7.4-gd \
        php7.4-gmp \
        php7.4-imap \
        php7.4-intl \
        php7.4-json \
        php7.4-ldap \
        php7.4-mbstring \
        php7.4-mysql \
        php7.4-opcache \
        php7.4-pgsql \
        php7.4-soap \
        php7.4-sqlite3 \
        php7.4-tidy \
        php7.4-xml \
        php7.4-zip \
        php7.4-dev \
        php-apcu \
        php-ast \
        php-bcmath \
        php-bz2 \
        php-common \
        php-curl \
        php-gd \
        php-db \
        php-geoip \
        php-geos \
        php-gmp \
        php-imagick \
        php-json \
        php-mbstring \
        php-memcached \
        php-oauth \
        php-parser \
        php-xdebug \
        php-xml \
        php-zip \
        make \
    && php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    && php composer-setup.php --install-dir=bin \
    && php -r "unlink('composer-setup.php');" \
    && apt-get autoremove -y \
    && apt-get clean -y \
    && rm -rf /tmp/* /var/tmp/* \
    && find /var/cache/apt/archives /var/lib/apt/lists -not -name lock -type f -delete \
    && find /var/cache -type f -delete \
    && find /var/log -type f | while read f; do echo -n '' > ${f}; done

RUN sed -i "s/;date.timezone =.*/date.timezone = UTC/" /etc/php/7.4/cli/php.ini
RUN sed -i "s/;date.timezone =.*/date.timezone = UTC/" /etc/php/7.4/fpm/php.ini
RUN sed -i "s/display_errors = Off/display_errors = On/" /etc/php/7.4/fpm/php.ini
RUN sed -i "s/upload_max_filesize = .*/upload_max_filesize = 30M/" /etc/php/7.4/fpm/php.ini
RUN sed -i "s/post_max_size = .*/post_max_size = 12M/" /etc/php/7.4/fpm/php.ini
RUN sed -i "s/;cgi.fix_pathinfo=1/cgi.fix_pathinfo=0/" /etc/php/7.4/fpm/php.ini
RUN sed -i -e "s/pid =.*/pid = \/var\/run\/php7.4-fpm.pid/" /etc/php/7.4/fpm/php-fpm.conf
RUN sed -i -e "s/error_log =.*/error_log = \/proc\/self\/fd\/2/" /etc/php/7.4/fpm/php-fpm.conf
RUN sed -i -e "s/;daemonize\s*=\s*yes/daemonize = no/g" /etc/php/7.4/fpm/php-fpm.conf
RUN sed -i "s/listen = .*/listen = 9000/" /etc/php/7.4/fpm/pool.d/www.conf
RUN sed -i "s/;catch_workers_output = .*/catch_workers_output = yes/" /etc/php/7.4/fpm/pool.d/www.conf

EXPOSE 80 443 8080
CMD apachectl -D FOREGROUND
