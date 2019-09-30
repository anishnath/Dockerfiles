FROM alpine:latest
LABEL org.label-schema.schema-version="HTTPD +PHP Alpine" \
    maintainer="https://0cloud0.com" \
    org.label-schema.vcs-description="HTTPD +PHP App" \
    org.label-schema.docker.cmd="docker run -p80:80 anishnath/dockerfile:httpd" \
    image-size="26MB" \
    ram-usage="1GB" \
    cpu-usage="High"

WORKDIR /app/public
COPY info.php /app/public
COPY index.html /app/public
RUN set -x \
    apk add --update  && \
    apk add libcap  bash apache2 php7-apache2 php7-simplexml \
    php7 php7-phar php7-json php7-iconv php7-openssl \
    openntpd openssl tzdata && \
    rm -f /var/cache/apk/* && \
    addgroup -g 1000 -S apache2 && \
    adduser -S -D -H -u 1000 -h /etc/apache2/ -s /sbin/nologin -G apache2 -g apache2 apache2 && \ 
    chown -R apache2:apache2 /app && \ 
    chown -R apache2:apache2 /etc/apache2 && \
    chown -R apache2:apache2 /var/www/ && \
    chown -R apache2:apache2 /var/log/apache2 && \
    chown -R apache2:apache2 /run/apache2 && \
    setcap 'cap_net_bind_service=+ep' /usr/sbin/httpd && \
    ln -sf /dev/stdout /var/log/apache2/access.log && \
    ln -sf /dev/stderr /var/log/apache2/error.log

RUN getcap /usr/sbin/httpd


USER apache2
# Add apache to run and configure
RUN sed -i "s/#LoadModule\ rewrite_module/LoadModule\ rewrite_module/" /etc/apache2/httpd.conf \
    && sed -i "s/#LoadModule\ session_module/LoadModule\ session_module/" /etc/apache2/httpd.conf \
    && sed -i "s/#LoadModule\ session_cookie_module/LoadModule\ session_cookie_module/" /etc/apache2/httpd.conf \
    && sed -i "s/#LoadModule\ session_crypto_module/LoadModule\ session_crypto_module/" /etc/apache2/httpd.conf \
    && sed -i "s/#LoadModule\ deflate_module/LoadModule\ deflate_module/" /etc/apache2/httpd.conf \
    && sed -i "s#^DocumentRoot \".*#DocumentRoot \"/app/public\"#g" /etc/apache2/httpd.conf \
    && sed -i "s#/var/www/localhost/htdocs#/app/public#" /etc/apache2/httpd.conf \
    && printf "\n<Directory \"/app/public\">\n\tAllowOverride All\n</Directory>\n" >> /etc/apache2/httpd.conf

EXPOSE 80
CMD ["httpd", "-D", "FOREGROUND"]
