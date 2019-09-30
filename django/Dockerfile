ARG BASE_IMAGE=alpine
ARG VERSION=3.10
FROM ${BASE_IMAGE}:${VERSION}
LABEL org.label-schema.schema-version="Django " \
    maintainer="https://0cloud0.com" \
    org.label-schema.vcs-description="Django" \
    org.label-schema.docker.cmd="docker build -t anishnath/dockerfile:django ." \
    image-size="92.3MB" \
    ram-usage="13.2MB to 70MB" \
    cpu-usage="Low"

RUN echo "**** install Python django ****" && \
    apk add --no-cache python3 bash && \
    if [ ! -e /usr/bin/python ]; then ln -sf python3 /usr/bin/python ; fi && \
    \
    echo "**** install pip ****" && \
    python3 -m ensurepip && \
    rm -r /usr/lib/python*/ensurepip && \
    pip3 install --no-cache --upgrade pip setuptools wheel && \
    if [ ! -e /usr/bin/pip ]; then ln -s pip3 /usr/bin/pip ; fi && \
    pip install django db-sqlite3 && \
    addgroup -g 101 -S django  && \
    adduser -S -D -H -u 101 -h /opt/app/djangodemo -s /sbin/nologin -G django -g django django

WORKDIR /opt/app/
RUN echo "**** Creating Sample Site ****" && \
    django-admin startproject djangodemo && \
    cd djangodemo && \
    python manage.py migrate  && \
    chown -R django:django /opt/

EXPOSE 8080
USER django
WORKDIR /opt/app/djangodemo
CMD ["python",  "manage.py", "runserver", "0.0.0.0:8080"]

