---
layout: post
title: "Un vistazo a Xdebug"
date: 2013-10-11 17:15
comments: true
categories: [xdebug, php]
main_image_src: "posts/un-vistazo-a-xdebug.jpg"
main_image_index_src: "posts/un-vistazo-a-xdebug-min.jpg"
---

El pasado viernes 4 de octubre estuve en la reunión mensual de [@Symfony_VLC](https://twitter.com/Symfony_VLC) dando una charla sobre Xdebug. No es que sea un *ninja*, pero es una herramienta que me facilita bastante algunas cosas del trabajo diario.

<!-- more -->

Una de las cosas para las que viene bien es que te obliga a indagar un poco más, sea repasándote la documentación o viendo algunos posts.

Para la preparación de la charla encontré algunos enlaces que me parecen interesantes (aparte de la [documentación](http://xdebug.org/docs/)):


   * [Debug PHP CLI on Remote Server with Xdebug and PHPStorm](http://devincharge.com/debug-cli-remote-server/)
   * [Remote debugging with vim and xdebug](http://mark-story.com/posts/view/remote-debugging-with-vim-and-xdebug)
   * [Vdebug](https://github.com/joonty/vdebug)

En la charla mostré un pequeño ejemplo de cómo configurar Xdebug con PHPStorm en un servidor de [desarrollo montado sobre Vagrant](https://github.com/pedronofuentes/symfonyvalencia-vagrant). Al archivo de configuración de Puppet del repositorio hay que añadirle la configuración de la IP del host remoto:

``` plain maifests/default.pp https://github.com/pedronofuentes/symfonyvalencia-vagrant/blob/master/manifests/default.pp
puphpet::ini { 'xdebug':
     value => [
          …
          'debug.remote_host = 192.168.56.1',
          …
     ],
     ini     => '/etc/php5/conf.d/zzz_xdebug.ini',
     notify     => Service['apache'],
     require      => Class['php'],
}
```

--

{% raw %}
<script async class="speakerdeck-embed" data-id="813b5e50140601318e8326b064201532" data-ratio="1.33333333333333" src="//speakerdeck.com/assets/embed.js"></script>
{% endraw %}
