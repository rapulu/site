---
title: "How to Install Php7.4 on Ubuntu 18.04 and 20.04"
date: 2020-06-06T14:31:12+01:00
draft: false
---

PHP is a popular server scripting language known for creating dynamic and interactive Web pages. PHP is widely-used programming language in the Web. Use the steps below to install PHP 7.4 on Ubuntu 20.04/19.04/18.04/16.04.

## Install PHP 7.4 on Ubuntu 18.04

You need PHP PPA repository to install PHP 7.4 on ubuntu 19.04, 18.04 and 16.04.

### Step 1

Add ppa:ondrej/php PPA repository which has the latest build packages of PHP.

```

sudo apt-get update

sudo apt -y install software-properties-common

sudo add-apt-repository ppa:ondrej/php

sudo apt-get update

```

### Step 2

Install PHP 7.4 on Ubuntu 18.04, 19.04 and 16.04 with this command:

```sudo apt -y install php7.4```

Check for version:

```php -v```

For additional packages use the command below to install:

```sudo apt-get install php7.4-xxx```

or

```sudo apt-get install php7.4-{xxx, xxx, xxx}```

Example:

```sudo apt-get install -y php7.4-{mbstring,mysql,bcmath,bz2,intl,gd,zip}```


## Install PHP 7.4 on Ubuntu 20.04

Ubuntu 20.04 ships with PHP 7.4 in its main repositories. Install it and the extensions you need with the apt package manager.

```
sudo apt update

sudo apt install php php-cli php-fpm php-json php-pdo php-mysql php-zip php-gd  php-mbstring php-curl php-xml php-pear php-bcmath
```

Check to confirm PHP version:

```php --version```