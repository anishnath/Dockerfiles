ARG BASE_IMAGE=centos
ARG CENTOS_VERSION=latest
FROM ${BASE_IMAGE}:${CENTOS_VERSION}
LABEL org.label-schema.schema-version="TOmcat9 Centos7 Demo" \
    maintainer="https://0cloud0.com" \
    org.label-schema.vcs-description="0cloud.com Tomcat9 Base Image" \
    org.label-schema.docker.cmd="docker run -d -p 8080:8080 ubuntu:centos" \
    image-size="71.6MB" \
    ram-usage="13.2MB to 70MB" \
    cpu-usage="Low"

ENV TOMCAT_MAJOR=9 \
    TOMCAT_VERSION=9.0.22 \
    TOMCAT_HOME=/opt/tomcat \
    CATALINA_HOME=/opt/tomcat \
    CATALINA_OUT=/dev/null

RUN yum -y update && \
    yum -y install curl java-1.8.0-openjdk gzip  && \
    curl -jksSL -o /tmp/apache-tomcat.tar.gz http://archive.apache.org/dist/tomcat/tomcat-${TOMCAT_MAJOR}/v${TOMCAT_VERSION}/bin/apache-tomcat-${TOMCAT_VERSION}.tar.gz && \
    gunzip /tmp/apache-tomcat.tar.gz && \
    tar -C /opt -xf /tmp/apache-tomcat.tar && \
    ln -s /opt/apache-tomcat-${TOMCAT_VERSION} ${TOMCAT_HOME} && \
    rm -rf ${TOMCAT_HOME}/webapps/* && \
    rm -rf /tmp/* && \
    adduser tomcat --system --uid 1000 --no-create-home && \
    chown tomcat -R /opt/ 

COPY ROOT.war ${TOMCAT_HOME}/webapps/
CMD ["/opt/tomcat/bin/catalina.sh", "run"]
EXPOSE 8080
USER tomcat
