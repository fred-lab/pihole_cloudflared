FROM alpine:latest

LABEL maintainer="Fred Gauthier <fred.gauthier.dev@gmail.com>" \
    Description="A Cloudflared container, build with Alpine"

ARG  ARCHITECTURE
ENV  ARCHITECTURE $ARCHITECTURE

ARG  PORT
ENV  PORT $PORT

RUN apk update \
    && apk add --no-cache --virtual install-dependencies curl tar\
    && apk add --no-cache libc6-compat \
    && addgroup -S cloudflared && adduser -S cloudflared -G cloudflared \
    && curl "https://bin.equinox.io/c/VdrWdbjqyF/cloudflared-stable-linux-${ARCHITECTURE}.tgz" | tar -xzC /usr/local/bin \
    && chmod +x /usr/local/bin/cloudflared \
    && chown cloudflared:cloudflared /usr/local/bin/cloudflared \
    # Clean up
    && apk del --purge install-dependencies \
    && rm -rf /var/cache/apk/*

EXPOSE $PORT/tcp $PORT/udp

USER cloudflared

CMD cloudflared proxy-dns --port $PORT --address 0.0.0.0 --upstream https://1.1.1.1/dns-query --upstream https://1.0.0.1/dns-query