# Linux_Challenges_Kodekloud
![Screenshot from 2023-10-17 15-35-34](https://github.com/Althaf-official/Linux_Challenges_Kodekloud/assets/105126131/3de6eb8b-1de6-41c3-acc0-98dda0e950dd)


<h1>5th Challange  We got a couple of tasks that need to be done on centos-host server. Most of these tasks are dependent on each other but not all of them.</h1>

1
```
sudo su -
```



2.Add a local DNS entry for the database hostname "mydb.kodekloud.com" so that it can resolve to "10.0.0.50" IP address.

```
vi /etc/hosts
```
```ruby
10.0.0.50   mydb.kodekloud.com
```






3.install "mariadb" database server on this server and "start/enable" its service.

```
yum install mariadb-server
```
```
systemctl start mariadb
systemctl enable mariadb
```






4.Set a password for mysql root user to "S3cure#321"

```
mysql -u root -p
```
```ruby
ALTER USER 'root'@'localhost' IDENTIFIED BY 'S3cure#321';
```
```ruby
exit
```







5.The "root" account is currently locked on "centos-host", please unlock it.
Make user "root" a member of "wheel" group
```
sudo passwd -u root
```
```
sudo usermod -aG wheel root
```





6.Pull "nginx" docker image.
```
docker pull nginx
```





7.Create and run a new Docker container based on the "nginx" image. The container should be named as "myapp" and the port "80" on the host should be mapped to the port "80" on the container.

```
docker run -d -p 80:80 --name myapp nginx
```





8.Create a bash script called "container-stop.sh" under "/home/bob/" which should be able to stop the "myapp" container. It should also display a message "myapp container stopped!"
```
cd /home/bob/
```
```
vi container-stop.sh
```



```ruby
#!/bin/bash

# Stop the "myapp" container
docker stop myapp

# Display a message
echo "myapp container stopped!"
```


```
chmod +x container-stop.sh
```
```
./container-stop.sh
```







9.Create a bash script called "container-start.sh" under "/home/bob/" which should be able to "start" the "myapp" container. It should also display a message "myapp container started!"

```
vi container-start.sh
```

```ruby
#!/bin/bash

# Start the "myapp" container
docker start myapp

# Display a message
echo "myapp container started!"
```


```
chmod +x container-start.sh
```
```
./container-start.sh
```







10.Add a cron job for the "root" user which should run "container-stop.sh" script at "12am" everyday.

Add a cron job for the "root" user which should run "container-start.sh" script at "8am" everyday.

```
sudo crontab -e
```


```ruby
# Schedule container-stop.sh to run at 12 am (midnight)
0 0 * * * /bin/bash /home/bob/container-stop.sh

# Schedule container-start.sh to run at 8 am
0 8 * * * /bin/bash /home/bob/container-start.sh
```






11.Edit the PAM configuration file for the "su" utility so that this utility only accepts the requests from the users that are part of the "wheel" group and the requests from the users should be accepted immediately, without asking for any password.

```
cd ~
```
```
sed -i 's/#auth/auth/' /etc/pam.d/su
```






12.Add an extra IP to "eth1" interface on this system: 10.0.0.50/24
```
ip addr add 10.0.0.51/24 dev eth1
```
