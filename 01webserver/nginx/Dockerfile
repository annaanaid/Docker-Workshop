FROM nginx:mainline-alpine
#see https://github.com/nginxinc/docker-nginx/blob/866b071f099f96898563f9a003c2dbb03bb90339/mainline/stretch/Dockerfile 
LABEL MAINTAINER=<christian.strobel.cst@gmail.com>
ARG VAR

ENV TESTVAR=${VAR:-default}

RUN rm /etc/nginx/conf.d/* && echo "var=${VAR:-default}"
ADD conf.conf /etc/nginx/conf.d/
ADD www/index.html /usr/share/nginx/html/