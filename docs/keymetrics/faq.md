---
layout: docs
title: FAQ/Troubleshooting
description: FAQ
permalink: /docs/pages/faq-troubleshooting/
---

If you have a suggestion or an issue to report, please read the troubleshooting process below first. If this does not help you please reach out to us on our in app-chat

## Troubleshooting for Keymetrics/PM2

### I can't seem to connect my local PM2 to the Keymetrics dashboard

If you are in this situation, it might be for several reasons.

- You are behind a company proxy or firewall.
Make sure that the ports 80 (TCP outbound), 443 (HTTPS) and 43554 (TCP inbound/outbound) are open.

If you need to whitelist IPs, allow these ones: 62.210.102.213, 62.210.94.153, 62.210.100.99, 62.210.101.249, 195.154.146.228, 163.172.20.79
 
- You are using an old version of Node.js or PM2.
Make sure you are using at least Node.js v0.12.x or higher (node v0.12.x or iojs v.2.x is recommended).
Make sure you are using the latest version of PM2 https://github.com/Unitech/PM2/releases.

- You have concurrent PM2 sending data to the same bucket with an identical server name.
Make sure you have only one PM2 instance launched `ps -ax | grep PM2`

- Refresh your connection to Keymetrics. `pm2 interact stop` then `pm2 interact start`. Also don't forget to refresh the dashboard itself, it might help sometimes.

### The dashboad tells "Reverse connection not established"

It means that PM2 have not managed to initialize the full duplex connection. Not any actions will work (restart, pull, module install...)

Please make sure that the port 43554 (TCP inbound/outbound) is opened and check the logs in ~/.pm2/agent.log.

Type `pm2 link` to re-try the connection.

## I cannot link new servers but not any server is linked

Go to the setting page of your bucket and delete Servers in the Server box.

### The versioning buttons (Rollback/Pull/Upgrade) aren't working

- If the buttons are disabled, make sure that the `Local changes` and `Local commit` indicators are green.

- If you get a warning `Not authorized` when trying to perform such actions, it means you have not the admin privileges in this bucket.

- If none of the above happens, but the precedure just hangs, make sure you have a recent version of Node.js as well as the latest version of PM2.

- Also, your repository should not ask for a password input (it means you must clone it via ssh), try typing `git remote update` manually in the folder and see if it asks for a password or not.

### 3. The versioning block tell me `File modified (unstaged changes)`

It means that there are local files that has been changed and not comited.

To see which files has been modified do a `git status`. Once it is fixed (via git commit or git stash) do a `pm2 restart all`.

### In the dashboard I've linked two servers and they are continiously flickering

You made a `pm2 link <public_id> <private_id> [name]` without setting the name option. By default if the name is empty, it becomes the $HOSTNAME env variable.

To fix this:

```
# Server 1
$ pm2 link <private_id> <public_id> server1

# Server 2
$ pm2 link <private_id> <public_id> server2
```

## Documentation

[PM2 Documentation](http://pm2.keymetrics.io/)

[Keymetrics Documentation](http://docs.keymetrics.io/)
