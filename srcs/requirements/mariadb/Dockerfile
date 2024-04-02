FROM debian:oldstable

ARG WORDPRESS_DATABASE
ARG WORDPRESS_USER
ARG WORDPRESS_PASSWORD

RUN apt update -y && apt upgrade && apt install mariadb-server -y

# Eesse comando RUN está adicionando essas configurações ao arquivo de configuração do MariaDB para permitir conexões de rede TCP/IP 
# e desabilitar a vinculação do MariaDB a um endereço IP específico.
# Isso pode ser útil ao executar o MariaDB dentro de um contêiner Docker e permitir que outros contêineres ou aplicativos externos se conectem a ele por meio da rede.
RUN echo "[mysqld] \n\
skip-networking=0 \n\
skip-bind-address" >> /etc/mysql/my.cnf

# Quando você cria uma imagem Docker a partir de um Dockerfile usando o comando docker build,
# cada instrução RUN é executada no contexto de construção da imagem, dentro de um contêiner temporário, que é criado para esse propósito.
# Portanto, no seu Dockerfile, os comandos RUN service mariadb start e RUN service mariadb stop não são executados diretamente no contêiner em execução, mas sim durante o processo de construção da imagem Docker.
RUN service mariadb start && \
	mariadb -u root -e \
	"CREATE DATABASE IF NOT EXISTS $WORDPRESS_DATABASE; \
	CREATE USER '$WORDPRESS_USER'@'%' identified by '$WORDPRESS_PASSWORD'; \
	GRANT ALL ON $WORDPRESS_DATABASE.* TO '$WORDPRESS_USER'@'%' IDENTIFIED BY '$WORDPRESS_PASSWORD'; \
	FLUSH PRIVILEGES;" && \
	service mariadb stop

# Quando iniciaa um container Docker com essa imagem, 
# o MariaDB será iniciado automaticamente e ficará em execução enquanto o contêiner estiver em execução.
# CMD é o primeiro comando rodado quando o container está em execução
CMD ["mysqld_safe"]
