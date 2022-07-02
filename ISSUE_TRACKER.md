# ISSUE_TRACKER
Issue with the Carputer firmware project will be documented in this file.
File:  D:\Users\bdoerr\Development\RaspberryPi\Carputer\ISSUE_TRACKER.md

    
## Issue #
### Status - [Unresolved]
### Date Reported: 
### Last Update: 
#### Details
- 
#### Resolved:
- 
#### Corrective Action
-  
#### Next Steps
- 
#### Updates
-  


 

## Issue #6
### Status - [Resolved]
### Date Reported: 15Dec2021 
### Last Update: 9Jan2022   
#### Details
- Camera-Rear node not syncing with master.  Suspect with the addition of the UPS, there is a delay with this node powering up before master has booted up. 
#### Corrective Action
- New timesync.sh.  SSH certificate not working for some reason. Another script modification is to use the 'sshpass' package and just hard code
the password. Doesn't matter, RPi's have no security. 
- 9Jan2022: FINAL RESOLUTION
    - New timesync.sh
    - Disable systemd-timesyncd
    - sudo chmod 777 timesync.sh
    - Run in 'crontab -e' as user pi
#### Next Steps
- Create v1.2.1 image and release.
#### Updates
- 24Dec2021: timesync.sh implemented.
- 29Dec2021: Periodic checks show that node is in sync with master.
- 30Dec2021: Node in sync with master.
- 31Dec2021: Node in sync with master.
- 5Jan2022: Node in sync with master. Monitor a few more times then consider this fixed. Not really a big issue since movies are timestamped with the master date since the node is stream through
the master motionEye service.
- 9Jan2022: v1.2.1 released.
- 9Jan2022: Not syncing.  systemd-private-2b64ede74618438d88f5a098c2c3d800-systemd-timesyncd.service-LLIXvd/: Permission denied
- 9Jan2022: Crap could this be my problem? The master is disabled.
	Disable systemd-timesyncd - systemd-timesyncd is a system service that may be used to synchronize the local system clock with a remote Network Time Protocol server.
		• sudo systemctl stop systemd-timesyncd.service 
		• sudo systemctl disable systemd-timesyncd.service
		• sudo systemctl status systemd-timesyncd.service 
- 10Jan2022: Node didn't sync with master after reinstalling in auto. Required a date sync via the app. Will monitor. 
- 12Jan2022: Date/time in sync.

 
## Issue #5
### Status - [Resolved]
### Date Reported: 3Dec2021 
### Last Update:  2Jul2022   
#### Details     
- Intermittent issue with movie archives not being purged after. Configuration is set to archive 3 days.
    - Finding one or more directories corrupted and not able to delete on a Windows machine. Fix is to reformat the USB drive.
    - Monitor if still an issue after implementing Juice4halt (see Carputer v1.6). THIS IS STILL AN ISSUE.  
#### Resolved:
- FIRMWARE_MASTER v1.6.1    
#### Corrective Action
- Change USB partition from NTFS to EXT4. 
    
#### Next Steps
- 
#### Updates
- 24Dec2021: Cron job implemented.
- 29Dec2021: Cron job not working. Need to verify if the is implemented under 'sudo' or 'pi' user. Maybe need to schedule this cron job rather that at reboot?
- 30Dec2021: Implemented as 'sudo crontab -e'.
- 5Jan2022: There is directory dated 1Jan2022 that should have been purged. Follow-up on 6Jan2022.
- 9Jan2022: Directories are becoming corrupt. Requires a disk format to resolve. This explains why the archive purge is not working.
- 18Jan2022: Some directories becoming corrupt. Used Windows chkdsk command to fix. Once RPi was rebooted the directories were purged. Next time try the 
    Best practices
    • Always shutdown with shutdown -h 0 or halt -h - never pull the power cable.
    • If you are using the drive only temporarily then type in sudo umount /mnt/motioneye before pulling out the USB cable - or shutdown the system first.
    • If you have a power-cut or accidental power-out then you can repair the filesystem like this:
        $ sudo umount /mnt/motioneye 
        $ sudo fsck /dev/sda
            fsck from util-linux 2.25.2
            e2fsck 1.42.12 (29-Aug-2014)
        $ sudo mount /mnt/motioneye 

- 18Jan2022: syslog only had once entry pertaining to low power. Don't believe this is the cause of the corrupted directories.
    Dec  8 06:24:32 carputer kernel: [   10.391641] Under-voltage detected! (0x00050005)
- 24Jan2022: Changed file system from ntfs to vfat. 
- 25Jan2022: Change working and appears faster when viewing with the Android app. I expected to be able to view files using Windows, but this is not the case. 
             Need to test for a week to see if cron jobs are working correctly. 
- 27Jan2022: Not sure what I did wrong but changes made on 27Jan2022 to use vfat didn't work. Files were writing to SD card (I think). I redid the USB file system to use ext4. 
             Need to test for a week to see if cron jobs are working correctly. Will use WinSCP to transfer files to Windows, if needed.
             
                pi@carputer:~ $ lsblk
                NAME        MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
                sda           8:0    1 58.4G  0 disk 
                `-sda1        8:1    1 58.4G  0 part /mnt/motioneye
                mmcblk0     179:0    0 29.7G  0 disk 
                |-mmcblk0p1 179:1    0 43.9M  0 part /boot
                `-mmcblk0p2 179:2    0 29.7G  0 part /

                pi@carputer:~ $ df -a
                Filesystem     1K-blocks    Used Available Use% Mounted on
                /dev/root       30562828 3004032  26255788  11% /
                ...
                /dev/sda1       60027788   68324  56880452   1% /mnt/motioneye

- 27Jan2022: Disabled cron jobs to test if motioneEye purges files.
- 27Jan2022: Verified able to transfer files using WinSCP. Noticed about 50% were corrupted videos. 
- 5Feb2022: Changed USB file system from ntfs to ext4. This appears to have solve the  archive deletion issue. 
            Next Step: Create a flash a new v1.6 image and apply v1.6.1 changes.  
- 27Jun2022: Validation of v1.6.1
 
 
## Issue #4
### Status - [Unresolved][Troubleshooting]
### Date Reported: 3Dec2021
### Last Update:  31Dec2021
#### Details     
- High rate of corrupt saved movies. More so with rear camera (Pi Camera). Implemented Juice4halt (see Carputer v1.6) and monitoring failure rate. Large number of failures due to improper Raspberry Pi shutdowns.
    - Need to verify if last movie saved after hardware is powered off is not corrupt.
    - 
#### Corrective Action
-  
#### Next Steps
- 
#### Updates
- 30Dec2021: Both front and rear camera has mostly corrupt movies. Doesn't make any sense why the front isn't saving. Could this be a low power issue?
- 31Dec2021: Both front and rear camera has mostly corrupt movies. WTF.  Aaaaahhh!
- 31Dec2021: Changed movie archive length from 600 to 300 seconds.
- 31Dec2021: Enabled more verbose logging.
    /etc/motion/motion.conf -> log_level 8
- 5Jan2022: Only a few videos were corrupt. Have not reviewed log yet.
- 9Jan2022: I suspect node is loosing wifi connectivity with the master which I would suspect is causing video corruption from the node, but doesn't explain corrupted videos on the master.
- 10Jan2022: Try these links
    https://forums.raspberrypi.com/viewtopic.php?t=294556 
    https://raspberrypi.stackexchange.com/questions/58522/raspberry-pi-3-losing-wi-fi-connection-frequently
    https://raspberrypi.stackexchange.com/questions/15142/wifi-drops-on-raspberry-pi
    
    TRY THIS FIRST!!
    DIDN'T FUCKING WORK!!
    https://learn.adafruit.com/adafruits-raspberry-pi-lesson-3-network-setup/test-and-configure#fixing-wifi-dropout-issues
    https://forums.adafruit.com/viewtopic.php?f=50&t=44171&p=220622#p220593
        Create and edit a new file in /etc/modprobe.d/8192cu.conf

        sudo nano /etc/modprobe.d/8192cu.conf

        and paste the following in

        # Disable power saving
        options 8192cu rtw_power_mgnt=0 rtw_enusbss=1 rtw_ips_mode=1

        Then reboot with sudo reboot
 
    ENABLE wpa_supplicant DEBUG
    https://forums.raspberrypi.com/viewtopic.php?f=36&t=234058&p=1432916#p1432916
    https://forum.pistar.uk/viewtopic.php?t=2948
    https://netbeez.net/blog/linux-wireless-engineers-read-wpa-supplicant-logs/
    wpa_supplicant -u -s -O
- 11Jan2022: It turns out I don't have that many corrupt files. The rear camera has a large amount of 'camera not available'. Other movies I thought were corrupt, those that display a traffic cone, are not corrupt but video is recorded later in the video.
- 11Jan2022: ost common causes of file system corruption are due to improper shutdown or startup procedures, hardware failures, or NFS write errors. 
    https://frameboxxindore.com/linux/quick-answer-what-causes-file-corruption-linux.html
    https://www.google.com/search?client=firefox-b-1-d&q=linux+what+would+cause+a+corrupt+directory
    https://askubuntu.com/questions/906044/recover-a-corrupt-directory
- 12Jan2022: Rear camera has majority corrupted videos and should be the focus. Front corruption is rare.

 
## Issue #3
### Status - [Resolved]
### Date Reported:  26Nov2019
### Date Resolved:  28Nov2019 
### Version Resolved:  v1.5 
### Last Update: 
#### Details 
- MotionEye - does it run on Chrome 76.0.3809.100
- <https://github.com/ccrisan/motioneye/issues/1449>
- Work around for browser but not WebClient.  
    - In your Chrome browser you can go to chrome://flags/#enable-lazy-image-loading and change the option from Default to Disable and reload Chrome.
#### Corrective Action
- Upgrade motionEye to 0.41.
    - Fixed video streams not working on Chrome 76+ (thanks @rajendrant)
#### Next Steps
- 
#### Updates
-  

 
## Issue #2
### Status - [Resolved]
### Date Reported:  13Jun2019 06:10
### Date Resolved:  21Jun2019
### Version Resolved: N/A Refer to corrective action below.
### Last Update:   21Jun2019 08:54
#### Details
- USB Stick full.  motionEye should be removing old files greater than 1 week.
- Neglected to document the date of the oldest archive folder.  The newest and last was dated 5Jun2019.
#### Corrective Action
- Ran below commands to remove data.  
    - cd /mnt/motioneye
    - sudo rm -r Front  
    - sudo rm -r RearCam  
    Note:  Some files were corrupted and not able to delete.  Used Windows to perform quick format.
#### Next Steps
- Monitor for period of time.  Review logs. 
- Could this be related to changing over to NTFS from EXT4?
- Might need to set debug flag in motionEye.
#### Updates
- 21Jun2019 08:54:  motionEye was not removing video archives after 7 days.  Set value for saving archive for video and pictures from '7 days' to '1 day' then
back to '7 days'.  Archives were deleted when value was set to '1 day'.  Check again in 7 days to verify archive files are being removed.  Issue appears to 
be related to changing the USB disk format from ext4 to ntfs.
- 30Jun2019 15:08:  motionEye not removing /mnt/motioneye files after 7 days.  RPi froze and couldn't access motionEye admin 
which required me to reimage from v1.3.  Changed movie storage setting of 7 days to 'custom 3 days'.  Purged /mnt/motioneye before 
rebooting.  Monitor and and review.
- 4Jul2019:  Still monitoring. 

 
## Issue #1
### Status - [Resolved]
### Date Resolved:  21Jun2019
### Version Resolved: N/A Refer to corrective action below.
### Date Reported:  13Jun2019 06:10
### Last Update:   21Jun2019 08:54
#### Details
- RTC date/time off.  Noticed on 13Jun2019 06:10 the date was reporting 12Jun2019 18:xx (did not capture exact time).
#### Corrective Action
- Reset time by running the below commands.  
    - sudo date -s "Thu Jun 13 06:23:30 PDT 2019"
    - sudo hwclock -w  
    - sudo hwclock -r  
#### Next Steps
- Monitor for period of time.  Review logs.   
#### Updates
- 21Jun2019 08:54:  RTC reporting correct date/time.
- 30Jun2019 15:08:  RTC reporting correct date/time.
- 4Jul2019 07:06:   RTC reporting correct date/time.



