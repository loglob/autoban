# autoban
A tiny shell script that listens to ssh logs and bans any IPs trying to log into root to stop bruteforce attacks. Legitimate logins into root (although they really shouldn't be enabled) will not result in bans, as will any failed attempts on different users.

## Installation
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
and change the script's second definition accordingly:
```
SSHUnit="sshd"
```

If you want to keep your */etc/hosts.deny* clean, add a line like this to it:
```
ALL: /usr/autoban.banned
```
Then, change the first definition in the script to the same path, like this:
```
DenyFile="/usr/autoban.banned"
```

The script is designed to run in the background so you may want to add it to /etc/rc.local or use some other method to make it start up with the computer.
