FROM debian:buster-slim

RUN apt-get update -q -q && \
    DEBIAN_FRONTEND=noninteractive apt-get -y --no-install-recommends install \
    nginx \
    socat \
    supervisor \
    sympa

RUN apt-get update -q -q && \
    DEBIAN_FRONTEND=noninteractive apt-get -y --no-install-recommends install \
    postfix

COPY ./supervisord.conf /etc/supervisor/supervisord.conf

ADD ./supervisor/conf.d /etc/supervisor/conf.d
# COPY ./nginx.ini /etc/supervisor/conf.d/nginx.ini
# COPY ./postfix.ini /etc/supervisor/conf.d/postfix.ini
# COPY ./socat.ini /etc/supervisor/conf.d/socat.ini

COPY ./postfix.sh /usr/local/bin/postfix.sh

COPY ./nginx.conf /etc/nginx/sites-available/default
COPY ./fastcgi_sympa /etc/nginx/fast_cgi_sympa

COPY ./postfix/main.cf /etc/postfix/main.cf 
COPY ./postfix/aliases.sympa.postfix /etc/postfix/aliases.sympa.postfix 

COPY ./opusvl_black_blue.svg /usr/share/sympa/static_content/opusvl_black_blue.svg
VOLUME [ "/run/sympa" ]

EXPOSE 80

ENTRYPOINT [ "supervisord", "-c", "/etc/supervisor/supervisord.conf" ]