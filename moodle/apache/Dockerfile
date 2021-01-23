FROM debian:latest
ENV TERM xterm
LABEL maintainer=javierfp@iessanclemente.net

#Copiamos el envfile con variables de entorno al container
COPY envfile /tmp/envfile

#Instalamos apache2 y php tomando los paquetes de la variable PACKAGES
RUN /bin/bash -c 'source /tmp/envfile; \
    apt update && apt install -y $PACKAGES'

#Creamos directorio de datos de moodle
RUN mkdir /var/www/moodledata && chown www-data:www-data /var/www/moodledata

#Descargamos moodle y movemos al directorio adecuado
RUN /bin/bash -c 'source /tmp/envfile; \
    git clone -b $VERSION --depth 1 https://github.com/moodle/moodle.git && \
    mv moodle /var/www/html && \
    chown -R www-data:www-data /var/www/html/moodle'

#Comando para arrancar apache
ENTRYPOINT ["apachectl","-DFOREGROUND"]
