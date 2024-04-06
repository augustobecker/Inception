FROM debian:oldstable

ARG WORDPRESS_URL
ARG CERTS_PATH
ARG CERTS_KEY

RUN apt update && apt upgrade -y && \
	apt install -y nginx && apt install openssl -y

RUN openssl req -batch -x509 -sha256 -nodes -newkey rsa:2048 -days 365 \
	-keyout $CERTS_KEY \
	-out $CERTS_PATH

COPY ./conf/nginx.conf /etc/nginx/conf.d

RUN sed -i "s|{{CERTS_PATH}}|$CERTS_PATH|g" /etc/nginx/conf.d/nginx.conf && \
    sed -i "s|{{CERTS_KEY}}|$CERTS_KEY|g" /etc/nginx/conf.d/nginx.conf && \
    sed -i "s|{{WORDPRESS_URL}}|$WORDPRESS_URL|g" /etc/nginx/conf.d/nginx.conf

EXPOSE 443

CMD [ "nginx", "-g", "daemon off;" ]
