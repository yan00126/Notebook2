# Network Operating Systems - Final PRACTICAL - Raspberry PI

## WARNING: This version of the Final Practical is for the use on a Raspberry PI Zero W 2. If you are using a Virtual Machine instead or a Raspberry Pi 3-4-5, please refer to the appropriate version.

## Recommendation: Do not simply copy/paste everything as-is from the text below. Take the time to ***read carefully***. Some text may not have to be pasted. Go one line at a time. 

*Time Limit:* 3 Hours

*Actual Time:* Assuming everything goes right, this should  take 2 hours or less, allowing you time to make a few mistakes, but try not to do that.

------

## Introduction

Please follow the steps in order, one after the other. Test every step along the way to check if it worked out, if it did not and you continue it will be harder to diagnose going forward. Any thing you see in a ``format like this`` can be assumed to be something you should type, into your terminal.

## Name your Pi

> 2 marks

Log on to your Pi

Rename it as your Algonquin user name and Enable `VNC`.

Issue the following command:

```
sudo raspi-config
```

Then use the menu and options to setup the name of your device to your username, and enable VNC..................... Note that your device will reboot. 

Issue the following commands:

```
ifconfig
uname -n
```

Take a screenshot  **s0.jpg** of the two above commands, upload the file to brightspace **NOW**

## House Keeping

> 5 marks
- Install `SSH`. If you already did, there is no harm in running this command.

  ```
  sudo apt install openssh-server -y
  ```

  Install `wget` and `ufw`

  ```
  sudo apt install wget ufw -y
  ```

  Issue the following commands in sequence

  ```
  clear
  hostname -I
  df
  pwd
  date
  ```
  
  Take a screenshot  **s1.jpg**, upload the file to brightspace **NOW**
  
  Issue the following command
  
  ```
  cd /etc
  clear
  ls -haltd */ | head -30 > ~/start.txt
  ```
  
  Transfer the file `start.txt` to your laptop,  upload the file to brightspace **NOW**.

## Firewall
> 8 marks

Create firewall rules in order to 

- Allow `SSH` traffic 
- Allow `HTTP` traffic
- Allow `VNC` traffic
- Allow `HTTPS` traffic
- Allow mySql traffic
- Allow traffic on port 8081
- Enable Port 1337 via TCP only

Turn on the firewall 

**(Figure out the commands to use..................)**

Issue the following command:

```
sudo ufw status
```

You should see the rules you added, if you see nothing or 'firewall disabled' you have done this wrong.

Take a screenshot  **s2.jpg**,  upload the file to brightspace **NOW**

## Install Services

> 8 marks

Install `apache2`, `mariadb-server`, `php` and `phpmyadmin` with the command:

```
sudo apt-get install -y apache2 mariadb-server php phpmyadmin
```

--> Also answer the questions that will be asked during the install. There will be two sets of questions......

Use the following command to add the `pi` user to the `www-data` group

```
sudo usermod -a -G www-data pi
```

Issue the following commands (read carefully !!!, not everything is to be blindly pasted)

```
sudo mysql
MariaDB [(none)]> USE mysql;
CREATE USER 'pi'@'localhost' IDENTIFIED BY 'Password123';
GRANT ALL PRIVILEGES ON *.* TO 'pi'@'localhost'; 
MariaDB [mysql]> FLUSH PRIVILEGES;
MariaDB [mysql]> exit;
```

Open a browser and navigate to `http://yourLinuxIp/phpmyadmin`

where `yourLinuxIp` is the IP address of your Pi.

Logon as a pi

Take a screenshot  **s3.jpg**,  upload the  file to brightspace **NOW**, then close the brower window. 

On the Pi, issue the following command in a terminal window:

```
netstat -at
```

Take a screenshot  **s4.jpg**,  upload the  file to brightspace **NOW**

### Download  Joomla

Get the Joomla file (Note that you must **NOT** install the latest version of Joomla but version 4.2.9)

```
cd ~

sudo wget https://downloads.joomla.org/cms/joomla4/4-2-9/Joomla_4-2-9-Stable-Full_Package.tar.gz

```

Move the files to the Apache folder

```
sudo mv Joomla_4-2-9-Stable-Full_Package.tar.gz /var/www/html
```

Extract the files

```
cd /var/www/html
sudo tar -xvf Joomla_4-2-9-Stable-Full_Package.tar.gz
```

Remove the tar file and change ownership of root folder

```
sudo rm Joomla_4-2-9-Stable-Full_Package.tar.gz

sudo chown -R www-data:www-data /var/www/
```

On the Pi, 

`cd ~`

Create a mariadb database called joomla.

------

### Setup Joomla

> 12 marks

- Start a `VNC` session connecting to your Pi.

- Open a Chromium browser and go to **http://***yourLinuxIp***/installation**

- Take a screenshot  **s4-1.jpg**,  upload the  file to brightspace **NOW**

- Enter the Site Name: Practical <Your name>

- Set the Super User name, login info and email

- Enter the database credentials

- It takes a few moments, ***be patient***

  

### Check your work 

Back on your laptop, type sudo reboot. Your PI will restart give it 10-15 seconds and then reconnect via SSH again to confirm it is back online.

From your laptop, browse to your new website via `http://yourLinuxIp/index.php`

where `yourLinuxIp` is the IP address of your Pi.

Take a screenshot  **s5.jpg**,  upload the  file to brightspace **NOW**

Take a screenshot of a directory listing of `/var/www/html` **s6.jpg**, upload the  file  to brightspace **NOW**

Issue the following commands on your Pi in a terminal window

```
clear
hostname -I
df
pwd
date
```

Take a screenshot  **s7.jpg**,  upload the  file to brightspace **NOW**

Issue the following command

```
cd /etc
ls -haltd */ | head -30 > ~/end.txt
```

Transfer the file `end.txt` to your laptop,  upload the  file to brightspace **NOW**.

### Virtual  hosts

> 5 marks

Here again, replace 'yourname' with your actual name for the domain name 'yourname.finalpractical.ca'. You will also need to access your web site with a browser from inside your Linux server as you will need to locally register the domain name within /etc/hosts. You will need to use VNC. 

Adjust then use the following command to generate your SSL certificate:

`sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/ private/yourname.pem -out /etc/ssl/private/yourname.pem`

--> Make sure that the response to the FQDN prompt is `yourname.finalpractical.ca`

--> Replace "yourname" with your real name. 

You can then use the command:

`openssl x509 -text -noout -in /etc/ssl/private/yourname.pem` 

to view the content of your certificate.

--> Replace "yourname" with your real 	name. 

Secure your joomla site with `https`

Make your site replies to `https://yourname.finalpractical.ca`

Add the appropriate entry to /etc/hosts using vi or nano (remember to use sudo)

Add the appropriate vhost entry in /etc/httpd/conf/httpd.conf. 

Browse to your new website via `https://yourname.finalpractical.ca`

Take a screenshot  **s8.jpg**, upload the file to brightspace **NOW**



