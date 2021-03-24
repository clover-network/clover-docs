---
description: 'from https://wiki.polkadot.network/docs/en/maintain-guides-how-to-systemd'
---

# Using systemd for the Validator Node

You can run your validator as a [systemd](https://en.wikipedia.org/wiki/Systemd) process so that it will automatically restart on server reboots or crashes \(and helps to avoid getting slashed!\).

Before following this guide you should have already set up your validator by following the [How to validate](https://wiki.polkadot.network/docs/en/learn-validator) article.

First create a new unit file called `clover-validator.service` in `/etc/systemd/system/`.

```text
touch /etc/systemd/system/clover-validator.service
Copy
```

In this unit file you will write the commands that you want to run on server boot / restart.

```text
[Unit]
Description=clover Validator

[Service]
ExecStart=PATH_TO_CLOVER_BIN --validator --name SHOW_ON_TELEMETRY
Restart=always
RestartSec=120

[Install]
WantedBy=multi-user.target
Copy
```

> **WARNING:** It's recommended to delay the restart of a node with `RestartSec` in the case of node crashes. It's possible that when a node crashes, consensus votes in GRANDPA aren't persisted to disk. In this case, there is potential to equivocate when immediately restarting. What can happen is the node will not recognize votes that didn't make it to disk, and will then cast conflicting votes. Delaying the restart will allow the network to progress past potentially conflicting votes, at which point other nodes will not accept them.

To enable this to autostart on bootup run:

```text
systemctl enable clover-validator.service
Copy
```

Start it manually with:

```text
systemctl start clover-validator.service
Copy
```

You can check that it's working with:

```text
systemctl status clover-validator.service
Copy
```

You can tail the logs with `journalctl` like so:

```text
journalctl -f -u clover-validator
```

