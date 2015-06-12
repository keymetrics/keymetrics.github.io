---
layout: docs
title: Programmatic API and Specific
description: How to use PM2.
permalink: /docs/usage/pm2-progra-and-specific/
---

##Programmatic API

<table class="table table-striped table-bordered">
    <tr>
        <th>Method name</th>
        <th>API</th>
    </tr>
     <tr>
      <td><b>Connect/Launch</b></td>
      <td>pm2.connect(fn(err){})</td>
    </tr>
     <tr>
      <td><b>Disconnect</b></td>
      <td>pm2.disconnect(fn(err, proc){})</td>
    </tr>
</table>

**Consideration with .connect**: the .connect method connect to the local PM2, but if PM2 is not up, it will launch it and will put in in background as you launched it via CLI.

<table class="table table-striped table-bordered">
    <tr>
        <th>Method name</th>
        <th>API</th>
    </tr>
    <tr>
      <td><b>Start</b></td>
      <td>pm2.start(script_path|json_object|json_path, options, fn(err, proc){})</td>
    </tr>
    <tr>
      <td>Options </td>
      <td>
      nodeArgs(arr), scriptArgs(arr), name(str), instances(int), error(str), output(str), pid(str), cron(str), mergeLogs(bool), watch(bool), runAsUser(int), runAsGroup(int), executeCommand(bool), interpreter(str), write(bool)</td>
    </tr>
    <tr>
      <td><b>Restart</b></td>
      <td>pm2.restart(proc_name|proc_id|all, fn(err, proc){})</td>
       </tr>
     <tr>
      <td><b>Stop</b></td>
      <td>pm2.stop(proc_name|proc_id|all, fn(err, proc){})</td>
    </tr>
    <tr>
      <td><b>Delete</b></td>
      <td>pm2.delete(proc_name|proc_id|all, fn(err, proc){})</td>
    </tr>


    <tr>
      <td><b>Reload</b></td>
      <td>pm2.reload(proc_name|all, fn(err, proc){})</td>
    </tr>
      <tr>
      <td><b>Graceful Reload</b></td>
      <td>pm2.gracefulReload(proc_name|all, fn(err, proc){})</td>
    </tr>
</table>

<table class="table table-striped table-bordered">
    <tr>
        <th>Method name</th>
        <th>API</th>
    </tr>
    <tr>
      <td><b>List</b></td>
      <td>pm2.list(fn(err, list){})</td>
    </tr>
    <tr>
      <td><b>Describe process</b></td>
      <td>pm2.describe(proc_name|proc_id, fn(err, list){})</td>
    </tr>
    <tr>
      <td><b>Dump (save)</b></td>
      <td>pm2.dump(fn(err, ret){})</td>
    </tr>
    <tr>
      <td><b>Flush logs</b></td>
      <td>pm2.flush(fn(err, ret){})</td>
    </tr>
     <tr>
      <td><b>Reload logs</b></td>
      <td>pm2.reloadLogs(fn(err, ret){})</td>
    </tr>
         <tr>
      <td><b>Send signal</b></td>
      <td>pm2.sendSignalToProcessName(signal,proc,fn(err, ret){})</td>
    </tr>
     <tr>
      <td><b>Generate start script</b></td>
      <td>pm2.startup(platform, fn(err, ret){})</td>
    </tr>
     <tr>
      <td><b>Kill PM2</b></td>
      <td>pm2.killDaemon(fn(err, ret){})</td>
    </tr>
</table>

## Special features

Launching PM2 without daemonizing itself:

```bash
$ pm2 start app.js --no-daemon
```

Sending a system signal to a process:

```bash
$ pm2 sendSignal SIGUSR2 my-app
```

## Configuration file

You can specify the following options by editing the file `~/.pm2/custom_options.sh`:

```
PM2_RPC_PORT
PM2_PUB_PORT
PM2_BIND_ADDR
PM2_API_PORT
PM2_GRACEFUL_TIMEOUT
PM2_MODIFY_REQUIRE
PM2_KILL_TIMEOUT
```

## API health endpoint

```bash
$ pm2 web
```

## Enabling Harmony ES6

The `--node-args` option permit to launch script with V8 flags, so to enable harmony for a process just do this:
```bash
$ pm2 start my_app.js --node-args="--harmony"
```

And with JSON declaration:

```json
[{
  "name" : "ES6",
  "script" : "es6.js",
  "node_args" : "--harmony"
}]
```

## CoffeeScript

```bash
$ pm2 start server.coffee --interpreter coffee
```

That's all!
