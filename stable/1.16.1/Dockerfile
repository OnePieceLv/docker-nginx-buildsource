FROM debian:buster-slim
LABEL maintainer "steven lv - https://github.com/OnePieceLv"

EXPOSE 80 443
CMD ["nginx", "-g", "daemon off;"]

# COPY rootfs /
# https://github.com/phusion/baseimage-docker/issues/319#issuecomment-272568689
ENV DEBIAN_FRONTEND noninteractive

ENV NGINX_VERSION 1.16.1

RUN set -ex \
  && apt-get update \
  && apt-get install -y wget \
  tar \
  gzip \
  && apt-get install -y build-essential \
  libtool \
  libpcre3 \
  libpcre3-dev \
  zlib1g-dev \
  openssl \
  libssl-dev \
  && cd /tmp \
  && wget http://nginx.org/download/nginx-${NGINX_VERSION}.tar.gz \
  && tar xzf nginx-${NGINX_VERSION}.tar.gz \
  && cd /tmp/nginx-${NGINX_VERSION} \
  && ./configure \
  \
  --prefix=/usr/local/nginx \
  --sbin-path=/usr/local/nginx/sbin/nginx \
  --conf-path=/usr/local/nginx/conf/nginx.conf \
  --error-log-path=/usr/local/nginx/logs/error.log \
  \
  --user=nginx \
  --group=nginx \
  \
  --with-threads \
  \
  --with-file-aio \
  \
  --with-http_ssl_module \
  --with-http_v2_module \
  --with-http_realip_module \
  --with-http_addition_module \
  --with-http_sub_module \
  --with-http_dav_module \
  --with-http_flv_module \
  --with-http_mp4_module \
  --with-http_gunzip_module \
  --with-http_gzip_static_module \
  --with-http_auth_request_module \
  --with-http_random_index_module \
  --with-http_secure_link_module \
  --with-http_slice_module \
  --with-http_stub_status_module \
  \
  --http-log-path=/usr/local/nginx/logs/access.log \
  --http-client-body-temp-path=/usr/local/nginx/client_body_temp \
  --http-proxy-temp-path=/usr/local/nginx/proxy_temp \
  --http-fastcgi-temp-path=/usr/local/nginx/fastcgi_temp \
  --http-uwsgi-temp-path=/usr/local/nginx/uwsgi_temp \
  --http-scgi-temp-path=/usr/local/nginx/scgi_temp \
  \
  --with-mail \
  --with-mail_ssl_module \
  \
  --with-stream \
  --with-stream_ssl_module \
  --with-stream_realip_module \
  && make -j$(getconf _NPROCESSORS_ONLN) \
  && make install \
  && sed -i -e 's/#access_log  logs\/access.log  main;/access_log \/dev\/stdout;/' -e 's/#error_log  logs\/error.log  notice;/error_log stderr notice;/' /usr/local/nginx/conf/nginx.conf \
  && useradd -ms /bin/bash nginx \
  && whoami \
  && ln -s /usr/local/nginx/sbin/nginx /usr/bin/nginx \
  && rm -rf /tmp/* 

USER nginx
WORKDIR /home/nginx

# https://github.com/phusion/baseimage-docker/issues/319#issuecomment-272568689s
ENV DEBIAN_FRONTEND teletype