FROM ubuntu:18.04

ARG PHP_VERSION
ARG PHP_EXTENSIONS="bcmath cli common gd gmp intl json mbstring \
    mcrypt sodium mysqlnd opcache pdo pecl-msgpack pecl-amqp pecl-redis pecl-imagick pecl-zip process soap xml xmlrpc"

RUN apt update
# RUN apt install -y \
    # && dnf clean all \
    # && rm -rf /var/cache/dnf

RUN set -eux \
    && apt install -y php \
    && PHP_VERSION=$(echo ${PHP_VERSION} | awk -F '.' '{print $1$2}') \
    && PHP_PACKAGES= && for PKG in ${PHP_EXTENSIONS}; do \
        if [[ ${PKG} = "mcrypt" ]] && (( ${PHP_VERSION} > 71 )); then continue; fi; \
        if [[ ${PKG} = "sodium" ]] && (( ${PHP_VERSION} < 72 )); then continue; fi; \
        PHP_PACKAGES+="php-${PKG} "; \
    done \
    && apt install -y ${PHP_PACKAGES} git perl which npm patch gettext unzip bzip2 pwgen nmap-ncat jq 
    # && apt clean all \
    # && rm -rf /var/cache/dnf

RUN set -eux \
    && cp /etc/php.ini /etc/php-cli.ini \
    && perl -pi -e 's/memory_limit(.*)/memory_limit=-1/g' /etc/php-cli.ini

ENV COMPOSER_ALLOW_SUPERUSER 1
ENV COMPOSER_HOME /tmp/composer
COPY --from=composer:1 /usr/bin/composer /usr/bin/composer
COPY --from=composer:1 /usr/bin/composer /usr/bin/composer1
COPY --from=composer:2 /usr/bin/composer /usr/bin/composer2
