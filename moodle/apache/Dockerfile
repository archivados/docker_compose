FROM debian:latest
ENV TERM xterm
LABEL maintainer=javierfp@iessanclemente.net

#Instalamos apache2 y php
RUN apt update && apt install -y vim git apache2 php php-common php-pear php-mysql php-curl \
    php-gd php-zip php-mbstring php-intl

#Creamos directorio de datos de moodle
RUN mkdir /var/www/moodledata && chown www-data:www-data /var/www/moodledata

#Descargamos moodle
RUN git clone -b MOODLE_311_STABLE --depth 1 https://github.com/moodle/moodle.git && \
    mv moodle /var/www/html && \
    chown -R www-data:www-data /var/www/html/moodle

#Comando para arrancar apache
ENTRYPOINT ["apachectl","-DFOREGROUND"]
