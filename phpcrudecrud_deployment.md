# phpcrudecrud_deployment.md
## Luke Treiber
- This is a guide to assit in the deployment of the PHP application. The guide will walk you through creating an setting up the environment to allow for the PHP crude CRUD application to run. 

## Purpose
- This application was designed to teach a user how to set up and implement a baisc PHP Application. The Application runs on a webserver and allows a user to have full CRUD (Create, Read, Update, and Delete) operations from a browser connection. 
## Overview
- The guide will begin by explaining how to create and install a Ubuntu Virutal Machine. The virutal machine will act as our both our Webserver (Apache2) and Database. We will need to set up a connection to the database. From this, we will be able to deploy our PHP application which can be accessed via a connection. 
## Virtual Machine Configuration
- While the Virutal Machine configuration will depend on how many users you expect to connect, the application is quite light weight. Thus, requirements for our virutal machine are quite low.
    - CPU: 1 Core
    - RAM: 2 GB
    - Disk: 10GB

### - Setting up VM
This guide is assuming that Virtualbox is being used and has already been installed. Here is a link to the Virtualbox download page: [Virutalbox Downloads](https://www.virtualbox.org/wiki/Downloads)
1. Let's launch Virtualbox and click the blue star button in the top-row.
2. Next, lets name our virtual machine: phpcrudecrud_VM
3. Download [Ubuntu 20.04 ISO](https://ubuntu.com/download/alternative-downloads)
4. Select this download as the ISO image in Virtualbox
5. Click the dropdown unattended install and add your login information you want to use to login to your Virtualbox.
6. Under the Hardware section, the default setting are okay. But, verify they are with the requirements listed above
7. Under the Disk section, feel free to reduce the space to 10GB or keep it at the default.
8. Hit Enter on your keyboard and wait for the VM to boot!
### - Installing Ubuntu 20.04 Server
1. When the Virutal Machine is launched, you should select your perfered language.
2. Next, continue without updating the installer.
3. Many of these confiugrations can remain as default by using the arrow keys to select DONE. Done with heighligh as green when it is selected.  
4. Create your profile
4.1 Add your name and make the server's name <yourname>phpcrudecrud 
4.2 Create a username to login
4.3 Create and confirm your login password
NOTE: Please ensure that you have recorded the username and password. Otherwise, you cannot login
5. Enable the installation of OpenSSH server
6. Wait for the Installation to complete
    This will be indicated in the top left and highlighted in orange
7. Shutdown the Virtual Machine
## Setting up network (HIGHLY RECOMMENDED)
### This set allows us to us SSH to access the machines. You can skip this if you only want to use the virtual box. 
1. While the Virutal Machine is turned off, let's change it's 'hardware', so we can use SSH.
2. Under tools, create a host-only adapter
3. The adapter should be configured automatically and have DHCP server enabled
4. Now, select our virutal machine back on the homepage and hit settings. This should display a pop-up windows with the name of the VM.
5. On the left side, there is a network menu. 
6. Under this menu, select and enable Adapter 2
7. The 'attached to' should no longer be greyed out, so we can select the host-only adapter. This should automaticlly select our adapter we created unless you have created other adapters
8. Now, we can hit okay and boot/login to our virtual machine again.
9. We still need to link our adapter to the virutal machine. We can do this by editing the /etc/network/interface file.
10. First, we need to know the name of our new interface.
11. Run "ip addr show"
12. Take note of the only interface without an IP. In my case, it is enp0s8. In your case this might be different.
13. Let's create and edit this file by running the command 
    - sudo nano /etc/network/interfaces
    - Inside add
    auto enp0s8
    iface enp0s8 inet dhcp
    - Ensure you saved the file!!
14. Next, restart the networking by rebooting
15. To bring the network interface up, we need to install ifupdown to our machine.
16. sudo apt install ifupdown
17. Next, we can bring the interface up with "sudo ifup enp0s8"
18. Now, use ip addr show to display the ip address that was given by the dchp server. It should be something like 192.168.X.X
## Software Dependencies and Installation
- To allow for our application to operate correctly, we need to install and configure Apache2, PHP, and MySQL.
- Now, we can use our host machine's terminal to ssh and access the server.

### - Installing Apache2 and PHP
- Now, let's install apache2 and php
- First, run a quick update
$ sudo apt update
$ sudo apt upgrade
- Next, install apache2 and php
    - sudo apt install apache2
    - sudo apt install php
    - sudo apt install libapache2-mod-php
    - sudo apt install php-cli
    - sudo apt install php-mysql
    - sudo apt install php-pgsql

#### Verifying Installions
- Let's bring our apache2 service online with
 sudo systemctl start apache2
- You can verify it's online with
 sudo systemctl status apache2
and by going to your browser and typing in the ip address
(Taken from Canvas)
- Create a new web page in /var/www/html to test PHP. Name the file phptest.php
sudo nano phptest.php
Add the following lines of code:
<?php
phpinfo()
?>
-Move the phptest.php to the /var/www/html directory.    It is owned by root, so you may need to use "sudo."
-sudo mv phptest.php /var/www/html
Launch your browser on your laptop and point it to the following url:
http://<ip address of your vm>/phptest.php

### - Installing MySQL (MariaDB)
Now, since apache2 and PHP are installed and verified, we can move on to installing MariaDB. 
- sudo apt install mariadb-server
- sudo systemctl start mariadb
Run sudo systemctl status mariadb to verify that the database is up and running
Now, let's properly install.
sudo mysql_secure_installation  
There will be multiple prompts
-Enter current password : hit enter
-Set root password : Y
- New Password : <your chosen password>
- Remove anonymous users: y
- Disallow root login remotely: y
- Remove test database: y
- Reload? : y

Let's restart MariaDB to ensure changes will take place
sudo systemctl restart mariadb
Now, let's begin to configure our database

### - Configuring Connection and Cridentials
Now, let's create a new user and grant it all privileges

1. sudo mysql
2. create user '<yourusername>'@'localhost' identified by '<somepassword>';
3. grant all privileges on *.* to '<yourusername>'@'localhost';
4. flush privileges
Verify: 
Use select Host, User from mysql.user; to verify your user has been created

Now, create put new data in our database and we will login with our new user. 
1. exit out of mariadb with quit
2. run git clone https://github.com/datacharmer/test_db.git
3. cd test_db/
 mysql -u <youruser> -p


## Deploy PHP Crude CURD Application

### - Testing Deployment 

