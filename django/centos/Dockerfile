FROM centos:centos7
LABEL org.label-schema.schema-version="Django " \
    maintainer="https://0cloud0.com" \
    org.label-schema.vcs-description="Django" \
    org.label-schema.docker.cmd="docker build -t anishnath/dockerfile:djangocentos7 ." \
    image-size="363MB" \
    ram-usage="13.2MB to 70MB" \
    cpu-usage="Low"

RUN echo "**** install Python django ****" && \
    yum -y install python36 && \
    if [ ! -e /usr/bin/python ]; then ln -sf python3 /usr/bin/python ; fi && \
    \
    echo "**** install pip ****" && \
    /usr/bin/python3 -m ensurepip && \
    pip3 install --no-cache --upgrade pip setuptools wheel && \
    if [ ! -e /usr/bin/pip ]; then ln -s pip3 /usr/bin/pip ; fi && \
    pip install Django==2.1.* db-sqlite3 && \
    groupadd --system --gid 101 django && \
    adduser --system   --no-create-home --home /nonexistent --comment "django user" --shell /bin/false --uid 101 -g django django 

WORKDIR /opt/app/
RUN echo "**** Creating Sample Site ****" && \
    django-admin startproject djangodemo && \
    cd djangodemo && \
    python3 manage.py migrate  && \
    sed -i "s/ALLOWED_HOSTS = \[\]/ALLOWED_HOSTS = \['*'\]/g" /opt/app/djangodemo/djangodemo/settings.py && \
    chown -R django:django /opt/

EXPOSE 8080
USER django
WORKDIR /opt/app/djangodemo
CMD ["python3",  "manage.py", "runserver", "0.0.0.0:8080"]