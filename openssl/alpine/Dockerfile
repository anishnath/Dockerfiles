FROM alpine:3.10
ENV OPENSSL_VERSION 1.1.1d
ENV OPENSSL_SOURCE_SHA256="1e3a91bc1f9dfce01af26026f856e064eab4c8ee0a8f457b5ae30b40b8b711f2"
# https://www.openssl.org/community/omc.html
ENV OPENSSL_PGP_KEY_IDS="0x8657ABB260F056B1E5190839D9C4D26D0E604491 0x5B2545DAB21995F4088CEFAA36CEE4DEB00CFE33 0xED230BEC4D4F2518B9D7DF41F0DB4D21C1D35231 0xC1F33DD8CE1D4CC613AF14DA9195C48241FBF7DD 0x7953AC1FBC3DC8B3B292393ED5E9E43F7DF9EE8C 0xE5E52560DD91C556DDBDA5D02064C53641C25E5D"

LABEL org.label-schema.schema-version="OpenSSL 1.1.1d  10 Sep 2019" \
    maintainer="https://0cloud0.com" \
    org.label-schema.vcs-description="0cloud.com OpenSSL 1.1.1d  Image" \
    org.label-schema.docker.cmd="docker run -it anishnath/dockerfile:openssl_1.1.1d sh " \
    image-size="183MB" \
    ram-usage="13.2MB to 70MB" \
    cpu-usage="Low"


ARG PGP_KEYSERVER=ha.pool.sks-keyservers.net
RUN set -eux; \
	\
	apk add --no-cache --virtual .build-deps \
		ca-certificates \
		dpkg-dev dpkg \
		gcc \
		gnupg \
		libc-dev \
		linux-headers \
		make \
		ncurses-dev \
	; \
	\
	OPENSSL_SOURCE_URL="https://www.openssl.org/source/openssl-$OPENSSL_VERSION.tar.gz"; \
	OPENSSL_PATH="/usr/local/src/openssl-$OPENSSL_VERSION"; \
	OPENSSL_CONFIG_DIR=/usr/local/etc/ssl; \
	\
# /usr/local/src doesn't exist in Alpine by default
	mkdir /usr/local/src; \
	\
# Required by the crypto
	wget --output-document "$OPENSSL_PATH.tar.gz.asc" "$OPENSSL_SOURCE_URL.asc"; \
	wget --output-document "$OPENSSL_PATH.tar.gz" "$OPENSSL_SOURCE_URL"; \
	export GNUPGHOME="$(mktemp -d)"; \
	for key in $OPENSSL_PGP_KEY_IDS; do \
		gpg --batch --keyserver "$PGP_KEYSERVER" --recv-keys "$key"; \
	done; \
	gpg --batch --verify "$OPENSSL_PATH.tar.gz.asc" "$OPENSSL_PATH.tar.gz"; \
	gpgconf --kill all; \
	rm -rf "$GNUPGHOME"; \
	echo "$OPENSSL_SOURCE_SHA256 *$OPENSSL_PATH.tar.gz" | sha256sum -c -; \
	mkdir -p "$OPENSSL_PATH"; \
	tar --extract --file "$OPENSSL_PATH.tar.gz" --directory "$OPENSSL_PATH" --strip-components 1; \
	\
# Configure OpenSSL for compilation
	cd "$OPENSSL_PATH"; \
# OpenSSL's "config" script uses a lot of "uname"-based target detection...
	MACHINE="$(dpkg-architecture --query DEB_BUILD_GNU_CPU)" \
	RELEASE="4.x.y-z" \
	SYSTEM='Linux' \
	BUILD='???' \
	./config \
		--openssldir="$OPENSSL_CONFIG_DIR" \
		-Wl,-rpath=/usr/local/lib \
	; \
# Compile, install OpenSSL, verify that the command-line works & development headers are present
	make -j "$(getconf _NPROCESSORS_ONLN)"; \
	make install_sw install_ssldirs; \
	cd ..; \
	rm -rf "$OPENSSL_PATH"*; \
# use Alpine's CA certificates
	rmdir "$OPENSSL_CONFIG_DIR/certs" "$OPENSSL_CONFIG_DIR/private"; \
	ln -sf /etc/ssl/certs /etc/ssl/private "$OPENSSL_CONFIG_DIR"; \
# smoke test
	openssl version; 

WORKDIR /openssl