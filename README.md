Eltex disabled admin account for users and blocked shell access even for admin in last firmware versions

Admin and shell access can be restored by flashing old firmware

Note that flashing old images and changing working firmware may result in bricking
Be careful with read-write access to firmware

### Preventing firmware updates from ISP

If you ISP force-updating firmware, it is possible break forced update
I patched some binaries by replacing cmsImg_writeValidatedImage, cmsImg_writeValidatedImage and cmsImg_validateImage symbols by getuid, 
returning 0 as root
It must be installed before connecting PON cable, otherwise ISP will flash and reboot system

* Install tftpd or atftpd server and start it in this repo

```
atftpd --daemon /path/to/repo
```

* Disconnect PON cable

* Upload firmware with web-interface

* Telnet and admin login now availiable with login admin:password

* connect to telnetd and do following

```
sh
mount -o remount,rw none /
rm /bin/tr69c
rm /bin/omcid
tftp -g -l /bin/tr69c -r /tr69c <tftp server ip>
chmod +x /bin/tr69c
tftp -g -l /bin/omcid -r /omcid <tftp server ip>
chmod +x /bin/omcid
sync
reboot
```
 * Wait until tr69c configure connection

* If you do not want ISP to change configuration. do following
```
sh
mount -o remount,rw none /
rm /bin/tr69c
```
Re-upload pathched files if you want to update configuration again
### Using tftp (in telnet or shell)

Getting files from router:

```
tftp -p -l <path on router> -r <path in tftpd directory>
```
Uploading files to router:
```
tftp -g -l <path on router> -r <path in tftpd directory>
```
