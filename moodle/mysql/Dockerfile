FROM debian:latest
ENV TERM xterm
LABEL maintainer=javierfp@iessanclemente.net

#Instalamos mysql. El export evita que pida la password para root de mysql
RUN set export DEBIAN_FRONTEND=noninteractive; apt update && apt install -y mariadb-server

#Habilitamos la escucha de mysql en todas las interfaces
RUN sed -i 's/127.0.0.1/0.0.0.0/' /etc/mysql/mariadb.conf.d/50-server.cnf

#Habilitamos el socket para mysql
RUN echo [mysqld] > /etc/mysql/conf.d/mysql.cnf
RUN echo socket=/var/lib/mysqld/mysqld.sock >> /etc/mysql/conf.d/mysql.cnf

#Arrancamos mysql, creamos base de datos y damos permisos
RUN service mysql restart && \
    echo \
    "CREATE DATABASE moodle DEFAULT CHARACTER SET utf8 COLLATE utf8_unicode_ci" | mysql -u root && \
    echo \
   "GRANT SELECT,INSERT,UPDATE,DELETE,CREATE,CREATE TEMPORARY TABLES,DROP,INDEX,ALTER ON moodle.* TO \
   'moodle'@'%' IDENTIFIED BY 'abc123.'" | mysql -u root

#Comando para arrancar mysql
ENTRYPOINT ["mysqld_safe"]
