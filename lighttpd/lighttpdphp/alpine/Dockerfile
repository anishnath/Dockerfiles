ARG BASE_IMAGE=alpine
ARG VERSION=3.10
FROM ${BASE_IMAGE}:${VERSION}
LABEL org.label-schema.schema-version="lighttpd " \
    maintainer="https://0cloud0.com" \
    org.label-schema.vcs-description="Django" \
    org.label-schema.docker.cmd="docker build -t anishnath/lighttpd:php7 ." \
    image-size="13.7MB" \
    ram-usage="13.2MB to 70MB" \
    cpu-usage="Low"

RUN echo "**** install lighttpd ****" && \
    apk add --no-cache && \
    apk add  lighttpd php7-common php7-iconv php7-json php7-gd php7-curl php7-xml php7-mysqli php7-imap php7-cgi fcgi php7-pdo php7-pdo_mysql php7-soap php7-xmlrpc php7-posix php7-mcrypt php7-gettext php7-ldap php7-ctype php7-dom && \
    #Enable FAST CGI
    echo "include \"mod_fastcgi.conf\"" >> /etc/lighttpd/lighttpd.conf && \
    sed -i -- 's/php-cgi/php-cgi7/g' /etc/lighttpd/mod_fastcgi.conf && \
    # simple test page 
    echo "<?php phpinfo(); ?>" > /var/www/localhost/htdocs/index.php && \
    mkdir /var/run/lighttpd && \
    touch /var/run/lighttpd/php-fastcgi.socket && \
    chown -R lighttpd:lighttpd /var/run/lighttpd && \
    chown -R lighttpd:lighttpd /var/www/localhost && \
    chown -R lighttpd:lighttpd /var/log/lighttpd && \
    rm -rf /var/cache/*

EXPOSE 80
WORKDIR /var/www/localhost/htdocs
COPY index.html .
CMD ["/usr/sbin/lighttpd","-D","-f","/etc/lighttpd/lighttpd.conf"]