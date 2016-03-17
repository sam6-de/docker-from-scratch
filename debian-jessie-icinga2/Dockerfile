FROM bundeskanzler4711/debian-jessie-base

RUN apt-get -y update 
RUN apt-get -y --force-yes dist-upgrade
RUN echo "deb http://debmon.org/debmon debmon-jessie main" | tee /etc/apt/sources.list.d/debmon.list
RUN wget -O - http://debmon.org/debmon/repo.key 2>/dev/null | apt-key add -
RUN apt-get update
RUN DEBIAN_FRONTEND=noninteractive apt-get -y --force-yes install icinga2 graphite-web graphite-carbon libapache2-mod-wsgi apache2

RUN icinga2 feature enable graphite
RUN systemctl enable icinga2.service
RUN service icinga2 restart

RUN graphite-manage syncdb --noinput
RUN chown _graphite:_graphite /var/lib/graphite/graphite.db
RUN a2enmod wsgi

RUN echo "Listen 8000" >> /etc/apache2/ports.conf
RUN cp /usr/share/graphite-web/apache2-graphite.conf /etc/apache2/sites-available/graphite.conf
RUN sed -i 's/:80/:8000/g' /etc/apache2/sites-available/graphite.conf
RUN a2ensite graphite
RUN systemctl enable apache2.service
RUN service apache2 restart

RUN apt-get -y --force-yes install apt-transport-https
RUN echo "deb https://packagecloud.io/grafana/stable/debian/ jessie main" | tee /etc/apt/sources.list.d/grafana.list
RUN wget -O - https://packagecloud.io/gpg.key 2>/dev/null | apt-key add -
RUN apt-get update
RUN apt-get -y --force-yes install grafana
RUN systemctl enable grafana-server.service
RUN service grafana-server start

RUN DEBIAN_FRONTEND=noninteractive apt-get -y --force-yes install mysql-server mysql-client icinga2-ido-mysql
RUN icinga2 feature enable ido-mysql
RUN icinga2 feature enable command
RUN icinga2 feature list
RUN addgroup --system icingacmd
RUN usermod -a -G icingacmd www-data
RUN apt-get -y --force-yes install icingaweb2
RUN icingacli setup config webserver apache --document-root /usr/share/icingaweb2/public > /etc/apache2/sites-available/icingaweb2.conf
RUN a2dissite 000-default.conf
RUN a2ensite icingaweb2.conf
RUN icingacli setup config directory --group icingaweb2;
RUN icingacli setup token create;
RUN service apache2 restart

RUN apt-get -y --force-yes autoremove
RUN apt-get clean

EXPOSE 80
EXPOSE 8000
EXPOSE 3000

ENTRYPOINT service icinga2 start
ENTRYPOINT service grafana-server start
ENTRYPOINT service icinga2 start && service grafana-server start && service apache2 start && bash

