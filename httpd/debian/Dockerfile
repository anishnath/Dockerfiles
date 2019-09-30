FROM debian
LABEL org.label-schema.schema-version="HTTPD +PHP debian" \
    maintainer="https://0cloud0.com" \
    org.label-schema.vcs-description="HTTPD +PHP " \
    org.label-schema.docker.cmd="docker run -p8080:80 anishnath/dockerfile:httpddebian" \
    image-size="299MB" \
    ram-usage="1GB" \
    cpu-usage="High"

ENV APACHE_RUN_DIR=/var/run/apache2 \
    APACHE_RUN_USER=www-data \
    APACHE_RUN_GROUP=www-data \
    APACHE_LOG_DIR=/var/log/apache2 \
    APACHE_PID_FILE=/var/run/apache2/apache2.pid

WORKDIR /app/public
COPY info.php /app/public
COPY index.html /app/public
RUN apt-get -y update  && \
    apt-get -y install libcap2 apache2 php7.3 libapache2-mod-php7.3 php7.3-mysql php7.3-xml \
    ntp openssl tzdata vim && \
    rm -rf /var/cache/* && \
    chown -R www-data:www-data /app && \ 
    chown -R www-data:www-data /etc/apache2 && \
    chown -R www-data:www-data /var/www/ && \
    chown -R www-data:www-data /run/apache2 && \
    setcap 'cap_net_bind_service=+ep' /usr/sbin/apache2 && \
    ln -sf /dev/stdout /var/log/apache2/access.log && \
    ln -sf /dev/stderr /var/log/apache2/error.log && \
    chown -R www-data:www-data /var/log/apache2

USER www-data
# Add www-data to run and configure
RUN sed -i "s#^DocumentRoot \".*#DocumentRoot \"/app/public\"#g" /etc/apache2/apache2.conf && \
    sed -i "s#/var/www/#/app/public#" /etc/apache2/apache2.conf && \
    printf "\n<Directory \"/app/public\">\n\tAllowOverride All\n</Directory>\n" >> /etc/apache2/apache2.conf && \
    sed -i "s#/var/www/html#/app/public#" /etc/apache2/sites-enabled/000-default.conf

EXPOSE 80
CMD ["apache2", "-D", "FOREGROUND"]