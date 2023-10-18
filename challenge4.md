## Some of our apps generate some raw data and store the same in /home/bob/preserved directory. We want to clean and manipulate some data and then want to create an archive of that data.
![Screenshot from 2023-10-18 06-52-07](https://github.com/Althaf-official/Linux_Challenges_Kodekloud/assets/105126131/d13502cf-b9e4-4420-a9a0-4b835945529f)

```
1. At first  switch to root user and list the /opt folders

[bob@centos-host ~]$ sudo su

[root@centos-host ~]# ls -l /opt/
total 0


2. Make directories hidden & files under /opt/appdata


[root@centos-host ~]# mkdir -p /opt/appdata/hidden
[root@centos-host ~]# mkdir -p /opt/appdata/files


[root@centos-host ~]# ls -l /opt/appdata/
files/  hidden/

[root@centos-host ~]# ls -l /opt/appdata/
total 8
drwxr-xr-x 2 root root 4096 Nov 13 15:10 files
drwxr-xr-x 2 root root 4096 Nov 13 15:09 hidden



3. Find the"non-hidden" files in "/home/bob/preserved" directory and copy them in "/opt/appdata/files/" directory
   Find the "hidden" files in "/home/bob/preserved" directory and copy them in "/opt/appdata/hidden/" directory


[root@centos-host ~]# find /home/bob/preserved -type f -not -name ".*" -exec cp "{}" /opt/appdata/files/ \;

[root@centos-host ~]# find /home/bob/preserved -type f -name ".*" -exec cp "{}" /opt/appdata/hidden/ \;



4. Find and delete the files in "/opt/appdata" directory that contain a word ending with the letter "t" (case sensitive) 


[root@centos-host ~]# rm -f $(find /opt/appdata/ -type f -exec grep -l 't\>' "{}"  \; )

[root@centos-host ~]#

5. Change all the occurrences of the word "yes" to "no" in all files present under "/opt/appdata/" directory.

Change all the occurrences of the word "raw" to "processed" in all files present under "/opt/appdata/" directory. It must be a "case-insensitive" replacement, means all words must be replaced like "raw , Raw , RAW" etc

[root@centos-host ~]# find /opt/appdata -type f -name "*" -exec sed -i 's/\byes\b/no/g' "{}" \;

[root@centos-host ~]#

[root@centos-host ~]# find /opt/appdata -type f -name "*" -exec sed -i 's/\braw\b/processed/ig' "{}" \;

[root@centos-host ~]#

6. Create a "tar.gz" archive of "/opt/appdata" directory and save the archive to this file: "/opt/appdata.tar.gz"


[root@centos-host ~]# cd /opt

[root@centos-host opt]# tar -zcf appdata.tar.gz appdata

[root@centos-host opt]#

[root@centos-host opt]# ls

appdata  appdata.tar.gz

7. Add the "sticky bit" special permission on "/opt/appdata" directory (keep the other permissions as it is).

·       Make "bob" the "user" and the "group" owner of "/opt/appdata.tar.gz" file.
 The "user/group" owner should have "read only" permissions on "/opt/appdata.tar.gz" file and "others" should not have any permissions.


[root@centos-host opt]# chmod +t /opt/appdata

[root@centos-host opt]# ls -lsd /opt/appdata

4 drwxr-xr-t 4 root root 4096 Nov 13 15:10 /opt/appdata

[root@centos-host opt]# chown bob:bob /opt/appdata.tar.gz

[root@centos-host opt]#

[root@centos-host opt]# ls -lsd /opt/appdata

4 drwxr-xr-t 4 root root 4096 Nov 13 15:10 /opt/appdata

[root@centos-host opt]# chmod 440 /opt/appdata.tar.gz

[root@centos-host opt]#

8. Create a "softlink" called "/home/bob/appdata.tar.gz" of "/opt/appdata.tar.gz" file.



[root@centos-host opt]# ln -s /opt/appdata.tar.gz /home/bob/appdata.tar.gz

[root@centos-host opt]#

9. Create a script called "/home/bob/filter.sh".


[root@centos-host opt]# vi /home/bob/filter.sh

[root@centos-host opt]# cat /home/bob/filter.sh

#!/bin/bash

tar -xzOf /opt/appdata.tar.gz | grep processed > /home/bob/filtered.txt

[root@centos-host opt]#

[root@centos-host opt]# chmod +x /home/bob/filter.sh

[root@centos-host opt]#

[root@centos-host opt]# ls -l /home/bob/

.bash_logout    .bash_profile   .bashrc         appdata.tar.gz  filter.sh       preserved/     

[root@centos-host opt]#

10. Execute the scrtip and filter the My processed data in filter.txt file




[root@centos-host opt]# /home/bob/filter.sh

[root@centos-host opt]#

[root@centos-host opt]# ls -l /home/bob/

.bash_logout    .bash_profile   .bashrc         appdata.tar.gz  filter.sh       filtered.txt    preserved/     

[root@centos-host opt]#

[root@centos-host opt]# cat /home/bob/filtered.txt

My processed data

My processed data

My processed data

My processed data

[root@centos-host opt]#



```
