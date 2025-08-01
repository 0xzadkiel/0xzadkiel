---
title: "The art of Linux Enumeration"
published: 2025-08-01
image: "./127.gif"
tags: ["Linux"]
category: "Guide"
draft: false
---


Enumeration is key in hacking and one of the major factors that differentiates a good hacker from a great hacker. We should not only take time doing enumeration but also be thorough, because missing something can create a cycle of going over everything again, eventually consuming more `time` and creating `stress`, so it should be done once and very thoroughly in my opinion.


# Why do it manually ?
Automated tools can do some jobs for us and make the process faster, but knowing how to do it manually provides us with an opportunity to understand the inner workings of the target system. Not to mention that these scripts are extremely `noisy`.


---

# The thinking process 
On Linux, everything is a `file`, and every tool fetches its data from a file somewhere in the `file system`. When performing enumeration, relying solely on automated tools can be risky they might be `flagged`, `restricted`, or simply `unavailable`. That’s why knowing where tools get their information is crucial. By understanding the Linux `file structure` and the types of files stored in each directory, we can manually extract the same data that tools display to us. Knowing the Linux file structure helps us in this scenario. Knowing which types of files reside in which folders makes our job easier.

You'll develop a deeper understanding of Linux internals and become a more effective assessor. Over time, Your `checklist` will shrink as your intuition grows, but `documentation` remains key to thoroughness. The information obtained that might be interesting to us should be `noted` immediately in an organized format so we can make connections between these pieces of information, which is an essential part of enumeration.

You will develop your own priority checklist, what things you should consider checking before the others drived from `experiences`.

---

# Automated Tools
 Some of these tools can create `false positives` , So there results should be taken as a grain of salt most of the time and flaged entries should be check `manually`.

+ [Linenum](https://github.com/rebootuser/LinEnum )
+ [Linux smart Enum](https://github.com/diego-treitos/linux-smart-enumeration )
+ [Linpeas](https://github.com/peass-ng/PEASS-ng/tree/master/linPEAS )
+ [Exploit-Suggester](https://github.com/The-Z-Labs/linux-exploit-suggester )
+ etc ..



---

# Manual Linux enumeration
We will cover these topics below : 

+ System information
+ Users & Groups 
+ Network and Ports
+ Process & Services
+ Installed applications
+ Scheduled Tasks 
+ Containers
+ Interesting files and extensions




## System information
Once we are on a system, the first thing we should know about is what `version` of operating system is being used. We can do this by looking at `/etc/issue` or `/etc/os-release`, which is a symbolic link to `/usr/lib/os-release` and, in order to check the `kernel` version, we can read `/proc/version` or by using `uname` utility.

We should also look for available `environment variables` by utilizing `env`utility and hostname using the `hostname` utility and lastly, for system `uptime`, we can utilize the `uptime` utility or by simply looking at `/proc/uptime` and `boot time` by utilizing `who`. We can also utilize python for this.

---

## User and groups
Users and groups are some of the weakest links in a Linux environment because they are very easy to misconfigure and can be a easy way to privilege escalation. 

To see the available users, we can read the `/etc/passwd` file to see which users are currently `logged in`. We can utilize the `w` or `who` utilities and to see login history we can use the `lastlog` utility. To see our current user `id`, we can either read `/etc/passwd` or use the `id` command and lastly, to check if our user holds any sudo privileges, we can use `sudo -l`.

To list available groups, we can utilize the `getent group groupname` command or read the `/etc/group` file. To see which groups our users are part of we can use the `groups` command.


---

## Network and Ports
To list available interfaces, we can use `ifconfig`, `ip` or Read files under the `/proc/net` folder for Kernel network stats and `/sys/class/net` for Device attributes. Configuration files are stored in the `/etc/network/` folder.

For hostname mapping `/etc/hosts` is used and for dns servers `/etc/resolv.conf` and lastly, for resolution order `/etc/nsswitch.conf`.

To see open ports, we can use the `ss` or `netstat` utility as well as `lsof` with the flag `-i`. Not only will we get the name of the program using those ports, but also the process ID `pid`.

---

## Process and Services
We can utilize the `ps` utility to see which processes are running. Not only that, it will show us which command started that `process`, including the exact `time` it was started at as well as its `pid`.

`PID` is a form of unique identity given to a process by the kernel. We can then use this identity to look for further information about it. We don't just have to rely on binary name.

We can utilize the `lsof` utility to see everything being utilized by the process: `files`, `Network ports`, `Unix sockets`, `libraries`, `procotols` etc...
```bash
lsof -p -i -p <pid>
```

`Systemctl` specifically can show us information about a service including its `main pid`, the `memory` it's using and lastly the path and name of the `binary` that is being utilized to run the service and a few other things.

---

## Installed applications
We can list installed applications utilizing `dpkg` and `apt` utilities or by looking at files under `/usr/bin`,`/usr/bin` and we can always look up directories where other programs, such as `snap` put its binaries in `/snap/bin` and `flatpak`in `/var/lib/flatpak/exports/bin` and user installed flatpak binaries in `~/.local/share/flatpak/exports/bin`. Lastly, `pip` keeps its binaries in `/usr/localbin`, for user `~/.local/bin`, and in available virtual environment directories using `find` utility

```bash
dpkg --list
```

```bash
apt list --installed
```

To see packages that are explicitly installed by the user and not installed as a dependency for other packages, we can do
```bash
apt-mark showmanual
```

To see snap packages and there versions
```bash
snap list
```

For flatpak
```bash
flatpak list
```

Lastly packages installed by pip can be seen using 
```bash
pip3 list
```


---

## Scheduled tasks
There are 2 ways on Linux to schedule a task, either by using the `cron` utility or by setting up `systemd timers`. To see available cronjobs we can look at `/etc/crontab` or utilize the `crontab` utility.

System timers are stored under multiple directories, but we can utilize the `systemctl` utility with the flag `list-timers -all` to look at all of them.

+ `system-wide timers`: `/etc/systemd/system`&`/usr/lib/systemd/system`
+ `User-Specific Timers`: `~/.config/systemd/user/`&`/etc/systemd/user/`

We also use `systemctl` to enable/disable as well as check the timer's status.

There’s a useful tool called `pspy` that monitors `/proc` folder in `real-time` and can reveal hidden cron jobs or processes running as other users
::github{repo="DominicBreuker/pspy"}


---

## Containers
Containers are isolated environments and usually container services create `groups` that the user has to be part of in order to operate them.

If we are not part of the group that a container management system has created, our options are to look at processes by utilizing the `ps` utility, and then we can use `grep` to filter out processes based on names such as `docker|lxc|containerd` or check for sockets they utilize, if they are active that means that the service is running. Or look for `mounted volumes` and `network interfaces` as well as `logs` and lastly `open ports`.

However, if we are part of one of those groups, we can list running `containers` and interact with them.


---

## Interesting files and Extenshions
We should always look at interesting files because they can hold alot of valuable information about the environment as well as contain usernames, passwords etc... We can utilize `find` with `grep`,`xargs` to find these files

Example: 
```bash
find / -type f  \( -name "*.sh" -o -name "*.py" \) -exec ls -la {} \; 2>/dev/null
```
Above command says:  Find every thing under `/` directory with the type of `file` and name ending with `.sh` and `.py` and lastly execute ls-la on the results found and send errors to /dev/null.

Some of the extensions that we should look for:
+ `.sh`
+ `.py`
+ `.bak`
+ `.conf|.config`
+ `.log`
+ `php,js,pl,rb,go`
+ `json,ini,env,yaml`
+ Hidden files
+ history files
+ ssh files 
+ Temporary files
+ swap files
+ Audit logs
+ Vpn configs
+ Available databases
+ Git repos


---

# Resources
::github{repo="AlessandroZ/LaZagne"}
::github{repo="DominicBreuker/pspy"}
::github{repo="huntergregal/mimipenguin"}
::github{repo="cdk-team/CDK"}
