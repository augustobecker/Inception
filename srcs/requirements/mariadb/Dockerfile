FROM debian:oldstable

ARG WORDPRESS_ROOT_PASSWORD
ARG WORDPRESS_DATABASE
ARG WORDPRESS_USER
ARG WORDPRESS_PASSWORD

RUN apt update && apt install mariadb-server -y

RUN echo "[mysqld] \n\
skip-networking=0 \n\
skip-bind-address" >> /etc/mysql/my.cnf

RUN if [ -f backup.sql ]; then \
        mysql -uroot -p$WORDPRESS_ROOT_PASSWORD $WORDPRESS_DATABASE < backup.sql; \
    fi && \
    service mariadb start && \
    mariadb -u root -e \
    "CREATE DATABASE IF NOT EXISTS $WORDPRESS_DATABASE; \
    CREATE USER '$WORDPRESS_USER'@'%' identified by '$WORDPRESS_PASSWORD'; \
    GRANT ALL ON $WORDPRESS_DATABASE.* TO '$WORDPRESS_USER'@'%' IDENTIFIED BY '$WORDPRESS_PASSWORD'; \
    CREATE USER 'root'@'%' IDENTIFIED BY '$MYSQL_ROOT_PASSWORD'; \
    GRANT ALL ON *.* TO 'root'@'%' IDENTIFIED BY '$WORDPRESS_ROOT_PASSWORD' WITH GRANT OPTION; \
    FLUSH PRIVILEGES;" && \
    service mariadb stop

CMD ["mysqld_safe"]
