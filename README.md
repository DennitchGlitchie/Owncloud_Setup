# Owncloud_Setup

This project attempts to utilize a Raspberry pi to host Owncloud server and set it up for public accessutilizing a cloud VM and port translation

Resources:
https://thepi.io/how-to-set-up-a-raspberry-pi-owncloud-server/
https://medium.com/jj-innovative-results/how-to-access-a-raspberry-pi-anywhere-with-reverse-ssh-and-google-cloud-platform-59b6a89501a

(1) Gathering Node Hardware:
-Raspberry pi version XXXX
-DietPi Image
-Setting up the wifi on the config file 
-Harddrive XXXX
-

(2) Setting up Raspberry Pi for Owncloud 
-Downloading the software
-Mounting the drive with correct ownership 
-Logging in on local network

(3) Setting up VM for port translation public access
-Setting up SSH
-Setting up Reverse Tunnelling script
-Setting up port forwarding with the right rules in the VM
-Any rules for this (0.0.0.0 thing)
-Chacning the config.php file 



Notes:
https://10.0.0.197/owncloud
http://X.x.x.x:6080/owncloud
admin dietpi
netcat -nlpt

How to ssh from macbook to vm
How to ssh from raspberry pi to vm 
How to complete the tunnel from vm to go to raspberry pi

Where is the different types of data located on the pi
Where is the config file located






To set up the raspberry pi, I installed dietpi and used the GUI to install owncloud. The config file provided the logins to the server (admin, dietpi). 
Hooking up an external drive I had to mount it with the write owner.... [will fill this in]

Setting up a cloud VM with IP address and ssh access: ssh garges@34.125.40.187
Had to do some port forwarding set up on the vm...

Set up reverse tunnelling with a script. Also set up a tunnel for the cloud vm but had to change a couple of settings... 0.0.0.0
using netcat? -nlpt 
Had to add the cloud VM IP to the config file 



http://34.x.x.x:6080/owncloud
https://10.0.0.197/owncloud (when on the network)
