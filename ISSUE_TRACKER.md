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



