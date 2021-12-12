---
title: Easy Authentication with Breeze
date: "2021-12-01 02"
description: Adding authentication with Laravel Breeze.
---

## Installation

Laravel Breeze offers a minimal set up of Laravel authentication features. It comes with login, registration, password reset etc.

The installation is simple. First I run:

```bash
php artisan migrate
```

This creates the migration for the users table

I then install Breeze using composer

```bash
composer require laravel/breeze --dev
```

I then run the `sail artisan breeze:install` command.

The creates all the required routes and authentication features. It also provides helpful components for inputs and dropdowns.


### A Small Warning

Please be aware when installing breeze, files will be overwritten. 

I was unaware of this and due to having the same filenames for some of my views. I lost an hour or two of progress due to files being overwritten.

