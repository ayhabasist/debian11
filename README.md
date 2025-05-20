# Konfigurasi Web Server (apache2), php 7.4, dan MariaDB (database MySQL) Debian 11

# Install Apache 2 
$ apt update\
$ apt -y install apache2

# Verifikasi instaltasi Apache
$ apache2 -version

# Melihat Status Apache
$ systemctl status apache2


# Membuat Virtual Host
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


# Mengaktifkan Virtual Host 
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

# Perintah pada apache
$ systemctl start apache2 --> menjalankan server web\
$ systemctl stop apache2 --> mematikan server web\
$ systemctl restart apache2 --> merestart server web\
$ systemctl reload apache2 --> mereload server web\
$ systemctl enable apache2 --> mengatifkan starup boot web server  \
$ systemctl disable apache2 --> menonaktifkan web server saat boot



# Install PHP 7.4
$ su\
$ apt update\
$ apt -y install php7.4

# extension php7.4
$ apt -y install php7.4-common php7.4-mysql php7.4-xml php7.4-xmlrpc \
php7.4-curl php7.4-gd php7.4-imagick php7.4-cli php7.4-dev \
php7.4-imap php7.4-mbstring php7.4-opcache php7.4-soap \
php7.4-zip php7.4-intl php7.4-cli \
imagemagick git zip libgd-dev libapache2-mod-php 


# edit php.ini
$ nano /etc/php/7.4/apache2/php.ini

upload_max_filesize = 100M \
post_max_size = 48M \
memory_limit = 512M \
max_execution_time = 600 \
max_input_vars = 5000 \
max_input_time = 1000

$ systemctl restart apache2.service\
$ systemctl enable apache2.service

# Install MariaDB
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

# membuat database baru:\
mysql -u root -p \
CREATE DATABASE testdb; \
CREATE USER 'testuser' IDENTIFIED BY 'password'; \
GRANT ALL PRIVILEGES ON testdb.* TO 'testuser'; \
FLUSH PRIVILEGES; \
exit


# jika folder root web memakai .htaccess
$ nano /etc/apache2/apache2.conf

<Directory /var/www/> \
        Options Indexes FollowSymLinks \
        AllowOverride None -->All \
        Require all granted \
</Directory> \
*tekan CTRL+X dan Y untuk menyimpan perubahan
