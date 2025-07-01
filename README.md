# ☁️ Personal Home Cloud Server (DIY) Using Nextcloud + Linux Mint XFCE

> 🚀 Turned an old Lenovo G550 laptop (with a broken screen) into a fully functional home-based personal cloud server using Linux Mint XFCE and Nextcloud.  
> 🌍 Accessible over local Wi-Fi and ready for DuckDNS-based global access.

---

## 🧠 Problem Statement

Family members often face storage issues on their phones, and cloud services like Google Drive have limitations or privacy concerns. The goal was to repurpose an old, low-spec laptop into a self-hosted, reliable, and secure cloud server — accessible from both mobile and desktop, and usable even without a working built-in display.

---

## 🎯 Objectives

- Build a 24/7 file and media cloud server on old hardware
- Enable mobile-to-server file sync (like Google Photos/Drive)
- Make the server remotely accessible (via DuckDNS and port forwarding)
- Prevent system sleep or shutdown on lid-close
- Showcase full-stack integration using Linux, Web UI, mobile, and network config

---

## 💻 Hardware Used

| Component         | Specification                                   |
|------------------|-------------------------------------------------|
| Laptop           | Lenovo G550 (Core2Duo, 3GB RAM, 320GB HDD)       |
| Screen           | Broken — relied on HDMI external display         |
| Network          | Wi-Fi LAN (router with port forwarding)          |
| Clients          | Android phone, Dell laptop (secondary machine)   |

---

## 🧰 Software Stack

| Layer           | Technology                                  |
|----------------|---------------------------------------------|
| OS              | Linux Mint XFCE 22.1                        |
| Server App      | Nextcloud (Snap version)                   |
| Mobile Bridge   | KDE Connect (Android ↔ Linux sync)         |
| DNS             | DuckDNS (Dynamic Public Domain)            |
| Sleep Control   | Caffeine + XFCE Power Config + systemd     |
| Access Protocols| HTTP, WebDAV, Web UI, local mount          |

---

## 🔧 Step-by-Step Setup Guide

### 1. 🧑‍💻 Install Linux Mint XFCE on Old Laptop

- Created a bootable USB using another PC (Mintstick)
- Booted Linux Mint Live session via HDMI (since screen was broken)
- Installed OS to internal HDD
- Set up persistent external monitor using `xrandr` and `~/.xprofile`

### 2. 🔌 Prevent Sleep, Lid Suspend, Screensaver Lock

![photo_2025-07-01_16-21-49](https://github.com/user-attachments/assets/4b9046c0-46d1-4230-aa6e-335e244ea31d)


- Edited `/etc/systemd/logind.conf`:

```ini
HandleLidSwitch=ignore
HandleLidSwitchDocked=ignore
IdleAction=ignore
```
![photo_2025-07-01_16-21-49 (2)](https://github.com/user-attachments/assets/c83d3964-4b90-4a5a-b54f-f1f0a6cf65aa)

# Disabled screensaver:
```ini
xfce4-screensaver-preferences
xfce4-power-manager-settings
```
![photo_2025-07-01_16-21-49 (3)](https://github.com/user-attachments/assets/57519107-40f7-48ff-b2be-411a31431e0a)
![photo_2025-07-01_16-21-49 (4)](https://github.com/user-attachments/assets/60cd2c00-eba2-471b-b123-5438f10b37ad)

- Added caffeine & to auto-start session to prevent sleep

### 3. ☁️ Install and Run Nextcloud Snap Server
```ini
sudo apt update
sudo apt install snapd
sudo snap install nextcloud
```

- Accessed the server from another device on the LAN:
  http://192.168.1.XX/

### 📂 Exposing Local Folders to Nextcloud
Since Snap version can't access /home, we moved files to /media/.
```ini
sudo mkdir /media/nextcloud-data
sudo cp -rv ~/Videos/. /media/nextcloud-data/
sudo cp -rv ~/Documents/. /media/nextcloud-documents/
```

Then configured them via:

> Nextcloud Admin → Settings → External Storage
> Set type to: Local
> Path: /media/nextcloud-data and /media/nextcloud-documents

✅ Successfully browsed these folders on both mobile and web interface.




