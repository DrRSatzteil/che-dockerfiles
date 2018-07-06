# Copyright (c) 2012-2016 Codenvy, S.A.
# All rights reserved. This program and the accompanying materials
# are made available under the terms of the Eclipse Public License v1.0
# which accompanies this distribution, and is available at
# http://www.eclipse.org/legal/epl-v10.html
# Contributors:
# Codenvy, S.A. - initial API and implementation

FROM localhost:5000/myubuntu_stackbase

ENV DEBIAN_FRONTEND noninteractive
ENV CHE_MYSQL_PASSWORD=che
ENV CHE_MYSQL_DB=che_db
ENV CHE_MYSQL_USER=che

# install php with a set of most widely used extensions
RUN sudo sudo ln -fs /usr/share/zoneinfo/Europe/Berlin /etc/localtime && \
sudo add-apt-repository ppa:ondrej/php && \
sudo apt-get update && \
sudo apt-get install -y \
apache2 \
php7.1 \
php7.1-curl \
php7.1-mysql \
php7.1-gd \
libapache2-mod-php7.1 \
php7.1-cli \
php7.1-json \
php7.1-cgi \
php7.1-sqlite3 \
php7.1-dom \
php7.1-mbstring \
php7.1-xml \
php7.1-zip \
libcap2-bin \
npm

RUN sudo sed -i 's/\/var\/www\/html/\/projects/g' /etc/apache2/sites-available/000-default.conf && \
sudo sed -i 's/\/var\/www/\/projects/g' /etc/apache2/apache2.conf && \
sudo sed -i 's/None/All/g' /etc/apache2/sites-available/000-default.conf && \
echo "ServerName localhost" | sudo tee -a /etc/apache2/apache2.conf && \
sudo a2enmod rewrite

# Install the Zend Debugger php module
RUN sudo wget http://repos.zend.com/zend-server/9.1.3/deb_apache2.4/pool/zend-server-php-7.1-common_9.1.3+b165_amd64.deb && \
dpkg-deb --fsys-tarfile zend-server-php-7.1-common_9.1.3+b165_amd64.deb | sudo tar -xf - --strip-components=7 ./usr/local/zend/lib/debugger/php-7.1.x/ZendDebugger.so && \
sudo rm zend-server-php-7.1-common_9.1.3+b165_amd64.deb && \
sudo mv ZendDebugger.so /usr/lib/php/20160303 && \
sudo sh -c 'echo "; configuration for php ZendDebugger module\n; priority=90\nzend_extension=ZendDebugger.so" > /etc/php/7.1/mods-available/zenddebugger.ini' && \
sudo ln -s ../../mods-available/zenddebugger.ini /etc/php/7.1/cli/conf.d/90-zenddebugger.ini && \
sudo ln -s ../../mods-available/zenddebugger.ini /etc/php/7.1/apache2/conf.d/90-zenddebugger.ini && \
sudo sed -i 's/;opcache.enable=0/opcache.enable=0/g' /etc/php/7.1/apache2/php.ini && \
sudo setcap 'cap_net_bind_service=+ep' /usr/sbin/apache2 && \
sudo chmod -R 777 /var/run/apache2 /var/lock/apache2 /var/log/apache2

# Install NodeJS to improve startup time when the JSON language server is enabled
RUN curl -sL https://deb.nodesource.com/setup_6.x | sudo bash - && \
sudo apt-get update && \
sudo apt-get install -y nodejs

RUN sudo apt-get clean && \
    sudo apt-get -y autoremove && \
    sudo apt-get -y clean && \
    sudo rm -rf /var/lib/apt/lists/*

# label is used in Servers tab to display mapped port for Apache process on 80 port in the container
LABEL che:server:80:ref=apache2 che:server:80:protocol=http

EXPOSE 80 3306
