# deploy-laravel-with-nginx

link yt -> https://youtu.be/a0VmEYDmiPY

## **CONTENTS**
* [**PREREQUISITES**](#prerequisites)
* [**PHP**](#php)
* [**Composer**](#composer)
* [**Mysql**](#mysql)
* [**Application Environtment**](#application-environment)
* [**Nginx**](#nginx)
* [**SSL**](#ssl)
* [**Some Errors**](#some-errors)
* [**Important Artisan Package**](#important-artisan-package)

## PREREQUISITES
**install nginx**
```
sudo apt-get install nginx
```
**cek status nginx**
```
sudo systemctl status nginx
```
**start nginx**
```
sudo systemctl start nginx
```
**firewall permission (optional)**
```
sudo ufw enable
sudo ufw allow ssh
sudo ufw allow 'Nginx HTTP'
sudo ufw allow 443
sudo ufw allow 80
sudo ufw allow 22
sudo ufw status
```
**install mysql**
```
sudo apt-get install mysql-server
sudo mysql_secure_installation
```
**checking mysql and nginx**
```
sudo systemctl status nginx
sudo systemctl status mysql
```
## PHP
I use PHP version 8.0 in this chapter because my Laravel project uses PHP version 8.0.As a result, it is dependent on your Laravel project.  
**Add odrej/php PPA**  
```
sudo apt install  ca-certificates apt-transport-https software-properties-common
sudo add-apt-repository ppa:ondrej/php
sudo apt update
```
**Install php8.0**
```
sudo apt install php8.0-common php8.0-cli -y
```
**Install dependencies package**
```
sudo apt-get install php8.0-mbstring php8.0-xml composer unzip
sudo apt-get install php8.0-curl php8.0-mysql php8.0-fpm
```
**Cek status**
```
sudo service php8.0-fpm status
```
## Composer
Make your way to your Laravel project's location and change the permissions.  
```
sudo cd /var/www/(your project location)
sudo chown -R www-data:www-data .
```
**Install composer**
```
sudo composer install
```
**Update composer (optional)**
```
sudo composer update
```
**Upgrade composer (optional)**
```
sudo run which composer (output /usr/bin/composer)
sudo run php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
sudo run sudo php composer-setup.php --install-dir=/usr/bin --filename=composer
```
## Mysql
```
mysql -u root -p -> default password is null
CREATE DATABASE nama_database;
CREATE USER 'nama_user'@'%' IDENTIFIED BY 'PASSWORD';
GRANT ALL PRIVILEGES ON nama_database.* TO 'nama_user'@'%' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```
## Application Environment
**Open .env**
```
sudo cp .env.example .env
sudo nano /var/www/(your project location)/.env
```
**Config .env**  
![Screenshot from 2021-06-01 17-03-21 (2)](https://user-images.githubusercontent.com/55046884/133882603-1993a233-5c45-4e5a-af81-5921b38fa45b.png)  
config db_database, db_username, db_password and other

**Artisan**
```
sudo php artisan key:generate
sudo php artisan migrate
```
## Nginx
**Making an nginx configuration**
```
sudo touch /etc/nginx/sites-available/example.conf
sudo nano /etc/nginx/sites-available/example.conf
```
**Fill with it**
```
server {
        listen 80;
        root /var/www/html/(your project name)/public;
        index index.php index.html index.htm index.nginx-debian.html;

        server_name (depending your server name);

        location / {
                try_files $uri $uri/ /index.php?$query_string;
        }

        location ~ \.php$ {
                include snippets/fastcgi-php.conf;
                fastcgi_pass unix:/var/run/php/php8.0-fpm.sock;
        }

        location ~ /\.ht {
                deny all;
        }
}
```
**Copy to sites-enabled**
```
sudo ln -s /etc/nginx/sites-available/example.conf /etc/nginx/sites-enabled/
```
**Running your app**
```
sudo nginx -t
sudo systemctl reload nginx
```
## SSL 
add package
```
sudo add-apt-repository ppa:certbot/certbot
sudo apt-get update
```
install certbot
```
sudo apt-get install python3-certbot-nginx
```
obtain SSL for **subdomain**
```
certbot -d subdomain.domain.com --manual --preferred-challenges dns certonly
```

## Some Errors
**This Page isn't working**  
![Untitled](https://user-images.githubusercontent.com/55046884/132945936-948a1710-e90b-4d50-b39b-4321d6247b4f.png)  
solution change storage laravel permission
```
sudo chgrp -R www-data storage bootstrap/cache
sudo chmod -R ug+rwx storage bootstrap/cache
```

## Important Artisan Package
```
php artisan storage:link
php artisan passport:install --force
```
