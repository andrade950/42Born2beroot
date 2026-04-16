<div align="center">

# 🖥️ Born2beRoot

**A System Administration project from 42 School**

*Set up your first virtual server — and understand every choice you make.*

[![Tutorial](https://img.shields.io/badge/📖_Tutorial-English-4A90D9?style=for-the-badge)](born2beroot_tutorial_en.md)
[![Evaluation Guide](https://img.shields.io/badge/✅_Evaluation_Guide-English-2ECC71?style=for-the-badge)](born2beroot_evaluation_guide_en.md)
[![Subject PDF](https://img.shields.io/badge/📄_Subject-PDF-E74C3C?style=for-the-badge)](en.subject.pdf)

</div>

---

## 📌 Overview

**Born2beRoot** is a system administration project that introduces you to the world of **virtualization**. You will create and configure a virtual machine from scratch, set up a secure Linux server environment, and demonstrate a solid understanding of how the system works — not just that it works.

> *"You can do anything you want to do. This virtual machine is your world."*

---

## 🎯 Objectives

By completing this project, you will be able to:

- Set up a **virtual machine** using VirtualBox (or UTM)
- Install and configure a **Debian** or **Rocky Linux** server with no graphical interface
- Implement **LVM encrypted partitions**
- Configure and harden **SSH**, **UFW/Firewalld**, and **sudo**
- Enforce a strict **password policy**
- Write a **bash monitoring script** that broadcasts system info every 10 minutes
- Understand and explain every component of your setup during the defense

---

## 🗂️ Project Structure

```
repository/
└── signature.txt     ← SHA1 signature of your virtual disk (.vdi)
```

> ⚠️ You **must not** submit the virtual machine itself. Only `signature.txt` goes in the repository.

---

## ⚙️ Mandatory Requirements

### Operating System
Choose one:
- **Debian** *(recommended for beginners)* — must have **AppArmor** running at startup
- **Rocky Linux** — must have **SELinux** running at startup (KDump not required)

No graphical interface allowed. Installing X.org or equivalent results in a **grade of 0**.

---

### Partitioning
You must create **at least 2 encrypted LVM partitions**. Example structure:

```
sda
├── sda1       487M   /boot
├── sda2         1K
└── sda5       7.5G
    └── sda5_crypt  (LVM)
        ├── root    2.8G   /
        ├── swap    976M   [SWAP]
        └── home    3.8G   /home
```

---

### SSH
- Runs exclusively on **port 4242**
- **Root login via SSH is disabled**
- Tested during evaluation by connecting with a newly created user

---

### Firewall (UFW / Firewalld)
- Must be **active on startup**
- Only **port 4242** open by default
- Rocky Linux uses **Firewalld**; Debian uses **UFW**

---

### User & Groups
- A non-root user with your login must exist
- That user must belong to both `sudo` and `user42` groups

---

### Password Policy

| Rule | Value |
|---|---|
| Maximum password age | 30 days |
| Minimum days before change | 2 days |
| Warning before expiry | 7 days |
| Minimum length | 10 characters |
| Must contain | uppercase, lowercase, digit |
| Max consecutive identical chars | 3 |
| Cannot contain | the username |
| Must differ from old password by | 7 characters *(non-root only)* |

---

### Sudo Configuration

| Setting | Value |
|---|---|
| Password attempts | 3 max |
| Wrong password message | Custom message |
| Log location | `/var/log/sudo/` |
| TTY mode | Enabled |
| Restricted paths | `/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin` |

---

### Monitoring Script (`monitoring.sh`)

A bash script that broadcasts the following info to all terminals every **10 minutes** via `wall`:

- OS architecture and kernel version
- Number of physical and virtual processors
- RAM usage (used / total / percentage)
- Disk usage (used / total / percentage)
- CPU load percentage
- Date and time of last reboot
- Whether LVM is active
- Number of active TCP connections
- Number of logged-in users
- IPv4 address and MAC address
- Number of `sudo` commands executed

**Example output:**
```
#Architecture: Linux wil 4.19.0-16-amd64 #1 SMP Debian 4.19.181-1 x86_64 GNU/Linux
#CPU physical : 1
#vCPU : 1
#Memory Usage: 74/987MB (7.50%)
#Disk Usage: 1009/2Gb (49%)
#CPU load: 6.7%
#Last boot: 2021-04-25 14:45
#LVM use: yes
#Connections TCP : 1 ESTABLISHED
#User log: 1
#Network: IP 10.0.2.15 (08:00:27:51:9b:a5)
#Sudo : 42 cmd
```

> During evaluation, you will be asked to **explain the script** and to **stop it without modifying it** (hint: use `cron`).

---

## 🎁 Bonus Part

> ⚠️ Bonus is only evaluated if the **mandatory part is perfect**.

- Set up **advanced partitioning** with additional LVM volumes (`/var`, `/srv`, `/tmp`, `/var/log`)
- Deploy a **WordPress website** using **lighttpd**, **MariaDB**, and **PHP**
- Set up one additional useful service of your choice *(NGINX and Apache2 are excluded)*

---

## 🔏 Submission

1. Turn off your VM
2. Generate the SHA1 signature of your `.vdi` file:
   ```bash
   # Linux / macOS
   sha1sum your_vm.vdi
   # or
   shasum your_vm.vdi
   ```
3. Save the output to `signature.txt` at the root of your repository:
   ```bash
   echo <your_signature> > signature.txt
   ```
4. **Clone or snapshot** your VM before any evaluation to preserve the signature

---

## 📚 Resources

| Resource | Link |
|---|---|
| 📖 Step-by-step Tutorial (EN) | [born2beroot_tutorial_en.md](born2beroot_tutorial_en.md) |
| ✅ Evaluation Guide (EN) | [born2beroot_evaluation_guide_en.md](born2beroot_evaluation_guide_en.md) |
| 📄 Official Subject (PDF) | [en.subject.pdf](en.subject.pdf) |

---

## 🧠 Key Concepts to Understand for Defense

- What is a **virtual machine** and why use one?
- Differences between **Debian** and **Rocky**
- **apt** vs **aptitude** (Debian) / **SELinux** and **DNF** (Rocky)
- What is **AppArmor**?
- How does **LVM** work?
- What is **cron** and how is it configured?
- How does **UFW** / **Firewalld** work?
- How does **SSH** work and why disable root login?

---