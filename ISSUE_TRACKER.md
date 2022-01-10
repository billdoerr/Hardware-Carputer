# ISSUE_TRACKER
Issue with the Carputer firmware project will be documented in this file.
File:  D:\Users\bdoerr\Development\RaspberryPi\Carputer\ISSUE_TRACKER.md

    
## Issue #
### Status - [Unresolved]
### Date Reported: 
### Last Update: 
#### Details
- 
#### Corrective Action
-  
#### Next Steps
- 
#### Updates
-  


 

## Issue #6
<span style="color:red">### Status - [Resolved]</span>
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
 
## Issue #5
### Status - [Unresolved][Testing]
### Date Reported: 3Dec2021
### Last Update:  29Dec2021  
#### Details    
- Intermittent issue with movie archives not being purged after. Configuration is set to archive 3 days.
    - Finding one or more directories corrupted and not able to delete on a Windows machine. Fix is to reformat the USB drive.
    - Monitor if still an issue after implementing Juice4halt (see Carputer v1.6). THIS IS STILL AN ISSUE.    
#### Corrective Action
-  Add cron job to delete directories older than 4 days

    Edit crontab file
    sudo crontab -e
    
    Add the following
    # Delete motionEye movie archives greater than 4 days
    @reboot find /mnt/motioneye/Front/* -mtime -4 -type d -exec sudo rm -rf {} \;
    @reboot find /mnt/motioneye/Rear-PiCam/* -mtime -4 -type d -exec sudo rm -rf {} \;
    
    Validate crontab file
    sudo crontab -l
    
    
-  Possible the .keep file is preventing the file cleanup to occur. Reference this link: https://github.com/ccrisan/motioneye/issues/1810.
    - DID NOT WORK.  REMOVED CHANGES.
        - BELOW IMPLEMENT 16Dec2021
        - In file: /usr/local/lib/python2.7/dist-packages/motioneye/mediafiles.py just comment all strings with word .keep and comment one associated string: if os.path.exists(target_dir)
    
#### Next Steps
- 
#### Updates
- 24Dec2021: Cron job implemented.
- 29Dec2021: Cron job not working. Need to verify if the is implemented under 'sudo' or 'pi' user. Maybe need to schedule this cron job rather that at reboot?
- 30Dec2021: Implemented as 'sudo crontab -e'.
- 5Jan2022: There is directory dated 1Jan2022 that should have been purged. Follow-up on 6Jan2022.
 
 
## Issue #4
### Status - [Unresolved]
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



