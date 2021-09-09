# deploy-laravel-with-nginx

## **CONTENTS**
* [**PREREQUISITES**](#prerequisites)
* [**PHP**](#php)
* [**Composer**](#composer)
* [**Mysql**](#mysql)
* [**Application Environtment**](#application-environment)
* [**Nginx**](#nginx)
* [**Some Errors**](#some-errors)

## PREREQUISITES
install nginx
```
sudo apt-get install nginx
```
nginx firewall permission
```
sudo ufw enable
sudo ufw allow ssh
sudo ufw allow 'Nginx HTTP'
sudo ufw status
```
install mysql
```
sudo apt-get install mysql-server
mysql_secure_installation
```
cek mysql nginx
```
systemctl status nginx
systemctl status mysql
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
  
## Composer
Make your way to your Laravel project's location and change the permissions.  
```
cd /var/www/(your project location)
chown -R www-data:www-data .
```
**Install composer**
```
composer install
```
**Upgrade composer**
```

```
## Mysql
```
mysql -u root -p
CREATE DATABASE testing;
CREATE USER 'root'@'%' IDENTIFIED BY 'PASSWORD';
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```
## Application Environment
```
sudo nano /var/www/(your project location)/.env
```
![Untitled](https://user-images.githubusercontent.com/55046884/120185953-0c4b7f00-c23d-11eb-82bd-cd66bbe5fdc0.png)  
Actually, you only need to configure the APP and DB configurations. Different configurations will be used for different projects.

## Nginx
**Making an nginx configuration**
```
touch /etc/nginx/sites-available/example.conf
sudo nano /etc/nginx/sites-available/example.conf
```
**Fill with it**
```
server {
        listen 80;
        root /var/www/(your project name)/public;
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
nginx -t
systemctl reload nginx
```

## Some Errors
