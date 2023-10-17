

The database server called centos-host is running short on space! You have been asked to add an LVM volume for the Database team using some of the existing disks on this server.

![Screenshot from 2023-10-17 22-54-48](https://github.com/Althaf-official/Linux_Challenges_Kodekloud/assets/105126131/404ed2ac-3557-47bf-b0d3-96601a556a66)


```
sudo su -
```
```ruby
#So, when you run "sudo su -," you are effectively asking the system to:Use sudo to gain superuser privileges.
#Use su to switch to the root user.The - option ensures that you get a clean root shell with root's environment.
#Once you execute this command, you'll be prompted to enter your password (assuming you have the necessary permissions).
#After providing the correct password, you'll be in a root shell, allowing you to execute administrative commands and make system-level changes.
#It's important to be cautious when using the root account
```
```
pvs
```
```
lvs
```
```ruby
LVS is like a magical librarian that helps you manage your virtual shelves,
and PVS is one of the real bookshelves in your computer where your data is physically stored.
 This combination makes it easier to manage and organize your computer's storage efficiently.
```
```
rpm -qa |grep lvm
```
```ruby
#rpm -qa | grep lvm" command, it will give you a list of installed packages that have "lvm" in their names.
#These packages are likely related to Logical Volume Management, and this command helps you find them among all the installed software on your system.
```



1.Install the correct packages that will allow the use of "lvm" on the centos machine.
```
 yum install -y lvm2
```

<h6>
#-y" option is used to automatically answer "yes" to any prompts or confirmation messages that might come up during the installation.
#It's a way to skip the need to manually confirm the installation
</h6>

check the file called lvm
```
rpm -qa |grep lvm
```
![Screenshot from 2023-10-17 22-12-18](https://github.com/Althaf-official/Linux_Challenges_Kodekloud/assets/105126131/d134f206-6b42-4c2c-8101-bb95ddf55b79)

2.Create a Physical Volume for "/dev/vdb" & "/dev/vdc"
```
 pvcreate /dev/vdb
```
```
pvcreate /dev/vdc
```




<h6>
pvcreate /dev/vdb," you're essentially telling your Linux system to prepare the storage device "/dev/vdb" to be used as a building block for logical volumes.
 Once this is done, you can add this physical volume to a volume group (VG) and create logical volumes (LVs) on top of it,
providing flexibility and control over your storage resources, such as resizing or combining volumes as needed.
</h6>

3.Create a volume group called "dba_storage" using the physical volumes "/dev/vdb" and "/dev/vdc"
```
vgcreate dba_storage /dev/vdb /dev/vdc

```
<h6>
# "vgcreate dba_storage /dev/vdb /dev/vdc," you are creating a volume group called "dba_storage" that combines the storage space of "/dev/vdb" and "/dev/vdc."
This volume group can then be used to create logical volumes (LVs) that can be mounted and used to store data or run applications,
 providing greater flexibility and management of your storage resources.
</h6>

4.Create an "lvm" called "volume_1" from the volume group called "dba_storage". Make use of the entire space available in the volume group.
```
lvcreate -n volume_1 -l 100%FREE dba_storage
```
<h6> "lvcreate -n volume_1 -l 100%FREE dba_storage," you're essentially creating a logical volume named "volume_1" within the "dba_storage" volume group, and you're instructing it to use up all the available free space in the volume group. This logical volume can then be formatted and used to store data or run applications within the constraints of the space allocated to it.</h6>

5.Format the lvm volume "volume_1" as an "XFS" filesystem
```
mkfs.xfs /dev/dba_storage/volume_1

```
![Screenshot from 2023-10-17 22-13-17](https://github.com/Althaf-official/Linux_Challenges_Kodekloud/assets/105126131/49c90c1b-a64d-4f95-be22-5c01d5a65405)


<h6>When you run "mkfs.xfs /dev/dba_storage/volume_1," you're essentially creating a new XFS file system on the logical volume "volume_1," which is contained within the "dba_storage" volume group. This formatted logical volume can then be mounted and used for storing data, just like a regular file system. XFS is known for its performance and reliability and is often used for large-scale storage in Linux systems.</h6>

6.Mount the filesystem at the path "/mnt/dba_storage".
```
mkdir -p /mnt/dba_storage
```
<h6>When you run "mkdir -p /mnt/dba_storage," it will create the "dba_storage" directory inside the "/mnt" directory. This can be useful for organizing and managing files and data within the "/mnt" directory, providing a dedicated location for the storage associated with "dba_storage" or any other purpose you have in mind. The "-p" option ensures that any necessary parent directories are created if they don't already exist.</h6>

```
mount -t xfs /dev/dba_storage/volume_1 /mnt/dba_storage
```

<h6>-t xfs: The "-t" option specifies the type of filesystem you are mounting. In this case, you're specifying "xfs" as the filesystem type. XFS is known for its performance and is one of the available filesystems in Linux.when you run "mount -t xfs /dev/dba_storage/volume_1 /mnt/dba_storage," you are effectively making the data within the XFS filesystem located on "volume_1" available and accessible through the "/mnt/dba_storage" directory. This allows you to work with and access the data in the logical volume as if it were a regular directory on your system.</h6>

```
df -h /mnt/dba_storage/
```

<h6>When you run "df -h /mnt/dba_storage/," you will get a summary of disk space information for the file system associated with the "/mnt/dba_storage" directory. The information typically includes details such as the total size of the file system, the amount of used space, the available space, the percentage used, and the mount point (the directory where it's mounted). This command is useful for checking how much space is being used and how much space is still available on a specific file system or directory.</h6>


7.Make sure that this mount point is persistent across reboots with the correct default options.

```
vi /etc/fstab
```

```ruby
UUID=a62c5b49-755e-41b0-9d36-de3d95e17232 /                       xfs     defaults        0 0

/swapfile none swap defaults 0 0

/dev/mapper/dba_storage-volume_1 /mnt/dba_storage xfs defaults 0 0

#VAGRANT-BEGIN

# The contents below are automatically generated by Vagrant. Do not modify.


```


8.Create a group called "dba_users" and add the user called 'bob' to this group

```
groupadd dba_users
usermod -G dba_users bob
id bob
```
<h6> "-G" option is used to specify the group or groups that you want to assign to the user.
  So, when you execute these commands, you are creating a new user group called "dba_users," adding the user "bob" to that group, and then using the "id" command to confirm that "bob" is now a member of the "dba_users" group and displaying relevant user information. This can be useful for managing user permissions and group memberships on a Linux system.</h6>




9.Ensure that the mountpoint "/mnt/dba_storage" has the group ownership set to the "dba_users" group

```
ll -lsd /mnt/dba_storage/
```

<h6>"ll -lsd /mnt/dba_storage/," you will get a long-format listing of the "/mnt/dba_storage" directory, which includes information about the directory itself (permissions, owner, group, size, and other details). This is a useful command to see metadata about a specific directory, especially when you want to check the attributes of the directory itself, rather than its contents.</h6>


```
chown :dba_users /mnt/dba_storage
```

<h6>When you run "chown :dba_users /mnt/dba_storage," you are changing the group ownership of the "/mnt/dba_storage" directory to "dba_users." This means that members of the "dba_users" group will have group-level permissions on the directory, which can include read, write, and execute permissions, depending on the group's permission settings. This command is useful for managing file and directory ownership and access control in a Linux system.</h6>


10.Ensure that the mount point "/mnt/dba_storage" has "read/write" and execute permissions for the owner and group and no permissions for anyone else

```
chmod 770 /mnt/dba_storage
```

<h6>When you run "chmod 770 /mnt/dba_storage," you are granting the owner and the group associated with the "/mnt/dba_storage" directory full read, write, and execute permissions, while denying any permissions to others. This command can be used to control who has access to the directory and what actions they can perform on it.</h6>

![Screenshot from 2023-10-17 22-13-43](https://github.com/Althaf-official/Linux_Challenges_Kodekloud/assets/105126131/d82daf4e-24c9-48bf-9c3f-e9ff7f6fa956)








```ruby
[bob@centos-host ~]$ sudo su -
[root@centos-host ~]# pvs
-bash: pvs: command not found
[root@centos-host ~]# lvs
-bash: lvs: command not found
[root@centos-host ~]# rpm -qa |grep lvm
[root@centos-host ~]#  yum install -y lvm2

Last metadata expiration check: 0:17:51 ago on Tue 17 Oct 2023 06:40:22 PM UTC.
Module yaml error: Unexpected key in data: static_context [line 9 col 3]
Module yaml error: Unexpected key in data: static_context [line 9 col 3]
Module yaml error: Unexpected key in data: static_context [line 9 col 3]
Module yaml error: Unexpected key in data: static_context [line 9 col 3]
Module yaml error: Unexpected key in data: static_context [line 9 col 3]
Module yaml error: Unexpected key in data: static_context [line 9 col 3]
Module yaml error: Unexpected key in data: static_context [line 9 col 3]
Module yaml error: Unexpected key in data: static_context [line 9 col 3]
Dependencies resolved.
============================================================================================================================================================================================================
 Package                                                        Architecture                            Version                                               Repository                               Size
============================================================================================================================================================================================================
Installing:
 lvm2                                                           x86_64                                  8:2.03.12-10.el8                                      baseos                                  1.6 M
Upgrading:
 device-mapper                                                  x86_64                                  8:1.02.177-10.el8                                     baseos                                  377 k
 device-mapper-libs                                             x86_64                                  8:1.02.177-10.el8                                     baseos                                  409 k
Installing dependencies:
 device-mapper-event                                            x86_64                                  8:1.02.177-10.el8                                     baseos                                  271 k
 device-mapper-event-libs                                       x86_64                                  8:1.02.177-10.el8                                     baseos                                  270 k
 device-mapper-persistent-data                                  x86_64                                  0.9.0-4.el8                                           baseos                                  925 k
 libaio                                                         x86_64                                  0.3.112-1.el8                                         baseos                                   33 k
 lvm2-libs                                                      x86_64                                  8:2.03.12-10.el8                                      baseos                                  1.2 M

Transaction Summary
============================================================================================================================================================================================================
Install  6 Packages
Upgrade  2 Packages

Total download size: 5.0 M
Downloading Packages:
(1/8): device-mapper-event-1.02.177-10.el8.x86_64.rpm                                                                                                                       1.8 MB/s | 271 kB     00:00    
(2/8): device-mapper-event-libs-1.02.177-10.el8.x86_64.rpm                                                                                                                  1.6 MB/s | 270 kB     00:00    
(3/8): libaio-0.3.112-1.el8.x86_64.rpm                                                                                                                                      1.2 MB/s |  33 kB     00:00    
(4/8): device-mapper-persistent-data-0.9.0-4.el8.x86_64.rpm                                                                                                                 4.8 MB/s | 925 kB     00:00    
(5/8): lvm2-libs-2.03.12-10.el8.x86_64.rpm                                                                                                                                   26 MB/s | 1.2 MB     00:00    
(6/8): device-mapper-1.02.177-10.el8.x86_64.rpm                                                                                                                             9.9 MB/s | 377 kB     00:00    
(7/8): lvm2-2.03.12-10.el8.x86_64.rpm                                                                                                                                        24 MB/s | 1.6 MB     00:00    
(8/8): device-mapper-libs-1.02.177-10.el8.x86_64.rpm                                                                                                                         14 MB/s | 409 kB     00:00    
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Total                                                                                                                                                                        20 MB/s | 5.0 MB     00:00     
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                                                                                                                                    1/1 
  Installing       : libaio-0.3.112-1.el8.x86_64                                                                                                                                                       1/10 
  Upgrading        : device-mapper-libs-8:1.02.177-10.el8.x86_64                                                                                                                                       2/10 
  Upgrading        : device-mapper-8:1.02.177-10.el8.x86_64                                                                                                                                            3/10 
  Installing       : device-mapper-event-libs-8:1.02.177-10.el8.x86_64                                                                                                                                 4/10 
  Installing       : device-mapper-event-8:1.02.177-10.el8.x86_64                                                                                                                                      5/10 
  Running scriptlet: device-mapper-event-8:1.02.177-10.el8.x86_64                                                                                                                                      5/10 
  Installing       : lvm2-libs-8:2.03.12-10.el8.x86_64                                                                                                                                                 6/10 
  Installing       : device-mapper-persistent-data-0.9.0-4.el8.x86_64                                                                                                                                  7/10 
  Installing       : lvm2-8:2.03.12-10.el8.x86_64                                                                                                                                                      8/10 
  Running scriptlet: lvm2-8:2.03.12-10.el8.x86_64                                                                                                                                                      8/10 
  Cleanup          : device-mapper-8:1.02.171-5.el8.x86_64                                                                                                                                             9/10 
  Cleanup          : device-mapper-libs-8:1.02.171-5.el8.x86_64                                                                                                                                       10/10 
  Running scriptlet: device-mapper-libs-8:1.02.171-5.el8.x86_64                                                                                                                                       10/10 
  Verifying        : device-mapper-event-8:1.02.177-10.el8.x86_64                                                                                                                                      1/10 
  Verifying        : device-mapper-event-libs-8:1.02.177-10.el8.x86_64                                                                                                                                 2/10 
  Verifying        : device-mapper-persistent-data-0.9.0-4.el8.x86_64                                                                                                                                  3/10 
  Verifying        : libaio-0.3.112-1.el8.x86_64                                                                                                                                                       4/10 
  Verifying        : lvm2-8:2.03.12-10.el8.x86_64                                                                                                                                                      5/10 
  Verifying        : lvm2-libs-8:2.03.12-10.el8.x86_64                                                                                                                                                 6/10 
  Verifying        : device-mapper-8:1.02.177-10.el8.x86_64                                                                                                                                            7/10 
  Verifying        : device-mapper-8:1.02.171-5.el8.x86_64                                                                                                                                             8/10 
  Verifying        : device-mapper-libs-8:1.02.177-10.el8.x86_64                                                                                                                                       9/10 
  Verifying        : device-mapper-libs-8:1.02.171-5.el8.x86_64                                                                                                                                       10/10 

Upgraded:
  device-mapper-8:1.02.177-10.el8.x86_64                                                             device-mapper-libs-8:1.02.177-10.el8.x86_64                                                            

Installed:
  device-mapper-event-8:1.02.177-10.el8.x86_64 device-mapper-event-libs-8:1.02.177-10.el8.x86_64 device-mapper-persistent-data-0.9.0-4.el8.x86_64 libaio-0.3.112-1.el8.x86_64 lvm2-8:2.03.12-10.el8.x86_64
  lvm2-libs-8:2.03.12-10.el8.x86_64           

Complete!
[root@centos-host ~]# 
[root@centos-host ~]# rpm -qa |grep lvm
lvm2-libs-2.03.12-10.el8.x86_64
lvm2-2.03.12-10.el8.x86_64
[root@centos-host ~]#  pvcreate /dev/vdb
  Physical volume "/dev/vdb" successfully created.
[root@centos-host ~]#  pvcreate /dev/vdc
  Physical volume "/dev/vdc" successfully created.
[root@centos-host ~]# vgcreate dba_storage /dev/vdb /dev/vdc
  Volume group "dba_storage" successfully created
[root@centos-host ~]# lvcreate -n volume_1 -l 100%FREE dba_storage
  Logical volume "volume_1" created.
[root@centos-host ~]# mkfs.xfs /dev/dba_storage/volume_1
meta-data=/dev/dba_storage/volume_1 isize=512    agcount=4, agsize=130560 blks
         =                       sectsz=512   attr=2, projid32bit=1
         =                       crc=1        finobt=1, sparse=1, rmapbt=0
         =                       reflink=1
data     =                       bsize=4096   blocks=522240, imaxpct=25
         =                       sunit=0      swidth=0 blks
naming   =version 2              bsize=4096   ascii-ci=0, ftype=1
log      =internal log           bsize=4096   blocks=2560, version=2
         =                       sectsz=512   sunit=0 blks, lazy-count=1
realtime =none                   extsz=4096   blocks=0, rtextents=0
[root@centos-host ~]# mkdir -p /mnt/dba_storage
[root@centos-host ~]# mount -t xfs /dev/dba_storage/volume_1 /mnt/dba_storage
[root@centos-host ~]# df -h /mnt/dba_storage/
Filesystem                        Size  Used Avail Use% Mounted on
/dev/mapper/dba_storage-volume_1  2.0G   47M  2.0G   3% /mnt/dba_storage
[root@centos-host ~]# vi /etc/fstab
[root@centos-host ~]# groupadd dba_users
[root@centos-host ~]# usermod -G dba_users bob
[root@centos-host ~]# id bob
uid=1002(bob) gid=1002(bob) groups=1002(bob),1003(dba_users)
[root@centos-host ~]# chown :dba_users /mnt/dba_storage
[root@centos-host ~]# chmod 770 /mnt/dba_storage
[root@centos-host ~]# 
```


