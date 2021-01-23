FROM debian:latest
ENV TERM xterm
LABEL maintainer=javierfp@iessanclemente.net

#Copiamos el envfile con variables de entorno al container
COPY envfile /tmp/envfile

#Instalamos mysql. El export evita que pida la password para root de mysql
RUN set export DEBIAN_FRONTEND=noninteractive; apt update && apt install -y mariadb-server

#Habilitamos la escucha de mysql en todas las interfaces
RUN sed -i 's/127.0.0.1/0.0.0.0/' /etc/mysql/mariadb.conf.d/50-server.cnf

#Habilitamos el socket para mysql
RUN echo [mysqld] > /etc/mysql/conf.d/mysql.cnf
RUN echo socket=/var/lib/mysqld/mysqld.sock >> /etc/mysql/conf.d/mysql.cnf

#Arrancamos mysql, creamos base de datos y damos permisos. Previa carga de las variables en el entorno
RUN /bin/bash -c 'source /tmp/envfile; \
    service mysql restart && \
    mysql -u root -e "CREATE DATABASE ${DBNAME} DEFAULT CHARACTER SET utf8 COLLATE utf8_unicode_ci" && \
    mysql -u root -e "GRANT SELECT,INSERT,UPDATE,DELETE,CREATE,CREATE TEMPORARY TABLES,DROP,INDEX,ALTER ON ${DBNAME}.* TO \"${DBUSER}\"@\"%\" IDENTIFIED BY \"${DBPASS}\""'

#Comando para arrancar mysql como entrypoint del container
ENTRYPOINT ["mysqld_safe"]
