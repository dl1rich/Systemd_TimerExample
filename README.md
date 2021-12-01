# Systemd_TimerExample
A simple timer setup for linux using systemd


## Example

For this example i will use "dl_backup" as my servic's name. 

Steps: 
Systemd files live in ```/etc/systemd/system/```
1. Create a service file
2. Create a timer file
3. Create a dummy file or script to run (edit on service file at ExecStart)
4. Hook up the timer and service file with systemd. 

dl_backup.service (file)

```bash
[Unit]
Description=Backup service for DefenceLogic backup scripts
Wants=dl_backup.timer

[Service]
#User=
Type=simple
WorkingDirectory=/home/dl/
ExecStart=/bin/bash -c 'cd /home/dl/ && ./test.sh '

[Install]
WantedBy=multi-user.target
```

dl_backup.timer (file)

```bash
[Unit]
Description=Run dl Backup daily
Requires=dl_backup.service

[Timer]
Unit=dl_backup.service
OnCalendar=*-*-* *:*:00
Persistent=true

[Install]
WantedBy=timers.target
```
