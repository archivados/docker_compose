FROM debian:buster

LABEL maintainer=javierfp@iessanclemente.net

ENV TERM xterm
ENV PASS=abc123.
ENV PATH /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/sbin

LABEL distribucion="Wordpress sobre LAMP en Debian 10"
LABEL descripcion="Imagen Debian con Wordpress para prácticas"
LABEL paquetes="curl"
LABEL acceso="usuario: root, password: abc123."

#Copiamos el entrypoint.sh al container
COPY entrypoint.sh /usr/bin/entrypoint.sh

#Damos permiso al entrypoint.sh
RUN chmod +x /usr/bin/entrypoint.sh

#Instalamos paquetes LAMP
RUN apt update && \
	apt install -y vim iputils-ping nano procps \
	apache2 mariadb-server mariadb-client php libapache2-mod-php php-gd php-mysql php-curl php-mbstring php-xml php-xmlrpc php-soap php-intl php-zip

#Configuración de mysql
RUN service mysql start && \
	echo "CREATE DATABASE wordpress_db;" | mysql -uroot && \
	echo "GRANT ALL ON wordpress_db.* TO 'wordpress_user'@'localhost' IDENTIFIED BY '$PASS';" | mysql -uroot && \
	echo "FLUSH PRIVILEGES;" | mysql -uroot

#Instalamos curl
RUN apt update && apt install -y curl

#Descarga de wordpress
RUN cd /var/www/html && \
	curl -O https://wordpress.org/latest.tar.gz && \
	tar -xvf latest.tar.gz && \
	rm latest.tar.gz

#Configurando entorno de wordpress
RUN chown -R www-data:www-data /var/www/html/wordpress && \
	find /var/www/html/wordpress/ -type d -exec chmod 750 {} \; && \
	find /var/www/html/wordpress/ -type f -exec chmod 640 {} \; && \
	cd /var/www/html/wordpress && \
	mv wp-config-sample.php wp-config.php

#Editamos el archivo wp-config.php
RUN sed -i 's/database_name_here/wordpress_db/' /var/www/html/wordpress/wp-config.php && \
	sed -i 's/username_here/wordpress_user/' /var/www/html/wordpress/wp-config.php && \
	sed -i 's/password_here/abc123./' /var/www/html/wordpress/wp-config.php

#Copiamos el virtualhost
COPY 000-default.conf /etc/apache2/sites-available/000-default.conf

#Activamos mod_rewrite
RUN set -x; \
	a2enmod rewrite

#Comando para arrancar ssh y entrar en bucle de espera
ENTRYPOINT ["/usr/bin/entrypoint.sh"]
