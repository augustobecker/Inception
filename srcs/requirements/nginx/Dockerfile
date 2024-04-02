FROM debian:oldstable

ARG CERTS_PATH
ARG CERTS_KEY

RUN apt update && apt upgrade -y && \
	apt install -y nginx && apt install openssl -y

RUN openssl req -batch -x509 -sha256 -nodes -newkey rsa:2048 -days 365 \
	-keyout $CERTS_KEY \
	-out $CERTS_PATH

COPY ./conf/nginx.conf /etc/nginx/conf.d

EXPOSE 443

CMD [ "nginx", "-g", "daemon off;" ]