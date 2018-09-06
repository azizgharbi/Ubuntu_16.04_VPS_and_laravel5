### Install apache2 + Mysql+phpmyadmin on ubuntu16.04 and install  Laravel 5

# Install apache2

* sudo apt-get update
* sudo apt-get install apache2
* sudo ufw app list
* sudo ufw app info "Apache Full"
* sudo ufw allow in "Apache Full"

# Install curl

* sudo apt-get install curl
* sudo apt-get install mysql-server

# Install PHP

* sudo apt-get install php libapache2-mod-php php-mcrypt php-mysql
* sudo nano /etc/apache2/mods-enabled/dir.conf

Change:

```
<IfModule mod_dir.c>
    DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm
</IfModule>
```

To

```
<IfModule mod_dir.c>
    DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
</IfModule>
```

* sudo systemctl restart apache2
* sudo systemctl status apache2

# Install php modules
apt-cache search php- | less

# Testing 

* sudo cd /var/www/html/
* sudo touch info.php
* vi info.php

Cut and paste :

```
<?php
phpinfo();
```

```
~Esc + i to insert 
~Esc + :wq to quite and save
```

- Serve to your_ip_adresse/info.php | it's work 

# Install phpmyadmin

* sudo apt-get install phpmyadmin php-mbstring php-gettext

* sudo phpenmod mcrypt
* sudo phpenmod mbstring

* sudo systemctl restart apache2

serve to Ip-adresse/phpmyadmin

if it didn't work :

* vi /etc/apache2/apache2.conf

add this line in the bottom of the page 

```
Include /etc/phpmyadmin/apache.conf
```

if you have a access denied problem:

* mysql -u root -p

Tape your password and put this :

```
CREATE USER 'USERNAME'@'localhost' IDENTIFIED BY 'PASSWORD';
GRANT ALL PRIVILEGES ON *.* TO 'USERNAME'@'localhost' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```

(it's work now)

# Install composer

* curl -sS https://getcomposer.org/installer | php
* sudo mv composer.phar /usr/local/bin/composer.phar
* alias composer='/usr/local/bin/composer.phar'

Run composer now

# Install Laravel

make sure to clone your application in /var/www/html

# Configuration

* sudo chgrp -R www-data /var/www/html/project
* sudo chmod -R 775 /var/www/html/project/storage

* cd /etc/apache2/sites-available
* sudo nano laravel.conf

```
<VirtualHost *:80>
    ServerName localhost

    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/html/project/public

    <Directory /var/www/html/project>
        AllowOverride All
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

* sudo a2dissite 000-default.conf
* sudo a2ensite laravel.conf
* sudo a2enmod rewrite
* sudo service apache2 restart


Good luck.











