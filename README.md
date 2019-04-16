# ubuntucloud

sudo apt install php7.1 libapache2-mod-php7.1 php7.1-common php7.1-mbstring php7.1-xmlrpc php7.1-soap php7.1-apcu php7.1-smbclient php7.1-ldap php7.1-redis php7.1-gd php7.1-xml php7.1-intl php7.1-json php7.1-imagick php7.1-mysql php7.1-cli php7.1-mcrypt php7.1-ldap php7.1-zip php7.1-curl

sudo nano /etc/php/7.1/apache2/php.ini

file_uploads = On
allow_url_fopen = On
memory_limit = 256M
upload_max_filesize = 100M
display_errors = Off
date.timezone = America/Chicago



CREATE DATABASE nextcloud;

CREATE USER 'nextclouduser'@'localhost' IDENTIFIED BY 'new_password_here';
GRANT ALL ON nextcloud.* TO 'nextclouduser'@'localhost' IDENTIFIED BY 'user_password_here' WITH GRANT OPTION;
FLUSH PRIVILEGES;
EXIT;


cd /tmp && wget https://download.nextcloud.com/server/releases/nextcloud-11.0.1.zip
unzip nextcloud-11.0.1.zip
sudo mv nextcloud /var/www/html/nextcloud/

sudo chown -R www-data:www-data /var/www/html/nextcloud/
sudo chmod -R 755 /var/www/html/nextcloud/

sudo nano /etc/apache2/sites-available/nextcloud.conf

<VirtualHost *:80>
     ServerAdmin admin@example.com
     DocumentRoot /var/www/html/nextcloud/
     ServerName example.com
     ServerAlias www.example.com
  
     Alias /nextcloud "/var/www/html/nextcloud/"

     <Directory /var/www/html/nextcloud/>
        Options +FollowSymlinks
        AllowOverride All
        Require all granted
          <IfModule mod_dav.c>
            Dav off
          </IfModule>
        SetEnv HOME /var/www/html/nextcloud
        SetEnv HTTP_HOME /var/www/html/nextcloud
     </Directory>

     ErrorLog ${APACHE_LOG_DIR}/error.log
     CustomLog ${APACHE_LOG_DIR}/access.log combined

</VirtualHost>




sudo a2ensite nextcloud.conf
sudo a2enmod rewrite
sudo a2enmod headers
sudo a2enmod env
sudo a2enmod dir
sudo a2enmod mime


sudo systemctl restart apache2.service
