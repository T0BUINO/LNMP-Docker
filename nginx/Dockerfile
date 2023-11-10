FROM debian:bullseye as builder
WORKDIR /nginx-build

RUN set -x; buildDeps='build-essential libpcre3-dev libssl-dev zlib1g-dev libgd-dev libjemalloc-dev wget git ca-certificates' \
    && apt update \
    && apt install -y $buildDeps --no-install-recommends --no-install-suggests \
    && wget https://nginx.org/download/nginx-1.24.0.tar.gz \
    && git clone https://github.com/google/ngx_brotli \
    && cd ngx_brotli \
    && git submodule update --init \
    && cd .. \
    && tar xzvf nginx-1.24.0.tar.gz \
    && cd nginx-1.24.0 \
    && ./configure --prefix=/usr/local/nginx \
        --user=www --group=www \ 
        --with-http_stub_status_module \
        --with-http_sub_module \
        --with-http_v2_module \
        --with-http_ssl_module \
        --with-stream \
        --with-stream_ssl_preread_module \
        --with-stream_ssl_module \
        --with-http_gzip_static_module \
        --with-http_realip_module \
        --with-http_flv_module \
        --with-http_mp4_module \
        --with-pcre \
        --with-pcre-jit \
        --with-ld-opt=-ljemalloc \
        --add-module=../ngx_brotli \
    && make \
    && make install \
    && rm -rf /nginx-build

FROM debian:bullseye-slim

COPY --from=builder /usr/local/nginx /usr/local/nginx
COPY --from=builder /usr/local/nginx/sbin/nginx /usr/sbin/nginx

# COPY --from=builder /usr/lib/x86_64-linux-gnu /usr/lib/x86_64-linux-gnu
# You could COPY the whole x84_64-linux-gnu folder to the last stage,
# but you could also COPY some specific files to control the image size.

COPY --from=builder /usr/lib/x86_64-linux-gnu/libjemalloc* /usr/lib/x86_64-linux-gnu/
COPY --from=builder /usr/lib/x86_64-linux-gnu/libbrotli* /usr/lib/x86_64-linux-gnu/

RUN groupadd www && useradd -g www -M -s /sbin/nologin www

EXPOSE 80 443

CMD ["nginx", "-g", "daemon off;"]