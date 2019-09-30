FROM debian
LABEL org.label-schema.schema-version="Django Debian " \
    maintainer="https://0cloud0.com" \
    org.label-schema.vcs-description="Django" \
    org.label-schema.docker.cmd="docker build -t anishnath/dockerfile:djangodebian ." \
    image-size="363MB" \
    ram-usage="13.2MB to 70MB" \
    cpu-usage="Low"

RUN echo "**** install Python django ****" && \
    apt-get -y update && \
    apt-get -y install python3 python3-pip && \
    pip3 install --no-cache --upgrade pip setuptools wheel && \
    if [ ! -e /usr/bin/pip ]; then ln -s pip3 /usr/bin/pip ; fi && \
    pip install Django==2.1.* db-sqlite3 && \
    addgroup --system --gid 1000 django && \
    adduser --system --disabled-login --ingroup django --no-create-home --home /nonexistent --gecos "django user" --shell /bin/false --uid 1000 django

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
CMD ["python3.7",  "manage.py", "runserver", "0.0.0.0:8080"]