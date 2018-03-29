#Based on php:7.0.14-apache

FROM debian:jessie

RUN apt-get update && apt-get install -y apache2-bin apache2.2-common --no-install-recommends && rm -rf /var/lib/apt/lists/*

ENV APACHE_CONFDIR /etc/apache2
ENV APACHE_ENVVARS $APACHE_CONFDIR/envvars

RUN set -ex \
    \
# generically convert lines like
#   export APACHE_RUN_USER=www-data
# into
#   : ${APACHE_RUN_USER:=www-data}
#   export APACHE_RUN_USER
# so that they can be overridden at runtime ("-e APACHE_RUN_USER=...")
    && sed -ri 's/^export ([^=]+)=(.*)$/: ${\1:=\2}\nexport \1/' "$APACHE_ENVVARS" \
    \
# setup directories and permissions
    && . "$APACHE_ENVVARS" \
    && for dir in \
        "$APACHE_LOCK_DIR" \
        "$APACHE_RUN_DIR" \
        "$APACHE_LOG_DIR" \
        /var/www/html \
    ; do \
        rm -rvf "$dir" \
        && mkdir -p "$dir" \
        && chown -R "$APACHE_RUN_USER:$APACHE_RUN_GROUP" "$dir"; \
    done

# logs should go to stdout / stderr
RUN set -ex \
    && . "$APACHE_ENVVARS" \
    && ln -sfT /dev/stderr "$APACHE_LOG_DIR/error.log" \
    && ln -sfT /dev/stdout "$APACHE_LOG_DIR/access.log" \
    && ln -sfT /dev/stdout "$APACHE_LOG_DIR/other_vhosts_access.log"

COPY apache2-foreground /usr/local/bin/
RUN chmod +x /usr/local/bin/apache2-foreground

#COPY 000-default.conf /etc/apache2/sites-enabled/000-default.conf
RUN a2enmod proxy proxy_fcgi

#Enable mod rewrite
RUN a2enmod rewrite

WORKDIR /var/www/html

EXPOSE 80

CMD ["apache2-foreground"]
