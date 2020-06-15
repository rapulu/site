---
title: "How to Deploy a Laravel Application Using Caddyserver on Ubuntu"
date: 2020-06-14T23:34:16+01:00
draft: false
---

Laravel is a very popular php framework, used for making simple and complex web applications. It's literally the only php framework I'm very conversant with and most of my projects are built on it.

In this tutorial and a backup note for myself i will show you how to setup and successfully run laravel app on ubuntu 20.04 server with caddyserver v2.


## Prerequisites

I would assume you already know the following.

* ubuntu setup on the cloud service (I use Google cloud platform GCE)

* SSH into ubuntu server.

* [Install and setup PHP](/post/how-to-install-php7.4-on-ubuntu-18.04-and-20.04/)

## Let's begin

### First step

* PHP extensions

For a Laravel application to run you need PHP, PHP extensions and a PHP dependency manager called Composer just like you will do in a basic LEMP/LAMP stack.

SSH into the server and update the package manager cache.

```sudo apt-get update ```

In this tutorial I'm using PHP 7.4, I will install PHP 7.4 extensions required by Laravel, and Caddyserver plus Composer.

If you followed my previous [tutorial on how to install PHP 7.4 on ubuntu](/post/how-to-install-php7.4-on-ubuntu-18.04-and-20.04/) versions I followed it up with examples of how to install respective extensions.

FastCGI Process Manager (php-fpm) is required for caddyserver v2. You can install these extensions, Composer, and unzip (which allows Composer to handle zip files) at the same time.

You can install these extensions:

```sudo apt install php-pdo php-mbstring php-token-stream php-xml php-json php-zip php-gd php-curl php-cli php-fpm php-mysql composer```

This should do it, your machine will be ready with required php and extentions.

## Second step

* Setting Up the Laravel Application

To setup laravel application in this tutorial I'm going to pulling laravel straight from github.

But first, create a directory within the root to hold the application. My application root directory would be ```/var/www/html```.

To create the directory:

```sudo mkdir -p /var/www/html```

Give necessary permission.

* Change the directory owner and group:
  
  ```sudo chown www-data:www-data /var/www/html```

* Allow the group to write to the directory with appropriate permissions:

  ```sudo chmod -R 775 /var/www```

* Add myself to the www-data group:

  ```sudo usermod -a -G www-data [username]```

Move to the new directory and clone the demo application using Git.

```cd /var/www/html```

```git clone https://github.com/rapulu/lcmp.git .```

Git will download all of the files from the demo application repository to the current directory because I used a period (full stop) at the end of the command.


P.S: If you get permission error while doing the above logout/exit of your server from the terminal then ssh back in, this should fix the error

Next, we need to install the project dependencies. Laravel utilizes Composer to handle dependency management, which makes it easy to install the necessary packages in one go.

```composer install```

Create env file from an existing one:

```cp .env.example .env```

Generate application key:

```php artisan key:generate```

or

```php artisan key:gen```

### Now let's install caddyserver

Now that we have the laravel project on the server, let's install caddy server. In this tutorial I will be using caddy server version 2. There are various ways of installing it which can be found [here](https://caddyserver.com/docs/download).

I will show the easiest method I use which can be found in the link above too.

Copy, paste and run the following commands one after the other.

```cd ~```

```echo "deb [trusted=yes] https://apt.fury.io/caddy/ /" \ | sudo tee -a /etc/apt/sources.list.d/caddy-fury.list```

```sudo apt update```

```sudo apt install caddy```

Test that it worked:

```caddy version```

Visit the hostname/ip address if you have Apache installed then you will have to disable it, The procedure to stop Apache from starting at boot time on Ubuntu is as follows:

```sudo systemctl disable apache2 && sudo systemctl stop apache2```

You can delete the apache2 server package using the apt command/apt-get command:

```sudo apt remove apache2```

Restart caddy with the following command:

```sudo systemctl restart caddy```

Check caddy status 

```sudo systemctl status caddy```

You should have an active running caddy by now revisit host/ip address, you would see the caddy default landing page.
To edit the caddy config file named Caddyfile which is located at ```/etc/caddy/```

```sudo nano /etc/caddy/Caddyfile```

Your caddyfile should look something like this:

```
# The Caddyfile is an easy way to configure your Caddy web server.
#
# Unless the file starts with a global options block, the first
# uncommented line is always the address of your site.
#
# To use your own domain name (with automatic HTTPS), first make
# sure your domain's A/AAAA DNS records are properly pointed to
# this machine's public IP, then replace the line below with your
# domain name.
:80

# Set this path to your site's directory.
root * /usr/share/caddy

# Enable the static file server.
file_server

# Another common task is to set up a reverse proxy:
# reverse_proxy localhost:8080
```

Edit and point the root to your laravel project directory but make sure it's pointing to the laravel public folder, mine being ```/var/www/html/public``` your caddyflie should look something like this.

```
:80

# Set this path to your site's directory.
root * /var/www/html/public

encode gzip

php_fastcgi unix//var/run/php/php7.4-fpm.sock

# Enable the static file server.
file_server
```
Save and exit Caddyfile. Next add caddy to www-data user group

```sudo usermod -a -G www-data caddy```

Reload caddy service:

```sudo service caddy restart```

Your laravel project should be working now, If you encounter storage permission error, These commands should fix it:


```
sudo chgrp -R www-data storage bootstrap/cache

sudo chmod -R ug+rwx storage bootstrap/cache
```

Congratulation you have laravel running on caddy server. 
