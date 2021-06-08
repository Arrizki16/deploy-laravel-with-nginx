# deploy-laravel-with-nginx

## **CONTENTS**
* [**PHP**](#php)
* [**Composer**](#composer)
* [**Application Environtment**](#application-environment)
* [**Mysql**](#mysql)
* [**Nginx**](#nginx)
* [**Some Errors**](#some-errors)

## PHP
In this chapter i use php version 8.0 because my laravel project using php 8.0. So it's depending to your laravel project.  
**Add odrej/php PPA**  
```
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
sudo apt-get install php8.0-curl
```
  
## Composer
Move to your laravel project location and make sure to change permission  
```
cd /var/www/(your project location)
chown -R www-data:www-data .
```
**Install composer**
```
composer install
```

## Application Environment
```
sudo nano /var/www/(your project location)/.env
```
![Untitled](https://user-images.githubusercontent.com/55046884/120185953-0c4b7f00-c23d-11eb-82bd-cd66bbe5fdc0.png)  
Actually you just need to config APP and DB also its config depending yours. Different project will have different config. 

## Mysql  -> its optional

## Nginx
**Making nginx configuration**
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
