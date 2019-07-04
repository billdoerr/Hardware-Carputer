# CHANGELOG_FIRMWARE_MASTER_NODE
All notable changes pertaining to the operating system of the hardware nodes used in the Carputer project will be documented in this file.
File:  D:\Users\bdoerr\Development\RaspberryPi\Carputer\CHANGELOG_FIRMWARE_MASTER_NODE.md


## [Unreleased]
### v1.x  (NOT STARTED)
- [ ]  Determine power usage with RaspberryPi + USBStick + two USB Cameras + router.
- [ ]  Setup RaspberryPi as Router.  https://www.instructables.com/id/Use-Raspberry-Pi-3-As-Router/
  - [ ]  IP Bridge.  **NOTE:**  Once enabling the bridge it no longer provices an IP address for a client connection.
##  Feature Creep - not assigned to any release
- [ ]  RaspberryPi:  Create REST API to obtain files (images, videos, data, etc).  Example using Python -> https://codeburst.io/this-is-how-easy-it-is-to-create-a-rest-api-8a25122ab1f3.
- [ ]
- [ ]

## [Unreleased]
### v1.X (NOT STARTED)
#### Added
- [ ] USB Power bank  
    http://raspi-ups.appspot.com/en/index.jsp  
    https://domoticproject.com/protect-raspberry-pi-ups-power-failure/  
#### Changed
- [ ] New Features:  listen_for_shutdown.py  
        - [ ] Press the momentary button and hold for 1 to 2 seconds to reboot  
        - [ ] Press the momentary button and hold for 3 to 7 seconds to implement safe shutdown  
        - [ ] Press the momentary button and hold for >8 seconds to force shutdown  
#### Removed

## [Unreleased]
### v1.X (NOT STARTED)
#### Added
#### Changed
- [ ] Rename USB memory stick label from MOTIONEYE to DATA.
        - cd /mnt
        - sudo mkdir data
        - sudo mv motioneye ./data/.
        - sudo vi /etc/fstab
        - LABEL=DATA /mnt/data ntfs    defaultsj,nofail,noatime  0    1
    - [ ] Change motionEye camera configuration file storage setting from /mnt/motioneye to /mnt/data/motioneye.

## [Unreleased]
### v1.X (NOT STARTED)
#### Added
#### Changed
- [ ] Rename USB memory stick label from MOTIONEYE to DATA.
        - cd /mnt
        - sudo mkdir data
        - sudo mv motioneye ./data/.
        - sudo vi /etc/fstab
        - LABEL=DATA /mnt/data ntfs    defaultsj,nofail,noatime  0    1
    - [ ] Change motionEye camera configuration file storage setting from /mnt/motioneye to /mnt/data/motioneye.    

## [Released]
### v1.4 (4Jul2019)
#### Added
#### Changed
- [x] Change to listen_for_shutdown.py.  Refer to D:\Users\bdoerr\Development\Python\PycharmProjects\carputer\CHANGELOG.md for details.
- [x] Changed motionEye file storage from 7 days to 3 days.  Troubleshooting issue where /mnt/motioneye archives not being purged.
- [x] Disabled motionEye Still Images.
- [x] Update firmware version file
sudo vi /etc/carputer/version
Master Node
v1.4
Released 4Jul2019
    
## [Released]
### v1.3 (17May2019)
#### Added
- [x]  Add power off switch and LED (Yellow) indicator.
    https://github.com/Howchoo/pi-power-button  
	https://github.com/TonyLHansen/raspberry-pi-safe-off-switch/
    https://www.cyberciti.biz/faq/remote-shutdown-linux-computer-from-the-cli/  
    
        sudo apt-get install python3-paramiko
        cd /etc/init.d
        sudo vi listen_for_shutdown.sh
        sudo chmod +x listen_for_shutdown.sh
        sudo mkdir /var/log/carputer
        cd /usr/local/bin
        sudo vi listen_for_shutdown.py
        sudo chmod +x listen_for_shutdown.py
        sudo update-rc.d listen_for_shutdown.sh defaults
        sudo reboot
        cd /var/log/carputer/
        ls
        tail -f listen_for_shutdown.log
        
        sudo update-rc.d listen_for_shutdown.sh defaults
        
        Wiring:  Power-Off Button
        BUTTON connects to board Pin 37 GPIO26
        BUTTON connects to board Pin 39 GND  
        
        Wiring:  Power-Off Button LED (Yellow) Indicator
        LED (Yellow) -> RESISTOR (330 ohm) connects to board Pin 33 GPIO13
        LED (Yellow) connects to board Pin 34 GND  
        
        
- [x] Add LED (Red) to indicate power-on.  Will blink on/off once per second.

        cd /etc/init.d
        sudo vi heartbeat.sh
        sudo chmod +x heartbeat.sh
        sudo mkdir /var/log/carputer
        cd /usr/local/bin
        sudo vi heartbeat.py
        sudo chmod +x heartbeat.py
        sudo update-rc.d heartbeat.sh defaults
        sudo reboot
        cd /var/log/carputer/
        ls
        tail -f heartbeat.log
        
        sudo update-rc.d heartbeat.sh defaults  

        Wiring:  Power-On LED (Red) Indicator
        LED (Red) -> RESISTOR (330 ohm) connects to board Pin 13 GPIO27
        LED (Red) connects to board Pin 14 GND 
        
#### Changed
- [x] Change filesystem on USB stick from ext4 to ntfs so that videos and be easily viewed from Windows OS.  
    https://pimylifeup.com/raspberry-pi-ntfs/  
#### Removed

## [Released]
### v1.2 (24Apr2019)
#### Added
- [x]  Auto sync dates of nodes that dont' have RTC with that of the master.  Refer to FIRMWARE_SLAVE_NODE.md v1.1 for details where an SSH cert was deployed to master.
#### Changed
- [x] Update version
sudo vi /etc/carputer/version
Master Node
v1.2
Released 24Apr2019
#### Removed

## [Released]
### v1.1 (24Apr2019)
#### Added
- [x]  Auto sync dates of nodes that dont' have RTC with that of the master.  

cd /bin
sudo vi carputer
#! /bin/sh
cat /etc/carputer/version

sudo chmod +x carputer


cd /etc
sudo mkdir carputer
cd carputer
sudo vi version
v1.1
Released 20Apr2019

- [x]  Add Real Time Clock (RTC)  
    https://pimylifeup.com/raspberry-pi-rtc/  
    https://learn.adafruit.com/adding-a-real-time-clock-to-raspberry-pi/overview  
    https://www.sparkfun.com/products/12708  
#### Changed
#### Removed


## [Released]
### v1.0 (28Feb2019)
- [x]  Install Raspbian.
- [x]  Enable SSH.
- [x]  Enable VNC.
- [x]  Install MotionEye.
- [x]  Setup RaspberryPi as Router.  https://www.instructables.com/id/Use-Raspberry-Pi-3-As-Router/
  - [x]  Access point.
- [x]  **Successfully deployed RaspberryPi as access point.**  Test and validate using RaspberryPi as router.  If this does not work out as planned then plan B would be to buy small router.
- [x]  **(ON ORDER)**  USBStick (16/32GB) for image archive.
- [x]  **Use Pi Zero as rear camera.**
    - [x] Install motioneye
	- [x] Configure main RaspberryPi motioneye Rear camera with streaming URL from Pi Zero.
	- [x] **(PI Camera cable on order)** Use PiCam rather than USB camera.  Validated USB camera works.  
- [x]  Create final Raspbian image for Carputer-Rear.
	- [x]  Need to configure motioneye to use PiCamera rather than USB camera.
- [x]  Create final Raspbian image for Carputer.
    - [x]  Need to redo mounting of usb flash drive.
	- [x]  Need to configure motioneye to use PiCamera from Carputer-Rear rather than the USB camera from Carputer-Rear.  
	



  
