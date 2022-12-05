FROM nginx:latest

ARG ENVIRONMENT="local" // buildspec-ciで指定する

# デバッグ用にcurlインストール
RUN apt-get update && apt-get install -y curl && apt-get clean

# php-fpmコンテナのuser-dataのuid,gidに合わせる
RUN usermod -u 82 www-data && \
    groupmod -g 82 www-data

COPY ./ecs/environments/${ENVIRONMENT}/docker/nginx/conf.d /etc/nginx/conf.d
COPY ./ecs/environments/${ENVIRONMENT}/docker/nginx/conf/nginx.conf /etc/nginx/nginx.conf
RUN nginx -t

RUN apt -y update

RUN mkdir -p /var/www/app
COPY ./src/ /var/www/app/
WORKDIR /var/www/app

EXPOSE 80