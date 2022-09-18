FROM nginx:alpine

LABEL maintainer="Natan Augusto <natanaugusto@gmail.com>"

RUN apk update --no-cache
RUN apk add --no-cache --update \
    build-base \
    logrotate \
    cargo \
    rust \
    openssl \
    certbot \
    certbot-nginx \
    bash \
    curl \
    libressl-dev \
    musl-dev \
    libffi-dev
RUN mkdir /etc/letsencrypt

RUN set -x ; \
    addgroup -g 82 -S www-data ; \
    adduser -u 82 -D -S -G www-data www-data && exit 0 ; exit 1

RUN touch /var/log/messages

COPY nginx.conf /etc/nginx/
COPY logrotate/nginx /etc/logrotate.d/
COPY crontabs/root /var/spool/cron/crontabs/root
ADD ./startup.sh /opt/startup.sh
RUN sed -i 's/\r//g' /opt/startup.sh
CMD [ "/bin/bash", "/opt/startup.sh" ]

EXPOSE 80 81 443
