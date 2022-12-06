FROM nginx:latest

ARG ENVIRONMENT="local" // buildspec-ciで指定する
ARG PHP_FPM_CONTAINER_WWW_DATA_USER_ID=82
ARG PHP_FPM_CONTAINER_WWW_DATA_GROUP_ID=82

# デバッグ用にcurlインストール
RUN apt-get update && apt-get install -y curl && apt-get clean

RUN usermod -u ${PHP_FPM_CONTAINER_WWW_DATA_USER_ID} www-data && \
    groupmod -g ${PHP_FPM_CONTAINER_WWW_DATA_GROUP_ID} www-data

COPY ./ecs/environments/${ENVIRONMENT}/docker/nginx/conf.d /etc/nginx/conf.d
COPY ./ecs/environments/${ENVIRONMENT}/docker/nginx/conf/nginx.conf /etc/nginx/nginx.conf
RUN nginx -t

RUN apt -y update

RUN mkdir -p /var/www/app/public
COPY ./public /var/www/app/public
WORKDIR /var/www/app/public
RUN chown -R www-data:www-data /var/www/app

ENTRYPOINT ["nginx", "-g", "daemon off;"]
EXPOSE 80
