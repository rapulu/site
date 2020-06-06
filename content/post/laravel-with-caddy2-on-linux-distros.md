---
title: "Laravel With Caddy2 on Linux Distros"
date: 2020-06-06T13:29:58+01:00
draft: true
---

Laravel is a very popular php framework, used for making simple and complex web applications. It's literally the only php framework I'm very conversant with and most of my projects are built on it.

In this tutorial and a backup note for myself i will show you how to setup and successfully run laravel app on an ubuntu server with caddyserver v2.


## Prerequisites

I would i assume you already know the following.

* ubuntu setup on of the cloud service (I use Google cloud platform GCE)

* SSH into a server.

## Let's begin

### First step - Laravel setup

For a Laravel application to run you need PHP, PHP extensions and a PHP dependency manager called Composer just like you will do in a basic LEMP/LAMP stack.

SSH into the server and update the package manager cache.

``` sudo apt-get update ```

