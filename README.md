::: {align="center"}
# 🐧 Linux Production Server Capstone

### A Beginner-Friendly, Step-by-Step Linux Lab on AWS EC2

![Linux](https://img.shields.io/badge/Linux-Amazon%20Linux%202023-FCC624?logo=linux&logoColor=black)
![AWS](https://img.shields.io/badge/Cloud-AWS%20EC2-FF9900?logo=amazonaws&logoColor=white)
![Python](https://img.shields.io/badge/Application-Python-3776AB?logo=python&logoColor=white)
![Bash](https://img.shields.io/badge/Scripting-Bash-4EAA25?logo=gnubash&logoColor=white)
![Port](https://img.shields.io/badge/HTTP-Port%208080-0EA5E9)

*Learn by doing: inspect a server, run a website, manage services, read
logs, create backups, and troubleshoot problems.*
:::

------------------------------------------------------------------------

## 🌟 1. About This Project

In this project, I created and managed a Linux server on **AWS EC2** and
deployed a simple web application on it.

The purpose is to learn how to:

-   🖥️ Inspect and manage a Linux server.
-   👥 Understand users, groups, and permissions.
-   🌐 Run and access a simple web application.
-   ⚙️ Start, stop, and manage an application service.
-   📋 Read logs to investigate problems.
-   💾 Check disk space and create backups.
-   🩺 Run a health-check script.
-   🛠️ Troubleshoot and restore an application.

This repository contains the commands, scripts, configuration files, and
reports used during the lab.

## 🏗️ 2. Architecture: How Does It Work?

``` mermaid
flowchart TD
    A["👤 Client / Web Browser"] -->|"HTTP · Port 8080"| B["☁️ AWS EC2 Linux Server"]
    B --> C["🌐 Web Application"]
    B --> D["⚙️ systemd Service"]
    B --> E["📋 Logs"]
    B --> F["💾 Backups"]
    B --> G["🩺 Health Check Script"]
```

### In simple words

  -----------------------------------------------------------------------
  Part                                What it means
  ----------------------------------- -----------------------------------
  👤 **Client**                       The person opening the website in a
                                      browser.

  ☁️ **Linux Server**                 The EC2 virtual machine that runs
                                      the application.

  🌐 **Application**                  The web page hosted on the server.

  ⚙️ **systemd**                      Starts, stops, and manages the
                                      application.

  📋 **Logs**                         Records that help us understand
                                      what happened.

  💾 **Backups**                      Copies of important application and
                                      configuration files.

  🩺 **Health Check**                 A script used to check application
                                      availability.
  -----------------------------------------------------------------------

## 🧰 3. What You Need Before Starting

-   An AWS account.
-   An EC2 instance running Amazon Linux 2023.
-   SSH access to the instance.
-   Python 3 installed.
-   An EC2 security group configured for SSH access.
-   TCP port `8080` allowed from your intended client when browser
    access is required.

> \[!CAUTION\] Never upload AWS credentials, passwords, private keys, or
> other secrets to GitHub. Restrict inbound network access to the
> sources that need it.

## 🚀 4. Step-by-Step Lab

Run these commands on your EC2 server unless a step says to use your
browser.

### 🖥️ Step 1 --- Check the Linux Server

``` bash
hostname
hostname -I
cat /etc/os-release
uname -r
```

**What these commands do:**

-   `hostname` --- shows the server name.
-   `hostname -I` --- shows the server's IP addresses.
-   `cat /etc/os-release` --- shows the Linux operating system.
-   `uname -r` --- shows the Linux kernel version.

**Goal:** Know which server and operating system you are working on.

### 📁 Step 2 --- Check the Directory Structure

``` bash
find /opt/akshara -maxdepth 2 -type d
```

This lists the folders used by the application.

  Folder      Purpose
  ----------- ---------------------
  `app`       Application files
  `config`    Configuration files
  `logs`      Application logs
  `backups`   Backup archives
  `scripts`   Automation scripts
  `data`      Lab data files

**Goal:** Understand where the application's files and supporting files
are stored.

### 👥 Step 3 --- Check Users and Groups

``` bash
whoami
id
getent group developers
```

A **user** is an account on Linux. A **group** lets several users share
access to files and directories.

-   `whoami` --- shows the current user.
-   `id` --- shows the user's ID and group memberships.
-   `getent group developers` --- looks up the `developers` group.

**Goal:** Know which user you are logged in as and which groups grant
access.

### 🔐 Step 4 --- Check File Permissions

``` bash
ls -ld /opt/akshara/app
ls -l /opt/akshara/app
```

Linux permissions determine who can read, change, or access files and
directories.

-   `ls -ld` --- shows ownership and permissions on the application
    directory.
-   `ls -l` --- shows ownership and permissions on files inside it.

**Goal:** Understand who owns the files and who is allowed to access
them.

### 🌐 Step 5 --- Access the Application

The application files are stored in:

``` text
/opt/akshara/app
```

The lab uses Python's built-in HTTP server on port `8080`, managed
through systemd.

Test the application from the EC2 server:

``` bash
curl http://localhost:8080
```

To open it from your own computer, visit:

``` text
http://SERVER-IP:8080
```

Replace `SERVER-IP` with the EC2 instance's reachable public IP address
or DNS name.

> \[!TIP\] If the website does not open, check the service, listening
> port, IP address, and EC2 security group. A private IP address
> generally works only from within the relevant private network.

**Goal:** Confirm that the application can respond to an HTTP request.

### ⚙️ Step 6 --- Manage the Application Service

Check the service status:

``` bash
sudo systemctl status akshara-app
```

Useful commands:

``` bash
sudo systemctl start akshara-app
sudo systemctl stop akshara-app
sudo systemctl restart akshara-app
sudo systemctl is-enabled akshara-app
```

  -----------------------------------------------------------------------
  Command                             Simple meaning
  ----------------------------------- -----------------------------------
  `start`                             Start the service

  `stop`                              Stop the service

  `restart`                           Restart the service

  `is-enabled`                        Check whether it is configured to
                                      start automatically during boot
  -----------------------------------------------------------------------

**Goal:** Learn to control the application without manually launching
the Python process.

### 🔎 Step 7 --- Check Processes and Ports

Check running Python processes:

``` bash
ps -ef | grep '[p]ython3'
```

Check whether port `8080` is listening:

``` bash
sudo ss -lntp | grep 8080
```

A **process** is a running program. A **port** is a numbered network
endpoint used by a program to receive connections.

**Goal:** Check whether the program is running and whether it is
listening for connections.

### 📋 Step 8 --- Check Application Logs

Check logs maintained by systemd:

``` bash
sudo journalctl -u akshara-app -n 20
```

Check the application log file:

``` bash
tail -n 20 /opt/akshara/logs/app.log
```

Logs help investigate errors and understand what happened when an
application fails.

> \[!NOTE\] The systemd journal and `app.log` are separate sources. They
> may contain different information; the application log file may not
> record every HTTP request.

**Goal:** Learn where to look for information when the application
behaves unexpectedly.

### 💽 Step 9 --- Check Disk Usage

``` bash
df -h
```

This shows how much disk space is used and available on each filesystem.

``` bash
sudo du -sh /opt/akshara/*
```

This shows how much space each item under `/opt/akshara` uses.

**Easy way to remember:** - `df` = filesystem space. - `du` = space used
by files and directories.

### 💾 Step 10 --- Create a Backup

Run the backup script:

``` bash
sudo /opt/akshara/scripts/backup.sh
```

Check the generated backup files:

``` bash
sudo ls -lh /opt/akshara/backups
```

Check the backup log:

``` bash
sudo tail -n 20 /opt/akshara/backups/backup.log
```

The script creates a timestamped compressed archive of the application
and configuration directories and records the result in the backup log.

**Goal:** Confirm that a backup archive was created and the script
reported success.

### 🩺 Step 11 --- Run the Health-Check Script

``` bash
sudo /opt/akshara/scripts/health-check.sh
```

This runs the lab's health-check script. Read its output to understand
which checks were performed and whether they passed.

The exact checks depend on the contents of `health-check.sh`.

**Goal:** Practise checking application health with a script rather than
relying only on a browser.

### ♻️ Step 12 --- Test Backup Restoration

First, list the contents of an actual backup archive:

``` bash
sudo tar -tzf /opt/akshara/backups/NAME-OF-BACKUP.tar.gz
```

Replace `NAME-OF-BACKUP.tar.gz` with a real archive filename.

Create a separate directory for the restore test:

``` bash
mkdir -p ~/restore-test
```

Extract the archive into that directory:

``` bash
sudo tar -xzf /opt/akshara/backups/NAME-OF-BACKUP.tar.gz -C ~/restore-test
```

Check the extracted files:

``` bash
find ~/restore-test -maxdepth 3 -type f
```

> \[!WARNING\] Use a separate directory for practice so you do not
> accidentally overwrite the live application. Confirm that the expected
> files exist before considering the restore test successful.

**Goal:** Verify that files can be recovered from a backup archive.

## 🛠️ 5. Final Troubleshooting Lab

**Scenario:** The instructor stops `akshara-app`, and the website
becomes unavailable.

Follow the checks in order instead of immediately restarting the
service.

``` mermaid
flowchart TD
    A["🌐 Website unavailable"] --> B["🔎 Check process"]
    B --> C["🔌 Check port 8080"]
    C --> D["⚙️ Check systemctl status"]
    D --> E["📋 Check journalctl logs"]
    E --> F["▶️ Start service"]
    F --> G["🔌 Verify port"]
    G --> H["✅ Test application with curl"]
```

### Step 1 --- Check the process

``` bash
ps -ef | grep '[p]ython3'
```

Check whether the application process is running.

### Step 2 --- Check the port

``` bash
sudo ss -lntp | grep 8080
```

If the application is stopped, there may be no output because nothing is
listening on port `8080`.

### Step 3 --- Check the service

``` bash
sudo systemctl status akshara-app
```

Check whether the service is running or stopped.

### Step 4 --- Check the logs

``` bash
sudo journalctl -u akshara-app -n 20
```

Review the available service logs for useful information.

### Step 5 --- Start the service

``` bash
sudo systemctl start akshara-app
```

### Step 6 --- Verify the port

``` bash
sudo ss -lntp | grep 8080
```

Confirm that the application is listening on port `8080`.

### Step 7 --- Verify the application

``` bash
sudo systemctl status akshara-app
curl http://localhost:8080
```

Confirm that the service is running and the application responds.

> \[!IMPORTANT\] **Remember the sequence:** Process → Port → Service
> status → Logs → Fix → Verify. Check first, understand the result, and
> then take action.

## 🚧 6. Common Problems and What to Check

### 🔴 Problem 1 --- Website Not Opening

Run:

``` bash
sudo systemctl status akshara-app
sudo ss -lntp | grep 8080
curl http://localhost:8080
```

Check the application status, listening port, and local response. If
local access works but browser access does not, check the public IP/DNS
and EC2 security group.

### 🟠 Problem 2 --- Permission Denied

Run:

``` bash
whoami
id
ls -ld /opt/akshara/app
ls -l /opt/akshara/app
```

Check the current user, group membership, ownership, and permissions
before changing anything.

### 🟡 Problem 3 --- Backup Failure

Run:

``` bash
sudo cat /opt/akshara/scripts/backup.sh
sudo tail -n 20 /opt/akshara/backups/backup.log
sudo ls -lh /opt/akshara/backups
```

Check the script, backup directory, permissions, log, and whether a new
archive was created.

## 📂 7. Repository Structure

``` text
linux-weekend-capstone/
├── README.md
├── reports/
│   ├── server-info.txt
│   ├── permission-test.txt
│   ├── network-test.txt
│   └── storage-report.txt
├── application/
│   ├── index.html
│   ├── version.txt
│   └── README.txt
├── config/
│   └── app.conf
├── scripts/
│   ├── backup.sh
│   └── health-check.sh
├── service/
│   └── akshara-app.service
├── logs/
│   └── app.log
└── screenshots/
```

  Folder           What it contains
  ---------------- ------------------------------------------
  `reports/`       Saved outputs from server checks
  `application/`   Web page and related application files
  `config/`        Application configuration
  `scripts/`       Backup and health-check scripts
  `service/`       systemd service configuration
  `logs/`          Application log file
  `screenshots/`   Screenshots of lab results, if collected

## 📚 8. Linux Commands Practiced

  Command        Simple purpose
  -------------- ---------------------------------------------
  `hostname`     Show server name
  `uname`        Show kernel information
  `whoami`       Show current user
  `id`           Show user and group IDs
  `getent`       Look up users and groups
  `ls`           List files and permissions
  `find`         Find files and directories
  `chmod`        Change permissions
  `chown`        Change ownership
  `systemctl`    Manage services
  `journalctl`   Read systemd logs
  `ps`           View processes
  `ss`           Inspect network sockets and listening ports
  `curl`         Test an HTTP endpoint
  `df`           Check filesystem space
  `du`           Check file and directory usage
  `tail`         Show the end of a file
  `grep`         Search text
  `tar`          Create or extract archives
  `gzip`         Compress files

## 🎯 9. What I Learned

By completing this lab, I practised how to:

-   Inspect and manage a Linux server.
-   Create users and groups and understand permissions.
-   Deploy a simple web application.
-   Manage an application using systemd.
-   Inspect processes, ports, and logs.
-   Check disk space.
-   Create and test backups.
-   Run shell scripts.
-   Troubleshoot a stopped application systematically.

## ⚠️ 10. Important Note

This is a learning project that demonstrates basic Linux administration.
Python's built-in HTTP server is suitable for this lab but is not
intended as a hardened production web server. Real production systems
require additional security, monitoring, and deployment controls.

------------------------------------------------------------------------

::: {align="center"}
**🐧 Learn Linux · Practise regularly · Troubleshoot methodically**

*Built as a hands-on learning project.*
:::
