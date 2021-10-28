# Pi Firmware Setup
#### Raspberry Pi 4 Firmware + 1 Wire Temp Sensor Modules + Touchscreen UI
<img src="https://img.shields.io/badge/-Contursi-blue" alt="GitHub release"/> <img src="https://awesome.re/badge.svg"> <img src="https://img.shields.io/github/license/cwcontur/Contursi-Raspi-OctoPrint-Firmware"> <img src="https://img.shields.io/github/v/release/cwcontur/Contursi-Raspi-OctoPrint-Firmware?color=yellow&include_prereleases"> <img src="https://img.shields.io/npm/v/npm?color=red"> <img src="https://img.shields.io/github/commit-activity/m/cwcontur/Contursi-Raspi-OctoPrint-Firmware?color=Green">

## SSH Access
***Clear SSH keys to access via SSH after fresh install***
```
ssh-keygen -R printer
```
## Pi Settings
```
sudo raspi-config
```
- Hostname -> Printer
- Password -> ******
- Wireless Connection
- Autologin
- 1-Wire Support -> Enable
- SSH -> Enable
- Camera -> Enable

```
sudo reboot
```

## Updates + NPM [^1]
```
sudo apt-get update
```
```
sudo apt update --allow-releaseinfo-change
```
```
sudo apt full-upgrade
```
```
sudo apt-get install nodejs npm
``` 
```
sudo shutdown - r now
```

[^1]:[NPM](https://www.npmjs.com/package/raspberry).

## Faster Boot [^2]
```
sudo nano /boot/config.txt
```
- `disable_splash=1`
- `dtoverlay=disable-bt`
- `boot_delay=0`

***Cooling Fan Needed for Overclocking!***
- `over_voltage=2`
- `arm_freq=2000`
- `gpu_freq=700`

```
sudo reboot
```

```
sudo nano /boot/cmdline.txt
```
- Delete 'splash' parameter
- Add 'quiet' parameter


- Change console=tty -> console=serial0
  - This will get rid of terminal text by redirecting it to serial [^11]
  [^11]: [Hide all Boot Processes](https://www.raspberrypi.org/forums/viewtopic.php?t=270219)
```
systemd-analyze blame
```

#### **[These may not work; one or more of these breaks SSH!]**

```
sudo systemctl disable dhcpcd.service
```
```
sudo systemctl disable networking.service
```
```
sudo systemctl disable ntp.service
```
```
sudo systemctl disable dphys-swapfile.service
```
```
sudo systemctl disable keyboard-setup.service
```
```
sudo systemctl disable apt-daily.service
```
```
sudo systemctl disable wifi-country.service
```
```
sudo systemctl disable hciuart.service
```
```
sudo systemctl disable raspi-config.service
```
```
sudo systemctl disable avahi-daemon.service
```
```
sudo systemctl disable triggerhappy.service
```

```
sudo reboot
```

[^2]: [Pi Fast Boot](https://singleboardbytes.com/637/how-to-fast-boot-raspberry-pi.htm) 

## GitHub SSH [^3]
```
ssh-keygen -t ed25519 -C "cwcontur@uncg.edu"
```
```
eval "$(ssh-agent -s)"
```
```
ssh-add ~/.ssh/id_ed25519
```
```
cat ~/.ssh/id_ed25519.pub
```
- Then select and copy the contents of the id_ed25519.pub file
displayed in the terminal to your clipboard
- Go here to add key -> [SSH Keys](https://github.com/settings/keys)

[^3]: [Github SSH Setup](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent) 

## Temp Sensor [^6]
#### ***DS18B20 Sensor Replaced With MCP9808[^6]***

[^6]: [MCP9808](https://github.com/fivdi/mcp9808-temperature-sensor)

#### MCP9808 [^7] [^8]

```
sudo apt-get install build-essential python-dev python-pip python-smbus git
```

```
sudo pip install RPi.GPIO
```
```
git clone git@github.com:cwcontur/Adafruit_Python_MCP9808.git
```
```
cd Adafruit_Python_MCP9808
```
```
sudo python setup.py install
```

[^7]: [MCP9808 Wiring](https://learn.adafruit.com/mcp9808-temperature-sensor-python-library/hardware)
[^8]: [MCP9808 Software](https://learn.adafruit.com/mcp9808-temperature-sensor-python-library/software)

## OctoPrint UI [^5]
```
bash <(wget -qO- https://github.com/UnchartedBull/OctoDash/raw/main/scripts/install.sh)
```

**[Enable needed plugins + CORS]**

**If stuck on initializing screen, use these to delete config.json using SSH**
```
mv ~/.config/octodash/config.json ~/.config/octodash/config.json.bak
```
```
sudo service getty@tty1 restart
```

[^5]: [OctoDash](https://github.com/UnchartedBull/OctoDash) 

## OctoPrint Plugins
- ABL Expert Plugin
- Arc-Welder
- BLTouch Plugin
- Consolidate Temp Control
- Consolidated Tabs
- DisplayLayerProgress
- Enclosure Plugin [^10]
  - Currently this is broken! Forces Marlin disconnect!
- Filament Manager
- Firmware Check
- GPIO Control
- OctoDash Companion
- OctoRelay
- Octolapse
- Plotly Temp Graph
- Preheat Button
- PrintTimeGenius Plugin
- Printer Dialogs
- Printer Notifications
- Slicer Thumbnails
- Spool Manager
- Tab Order
- Themeify
- Virtual Printer

[^10]: [Enclosure Plugin w/ Custom Graph](https://github.com/Dak0r/OctoPrint-Enclosure-with-Custom-Graphs)
