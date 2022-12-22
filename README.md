# Ubuntu 22.04 Server After Startup,  Pre Shotdown Script

## 1. After-Startup Script

> $ cat /usr/local/bin/telegram-send-startup

```
#!/usr/bin/bash

GROUP_ID=2040~~~~~
BOT_TOKEN=5617~~~~~
DATE="$( date "+%Y-%m-%d %H:%M")"

TXT="$DATE%0A$HOSTNAME: Server is Started"

curl -s \
  --data "text=$TXT" \
  --data "chat_id=$GROUP_ID" \
  'https://api.telegram.org/bot'$BOT_TOKEN'/sendMessage' > /dev/null
```

> $ sudo crontab -e

```
@reboot /usr/local/bin/telegram-send-startup
```


## 2. Pre-Shutdown Script

> $ cat /usr/local/bin/telegram-send-startup

```
#!/usr/bin/bash

GROUP_ID=2040~~~~~
BOT_TOKEN=5617~~~~~
DATE="$( date "+%Y-%m-%d %H:%M")"

TXT="$DATE%0A$HOSTNAME: Server is Stopped"

curl -s \
  --data "text=$TXT" \
  --data "chat_id=$GROUP_ID" \
  'https://api.telegram.org/bot'$BOT_TOKEN'/sendMessage' > /dev/null
```

> $ cat /usr/lib/systemd/system/telegram-stop.service

```
[Unit]
Description=Pre-Shutdown Send Telegram messages
DefaultDependencies=no
Before=shutdown.target

[Service]
Type=oneshot
ExecStart=/usr/local/bin/telegram-send-stop

[Install]
WantedBy=halt.target reboot.target shutdown.target
```

> $ sudo systemctl daemon-reload

> $ sudo systemctl enable telegram-stop.service
