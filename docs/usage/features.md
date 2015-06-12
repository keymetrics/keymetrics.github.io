---
layout: docs
title: Features
description: How to use features of PM2.
permalink: /docs/usage/features/
---

## Managing applications states

PM2 is a process manager.
It manages your applications states, so you can start, stop, restart and *delete* processes.

Start a process:

```bash
$ pm2 start app.js --name "my-api"
$ pm2 start web.js --name "web-interface"
```

Now let's say I need to stop the web-interface:

```bash
$ pm2 stop web-interface
```

As you can see **the process hasn't disappeared**. It's still there but in `stopped` status.

To restart it just do:

```bash
$ pm2 restart web-interface
```

Now I want to **delete** the app from the PM2 process list.
To do so:

```bash
$ pm2 delete web-interface
```

## Process listing

To list all running processes:

```bash
$ pm2 list
# Or
$ pm2 [list|ls|l|status]
```

To get more details about a specific process:

```bash
$ pm2 show 0
```

## Monitoring CPU/Memory

PM2 gives you a simple way to monitor the resource usage of your application.
You can monitor memory and cpu very easily, straight from your terminal:

```bash
$ pm2 monit
```

Please note that [Keymetrics](https://keymetrics.io) allows to monitor applications much better, from errors, performances, vital signs and much more.

## Clustering (cluster_mode)

The **cluster mode** allows to scale your Node.js application accross all CPUs available and to update them without any downtime!

It's perfectly fitted for networked applications handling HTTP(s)/UDP/TCP connections.

To enable the **cluster mode**, just pass the -i <instances> option:

```bash
$ pm2 start app.js -i 1
```

You can pass multiple values to the instances (-i) option:

```
# Start the maximum processes depending on available CPUs
$ pm2 start app.js -i 0

# Start the maximum processes -1 depending on available CPUs
$ pm2 start app.js -i -1

# Start 3 processes
$ pm2 start app.js -i 3
```

### Considerations

- You don't need to modify anything from you code to be able to use this nifty feature.
- In your application the environment variable `NODE_APP_INSTANCE` is exposed so you can listen for different port if needed. (e.g .listen(8000 + process.env.NODE_APP_INSTANCE))
- Be sure your **application is stateless** meaning that there is not any local data stored in the process, like sessions/websocket connections etc. Use Redis, Mongo or other DB to share states between processes

### Hot reload / 0s reload

As opposed to `restart`, which kills and restarts the process, `reload` achieves a 0-second-downtime reload.

**Warning** This feature only works for apps in **cluster mode**, that uses HTTP/HTTPS/Socket connections.

To reload an app:

```bash
$ pm2 reload api
```

If the reload system hasn't managed to reload your app, a timeout will fallback to a classic restart.

### Graceful reload

Sometimes you can experience a **very long reload, or a reload that doesn't work** (fallback to restart) meaning that your app still has open connections on exit.
Or you may need to close all databases connections, clear a data queue or whatever.

To work around this problem you have to **use the graceful reload**.

Graceful reload is a mechanism that will send a **shutdown** message to your process before reloading it.
You can control the time that the app has to shutdown via the `PM2_GRACEFUL_TIMEOUT` environment variable.

Example:

```javascript
process.on('message', function(msg) {
  if (msg == 'shutdown') {
    // Your process is going to be reloaded
    // You have to close all database/socket.io/* connections

    console.log('Closing all connections...');

    // You will have 4000ms to close all connections before
    // the reload mechanism will try to do its job

    setTimeout(function() {
      console.log('Finished closing connections');
      // This timeout means that all connections have been closed
      // Now we can exit to let the reload mechanism do its job
      process.exit(0);
    }, 1500);
  }
});
```

Then use the command:

```bash
$ pm2 gracefulReload [all|name]
```

When PM2 starts a new process to replace an old one, it will wait for the new process to begin listening to a connection or a timeout before sending the shutdown message to the old one. You can define the timeout value with the `PM2_GRACEFUL_LISTEN_TIMEOUT` environament variable. If a script does not need to listen to a connection, it can manually tell PM2 that the process has started up by calling `process.send('online')`.

## Logs management


PM2 allows you to manage logs easily. You can display all application logs in real-time, flush them, and reload them.
There are also different ways to configure how PM2 will handle your logs (separated in different files, merged, with timestamp...) without modifying anything in your code.

### Displaying logs in real-time

Displaying logs of a specified process or of all processes in real-time:

```bash
$ pm2 logs
$ pm2 logs big-api
```

### Flushing logs

```bash
$ pm2 flush # Clear all the logs
```

You can configure a Cron Job to call this command every X days.
Or you can install Log rotate to handle the log rotation.

### Reloading all logs

Reloading logs is specially usefull for Logrotate or any other rotating log system.

You can reload logs by sending `SIGUSR2` to the PM2 process.

You can also reload all logs via the command line with:

```bash
$ pm2 reloadLogs
```

### Configuring logs

#### CLI

```bash
$ pm2 start echo.js --merge-logs --log-date-format="YYYY-MM-DD HH:mm Z"
```

Options:

```bash
--merge-logs                 do not postfix log file with process id
--log-date-format <format>   prefix logs with formated timestamp
-l --log [path]              specify entire log file (error and out are both included)
-o --output <path>           specify out log file
-e --error <path>            specify error log file
```

### JSON / Programmatic

```
{
  "script"          : "echo.js",
  "error_file"        : "err.log",
  "out_file"        : "out.log",
  "merge_logs"      : true,
  "log_date_format" : "YYYY-MM-DD HH:mm Z"
}
```

**Note**: To merge all logs into the same file set the same value for `error_file`, `out_file`.

## Max Memory Restart

PM2 allows to restart an application based on a memory limit.

### CLI

```bash
$ pm2 start big-array.js --max-memory-restart 20M
```

### JSON

```json
{
  "name"   : "max_mem",
  "script" : "big-array.js",
  "max_memory_restart" : "20M"
}
```

### Programmatic

```
pm2.start({
  name               : "max_mem",
  script             : "big-array.js",
  max_memory_restart : "20M"
}, function(err, proc) {
  // Processing
});
```

### Units

Units can be K(ilobyte), M(egabyte), G(igabyte).

```
50M
50K
1G
```

## Startup script

PM2 can **generate startup scripts and configure them**.
PM2 is also smart enough to **save all your process list** and to **bring back all your processes at machine restart**.

```bash
$ pm2 startup
# auto-detect platform
$ pm2 startup [platform]
# render startup-script for a specific platform, the [platform] could be one of:
#   ubuntu|centos|redhat|gentoo|systemd|darwin|amazon
```

Once you have started the apps and want to keep them on server reboot do:

```bash
$ pm2 save
```

**Warning** It's tricky to make this feature work generically, so once PM2 has setup your startup script, reboot your server to make sure that PM2 has launched your apps!

### Startup Systems support

Three types of startup scripts are available:

- SystemV init script (with the option `ubuntu` or `centos`)
- OpenRC init script (with the option `gentoo`)
- SystemD init script (with the `systemd` option)

The startup options are using:

- **ubuntu** will use `updaterc.d` and the script `lib/scripts/pm2-init.sh`
- **centos**/**redhat** will use `chkconfig` and the script `lib/scripts/pm2-init-centos.sh`
- **gentoo** will use `rc-update` and the script `lib/scripts/pm2`
- **systemd** will use `systemctl` and the script `lib/scripts/pm2.service`
- **darwin** will use `launchd` to load a specific `plist` to resurrect processes after reboot.

### User permissions

Let's say you want the startup script to be executed under another user.

Just use the `-u <username>` option !

```bash
$ pm2 startup ubuntu -u www
```

### Related commands

Dump all processes status and environment managed by PM2:
```bash
$ pm2 dump
```
It populates the file `~/.pm2/dump.pm2` by default.

To bring back the latest dump:
```bash
$ pm2 [resurrect|save]
```

## Watch & Restart

PM2 can automatically restart your app when a file changes in the current directory or its subdirectories:

```bash
$ pm2 start app.js --watch
```

If `--watch` is enabled, stopping it won't stop watching:
- `pm2 stop 0` will not stop watching
- `pm2 stop --watch 0` will stop watching

Restart toggle the `watch` parameter when triggered.

To watch specific paths, please use a JSON app declaration, `watch` can take a string or an array of paths. Default is `true`:

```json
{
  "watch": ["server", "client"],
  "ignore_watch" : ["node_modules", "client/img"],
  "watch_options": {
    "followSymlinks": false
  }
}
```

As specified in the [Schema](#a988):
- `watch` can be a boolean, an array of paths or a string representing a path. Default to `false`
- `ignore_watch` can be an array of paths or a string, it'll be interpreted by [chokidar](https://github.com/paulmillr/chokidar#path-filtering) as a glob or a regular expression.
- `watch_options` is an object that will replace options given to chokidar. Please refer to [chokidar documentation](https://github.com/paulmillr/chokidar#api) for the definition.

PM2 is giving chokidar these Default options:

```
var watch_options = {
  persistent    : true,
  ignoreInitial : true
}
```

## JSON app declaration


PM2 empowers your process management workflow, by allowing you to fine-tune the behavior, options, environment variables, logs files... of each process you need to manage via JSON configuration.

It's particularly usefull for micro service based applications.

You can define parameters for your apps in a JSON file:

```json
{
  "apps" : [{
    "name"        : "worker-app",
    "script"      : "worker.js",
    "args"        : ["--toto=heya coco", "-d", "1"],
    "watch"       : true,
    "node_args"   : "--harmony",
    "merge_logs"  : true,
    "cwd"         : "/this/is/a/path/to/start/script",
    "env": {
        "NODE_ENV": "production",
        "AWESOME_SERVICE_API_TOKEN": "xxx"
    }
  },{
    "name"       : "api-app",
    "script"     : "api.js",
    "instances"  : 4,
    "exec_mode"  : "cluster_mode",
    "error_file" : "./examples/child-err.log",
    "out_file"   : "./examples/child-out.log",
    "pid_file"   : "./examples/child.pid"
  }]
}
```

Then you can run:

```bash
# Start all apps
$ pm2 start processes.json

# Stop
$ pm2 stop processes.json

# Delete from PM2
$ pm2 delete processes.json

# Restart all
$ pm2 restart processes.json
```

### Options

The following are valid options for JSON app declarations:

```json
{
  "name"             : "node-app",
  "cwd"              : "/srv/node-app/current",
  "args"             : ["--toto=heya coco", "-d", "1"],
  "script"           : "bin/app.js",
  "node_args"        : ["--harmony", " --max-stack-size=102400000"],
  "log_date_format"  : "YYYY-MM-DD HH:mm Z",
  "error_file"       : "/var/log/node-app/node-app.stderr.log",
  "out_file"         : "log/node-app.stdout.log",
  "pid_file"         : "pids/node-geo-api.pid",
  "instances"        : 6, //or 0 => 'max'
  "min_uptime"       : "200s", // 200 seconds, defaults to 1000
  "max_restarts"     : 10, // defaults to 15
  "max_memory_restart": "1M", // 1 megabytes, e.g.: "2G", "10M", "100K", 1024 the default unit is byte.
  "cron_restart"     : "1 0 * * *",
  "watch"            : false,
  "ignore_watch"      : ["[\\/\\\\]\\./", "node_modules"],
  "merge_logs"       : true,
  "exec_interpreter" : "node",
  "exec_mode"        : "fork",
  "autorestart"      : false, // enable/disable automatic restart when an app crashes or exits
  "vizion"           : false, // enable/disable vizion features (versioning control)
  "env": {
    "NODE_ENV": "production",
    "AWESOME_SERVICE_API_TOKEN": "xxx"
  }
}
```

### List of all JSON-declaration fields avaibles

|        Field       |   Type  |                  Example                  |                                                                                          Description                                                                                         |
|:------------------:|:-------:|:-----------------------------------------:|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|
|        name        |  string |                  "myAPI"                  |                                                                                name your app will have in PM2                                                                                |
|       script       |  string |                "bin/app.js"               |                                                                                       path of your app                                                                                       |
|        args        |   list  |       ["--enable-logs", "-n", "15"]       |                                                                        arguments given to your app when it is launched                                                                       |
|      node_args     |   list  |   ["--harmony", "--max-stack-size=1024"]  |                                                                          arguments given to node when it is launched                                                                         |
|         cwd        |  string |            "/var/www/app/prod"            |                                                                      the directory from which your app will be launched                                                                      |
|      exec_mode     |  string |                 "cluster"                 |                                                    "fork" mode is used by default, "cluster" mode can be configured with `instances` field                                                   |
|      instances     |  number |                     4                     | number of instances for your clustered app, `0` means as much instances as you have CPU cores. a negative value means CPU cores - value (e.g -1 on a 4 cores machine will spawn 3 instances) |
|  exec_interpreter  |  string |                   "node"                  |                       defaults to "node". can be "python", "ruby", "bash" or whatever interpreter you wish to use. "none" will execute your app as a binary executable                       |
|   log_date_format  |  string |            "YYYY-MM-DD HH:mm Z"           |                                                                   format in which timestamps will be displayed in the logs                                                                   |
|     error_file     |  string |  "/var/log/node-app/node-app.stderr.log"  |                             path to the specified error log file. PM2 generates one by default if not specified and you can find it by typing `pm2 desc <app id>`                            |
|      out_file      |  string |  "/var/log/node-app/node-app.stdout.log"  |                            path to the specified output log file. PM2 generates one by default if not specified and you can find it by typing `pm2 desc <app id>`                            |
|      pid_file      |  string |          "pids/node-geo-api.pid"          |                                path to the specified pid file. PM2 generates one by default if not specified and you can find it by typing `pm2 desc <app id>`                               |
|     merge_logs     | boolean |                   false                   |                                                      defaults to false. if true, it will merge logs from all instances of the same app into the same file                                                     |
|    cron_restart    |  string |                "1 0 * * *"                |                                      a cron pattern to restart your app. only works in "cluster" mode for now. soon to be avaible in "fork" mode as well                                     |
|        watch       | boolean |                    true                   |                 enables the watch feature, defaults to "false". if true, it will restart your app everytime a file change is detected on the folder or subfolder of your app.                |
|    ignore_watch    |   list  |     ["[\\/\\\\]\\./", "node_modules"]     |                                                            list of regex to ignore some file or folder names by the watch feature                                                            |
|    max_restarts    |  number |                     10                    |                   number of consecutive unstable restarts (less than 1sec interval) before your app is considered errored and stop being restarted by PM2. defaults to 15.                   |
| max_memory_restart |  string |                   "150M"                  |                      your app will be restarted by PM2 if it exceeds the amount of memory specified. human-friendly format : it can be "10M", "100K", "2G" and so on...                      |
|         env        |  object |   {"NODE_ENV": "production", "ID": "42"}  |                                                                          env variables which will appear in your app                                                                         |
|     autorestart    | boolean |                   false                   |                                                   true by default. if false, PM2 will not restart your app if it crashes or ends peacefully                                                  |
|       vizion       | boolean |                   false                   |                                               true by default. if false, PM2 will start without vizion features (versioning control metadatas)                                               |
|     post_update    |   list  | ["npm install", "echo launching the app"] |                                        a list of commands which will be executed after you perform a Pull/Upgrade operation from Keymetrics dashboard                                        |
|        force       | boolean |                    true                   |                                          defaults to false. if true, you can start the same script several times which is usually not allowed by PM2                                          |
|     next_gen_js    | boolean |                    true                   |                             defaults to false. if true, PM2 will launch your app using embedded BabelJS features which means you can run ES6/ES7 javascript code                             |

### Schema

The completely definitions:

```JSON
{
  "script": {
    "type": "string",
    "require": true
  },
  "args": {
    "type": [
      "array",
      "string"
    ]
  },
  "node_args": {
    "type": [
      "array",
      "string"
    ]
  },
  "name": {
    "type": "string"
  },
  "max_memory_restart": {
    "type": [
      "string",
      "number"
    ],
    "regex": "^\\d+(G|M|K)?$",
    "ext_type": "sbyte",
    "desc": "it should be a NUMBER - byte, \"[NUMBER]G\"(Gigabyte), \"[NUMBER]M\"(Megabyte) or \"[NUMBER]K\"(Kilobyte)"
  },
  "instances": {
    "type": "number",
    "min": 0
  },
  "log_file": {
    "type": [
      "boolean",
      "string"
    ],
    "alias": "log"
  },
  "error_file": {
    "type": "string",
    "alias": "error"
  },
  "out_file": {
    "type": "string",
    "alias": "output"
  },
  "pid_file": {
    "type": "string",
    "alias": "pid"
  },
  "cron_restart": {
    "type": "string",
    "alias": "cron"
  },
  "cwd": {
    "type": "string"
  },
  "merge_logs": {
    "type": "boolean"
  },
  "watch": {
    "type": "boolean"
  },
  "ignore_watch": {
    "type": [
      "array",
      "string"
    ]
  },
  "watch_options": {
    "type": "object"
  },
  "env": {
    "type": ["object", "string"]
  },
  "^env_\\S*$": {
    "type": [
      "object",
      "string"
    ]
  },
  "log_date_format": {
    "type": "string"
  },
  "min_uptime": {
    "type": [
      "number",
      "string"
    ],
    "regex": "^\\d+(h|m|s)?$",
    "desc": "it should be a NUMBER - milliseconds, \"[NUMBER]h\"(hours), \"[NUMBER]m\"(minutes) or \"[NUMBER]s\"(seconds)",
    "min": 100,
    "ext_type": "stime"
  },
  "max_restarts": {
    "type": "number",
    "min": 0
  },
  "exec_mode": {
    "type": "string",
    "regex": "^(cluster|fork)(_mode)?$",
    "alias": "executeCommand",
    "desc": "it should be \"cluster\"(\"cluster_mode\") or \"fork\"(\"fork_mode\") only"
  },
  "exec_interpreter": {
    "type": "string",
    "alias": "interpreter"
  },
  "write": {
    "type": "boolean"
  },
  "force": {
    "type": "boolean"
  }
}
```

### Considerations

- All command line options passed when using the JSON app declaration will be dropped i.e.
```bash
$ cat node-app-1.json

{
  "name" : "node-app-1",
  "script" : "app.js",
  "cwd" : "/srv/node-app-1/current"
}
```

- You can start as many JSON app declarations as you want.  Continuing from above:
```bash
$ pm2 start node-app-2.json
$ ps aux | grep node-app
root  14735  5.8  1.1  752476  83932 ? Sl 00:08 0:00 pm2: node-app-1
root  24271  0.0  0.3  696428  24208 ? Sl 17:36 0:00 pm2: node-app-2
```

Note that if you execute `pm2 start node-app-2` again, it will spawn an additional instance node-app-2.

- **cwd:** your JSON declaration does not need to reside with your script.  If you wish to maintain the JSON(s) in a location other than your script (say, `/etc/pm2/conf.d/node-app.json`) you will need to use the `cwd` feature (Note, this can be really helpful for capistrano style directory structures that uses symlinks). Files can be either relative to the `cwd` directory, or absolute (see example below).


- All the keys can be used in a JSON configured file, but will remain almost the same on the command line e.g.:

```
exec_interpreter  -> --interpreter
exec_mode         -> --execute_command
max_restarts      -> --max_restarts
force             -> --force
```

If the `alias` exists, you can use it as a CLI option, but do not forget to turn the camelCasing to underscore split `executeCommand` to `--execute_command`.


**Notes**
- Using quotes to make an ESC, e.g.:

  ```
  $pm2 start test.js --node-args "port=3001 sitename='first pm2 app'"
  ```

  The `nodeArgs` argument will be parsed as

  ```JSON
  [
    "port=3001",
    "sitename=first pm2 app"
  ]
  ```

  but not

  ```JSON
  [
    "port=3001",
    "sitename='first",
    "pm2",
    "app'"
  ]
  ```

- RegExp key

  Matches the keys of configured JSON by RegExp (not by string comparison), e.g. `^env_\\S*$` will match all `env` keys like `env_production`, `env_test`, and valid them according to the schemas specifications.

- Special `ext_type`

  - min_uptime

    Value of `min_uptime` can be:

      - **Number**
        e.g. `"min_uptime": 3000` means 3000 milliseconds.
      - **String**
        Therefore, we are making it short and easy to configure: `h`, `m` and `s`, e.g.: `"min_uptime": "1h"` means one hour, `"min_uptime": "5m"` means five minutes and `"min_uptime": "10s"` means ten seconds (those will be transformed into milliseconds).

  - max_memory_restart

    Value of `max_memory_restart` can be:
      - **Number**
        e.g. `"max_memory_restart": 1024` means 1024 bytes (**NOT BITS**).
      - **String**
        Therefore, we are making it short and easy to configure: `G`, `M` and `K`, e.g.: `"max_memory_restart": "1G"` means one gigabytes, `"max_memory_restart": "5M"` means five megabytes and `"max_memory_restart": "10K"` means ten kilobytes (those will be transformed into byte(s)).

- Optional values

  For example `exec_mode` can take `cluster` (`cluster_mode`) or `fork` (`fork_mode`) as possible values.

- Things to know

  - maximum

    `"instances": 0` means that we will launch the maximum processes possible according to the numbers of CPUs (cluster mode)
  - array

    `args`, `node_args` and `ignore_watch` could be type of `Array` (e.g.: `"args": ["--toto=heya coco", "-d", "1"]`) or `string` (e.g.: `"args": "--to='heya coco' -d 1"`)