# CHANGELOG_FIRMWARE_SLAVE_NODE
All notable changes pertaining to the operating system of the hardware nodes used in the Carputer project will be documented in this file.
File:  D:\Users\bdoerr\Development\RaspberryPi\Carputer\CHANGELOG_FIRMWARE_SLAVE_NODE.md


## [Unreleased]
### v1.X (TBD) 
#### Added
- [] 
#### Changed
- []  
#### Removed


## [Released]
### v1.2.1 (9Jan2022) 
#### Added 
-[x] sudo apt-get install sshpass
#### Changed  
- [x]  Somehow v1.2 lost configuration setting where motioneEye->Video Device->Frame Rate->30.  v1.2 has this set to zero.
- [x]  Add new bash script timesync.sh
    - [x] Install document updated
    - [x] Bash located within Python scripts, ensure gets added to git
    - [x] Disable systemd-timesyncd
    - [x] sudo chmod 777 timesync.sh
    - [x] Run in 'crontab -e' as user pi 
- [x] Update version
    cd /etc/carputer
    sudo vi version
    Carputer-Camera-Rear
    v1.2.1
    Released 9Jan2022  
- [x] Minor changes to install documentation 
    - [x] File archive locations
    - [x] Verify install documentation updated with current configuration steps
#### Removed


## [Released]
### v1.2 (28Nov2019)
#### Added
#### Changed
- [x] Upgrade motioneEye to 0.41.
    - Corrects Issue #3 - MotionEye - does it run on Chrome 76.0.3809.100.
        - Ensure date/time is correct
        - sudo pip install --upgrade --no-cache-dir motioneye==0.41
        - sudo vi motioneye-41.sh
            for file in /etc/motioneye/thread-*.conf; 
            do /usr/local/lib/python2.7/dist-packages/motioneye/scripts/migrateconf.sh $file; 
            done
            for file in /etc/motioneye/motion.conf; 
            do /usr/local/lib/python2.7/dist-packages/motioneye/scripts/migrateconf.sh $file; 
            done
        - sudo chmod +x motioneye-41.sh
        - sudo ./motioneye-41.sh        
        - sudo systemctl restart motioneye
- [x] Update version
    cd /etc/carputer
    sudo vi version
    Slave Node
    v1.2
    Released 28Nov2019      
#### Removed


## [Released]
### v1.1 (24Apr2019)
#### Added
- [x]  Auto sync dates of nodes that dont' have RTC with that of the master.  
        Use SSH command to retrieve date from master.  
        https://superuser.com/questions/685471/how-can-i-run-a-command-after-boot  
        
-------------------------------------------
1.  PERFORM THE FOLLOWING ON THE SLAVE NODE
-------------------------------------------
        
crontab -e
@reboot /bin/timesync.sh

sudo vi /bin/timesync.sh
#! /bin/sh

# Need delay
sleep 20

# Get date from master node and set date
sudo date -s "`ssh pi@192.168.4.1 'date'`" >> /tmp/timesync.log  

sudo chmod 755  /bin/timesync.sh

-------------------------------------------
2. PERFORM THE FOLLOWING ON THE SLAVE NODE
-------------------------------------------

ssh-keygen -t rsa -b 2048
ssh-copy-id -i ~/.ssh/id_rsa pi@192.168.4.1

- [x]  Version OS images independent of Android app versions

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
Released 24Apr2019

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
	



  
