FROM centos:centos7
LABEL org.label-schema.schema-version="HTTPD +PHP Centos" \
    maintainer="https://0cloud0.com" \
    org.label-schema.vcs-description="HTTPD +PHP " \
    org.label-schema.docker.cmd="docker run -p80:80 anishnath/dockerfile:httpdcentos" \
    image-size="318MB" \
    ram-usage="1GB" \
    cpu-usage="High"

WORKDIR /app/public
COPY info.php /app/public
COPY index.html /app/public
RUN set -x \
    yum -y update  && \
    yum -y install libcap httpd php php-xml php-fpm \
    ntp openssl tzdata && \
    rm -rf /var/cache/* && \
    chown -R apache:apache /app && \ 
    chown -R apache:apache /etc/httpd && \
    chown -R apache:apache /var/www/ && \
    chown -R apache:apache /run/httpd && \
    setcap 'cap_net_bind_service=+ep' /usr/sbin/httpd && \
    ln -sf /dev/stdout /var/log/httpd/access.log && \
    ln -sf /dev/stderr /var/log/httpd/error.log && \
    chown -R apache:apache /var/log/httpd

USER apache
# Add apache to run and configure
RUN sed -i "s/#LoadModule\ rewrite_module/LoadModule\ rewrite_module/" /etc/httpd/conf.modules.d/00-base.conf && \
    sed -i "s/#LoadModule\ session_module/LoadModule\ session_module/" /etc/httpd/conf.modules.d/00-base.conf && \
    sed -i "s/#LoadModule\ session_cookie_module/LoadModule\ session_cookie_module/" /etc/httpd/conf.modules.d/00-base.conf && \
    sed -i "s/#LoadModule\ session_crypto_module/LoadModule\ session_crypto_module/" /etc/httpd/conf.modules.d/00-base.conf && \
    sed -i "s/#LoadModule\ deflate_module/LoadModule\ deflate_module/" /etc/httpd/conf.modules.d/00-base.conf && \
    sed -i "s#^DocumentRoot \".*#DocumentRoot \"/app/public\"#g" /etc/httpd/conf/httpd.conf && \
    sed -i "s#/var/www/html#/app/public#" /etc/httpd/conf/httpd.conf && \
    printf "\n<Directory \"/app/public\">\n\tAllowOverride All\n</Directory>\n" >> /etc/httpd/conf/httpd.conf

EXPOSE 80
CMD ["httpd", "-D", "FOREGROUND"]