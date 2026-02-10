# Automated Restart Script

This should automatically restart your computer every week.
Be sure to go into the python script to set your preferences.

Default: Friday, 12pm CST

## Requirements
- python 3.7+
- `APScheduler` library
- `pytz` library

## Dependencies

1. **Python dependencies**
> bash
> pip install -r requirements.txt

## Setup

### Windows
#### Opt. 1: Run as a scheduled task
1. Open Task Scheduler

2. Create a new basic task

3. Set trigger: weekly 
(be sure to set a 10 minute buffer, was a problem in testing.)

4. Set action: Start a program

5. Program: `python.exe`

6. Arguments: `C:\path\to\restart_app.py`

#### Opt. 2: Run as a py script in the background on startup.
- Either run it manually, or...
1. Create a shortcut for provided `restart_scheduler.bat`.

2. Press Windows+r to open Run prompt.

3. Type "shell:startup" and press enter.

4. Move shortcut created to newly opened window.


### Linux
#### Run as systemd service
1. Check contents of restart-app.service, edit it as directed:
```ini
[Unit]
Description=Automated Restart Service
After=network.target

[Service]
Type=simple
User=your_username                          <-- INSERT YOUR USERNAME HERE
WorkingDirectory=/path/to/script            <-- ADD PATH TO SCRIPT, NO .PY
ExecStart=/usr/bin/python3 /path/to/script  <-- ADD PATH TO SCRIPT, W/ .PY
Restart=always
RestartSec=10
StandardOutput=journal
StandardError=journal

[Install]
WantedBy=multi-user.target
```

2. Enable and start the service:
```bash
sudo systemctl daemon-reload
sudo systemctl enable restart-app.service
sudo systemctl start restart-app.service
```

3. Check Status:
```bash
sudo systemctl status restart-app.service
```

4. View logs:
```bash
sudo journalctl -u restart-app.service -n 50 -f
```

>>> Note that if you have a method you are used to, go with that.

## Logging

This script should create a script in its own directory.
#WorksOnMyPC

## IMPORTANT
### Linux Permissions
The restart command requires `sudo` privileges, so you may need to:
1. Add your user to the sudo group
2. configure sudoers to allow restart without password
> prompt:
```bash
sudo visudo
```
> Add this line:
```
your_username ALL=(ALL) NOPASSWD: /sbin/shutdown
```

### Timezone
By default, this script uses CST- but this can be easily changed as of this release on line 26 of the Python script. Follow the provided format.

### Troubleshooting
- Check the generated logs for errors.
- Verify that the needed libraries are installed.
- Make sure you gave the script the correct permissions.

**Linux permission denied?**
- Verify sudo rights for shutdown command
- Check systemd service user permissions

## DEV

Verify the script works by modifiying the function:
> `restart()`  -> Set this to just log, comment out the shutdown command
> Change the Cron Schedule to a time within the next minute.
> Run, watch log output.

## NEEDS

> Need to add functionality to properly shut down running apps and servers...
> Need to add a warning system to alert user of impending restart...
