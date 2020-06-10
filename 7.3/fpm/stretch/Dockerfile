FROM beecomprg/docker-php-fpm-magento2:7.3.18-stretch-4

# http://nginx.org/en/download.html
ENV NGINX_VERSION 1.17.9

# https://www.openssl.org/source
ENV OPENSSL_VERSION 1.1.1c

RUN useradd -r -s /usr/sbin/nologin nginx \
	&& mkdir -p /var/log/nginx /var/cache/nginx \
	&& apt update \
	&& apt -y --no-install-recommends install \
		curl \
		unzip \
		wget \
		git-core \
		autoconf \
		automake \
		libtool \
		build-essential \
		zlib1g-dev \
		libpcre3-dev \
		libxslt1-dev \
		libxml2-dev \
		libgd2-xpm-dev \
		libgeoip-dev \
		libgoogle-perftools-dev \
		libperl-dev \
		uuid-dev \
		procps \
		supervisor \
	&& mkdir -p /var/log/supervisor \
	&& wget -q -O - -c https://www.openssl.org/source/openssl-${OPENSSL_VERSION}.tar.gz --tries=3 --no-check-certificate | tar xzf - -C /tmp \
	&& wget -q -O - -c http://nginx.org/download/nginx-${NGINX_VERSION}.tar.gz | tar xzf - -C /tmp \
	&& cd /tmp/nginx-${NGINX_VERSION} \
	&& ./configure \
		--prefix=/etc/nginx  \
		--sbin-path=/usr/sbin/nginx  \
		--conf-path=/etc/nginx/nginx.conf  \
		--error-log-path=/var/log/nginx/error.log \
		--http-log-path=/var/log/nginx/access.log \
		--pid-path=/var/run/nginx.pid \
		--lock-path=/var/run/nginx.lock \
		--http-client-body-temp-path=/var/cache/nginx/client_temp \
		--http-proxy-temp-path=/var/cache/nginx/proxy_temp \
		--http-fastcgi-temp-path=/var/cache/nginx/fastcgi_temp  \
		--http-uwsgi-temp-path=/var/cache/nginx/uwsgi_temp  \
		--http-scgi-temp-path=/var/cache/nginx/scgi_temp  \
		--user=nginx  \
		--group=nginx  \
		--with-http_ssl_module  \
		--with-http_realip_module  \
		--with-http_addition_module  \
		--with-http_sub_module  \
		--with-http_dav_module  \
		--with-http_flv_module  \
		--with-http_mp4_module  \
		--with-http_gunzip_module  \
		--with-http_gzip_static_module  \
		--with-http_random_index_module  \
		--with-http_secure_link_module \
		--with-http_stub_status_module  \
		--with-http_auth_request_module  \
		--with-debug  \
		--without-http_autoindex_module \
		--without-http_ssi_module \
		--with-threads  \
		--with-stream  \
		--with-stream_ssl_module  \
		--with-mail  \
		--with-mail_ssl_module  \
		--with-file-aio  \
		--with-http_v2_module \
		--with-cc-opt='-g -O2 -fstack-protector --param=ssp-buffer-size=4 -Wformat -Werror=format-security -Wp,-D_FORTIFY_SOURCE=2'  \
		--with-ld-opt='-Wl,-Bsymbolic-functions -Wl,-z,relro -Wl,--as-needed' \
		--with-ipv6 \
		--with-pcre-jit \
		--with-openssl=/tmp/openssl-${OPENSSL_VERSION} \
	&& make \
	&& make install \
	&& apt autoremove -yqq \
	&& apt clean \
	&& rm -Rf /tmp/* /var/tmp/* /var/lib/apt/lists/*

COPY ./etc/nginx/ /etc/nginx/

ADD ./conf/supervisord.conf /etc/supervisor/conf.d/supervisord.conf

EXPOSE 80 443

STOPSIGNAL SIGTERM

WORKDIR /var/www/html

CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/conf.d/supervisord.conf"]
