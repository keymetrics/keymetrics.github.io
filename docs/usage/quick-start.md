---
layout: docs
title: Quick Start
description: How to use PM2.
permalink: /docs/usage/quick-start/
---

## Installation

The latest PM2 stable version is installable via NPM:

```bash
$ npm install pm2@latest -g
```

If the above fails use:

```bash
$ npm install git://github.com/Unitech/pm2#master -g
```

## Usage

Hello world:

```bash
$ pm2 start app.js
```

## Raw Examples

```bash
# Fork mode
$ pm2 start app.js --name my-api # Name process

# Cluster mode
$ pm2 start app.js -i 0        # Will start maximum processes with LB depending on available CPUs
$ pm2 start app.js -i max      # Same as above, but deprecated yet.

# Listing

$ pm2 list               # Display all processes status
$ pm2 jlist              # Print process list in raw JSON
$ pm2 prettylist         # Print process list in beautified JSON

$ pm2 describe 0         # Display all informations about a specific process

$ pm2 monit              # Monitor all processes

# Logs

$ pm2 logs [--raw]       # Display all processes logs in streaming
$ pm2 flush              # Empty all log file
$ pm2 reloadLogs         # Reload all logs

# Actions

$ pm2 stop all           # Stop all processes
$ pm2 restart all        # Restart all processes

$ pm2 reload all         # Will 0s downtime reload (for NETWORKED apps)
$ pm2 gracefulReload all # Send exit message then reload (for networked apps)

$ pm2 stop 0             # Stop specific process id
$ pm2 restart 0          # Restart specific process id

$ pm2 delete 0           # Will remove process from pm2 list
$ pm2 delete all         # Will remove all processes from pm2 list

# Misc

$ pm2 reset <process>    # Reset meta data (restarted time...)
$ pm2 updatePM2          # Update in memory pm2
$ pm2 ping               # Ensure pm2 daemon has been launched
$ pm2 sendSignal SIGUSR2 my-app # Send system signal to script
$ pm2 start app.js --no-daemon
$ pm2 start app.js --no-vizion
$ pm2 start app.js --no-autorestart
```

## Different ways to launch a process

```bash
$ pm2 start app.js           # Start app.js

$ pm2 start app.js -- -a 23  # Pass arguments '-a 23' argument to app.js script

$ pm2 start app.js --name serverone # Start a process an name it as server one
                                    # you can now stop the process by doing
                                    # pm2 stop serverone

$ pm2 start app.js --node-args="--debug=7001" # --node-args to pass options to node V8

$ pm2 start app.js -i 0             # Start maximum processes depending on available CPUs (cluster mode)

$ pm2 start app.js --log-date-format "YYYY-MM-DD HH:mm Z"    # Log will be prefixed with custom time format

$ pm2 start app.json                # Start processes with options declared in app.json
                                    # Go to chapter Multi process JSON declaration for more

$ pm2 start app.js -e err.log -o out.log  # Start and specify error and out log

```

For scripts in other languages:

```bash
$ pm2 start echo.pl --interpreter=perl

$ pm2 start echo.coffee
$ pm2 start echo.php
$ pm2 start echo.py
$ pm2 start echo.sh
$ pm2 start echo.rb
```

The interpreter is set by default with this equivalence:

```json
{
  ".sh": "bash",
  ".py": "python",
  ".rb": "ruby",
  ".coffee" : "coffee",
  ".php": "php",
  ".pl" : "perl",
  ".js" : "node"
}
```

## Options

```
Options:

   -h, --help                           output usage information
   -V, --version                        output the version number
   -v --version                         get version
   -s --silent                          hide all messages
   -m --mini-list                       display a compacted list without formatting
   -f --force                           force actions
   -n --name <name>                     set a <name> for script
   -i --instances <number>              launch [number] instances (for networked app)(load balanced)
   -l --log [path]                      specify entire log file (error and out are both included)
   -o --output <path>                   specify out log file
   -e --error <path>                    specify error log file
   -p --pid <pid>                       specify pid file
   --max-memory-restart <memory>        specify max memory amount used to autorestart (in megaoctets)
   --env <environment_name>             specify environment to get specific env variables (for JSON declaration)
   -x --execute-command                 execute a program using fork system
   -u --user <username>                 define user when generating startup script
   -c --cron <cron_pattern>             restart a running process based on a cron pattern
   -w --write                           write configuration in local folder
   --interpreter <interpreter>          the interpreter pm2 should use for executing app (bash, python...)
   --log-date-format <momentjs format>  add custom prefix timestamp to logs
   --no-daemon                          run pm2 daemon in the foreground if it doesn't exist already
   --merge-logs                         merge logs from different instances but keep error and out separated
   --watch                              watch application folder for changes
   --ignore-watch <folders|files>       folder/files to be ignored watching, chould be a specific name or regex - e.g. --ignore-watch="test node_modules "some scripts""
   --node-args <node_args>              space delimited arguments to pass to node in cluster mode - e.g. --node-args="--debug=7001 --trace-deprecation"
   --no-color                           skip colors
   --no-vizion                          skip vizion features (versioning control)
   --no-autorestart                     do not automatically restart apps
```


## How to update PM2

Install the latest pm2 version :

```bash
$ npm install pm2@latest -g
```

Then update the in-memory PM2 :

```bash
$ pm2 update
```

## Allow PM2 to bind applications on ports 80/443 without root

It’s a general rule that you shouldn’t run node as root, but only root can bind to ports less than 1024. This is where authbind comes in. Authbind allows non-root users to bind to ports less than 1024.

```bash
$ sudo apt-get install authbind
$ sudo touch /etc/authbind/byport/80
$ sudo chown user /etc/authbind/byport/80
$ sudo chmod 755 /etc/authbind/byport/80
$ authbind --deep pm2 update
```

Now you can start applications with PM2 that can bind to port 80 without being root!

It's recommended to put an alias in your .bashrc file:

```bash
alias pm2='authbind --deep pm2'
```