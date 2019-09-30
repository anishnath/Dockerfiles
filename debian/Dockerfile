FROM scratch
LABEL org.label-schema.schema-version="1.0" \
	org.label-schema.name="Debian Base Image" \
	maintainer="https://leanpub.com/dockerfile Anish Nath" \
	org.label-schema.vcs-description="Debian Base Image-minimal" \
	org.label-schema.docker.cmd="docker run -it debian sh" \
	org.label-schema.vendor="Debian" \
	org.label-schema.license="GPLv2" \
	org.label-schema.build-date="20190801"
ADD rootfs.tar.xz /
RUN apt-get -y update
CMD ["/bin/bash"]
