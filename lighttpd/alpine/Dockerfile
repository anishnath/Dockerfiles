ARG BASE_IMAGE=alpine
ARG VERSION=3.10
FROM ${BASE_IMAGE}:${VERSION}
LABEL org.label-schema.schema-version="lighttpd " \
    maintainer="https://0cloud0.com" \
    org.label-schema.vcs-description="Django" \
    org.label-schema.docker.cmd="docker build -t anishnath/lighttpd ." \
    image-size="13MB" \
    ram-usage="13.2MB to 70MB" \
    cpu-usage="Low"

RUN echo "**** install lighttpd ****" && \
    apk add --no-cache && \
    apk add  lighttpd && \
    chown -R lighttpd:lighttpd /var/www/localhost && \
    #ln -sf /dev/stdout /var/log/lighttpd/access.log && \
    #ln -sf /dev/stderr /var/log/lighttpd/error.log && \
    chown -R lighttpd:lighttpd /var/log/lighttpd && \
    rm -rf /var/cache/*

EXPOSE 80
WORKDIR /var/www/localhost/htdocs
COPY index.html .
CMD ["/usr/sbin/lighttpd","-D","-f","/etc/lighttpd/lighttpd.conf"]
