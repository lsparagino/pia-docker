ARG PIA_VERSION=3.0.2
ARG NODE_SASS_VERSION=4.14.1
ARG ANGULAR_CLI_VERSION=latest

FROM alpine as source
ARG PIA_VERSION
RUN apk add --no-cache wget && mkdir /pia-$PIA_VERSION
RUN wget -qO- https://github.com/LINCnil/pia/archive/refs/tags/v$PIA_VERSION.tar.gz | tar xvz


FROM node:14 as builder
ARG PIA_VERSION=3.0.2
ARG NODE_SASS_VERSION=4.14.1
ARG ANGULAR_CLI_VERSION=latest

COPY --from=source /pia-$PIA_VERSION /build/pia-$PIA_VERSION
ENTRYPOINT ["tail", "-f", "/dev/null"]

USER root

WORKDIR /build/pia-$PIA_VERSION

RUN npm install -g @angular/cli@$ANGULAR_CLI_VERSION
RUN rm package-lock.json
RUN npm i
RUN npm install node-sass@$NODE_SASS_VERSION
RUN ng build --prod --build-optimizer
RUN mv /build/pia-$PIA_VERSION /build/pia

FROM nginx:1.21

RUN mkdir -p /var/www && chown www-data. /var/www/

COPY ./conf/cnil_pia.conf /etc/nginx/conf.d/default.conf

COPY --from=builder /build/pia/dist/pia /var/www/
