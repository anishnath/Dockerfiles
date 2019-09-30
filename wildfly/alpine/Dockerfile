FROM alpine:latest
LABEL org.label-schema.schema-version="WildFly" \
    maintainer="https://0cloud0.com" \
    org.label-schema.vcs-description="WildFly App" \
    org.label-schema.docker.cmd="docker run -p8080:8080 -p 9990:9990 anishnath/dockerfile:wildfly" \
    image-size="546MB" \
    ram-usage="1GB" \
    cpu-usage="High" \
    WILDFLY_DOWNLOAD="curl https://download.jboss.org/wildfly/17.0.0.Final/wildfly-17.0.0.Final.tar.gz"

ENV WILDFLY_VERSION 17.0.0.Final
ENV LAUNCH_JBOSS_IN_BACKGROUND true
COPY wildfly-17.0.0.Final.tar /opt/

RUN set -x \
    apk add --update && \
    apk add curl tar openjdk8 curl && \
    addgroup -g 101 -S wildfly && \
    adduser -S -D -H -u 101 -h /opt/wildfly -s /sbin/nologin -G wildfly -g wildfly wildfly && \ 
    cd /opt/ && \
    tar xvf wildfly-$WILDFLY_VERSION.tar && \
    mv wildfly-$WILDFLY_VERSION wildfly && \
    chown -R wildfly:wildfly /opt/wildfly && \
    rm -rf /tmp/* /var/cache/apk/* && \
    rm -rf /opt/wildfly-17.0.0.Final.tar

COPY ROOT.war /opt/wildfly/standalone/deployments/

EXPOSE 8080 9990
USER wildfly
WORKDIR /opt/wildfly/
CMD ["/opt/wildfly/bin/standalone.sh", "-c", "standalone-full.xml", "-b", "0.0.0.0", "-bmanagement", "0.0.0.0"]