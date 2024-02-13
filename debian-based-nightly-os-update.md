# Nightly OS Update

Login to your server and do a `sudo su -` to become root.

Create a `/root/bin` directory:
```
mkdir /root/bin
```
Create/Upload the a shell script named `/root/bin/nightly-os-update.sh` as:
```
#!/bin/sh

apt-get -y update
apt-get -y upgrade
reboot
```
> If you feel strongly about not rebooting nightly, then omit or comment out the `reboot` command.

Make the script executable:
```
chmod +x /root/bin/nightly-os-update.sh
```
Add a scheduled `crontab` job to run the script on the schedule you care to. Below is an example of running the update at 3am local time every day:
Go into crontab edit mode by executing the `crontab -e` command. If you are presented with a list of editors to choose from, and you do not know how to use `vi`, choose `nano`, it will make your life easier. I use `vi` but I am also comfortable with it.
```
crontab -e
```
You should be presented with a `crontab` devoid of any scheduled jobs but with a lot of documentation comments. Below is what mine looks like on a clean Unbutu installation:
```
# Edit this file to introduce tasks to be run by cron.
# 
# Each task to run has to be defined through a single line
# indicating with different fields when the task will be run
# and what command to run for the task
# 
# To define the time you can provide concrete values for
# minute (m), hour (h), day of month (dom), month (mon),
# and day of week (dow) or use '*' in these fields (for 'any').
# 
# Notice that tasks will be started based on the cron's system
# daemon's notion of time and timezones.
# 
# Output of the crontab jobs (including errors) is sent through
# email to the user the crontab file belongs to (unless redirected).
# 
# For example, you can run a backup of all your user accounts
# at 5 a.m every week with:
# 0 5 * * 1 tar -zcf /var/backups/home.tgz /home/
# 
# For more information see the manual pages of crontab(5) and cron(8)
# 
# m h  dom mon dow   command
```
Add a new line at the bottom that looks like what you see here. This will run your script at 3am every day, redirecting both error output (`2>`) and standard output (`>`) into a log file `/root/bin/nightly-os-update.log`. This will avoid flooding your `root` account local email with daily output of the script.
```
0 3 * * * /root/bin/nightly-os-update.sh 2>&1 >/root/bin/nightly-os-update.log
```
