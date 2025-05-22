# Konfigurasi Web Server (apache2), php 7.4, dan MariaDB (database MySQL) Debian 11


# <h3>Download Debian 11 v.11.11.0</h3>
link download : https://cdimage.debian.org/cdimage/archive/11.11.0/amd64/iso-cd/debian-11.11.0-amd64-netinst.iso

# <h3>Tutrial instalasi Debian : </h3>
link : https://www.debian.org/doc/manuals/debian-handbook/sect.installation-steps.id.html


# <h3>Install Apache 2 </h3>
$ apt update \
$ apt -y install apache2

# <h3> Verifikasi instaltasi Apache </h3>
$ apache2 -version

# <h3>Melihat Status Apache</h3>
$ systemctl status apache2


# <h3>Membuat Virtual Host</h3>
$ mkdir -p /var/www/demoapp.info/html \
$ chown -R $USER:$USER /var/www/demoapp.info/html \
$ chmod -R 755 /var/www/demoapp.info \
$ nano /var/www/demoapp.info/html/index.html

Contoh Halaman HTML : \
https://www.w3schools.com/html/html_basic.asp \
*tekan CTRL+X dan Y untuk menyimpan perubahan


$ nano /etc/apache2/sites-available/demoapp.info.conf

<VirtualHost *:80> \
  ServerAdmin admin@demoapp.info \
  ServerName demoapp.info \
  ServerAlias www.demoapp.info \
  DocumentRoot /var/www/demoapp.info/html \
  ErrorLog ${APACHE_LOG_DIR}/demapperror.log \
  CustomLog ${APACHE_LOG_DIR}/demoappaccess.log combined \
</VirtualHost>
*tekan CTRL+X dan Y untuk menyimpan perubahan


# <h3>Mengaktifkan Virtual Host</h3> 
$ a2ensite demoapp.info.conf --> enable host \
$ a2dissite 000-default.conf --> --> disable host

$ systemctl restart apache2 \
$ apache2ctl configtest 

$ nano /etc/apache2/conf-available/servername.conf \
ServerName testdomain.info
*tekan CTRL+X dan Y untuk menyimpan perubahan

$ sudo a2enconf servername \
$ systemctl reload apache2 \
$ sudo apache2ctl configtest 

buka browser dan ketikkan : \
http://demoapp.info

# <h3>Perintah pada apache</h3>
$ systemctl start apache2 --> menjalankan server web\
$ systemctl stop apache2 --> mematikan server web\
$ systemctl restart apache2 --> merestart server web\
$ systemctl reload apache2 --> mereload server web\
$ systemctl enable apache2 --> mengatifkan starup boot web server  \
$ systemctl disable apache2 --> menonaktifkan web server saat boot



# <h3>Install PHP 7.4</h3>
$ su\
$ apt update\
$ apt -y install php7.4

# <h3>extension php7.4</h3>
$ apt -y install php7.4-common php7.4-mysql php7.4-xml php7.4-xmlrpc \
php7.4-curl php7.4-gd php7.4-imagick php7.4-cli php7.4-dev \
php7.4-imap php7.4-mbstring php7.4-opcache php7.4-soap \
php7.4-zip php7.4-intl php7.4-cli \
imagemagick git zip libgd-dev libapache2-mod-php 


# <h3>edit php.ini</h3>
$ nano /etc/php/7.4/apache2/php.ini

upload_max_filesize = 100M \
post_max_size = 48M \
memory_limit = 512M \
max_execution_time = 600 \
max_input_vars = 5000 \
max_input_time = 1000

$ systemctl restart apache2.service\
$ systemctl enable apache2.service

# <h3>Install MariaDB</h3>
$ apt -y install mariadb-server\
$ systemctl start mariadb\
$ systemctl enable mariadb

$ mysql_secure_installation\
Enter current password for root (enter for none): \
Set root password? [Y/n] y \
New password: \
Re-enter new password: \ 
Remove anonymous users? [Y/n] y \
Disallow root login remotely? [Y/n] y \
Remove test database and access to it? [Y/n] y \
Reload privilege tables now? [Y/n] y

# <h3>membuat database baru:</h3>
mysql -u root -p \
CREATE DATABASE testdb; \
CREATE USER 'testuser' IDENTIFIED BY 'password'; \
GRANT ALL PRIVILEGES ON testdb.* TO 'testuser'; \
FLUSH PRIVILEGES; \
exit


# <h3>jika folder root web memakai .htaccess</h3>
$ nano /etc/apache2/apache2.conf

<Directory /var/www/> \
        Options Indexes FollowSymLinks \
        AllowOverride None -->All \
        Require all granted \
</Directory> \
*tekan CTRL+X dan Y untuk menyimpan perubahan


# <h3>backup database:</h3>
mysqldump -u database_username -p database_name > /home/username_SSH/www/backup/backup_database.sql

# <h3>restore database:</h3>
mysql -u database_username -p database_name < /home/username_SSH/www/backup/backup_database.sql


