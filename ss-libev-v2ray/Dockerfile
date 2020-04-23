FROM golang:alpine as builder
WORKDIR /root

RUN set -ex \
	&& runDeps="git" \
	&& apk add --no-cache --virtual .build-deps ${runDeps} \
  && mkdir -p /root/v2ray-plugin \
	&& cd /root/v2ray-plugin \
	&& git clone --depth=1 https://github.com/shadowsocks/v2ray-plugin.git . \
	&& go build 


FROM alpine
WORKDIR /root
COPY config.json /etc/shadowsocks-libev/config.json
COPY --from=builder /root/v2ray-plugin/v2ray-plugin /usr/bin/v2ray-plugin

RUN set -ex \
	&& runDeps="git build-base c-ares-dev autoconf automake libev-dev libtool libsodium-dev linux-headers mbedtls-dev pcre-dev" \
	&& apk add --no-cache --virtual .build-deps ${runDeps} \
	&& mkdir -p /root/libev \
	&& cd /root/libev \
	&& git clone --depth=1 https://github.com/shadowsocks/shadowsocks-libev.git . \
	&& git submodule update --init --recursive \
	&& ./autogen.sh \
	&& ./configure --prefix=/usr --disable-documentation \
	&& make install \
	&& apk add --no-cache \
		tzdata \
		rng-tools \
		ca-certificates \
		$(scanelf --needed --nobanner /usr/bin/ss-* \
		| awk '{ gsub(/,/, "\nso:", $2); print "so:" $2 }' \
		| xargs -r apk info --installed \
		| sort -u) \
	&& apk del .build-deps \
	&& cd /root \
	&& rm -rf /root/libev 

VOLUME /etc/shadowsocks-libev
ENV TZ=Asia/Shanghai
EXPOSE 8880
CMD [ "ss-server", "-c", "/etc/shadowsocks-libev/config.json" ]
