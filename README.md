# Pi Firmware Setup
#### Raspberry Pi 4 Firmware + 1 Wire Temp Sensor Modules + Touchscreen UI
![GitHub](https://img.shields.io/github/license/marlinfirmware/marlin.svg)
## SSH Acces
***Clear SSH keys to access via SSH***
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

```
systemd-analyze blame
```

#### **[These may not work!]**

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

## Temp Sensor [^4]
#### ***DS18B20 Sensor Being Replaced With MCP9808[^6]***

[^6]: [MCP9808](https://github.com/fivdi/mcp9808-temperature-sensor)

```
git clone git@github.com:thisdavej/ds18b20-raspi.git
```
```
npm install --save ds18b20-raspi
```
```
$ ds18b20 [deviceId] [options]

Options
  --all, -a       Get readings of all temperature sensors found
  --list, -l      List device ids of all 1-Wire sensors found
  --degf, -f      Get temperature in degF instead of degC
  --decimals, -d  Number of decimal digits to display
  --help, -h      Show help
  --version, -v   Display version information

Examples
  Get temperature of sensor (only works if there is exactly one DS18B20 1-Wire sensor present)
  $ ds18b20

  Get temperature readings of all 1-Wire sensors found
  $ ds18b20 -a

  Get temperature of a specific 1-Wire device id
  $ ds18b20 28-051724b238ff

  Get temperature of a specific 1-Wire device id in degF with 2 decimals
  $ ds18b20 28-051724b238ff -f -d 2

  List device ids of all 1-Wire sensors found
  $ ds18b20 --list
```

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
[^4]: [DS18B20](https://github.com/thisdavej/ds18b20-raspi) 

## OctoPrint UI [^5]
```
bash <(wget -qO- https://github.com/UnchartedBull/OctoDash/raw/main/scripts/install.sh)
```

**[Enable needed plugins + CORS]**

[^5]: [OctoDash](https://github.com/UnchartedBull/OctoDash) 

## OctoPrint Plugins
- ABL Expert Plugin
- Arc-Welder
- BLTouch Plugin
- Consolidate Temp Control
- Consolidated Tabs
- DisplayLayerProgress
- Enclosure Plugin [^10]
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
