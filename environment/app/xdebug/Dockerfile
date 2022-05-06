FROM ubnt/unms-crm:3.4.3

MAINTAINER Ryan Spaeth <rspaeth@spaethtech.com>

### XDEBUG
RUN  sed -i 's|$document_root;|$document_root;\n        include xdebug_params;|g' \
       /etc/nginx/available-servers/ucrm.conf \
  && apk add --update --no-cache $PHPIZE_DEPS \
  && pecl install -f xdebug \
  && docker-php-ext-enable xdebug

COPY ./xdebug.ini /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
COPY ./xdebug_params /etc/nginx/xdebug_params

RUN echo -n "Fixing Permissions & Ownerships..." \
    && mkdir -p /data/{ssl,ucrm,updates}   && chown -R nginx:nginx /data/{ssl,ucrm,updates} \
    #&& mkdir -p /data/ucrm/data/plugins    && chown -R nginx:nginx /data/ucrm/data/plugins \
    #&& mkdir -p /usr/src/ucrm/web/_plugins && chown -R nginx:nginx /usr/src/ucrm/web/_plugins \
    && echo "DONE!"
