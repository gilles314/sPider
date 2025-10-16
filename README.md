# sPider - Wireless Security Testing Platform

[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)

## Project Overview

sPider is a portable wireless penetration testing platform built on Kali Linux and Raspberry Pi. This streamlined setup provides the essential tools for authorized wireless security assessment using airgeddon and industry-standard wireless adapters. Please take a look at [the roadmap](https://github.com/users/gilles314/projects/2/views/1) for future updates.

**WARNING: This documentation contains procedures that may be illegal if performed without explicit authorization. Only use on networks you own or have written permission to test.**

---

## Table of Contents

- [Required Hardware](#required-hardware)
- [Kali Installation](#kali-installation)
- [First Boot and SSH Connection](#first-boot-and-ssh-connection)
- [Install AWUS036ACS Driver](#install-awus036acs-driver)
- [Install Dependencies](#install-dependencies)
- [Configure Airgeddon](#configure-airgeddon)
- [Usage](#usage)
- [Legal Disclaimer](#legal-disclaimer)

---

## Required Hardware

- **Raspberry Pi 3 or newer**
  - Raspberry Pi 5 recommended for onboard 5GHz support
  - Official Raspberry Pi power supply

- **MicroSD Card**
  - Minimum 32GB recommended
  - Class 10 or better
  - USB adapter (if your computer doesn't have a built-in SD card reader)

- **AWUS036ACS Wireless Adapter**
  - Supports monitor mode

- **Computer for Setup**
  - Windows, Linux, or macOS
  - For running Raspberry Pi Imager and SSH client
  - Connected to the same wireless network as the Raspberry Pi

---

## Kali Installation

1. Download Raspberry Pi Imager
   - Visit the official Raspberry Pi website and download the imager for your operating system:
     - [Raspberry Pi Imager Download](https://www.raspberrypi.com/software/)

2. Flash Kali Linux to MicroSD Card
   - Connect the MicroSD card to your PC
   - Open Raspberry Pi Imager
   - Select Device:
     - Choose your Raspberry Pi model (Pi 3, Pi 4, or Pi 5)
   - Select Operating System:
     - Click "Choose OS"
     - Scroll down to "Other specific-purpose OS"
     - Select "Kali Linux"
     - Choose the appropriate Kali Linux ARM image for your Pi model
   - Select Storage:
     - Click "Choose Storage"
     - Select your microSD card
   - Configure Custom Settings:
     - Click Next
     - When prompted, "Would you like to apply OS customization settings?" Click "Edit Settings"
     - General Tab:
       - Set hostname: `spider`
       - Set username: `kali`
       - Set a strong password
         - Note somewhere secure or remember
       - Configure wireless LAN:
         - SSID: (your wireless network name)
         - Password: (your wireless network password)
         - Wireless LAN country: (your country code, e.g., US, UK, etc.)
     - Services Tab:
       - Enable SSH
       - Use password authentication
   - Write to MicroSD Card:
     - Click "Next"
     - Confirm you want to erase all data on the microSD card
     - Click "Yes"
     - Wait for the write and verification process to complete (this may take several minutes)
   - Remove MicroSD Card:
     - Safely eject the microSD card from your computer

3. Boot Raspberry Pi
   - Insert the MicroSD card into the Raspberry Pi
   - Connect AWUS036ACS adapter to Pi USB port
   - Connect the power supply to the Raspberry Pi
     - The Pi will boot automatically
     - Wait 2-3 minutes for the first boot to complete
     - The Pi will automatically connect to your wireless network

---

## First Boot and SSH Connection
- From your computer, connect to the Pi via SSH:
  - Linux/macOS:
    ```bash
    ssh kali@spider.local
    ```
  - Windows (PowerShell or CMD):
    ```powershell
    ssh kali@spider.local
    ```
  - Windows (Using PuTTY):
    - Download [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)
    - Enter hostname: `spider.local`
    - Port: `22`
    - Connection type: SSH
    - Click "Open"
- Enter your password when prompted.

---

## Install AWUS036ACS Driver

1. Install driver:
   ```bash
   sudo apt install -y realtek-rtl88xxau-dkms
   ```

2. Reboot to load the driver:
   ```bash
   sudo reboot
   ```
   - Terminate your SSH connection and reconnect after a few minutes.

3. After reboot, verify adapters are recognized:
   ```bash
   lsmod | grep 88XXau
   iwconfig
   ```
   - You should see the `88XXau` module loaded and your wireless adapters listed with "WIFI@RTL8821AU" nickname.

---

## Install Dependencies

```bash
sudo apt update
sudo apt install -y \
gawk aircrack-ng tmux iproute2 pciutils procps \
reaver john crunch dnsmasq ettercap-graphical tcpdump tshark nftables pixiewps hashcat hostapd openssl curl \
bettercap hostapd-wpe isc-dhcp-server asleap mdk4 hcxdumptool hcxtools beef-xss lighttpd
```
 - This may take a few minutes.
 
---

## Configure Airgeddon
```bash
cd ~ && git clone --depth 1 https://github.com/v1s1t0r1sh3r3/airgeddon.git
```
```bash
nano ~/airgeddon/.airgeddonrc
```
 - Edit the last line 'AIRGEDDON_WINDOWS_HANDLING=xterm' to 'AIRGEDDON_WINDOWS_HANDLING=tmux'
 - Press "Ctrl + O", press "Enter", then press "Ctrl + X"
---

## Usage

```bash
sudo bash ~/airgeddon/airgeddon.sh
```
> Select your wireless interface
   - Choose your AWUS036ACS adapter from the menu (look for "Realtek 8812AU/8821AU" chipset)
   - Press "Ctrl + C" to exit airgeddon

> Follow airgeddon menu options
- [Airgeddon Documentation](https://github.com/v1s1t0r1sh3r3/airgeddon/wiki)
- [Airgeddon Video Tutorials](https://www.youtube.com/results?search_query=airgeddon+tutorial)

---

## Legal Disclaimer

This documentation is for authorized penetration testing and educational purposes only. Always ensure you have explicit written permission before testing any network. Wireless network testing and penetration activities described herein are **strictly illegal without explicit written authorization** from network owners.

**You are solely responsible for ensuring:**
- You have proper legal authorization before testing any wireless network
- You comply with all applicable local, state, national, and international laws
- You only use these tools and techniques on networks you own or have permission to test
- You follow responsible disclosure practices for any vulnerabilities discovered

**Unauthorized access to computer networks is a federal crime** in most jurisdictions. The authors and contributors of this documentation assume no liability for misuse of this information. By using this guide, you acknowledge that you understand and accept full legal responsibility for your actions.


