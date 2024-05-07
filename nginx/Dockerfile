FROM nginx:1.25.5-alpine

WORKDIR /var/www

RUN apk update && \
    apk upgrade && \
    apk add --no-cache curl

RUN rm /etc/nginx/conf.d/default.conf
COPY ./nginx.conf /etc/nginx/conf.d/nginx.conf
