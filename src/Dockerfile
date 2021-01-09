# https://gist.github.com/hermanbanken/96f0ff298c162a522ddbba44cad31081#gistcomment-3555604

FROM nginx:alpine

RUN curl "http://nginx.org/download/nginx-${NGINX_VERSION}.tar.gz" -o nginx.tar.gz

# For latest build deps, see https://github.com/nginxinc/docker-nginx/blob/master/mainline/alpine/Dockerfile
RUN apk add --no-cache --virtual .build-deps \
  gcc \
  libc-dev \
  make \
  git \
  openssl-dev \
  pcre-dev \
  zlib-dev \
  linux-headers \
  curl \
  gnupg \
  libxslt-dev \
  gd-dev \
  geoip-dev \
  # install libmyradclient
	&& cd /tmp \
	&& git clone https://github.com/qudreams/libmyradclient \
	&& cd lib* \
	&& make all \
	&& cd /tmp \
	&& git clone https://github.com/qudreams/nginx-http-radius-module \
        && mkdir -p /usr/local/nginx/raddb/ \
	&& cp -R /tmp/nginx-http-radius-module/raddb/ /usr/local/nginx/

# Reuse same cli arguments as the nginx:alpine image used to build
RUN CONFARGS=$(nginx -V 2>&1 | sed -n -e 's/^.*arguments: //p') \
    CONFARGS=${CONFARGS/-Os -fomit-frame-pointer/-Os} && \
    mkdir /usr/src && \
	tar -zxC /usr/src -f nginx.tar.gz && \
	cd /usr/src/nginx-$NGINX_VERSION && \
	./configure --with-compat $CONFARGS --add-module=/tmp/nginx-http-radius-module && \
	# make modules && \
	make build && \
	make install && \
	make clean && \
	rm rm -rf /tmp

COPY nginx/nginx.conf /etc/nginx/nginx.conf
COPY nginx/templates /etc/nginx/templates/

ENV RADIUS_SERVER_URL ""
ENV RADIUS_SERVER_SECRET ""
ENV RADIUS_SERVER_AUTH_METHOD PAP

EXPOSE 80
STOPSIGNAL SIGTERM
