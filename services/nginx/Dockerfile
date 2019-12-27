ARG NGINX_VERSION
FROM nginx:${NGINX_VERSION}

ARG TZ
ARG NGINX_VERSION
ARG CONTAINER_PACKAGE_URL
ARG NGINX_INSTALL_APPS

ENV INSTALL_APPS=",${NGINX_INSTALL_APPS},"
# 如果指定了国内镜像
RUN if [ "${CONTAINER_PACKAGE_URL}" != "" ]; then \
    sed -i "s/dl-cdn.alpinelinux.org/${CONTAINER_PACKAGE_URL}/g" /etc/apk/repositories; \
    fi
# 安装certbot等应用
RUN if [ -z "${INSTALL_APPS##*,certbot,*}" ]; then \
    echo "---------- Install certbot ----------"; \
    apk add --no-cache certbot; \
    fi
# 备份原配置
# 当前工作目录为/www
RUN mv /etc/nginx/nginx.conf /etc/nginx/nginx2.conf
WORKDIR /www
