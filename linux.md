# Linux A to Z Commands Cheat Sheet

A complete Linux command reference covering beginner, intermediate, and advanced commands used by System Administrators, DevOps Engineers, SREs, Cloud Engineers, and Developers.

---

# A. Archive Commands

## Create Archive

```bash
tar -cvf backup.tar files/
```

### Explanation

- `c` = create archive
- `v` = verbose output
- `f` = archive filename

## Extract Archive

```bash
tar -xvf backup.tar
```

## Compressed Archive

```bash
tar -czvf backup.tar.gz files/
```

---

# B. Basic File Commands

## List Files

```bash
ls
```

## Detailed List

```bash
ls -ltr
```

## Hidden Files

```bash
ls -la
```

### Explanation

- `l` = long format
- `a` = all files
- `t` = sort by time
- `r` = reverse order

---

# C. Change Directory

## Move Directory

```bash
cd /opt
```

## Home Directory

```bash
cd
```

## Previous Directory

```bash
cd -
```

---

# D. Disk Usage Commands

## Check Filesystem Usage

```bash
df -h
```

### Explanation

- `d` = disk filesystem
- `h` = human readable

## Directory Usage

```bash
du -sh /var
```

---

# E. Edit Files

## Using Vi Editor

```bash
vi file.txt
```

## Using Nano

```bash
nano file.txt
```

---

# F. Find Commands

## Find File

```bash
find / -name file.txt
```

## Find Directory

```bash
find / -type d -name logs
```

## Find Modified Files

```bash
find . -mtime -7
```

---

# G. Grep Commands

## Search Text

```bash
grep "error" application.log
```

## Ignore Case

```bash
grep -i "error" application.log
```

## Recursive Search

```bash
grep -r "password" .
```

---

# H. Hostname Commands

## View Hostname

```bash
hostname
```

## Change Hostname

```bash
hostnamectl set-hostname server01
```

---

# I. Information Commands

## OS Information

```bash
cat /etc/os-release
```

## Kernel Information

```bash
uname -a
```

## Architecture

```bash
arch
```

---

# J. Jobs & Background Tasks

## Run in Background

```bash
command &
```

## View Jobs

```bash
jobs
```

## Bring Job Foreground

```bash
fg
```

---

# K. Kill Commands

## Kill Process

```bash
kill PID
```

## Force Kill

```bash
kill -9 PID
```

## Kill by Name

```bash
pkill nginx
```

---

# L. Logs Commands

## View Log File

```bash
cat /var/log/messages
```

## Follow Logs

```bash
tail -f application.log
```

## Last 100 Lines

```bash
tail -100 application.log
```

---

# M. Memory Commands

## Memory Utilization

```bash
free -m
```

## VM Statistics

```bash
vmstat
```

## Top Processes

```bash
top
```

---

# N. Networking Commands

## IP Address

```bash
ip addr
```

## Routing Table

```bash
ip route
```

## Interface Information

```bash
ip link show
```

---

# O. Ownership Commands

## Change Owner

```bash
chown user:user file.txt
```

## Recursive Ownership

```bash
chown -R user:user folder/
```

---

# P. Process Commands

## Running Processes

```bash
ps -ef
```

## Search Process

```bash
ps -ef | grep nginx
```

## Interactive Process Viewer

```bash
top
```

---

# Q. Query System Information

## CPU Information

```bash
lscpu
```

## Memory Information

```bash
cat /proc/meminfo
```

## Block Devices

```bash
lsblk
```

---

# R. Remove Commands

## Remove File

```bash
rm file.txt
```

## Remove Directory

```bash
rm -rf directory/
```

### Warning

```bash
rm -rf /
```

Never run this command.

---

# S. Service Commands

## Start Service

```bash
systemctl start nginx
```

## Stop Service

```bash
systemctl stop nginx
```

## Restart Service

```bash
systemctl restart nginx
```

## Check Status

```bash
systemctl status nginx
```

---

# T. Text Processing

## Display File

```bash
cat file.txt
```

## First 10 Lines

```bash
head file.txt
```

## Last 10 Lines

```bash
tail file.txt
```

## Sort File

```bash
sort file.txt
```

---

# U. User Commands

## Current User

```bash
whoami
```

## Logged In Users

```bash
who
```

## Create User

```bash
useradd devops
```

## Change Password

```bash
passwd devops
```

---

# V. View File Contents

## View File

```bash
cat file.txt
```

## Page by Page View

```bash
less file.txt
```

## Line Numbers

```bash
nl file.txt
```

---

# W. Network/Web Commands

## Test Connectivity

```bash
ping google.com
```

## Download File

```bash
wget https://example.com/file.zip
```

## API Call

```bash
curl https://api.example.com
```

---

# X. Compression Commands

## Compress

```bash
gzip file.txt
```

## Extract

```bash
gunzip file.txt.gz
```

## ZIP Archive

```bash
zip backup.zip file.txt
```

## Unzip

```bash
unzip backup.zip
```

---

# Y. YAML & Environment Variables

## Show Environment Variables

```bash
env
```

## Export Variable

```bash
export JAVA_HOME=/usr/java/jdk17
```

## Verify Variable

```bash
echo $JAVA_HOME
```

---

# Z. Advanced Administration Commands

## Scheduled Tasks

```bash
crontab -l
```

## Edit Cron Jobs

```bash
crontab -e
```

## System Logs

```bash
journalctl -xe
```

## Reboot System

```bash
reboot
```

## Shutdown System

```bash
shutdown -h now
```

---

# File Management Commands

## Create File

```bash
touch file.txt
```

## Copy File

```bash
cp source.txt destination.txt
```

## Move/Rename File

```bash
mv old.txt new.txt
```

## Create Directory

```bash
mkdir projects
```

---

# Permission Commands

## View Permissions

```bash
ls -l
```

Example:

```text
-rwxr-xr-x
```

Meaning:

```text
Owner  : rwx
Group  : r-x
Others : r-x
```

## Change Permissions

```bash
chmod 755 script.sh
```

## Full Access

```bash
chmod 777 file.txt
```

Use carefully.

---

# SSH Commands

## Connect Server

```bash
ssh user@server
```

## SSH Key Generation

```bash
ssh-keygen
```

## Copy SSH Key

```bash
ssh-copy-id user@server
```

## Secure File Copy

```bash
scp file.txt user@server:/tmp/
```

---

# Package Management (Ubuntu)

## Update Packages

```bash
apt update
```

## Upgrade Packages

```bash
apt upgrade -y
```

## Install Package

```bash
apt install nginx -y
```

## Remove Package

```bash
apt remove nginx
```

---

# Package Management (RHEL/CentOS)

## Install Package

```bash
yum install nginx -y
```

## Update System

```bash
yum update -y
```

## Package Information

```bash
rpm -qa
```

---

# Top 50 Linux Commands Used Daily

```bash
pwd
ls -ltr
cd
mkdir
rm -rf
cp
mv
touch
cat
less
head
tail
grep
find
awk
sed
sort
uniq
cut
wc
chmod
chown
df -h
du -sh
free -m
top
ps -ef
kill
pkill
systemctl
journalctl
ip addr
ping
curl
wget
ssh
scp
tar
gzip
zip
unzip
cron
hostname
uname -a
env
export
whoami
mount
umount
lsblk
reboot
shutdown
```

---

# Linux Troubleshooting Commands

## CPU

```bash
top
htop
```

## Memory

```bash
free -m
vmstat
```

## Disk

```bash
df -h
du -sh *
```

## Network

```bash
ip addr
ping google.com
```

## Service

```bash
systemctl status nginx
```

## Logs

```bash
journalctl -xe
tail -f /var/log/messages
```

---

# Linux Boot Process

```text
Power ON
   │
   ▼
BIOS / UEFI
   │
   ▼
Bootloader (GRUB)
   │
   ▼
Kernel
   │
   ▼
Init/Systemd
   │
   ▼
Services
   │
   ▼
Login Prompt
```

---

# Linux File System Structure

```text
/
├── bin
├── boot
├── dev
├── etc
├── home
├── lib
├── media
├── mnt
├── opt
├── proc
├── root
├── run
├── sbin
├── srv
├── tmp
├── usr
├── var
```

---

# Summary

This cheat sheet covers:

- Linux Basics
- File Management
- User Management
- Permissions
- Networking
- Process Management
- Service Administration
- SSH & Security
- Package Management
- Monitoring
- Troubleshooting
- Storage Management
- System Administration

⭐ Keep this README as a complete Linux quick reference for DevOps, SRE, System Administration, Cloud, and Production Support activities.
