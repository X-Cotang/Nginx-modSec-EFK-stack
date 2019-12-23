FROM nginx:latest
RUN apt-get update

WORKDIR /home
RUN apt-get install -y apt-utils autoconf automake build-essential git libcurl4-openssl-dev libgeoip-dev liblmdb-dev libpcre++-dev libtool libxml2-dev libyajl-dev pkgconf wget zlib1g-dev
RUN git clone --depth 1 -b v3/master --single-branch https://github.com/SpiderLabs/ModSecurity

WORKDIR /home/ModSecurity
RUN git submodule init && git submodule update && ./build.sh && ./configure && make && make install

WORKDIR /home
RUN git clone --depth 1 https://github.com/SpiderLabs/ModSecurity-nginx.git
RUN wget http://nginx.org/download/nginx-$NGINX_VERSION.tar.gz 
RUN tar zxvf nginx-$NGINX_VERSION.tar.gz

WORKDIR /home/nginx-$NGINX_VERSION
RUN ./configure --with-compat --add-dynamic-module=../ModSecurity-nginx && make modules && cp objs/ngx_http_modsecurity_module.so /etc/nginx/modules
#RUN apt-get install nginx-plus-module-geoip2 # install geoip2

# COPY config cung geoip :)

RUN mkdir /etc/nginx/modsec 
RUN wget -P /etc/nginx/modsec/ https://raw.githubusercontent.com/SpiderLabs/ModSecurity/v3/master/modsecurity.conf-recommended
RUN mv /etc/nginx/modsec/modsecurity.conf-recommended /etc/nginx/modsec/modsecurity.conf
RUN sed -i 's/SecRuleEngine DetectionOnly/SecRuleEngine On/' /etc/nginx/modsec/modsecurity.conf
RUN cp /home/ModSecurity/unicode.mapping /etc/nginx/modsec/unicode.mapping

# CRS
WORKDIR /home
RUN wget https://github.com/SpiderLabs/owasp-modsecurity-crs/archive/v3.0.2.tar.gz
RUN tar -xzvf v3.0.2.tar.gz
RUN mv owasp-modsecurity-crs-3.0.2 /usr/local
WORKDIR /usr/local/owasp-modsecurity-crs-3.0.2
RUN cp crs-setup.conf.example crs-setup.conf
RUN cp /usr/local/owasp-modsecurity-crs-3.0.2/rules/REQUEST-900-EXCLUSION-RULES-BEFORE-CRS.conf.example /usr/local/owasp-modsecurity-crs-3.0.2/rules/REQUEST-900-EXCLUSION-RULES-BEFORE-CRS.conf
RUN cp /usr/local/owasp-modsecurity-crs-3.0.2/rules/RESPONSE-999-EXCLUSION-RULES-AFTER-CRS.conf.example /usr/local/owasp-modsecurity-crs-3.0.2/rules/RESPONSE-999-EXCLUSION-RULES-AFTER-CRS.conf

COPY ../config/nginx.conf /etc/nginx/nginx.conf
COPY ../config/modsecurity.conf /etc/nginx/modsec/modsecurity.conf
COPY ../config/main.conf /etc/nginx/modsec/main.conf
COPY ../config/default.conf /etc/nginx/conf.d/default.conf

RUN nginx -t && nginx -s reload

RUN rm -rf /home/*