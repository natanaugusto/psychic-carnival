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
    # python3 \
    # python3-dev \
    # py3-pip \
    libressl-dev \
    musl-dev \
    libffi-dev
RUN mkdir /etc/letsencrypt
# RUN pip3 install pip --upgrade

RUN set -x ; \
    addgroup -g 82 -S www-data ; \
    adduser -u 82 -D -S -G www-data www-data && exit 0 ; exit 1

# Create 'messages' file used from 'logrotate'
RUN touch /var/log/messages

# Copy 'logrotate' config file
COPY nginx.conf /etc/nginx/
COPY logrotate/nginx /etc/logrotate.d/
ADD ./startup.sh /opt/startup.sh
RUN sed -i 's/\r//g' /opt/startup.sh
CMD [ "/bin/bash", "/opt/startup.sh" ]

EXPOSE 80 81 443
