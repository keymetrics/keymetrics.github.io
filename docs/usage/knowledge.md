---
layout: docs
title: Knowledge
description: How to use PM2.
permalink: /docs/usage/knowledge/
---

## Stateless apps

We recommend (and you must) write stateless NodeJS apps. Apps that don't retain any form of local variables or local instances or whatever local.
Every data, states, websocket session, session data, must be shared via any kind of database.

We recommend using Redis for sharing session data, websocket.

- SocketIO with Redis : [https://github.com/LearnBoost/Socket.IO/wiki/Configuring-Socket.IO](Configuring SocketIO)
- Redis session store for Connect : [https://github.com/visionmedia/connect-redis](Connect-redis)

We recommend following the 12 factor convention : [http://12factor.net/](http://12factor.net/)

## Setup pm2 on a server

[How To Use pm2 to Setup a Node.js Production Environment On An Ubuntu VPS](https://www.digitalocean.com/community/articles/how-to-use-pm2-to-setup-a-node-js-production-environment-on-an-ubuntu-vps)

## Log and PID files

By default, logs (error and output), pid files, dumps, and PM2 logs are located in `~/.pm2/`:

```
.pm2/
├── dump.pm2
├── custom_options.sh
├── pm2.log
├── pm2.pid
├── logs
└── pids
```

## Execute any kind of script

In fork mode almost all options are the same as the cluster mode. But there is no [`reload`](#reloading-without-downtime) or `gracefulReload` command.

You can also exec scripts written in other languages:

```bash
$ pm2 start my-bash-script.sh -x --interpreter bash

$ pm2 start my-python-script.py -x --interpreter python
```

The interpreter is deduced from the file extension from the [following list](https://github.com/Unitech/pm2/blob/master/lib/interpreter.json).

### JSON configuration

To run a non-JS interpreter you must set `exec_mode` to `fork_mode` and `exec_interpreter` to your interpreter of choice.
For example:

```json
{
  "apps" : [{
    "name"       : "bash-worker",
    "script"     : "./a-bash-script",
    "exec_interpreter": "bash",
    "exec_mode"  : "fork_mode"
  }, {
    "name"       : "ruby-worker",
    "script"     : "./some-ruby-script",
    "exec_interpreter": "ruby",
    "exec_mode"  : "fork_mode"
  }]
}
```

## JSON app configuration via pipe from stdout

Pull-requests:
- [#273](https://github.com/Unitech/pm2/pull/273)
- [#279](https://github.com/Unitech/pm2/pull/279)

```bash
#!/bin/bash

read -d '' my_json <<_EOF_
[{
    "name"       : "app1",
    "script"     : "/home/projects/pm2_nodetest/app.js",
    "instances"  : "4",
    "error_file" : "./logz/child-err.log",
    "out_file"   : "./logz/child-out.log",
    "pid_file"   : "./logz/child.pid",
    "exec_mode"  : "cluster_mode",
    "port"       : 4200
}]
_EOF_

echo $my_json | pm2 start -
```

## Is my production server ready for PM2?

Just try the tests before using PM2 on your production server

```bash
$ git clone https://github.com/Unitech/pm2.git
$ cd pm2
$ npm install  # Or do NODE_ENV=development npm install if some packages are missing
$ npm test
```

If a test is broken please report us issues [here](https://github.com/Unitech/pm2/issues?state=open)
Also make sure you have all dependencies needed. For Ubuntu:

```bash
$ sudo apt-get install build-essential
# nvm is a Node.js version manager - https://github.com/creationix/nvm
$ wget -qO- https://raw.github.com/creationix/nvm/master/install.sh | sh
$ nvm install v0.11.14
$ nvm use v0.11.14
$ nvm alias default v0.11.14
```

## Contributing/Development mode

To hack PM2, it's very simple:

```bash
$ pm2 kill   # kill the current pm2
$ git clone my_pm2_fork.git
$ cd pm2/
$ DEBUG=* PM2_DEBUG=true ./bin/pm2 --no-daemon
```

Each time you edit the code, be sure to kill and restart PM2 to make changes taking effect.

## Install PM2 development

```bash
$ npm install git://github.com/Unitech/pm2#development -g
```

## Known bugs and workarounds

First, install the lastest PM2 version:

```bash
$ npm install -g pm2@latest
```

### Node 0.10.x doesn't free the script port when stopped in cluster_mode

Don't use the *cluster_mode* via -i option.

### User tips from issues
- [Vagrant and pm2 #289](https://github.com/Unitech/pm2/issues/289#issuecomment-42900019)
- [Start the same app on different ports #322](https://github.com/Unitech/pm2/issues/322#issuecomment-46792733)
- [Using ansible with pm2](https://github.com/Unitech/pm2/issues/88#issuecomment-49106686)
- [Cron string as argument](https://github.com/Unitech/pm2/issues/496#issuecomment-49323861)
- [Restart when process reaches a specific memory amount](https://github.com/Unitech/pm2/issues/141)
- [Sticky sessions and socket.io discussion](https://github.com/Unitech/PM2/issues/637)
- [EACCESS - understanding pm2 user/root rights](https://github.com/Unitech/PM2/issues/837)

## External resources and articles

- [Goodbye node-forever, hello pm2](http://devo.ps/blog/goodbye-node-forever-hello-pm2/)
- [https://serversforhackers.com/editions/2014/11/04/pm2/](https://serversforhackers.com/editions/2014/11/04/pm2/)
- http://www.allaboutghost.com/keep-ghost-running-with-pm2/
- http://blog.ponyfoo.com/2013/09/19/deploying-node-apps-to-aws-using-grunt
- http://www.allaboutghost.com/keep-ghost-running-with-pm2/
- http://bioselemental.com/keeping-ghost-alive-with-pm2/
- http://blog.chyld.net/installing-ghost-on-ubuntu-13-10-aws-ec2-instance-with-pm2/
- http://blog.marvinroger.fr/gerer-ses-applications-node-en-production-pm2/
- https://www.codersgrid.com/2013/06/29/pm2-process-manager-for-node-js/
- http://www.z-car.com/blog/programming/how-to-rotate-logs-using-pm2-process-manager-for-node-js
- http://yosoftware.com/blog/7-tips-for-a-node-js/
- https://www.exponential.io/blog/nodeday-2014-moving-a-large-developer-workforce-to-nodejs
- http://blog.rapsli.ch/posts/2013/2013-10-17-node-monitor-pm2.html
- https://coderwall.com/p/igdqyw
- http://revdancatt.com/2013/09/17/node-day-1-getting-the-server-installing-node-and-pm2/
- https://medium.com/tech-talk/e7c0b0e5ce3c

## Contributors

[Contributors](https://github.com/Unitech/PM2/graphs/contributors)

## Sponsors

Thanks to [Devo.ps](http://devo.ps/) and [Wiredcraft](http://wiredcraft.com/) for their knowledge and expertise.
