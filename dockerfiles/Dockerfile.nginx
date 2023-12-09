FROM debian:bullseye as builder
WORKDIR /nginx-build

COPY env.list ./

RUN set -x; . ./env.list \
    && apt update \
    && apt install -y $build_deps --no-install-recommends --no-install-suggests \
    && echo $wget_list | xargs -n 1 wget \
    && bash -c "$brotli_install" \
    && for file in *.tar.gz; do tar -zxf "$file"; done \
    && cd nginx-1.24.0 \
    && ./configure --prefix=/usr/local/nginx \
        --user=www --group=www \ 
        $module_list \
    && make \
    && make install \
    && mkdir /lib-to-copy \
    && ldd /usr/local/nginx/sbin/nginx | grep '=>' | cut -d '>' -f2 | cut -d '(' -f1 | cut -d ' ' -f2 > /lib.list \
    && sed -i '/^\/lib\/x86_64-linux-gnu\//d' /lib.list \
    && xargs -a /lib.list cp -t /lib-to-copy/

FROM debian:bullseye-slim as prod

COPY --from=builder /usr/local/nginx /usr/local/nginx
COPY --from=builder /usr/local/nginx/sbin/nginx /usr/sbin/nginx

# COPY --from=builder /usr/lib/x86_64-linux-gnu /usr/lib/x86_64-linux-gnu
# You could COPY the whole x84_64-linux-gnu folder to the last stage,
# but you could also COPY some specific files to control the image size.

# COPY --from=builder /usr/lib/x86_64-linux-gnu/libjemalloc* /usr/lib/x86_64-linux-gnu/
# COPY --from=builder /usr/lib/x86_64-linux-gnu/libbrotli* /usr/lib/x86_64-linux-gnu/

# Now, commands in line 34-37 could automatically generate a dynamic libraries folder to copy.
COPY --from=builder /lib-to-copy/* /usr/lib/x86_64-linux-gnu/

RUN groupadd www && useradd -g www -M -s /sbin/nologin www

EXPOSE 80 443

CMD ["nginx", "-g", "daemon off;"]