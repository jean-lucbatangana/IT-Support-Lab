# 🖥️ IT Support Lab — Remote Desktop & Linux Administration

> A hands-on IT helpdesk simulation lab practising real-world remote support, Linux administration, and end-user troubleshooting across Windows and Ubuntu environments.

---

## 📋 Project Overview

This lab simulates a real helpdesk environment where a Windows technician machine remotely supports an Ubuntu end-user machine via RDP. All tasks are structured as support tickets, mirroring real IT support workflows.

**Environment:**
- 🖥️ **Host Machine:** Windows 11
- 🐧 **Remote Machine:** Ubuntu 24.04 LTS (VirtualBox VM)
- 🔗 **Remote Access Tool:** xrdp (Remote Desktop Protocol)
- 🌐 **Network:** VirtualBox Host-Only Adapter (`192.168.56.101`)

---

## 🛠️ Lab Setup

### Prerequisites
- [VirtualBox](https://www.virtualbox.org/) installed on Windows host
- Ubuntu 24.04 LTS Desktop VM
- Windows Remote Desktop Connection (`mstsc`)

### Environment Configuration

| Component | Details |
|-----------|---------|
| VM Network Adapter | Host-Only (`192.168.56.101`) |
| Remote Desktop | xrdp + XFCE desktop session (Xorg) |
| Admin User | `jean-luc` (sudoer) |
| Standard User | `john` (no sudo privileges) |
| Additional User | `sarah` (created during lab) |
| Desktop Environment | XFCE4 (Wayland disabled for xrdp compatibility) |

### Key Configuration Fixes Applied
- Disabled Wayland in `/etc/gdm3/custom.conf` → `WaylandEnable=false`
- Set `~/.xsession` to `startxfce4` for all RDP users
- Fixed colord polkit permissions for session stability
- Installed VirtualBox Guest Additions for clipboard sharing
- Added xrdp to `ssl-cert` group

---

## 🎫 Support Tickets Completed

### Ticket #001 — Remote Software Installation
**Request:** User needs a text editor installed remotely.  
**Solution:** Escalated privileges via `su - jean-luc` → `sudo apt install gedit -y`  
**Skills:** Package management, privilege escalation

---

### Ticket #002 — Remote Folder Creation
**Request:** Create a folder called "Work Documents" on the user's Desktop.  
**Solution:** `mkdir "Work Documents"` navigating from `~/Desktop`  
**Skills:** File system navigation, directory management

---

### Ticket #003 — Remote File Management
**Request:** Move `notes.txt` from Desktop into the Work Documents folder.  
**Solution:** `mv ~/Desktop/notes.txt ~/Desktop/Work_Documents/`  
**Skills:** File operations, path navigation

---

### Ticket #004 — Display Settings Check
**Request:** User reports screen resolution looks wrong, requests 1920x1080.  
**Solution:** Verified via XFCE Display Settings — already set to 1920x1080. No change needed.  
**Skills:** Settings verification, end-user communication

---

### Ticket #005 — System Information Gathering
**Request:** Report the machine's IP address, RAM, and OS version.  
**Solution:**
```bash
ip a              # IP: 192.168.56.101
free -h           # RAM: 3.3GB
lsb_release -a    # Ubuntu 24.04 LTS
```
**Skills:** System diagnostics, information reporting

---

### Ticket #006 — Missing Package Diagnosis
**Request:** User reports `wget` command not found error.  
**Solution:** Verified `wget` was already installed. Identified as user error — educated user on correct usage: `wget <URL>`  
**Skills:** Troubleshooting, user education, diagnosing user error vs system error

---

### Ticket #007 — User Account Creation
**Request:** Create a new user account for colleague "sarah".  
**Solution:**
```bash
su - jean-luc
sudo adduser sarah
```
Verified with `cat /etc/passwd | grep sarah`  
**Skills:** User management, account provisioning

---

### Ticket #008 — Disk Space Check
**Request:** User worried the drive might be getting full.  
**Solution:**
```bash
df -h
```
**Finding:** Main drive `/dev/sda2` — 25GB total, 12GB used (49%). No immediate concern.  
**Skills:** Storage diagnostics, reporting findings

---

### Ticket #009 — Remote File Search
**Request:** User can't locate a file called `notes.txt`.  
**Solution:**
```bash
find / -name "notes.txt" 2>/dev/null
```
**Finding:** Located in `~/Desktop/Work_Documents/`  
**Skills:** File search, system navigation

---

### Ticket #010 — Network Connectivity Check
**Request:** Verify internet connectivity and confirm google.com is reachable.  
**Solution:**
```bash
ping -c 4 www.google.com
```
**Finding:** 0% packet loss, connection healthy.  
**Skills:** Network diagnostics, connectivity testing

---

### Ticket #011 — Performance Troubleshooting
**Request:** Machine running slow — check CPU and memory usage.  
**Solution:**
```bash
ps aux --sort=-%cpu | head -10    # Static snapshot
top                                # Live monitoring
```
**Finding:** All processes within normal ranges. Highest persistent CPU: Xorg at 1.9%. Recommended reboot and monitoring.  
**Skills:** Process management, performance analysis, `ps`, `top`

---

### Ticket #012 — SSH Service Management
**Request:** Check if SSH is installed and running. Start it if not and enable on boot.  
**Solution:**
```bash
sudo systemctl status ssh
sudo apt install openssh-server -y
sudo systemctl start ssh
sudo systemctl enable ssh
```
**Skills:** Service management, `systemctl`, SSH configuration

---

## 🧰 Commands Reference

| Command | Purpose |
|---------|---------|
| `ip a` | Show network interfaces and IP addresses |
| `free -h` | Display RAM usage in human-readable format |
| `lsb_release -a` | Show Ubuntu version information |
| `df -h` | Display disk space usage |
| `find / -name "file"` | Search for a file system-wide |
| `ps aux --sort=-%cpu` | List processes sorted by CPU usage |
| `top` | Live process and resource monitor |
| `ping -c 4 <host>` | Test network connectivity |
| `sudo apt install <pkg>` | Install a package |
| `sudo adduser <name>` | Create a new user account |
| `systemctl status <svc>` | Check service status |
| `systemctl start <svc>` | Start a service |
| `systemctl enable <svc>` | Enable service on boot |
| `su - <user>` | Switch to another user |
| `cat /etc/passwd` | View user accounts |

---

## 📚 Key Learnings

- **NAT vs Host-Only Networking:** NAT blocks host-to-VM connectivity. Host-Only adapter is required for RDP from a Windows host.
- **Wayland & xrdp conflict:** GNOME/Wayland causes xrdp session failures. XFCE + Xorg is the reliable workaround.
- **Privilege escalation:** Standard users can't run sudo. Use `su - adminuser` then sudo, or run admin commands directly as the admin user.
- **User error vs system error:** Always verify the issue exists before attempting a fix (Ticket #006).
- **File permissions:** Writing to other users' home directories requires `sudo bash -c` to handle permissions correctly.

---

## 🗺️ Roadmap

- [x] Remote desktop setup and configuration
- [x] Basic file and folder management tickets
- [x] User account management
- [x] System diagnostics and reporting
- [x] Service management (SSH)
- [ ] SSH remote access practice
- [ ] Secure remote access (key-based authentication)
- [ ] Multi-platform troubleshooting (Windows ↔ Linux)
- [ ] Simulated network fault diagnosis
- [ ] Scripting repetitive support tasks with Bash

---

## 👤 Author

**Jean-Luc**  
IT Support Lab — Self-directed learning project  
Built with VirtualBox · Ubuntu 24.04 · xrdp · Windows 11
