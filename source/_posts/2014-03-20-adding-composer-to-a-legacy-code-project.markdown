---
layout: post
title: "Adding Composer to a legacy code project"
date: 2014-03-20 23:26:12 +0100
comments: true
categories: 
    - PHP
    - composer
    - legacy code
main_image_src: "posts/dude-there-is-a-lot-of-work-here.jpg"
---


In a project of legacy code that I'm working with we have the needing to use some external libraries in order to do some tasks. Also, we planned to refactor the code step by step to introduce good practices, reduce coupling, code duplication, etc.

At this point we had to implement a cron job to send users like a newsletter. This task was the perfect opportunity to create the basic scaffold using [Symfony components](http://symfony.com/components) as it is isolated from the rest of the web application: we consume the data, but almost all the funcionality is new. Then, the first step was to introduce [Composer](https://getcomposer.org/) to manage the vendor libraries and autoloading.

<!-- more -->

## Context

Before we start, some points about the project:

- The project doesn't follow any naming or structure convention. It uses classes and namespacing, but as an example the class names are lowercase and variable names are uppercase.
- The code has an autoloading function, but the intention is to replace it with Composer's autoloading.
- There are some libraries included in the folder structure.
- There are some files with functions defined between class files.

The project has this folder structure:

```bash
.
|____core
|____imagenes
|____modulos
|____objetos
```

The folders containing classes are `core` and `modulos`, the other ones contain assets such images, javascript or templates.

## Goal

The goal is to configure Composer to:

- Handle the autoloading of existing code.
- Handle the autoloading of new code following [PHP-FIG](http://www.php-fig.org/) standards.
- Include files with functions.
- Handle vendor libraries.

Then we decided to add the known `vendor` folder and `src` for the new code.

## Configuring Composer

### Autoloading

The first step was configure autoloading. Insted the existing code naming is weird, it respects a class per file and the folder structure corresponding with the namespace, thus we can say that "follows" the [PSR-0](http://www.php-fig.org/psr/psr-0/) for autoloading. For the new code we will follow the [PSR-4](http://www.php-fig.org/psr/psr-4/). Then we can configure autoloading like this:

```json composer.json
{
    "autoload": {
        "psr-4": {
            "Acme\\": "src/"
        },
        "psr-0": {
            "": "./"
        }
    }
}
```

### Previous existing libraries

At this point we have some existing libraries included in the code without namespace. Then we have to configure Composer to include them using the [`classmap` references](https://getcomposer.org/doc/04-schema.md#classmap).

For example we are using the [TCPDF](http://www.tcpdf.org) library and is stored in `core/app/libs`:

```json composer.json
{
    "autoload": {
        "classmap": [
            "core/app/libs/tcpdf.php"
        ]
    }
}
```

### Files with functions

Finally to include individual files in the project we use the [files autoloading](https://getcomposer.org/doc/04-schema.md#files) implemented. For example, we have `modulos/sistema/funciones.php` that we want to include automatically:

```json composer.json
{
    "autoload": {
        "files": [
            "modulos/sistema/funciones.php"
        ]
    }
}
```
