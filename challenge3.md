# Some new developers have joined our team, so we need to create some users/groups and further need to setup some permissions and access rights for them.
![Screenshot from 2023-10-18 06-22-59](https://github.com/Althaf-official/Linux_Challenges_Kodekloud/assets/105126131/2ebfd642-4c24-4380-aa84-bbab96a2e04a)


```ruby
1. At first  switch to root user and create group devs & admins

[bob@centos-host ~]$ sudo su -

[root@centos-host ~]# groupadd devs
[root@centos-host ~]# groupadd admins

2.create users ray & lisa  with sh login shell
 
[root@centos-host ~]#  useradd -s /bin/sh ray

[root@centos-host ~]#  useradd -s /bin/sh lisa



3.Make user  ray & lisa a member of  "devs" group

[root@centos-host ~]# usermod -G devs ray

[root@centos-host ~]# usermod -G devs lisa







4.Set password for user called ray "D3vU3r321"
  Set password for user called lisa "D3vUd3r123" 

[root@centos-host ~]# passwd ray
Changing password for user ray.
New password: 
Retype new password: 
passwd: all authentication tokens updated successfully.
[root@centos-host ~]# passwd lisa
Changing password for user lisa.
New password: 
Retype new password: 
passwd: all authentication tokens updated successfully.





5.Create users david & natasha with zsh login shell & set password

[root@centos-host ~]# useradd -s /bin/zsh david
[root@centos-host ~]# useradd -s /bin/zsh natasha

[root@centos-host ~]# passwd david
Changing password for user david.
New password: 
Retype new password: 
passwd: all authentication tokens updated successfully.

[root@centos-host ~]# passwd natasha
Changing password for user natasha.
New password: 
Retype new password: 
passwd: all authentication tokens updated successfully.






6. Make user  david  & natasha a member of  "admins" group

[root@centos-host ~]# usermod -G admins david
[root@centos-host ~]# usermod -G admins natasha





7. Make sure "/data" directory is owned by user "bob" and group "devs" and
"user/group" owner has "full" permissions but "other" should not have any permissions.

[root@centos-host ~]# ls -lsd /data
0 drwxr-xr-x. 2 root root 6 Oct 18 01:56 /data

[root@centos-host ~]# chown bob:devs /data

[root@centos-host ~]# chmod 770 /data

[root@centos-host ~]# ls -lsd /data
0 drwxrwx---. 2 bob devs 6 Oct 18 01:56 /data







8. Give some additional permissions to "admins" group on "/data" directory so that any user
who is the member the "admins" group has "full permissions" on this directory.

[root@centos-host ~]# getfacl /data
getfacl: Removing leading '/' from absolute path names
# file: data
# owner: bob
# group: devs
user::rwx
group::rwx
other::---



[root@centos-host ~]# setfacl -m g:admins:rwx /data



[root@centos-host ~]# getfacl /data
getfacl: Removing leading '/' from absolute path names
# file: data
# owner: bob
# group: devs
user::rwx
group::rwx
group:admins:rwx
mask::rwx
other::---






9.Make sure all users under "devs" group can only run the "dnf" command with "sudo" and without
 entering any password.


[root@centos-host ~]# visudo
[root@centos-host ~]# cat /etc/sudoers |grep admins
%admins ALL=(ALL) NOPASSWD:ALL
[root@centos-host ~]# cat /etc/sudoers |grep dev
[root@centos-host ~]# visudo
[root@centos-host ~]# cat /etc/sudoers |grep dev
%devs ALL=(ALL) NOPASSWD:/usr/bin/dnf






10. Edit the disk quota for the group called "devs". Limit the amount of storage space it can use
(not inodes).Set a "soft" limit of "100MB" and a "hard" limit of "500MB" on "/data" partition.

[root@centos-host ~]# setquota -g devs 100M 500M 0 0 /dev/vdb1

[root@centos-host ~]# quota -g -s  devs /data
Disk quotas for group devs (gid 1003): 
     Filesystem   space   quota   limit   grace   files   quota   limit   grace
      /dev/vdb1      0K    100M    500M               1       0       0        
quota: group /data does not exist.


11. Configure a "resource limit" for the "devs" group so that this group (members of the group) can not run more than "30 processes" in their session.
This should be both a "hard limit" and a "soft limit", written in a single line.

[root@centos-host ~]# vi /etc/security/limits.conf

[root@centos-host ~]# cat /etc/security/limits.conf |grep dev
@devs            -       nproc           30


```
