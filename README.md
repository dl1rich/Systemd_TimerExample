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
OnCalendar=*-*-* *:*:00    # runs every 1 min, change to your needs
Persistent=true

[Install]
WantedBy=timers.target
```

## Install the service and timer. 

```
# Reload the files in the daemon /etc/systemd/system
sudo systemctl deamon-reload

# Start the service
sudo systemctl start dl_backup.service

# Start the timer for the service
sudo systemctl start dl_backup.service

```

## Extras

```bash
# Load the new file
sudo systemctl daemon-reload

# Start your service
sudo systemctl start dl_backup.service

# To check the status of your service
sudo systemctl status dl_backup.service
sudo systemctl status dl_backup.timer 

# To enable your service on every reboot
sudo systemctl enable dl_backup.service

# To disable your service on every reboot
sudo systemctl disable dl_backup.service
```

Watch service in real time 
```bash 
journalctl -S today -f -u dl_backup.service
```


## Resources

Extra infos. 
```
https://www.digitalocean.com/community/tutorials/how-to-use-systemctl-to-manage-systemd-services-and-units
```
