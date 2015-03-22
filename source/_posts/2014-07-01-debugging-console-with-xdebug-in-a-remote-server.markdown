---
layout: post
title: "Debugging console with XDebug in a remote server"
date: 2014-07-01 20:45:05 +0200
comments: true
categories:
  - php
  - xdebug
main_image_src: "posts/debugging-console-with-xdebug-remote-server.png"
---

These days I've been coding some commands to handle a newsletter with a work queue, so I needed the XDebug's benefits to debug the commands. In a previous post I published the [talk I did in Symfony VLC about the goodies of XDebug](/blog/2013/10/11/un-vistazo-a-xdebug/), but it lacked a part about debugging php scripts.

I usually use a dev server with Vagrant and provisioned with Puppet as dev environment. To get the basic configuration of the box I use [Puphpet](http://www.puphpet.com). Then I will explain what I did to use XDebug in this environment, that it's pretty straight forward:

1. Configure XDebug to enable the profiler
2. Configure console's environment
3. Debug!

<!-- more -->

XDebug
---------------------

The base configuration that provides Puphet normally it's like this:


```bash

xdebug.default_enable=1
xdebug.remote_autostart=0
xdebug.remote_connect_back=0
xdebug.remote_enable=1
xdebug.remote_handler=dbgp
xdebug.remote_port="9000"
xdebug.remote_host="192.168.56.1"

```

A remarkable point of this configuration is that with `xdebug.remote_connect_back=0` we tell XDebug that we will provide the IP address where it has to send the debug info. XDebug looks for `REMOTE_ADDRESS` if we connect through HTTP but in our case we would have to configure it via PHP config (`xdebug.remote_host`) or in our console environment.

In this case, as I am using a Vagrant box, I configure the host IP directly. All petitions will come from here.

Console's environment
-----------------------

In one hand we have to configure a server name in the environment. Thus our IDE will look for the path mapping configuration of this server.

```bash
export PHP_IDE_CONFIG="serverName=our-server.dev"
```

In the other hand we have to tell XDebug to start debugging.

```bash
export XDEBUG_CONFIG="idekey=PHPSTORM"
```

Debug!
------------------

To start debugging at this point is as simple as to set a breakpoint at our IDE and execute the script at the server.

```bash
php our_script.php
```

References
---------------

- [XDebug docs](http://xdebug.org/docs/remote)
- [Debugging remote CLI with phpstorm](https://www.adayinthelifeof.nl/2012/12/20/debugging-remote-cli-with-phpstorm/)
- [XDebug: how to debug remote console application? - StackOverflow](http://stackoverflow.com/questions/16518262/xdebug-how-to-debug-remote-console-application)
