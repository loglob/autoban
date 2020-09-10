# autoban
A tiny shell script that listens to ssh logs and bans any IPs trying to log into root to stop bruteforce attacks. Legitimate logins into root (although they really shouldn't be enabled) will not result in bans, as will any failed attempts on different users.
If multiple IPs from one subnet (with Mask 255.255.255.0) commit bannable offences, that subnet is blacklisted.

## Setup
Before using the script make sure the IP(s) that you trust are in the */usr/hosts.allow* file to prevent you from locking yourself out accidentily.
Make sure your ssh server (I made this for openSSH and can't guarantee it working on anything else) is using TCP wrapper or obeying the hosts.deny and allow rules. 

You probably want to make sure the script is listening to the right service, so check if
```
sudo journalctl -u ssh
```
returns any entries, if not check
```
sudo journalctl -u sshd
```
and change the autoban.conf setting accordingly:
```
unitname=sshd
```
## Installation
To enable the included systemD service, copy the autoban exetuable into the `/srv/autoban` directory and then copy the service file into the `/etc/systemd/system` directory.
Then run `service autoban start` to start autoban.
Finally, run `systemctl enable autoban` to start autoban automatically on system start.
