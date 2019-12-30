# php版本在.env中定义
ARG PHP_VERSION
FROM ${PHP_VERSION}

ARG TZ
ARG PHP_EXTENSIONS
ARG CONTAINER_PACKAGE_URL


RUN sed -i "s/dl-cdn.alpinelinux.org/${CONTAINER_PACKAGE_URL}/g" /etc/apk/repositories


COPY ./extensions /tmp/extensions
WORKDIR /tmp/extensions
RUN chmod +x install.sh \
  && sh install.sh \
  && rm -rf /tmp/extensions


RUN apk --no-cache add tzdata \
  && cp "/usr/share/zoneinfo/$TZ" /etc/localtime \
  && echo "$TZ" > /etc/timezone


# Fix: https://github.com/docker-library/php/issues/240
RUN apk add gnu-libiconv --no-cache --repository http://${CONTAINER_PACKAGE_URL}/alpine/edge/community/ --allow-untrusted
ENV LD_PRELOAD /usr/lib/preloadable_libiconv.so php


# 安装 composer并改变它的cache home
RUN curl -o /usr/bin/composer https://mirrors.aliyun.com/composer/composer.phar \
  && chmod +x /usr/bin/composer \
  # composer也使用阿里云镜像
  && composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/
ENV COMPOSER_HOME=/tmp/composer
# 修改环境变量
ENV  PATH ${COMPOSER_HOME}/vendor/bin:$PATH

# php image's www-data user uid & gid are 82, change them to 1000 (primary user)
RUN apk --no-cache add shadow && usermod -u 1000 www-data && groupmod -g 1000 www-data


WORKDIR /
