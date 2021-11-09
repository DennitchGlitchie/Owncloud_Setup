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
-Downloading the owncloud software 
-Mounting the drive with correct ownership 
-Logging in on local network 10.0.0.197/owncloud admin dietpi
-Setting up ssh key for raspberry pi from laptop: ssh -i owncloud_pi root@10.0.0.197
  ->Use ssh key-gen to generate a key with custom name and adding the .pub file to authorized_keys

(3) Setting up VM for port translation public access
-Creating a free tier cloud vm https://cloud.google.com/free/docs/gcp-free-tier#free-tier-usage-limits
-Adding firewall rule for reverse SSH https://www.youtube.com/watch?v=lTYXKAKv-ok (he does UDP 6000-6100 but I did allow all)
-Setting up ssh key for cloud_vm from laptop: ssh -i ~/.ssh/cloud_vm garges@34.127.85.102 (laptop needs the key path)
  -> Use ssh key-gen to generate a key with custom name and adding the .pub file to online interface or authorized_keys
-Setting up ssh key for cloud_vm from raspberry pi: ssh -i ~/.ssh/cloud_vm garges@34.127.85.102 (Raspberry pi needs the key path)
  -> Use ssh key-gen to generate a key with custom name and adding the .pub file to authorized_keys

-Testing a reverse tunnel set up on the raspberry pi: ssh -N -R 0.0.0.0:6000:localhost:22 -i ~/.ssh/cloud_vm garges@34.127.85.102
  -> 0.0.0.0 is wildcard for restrictions on IP address trying to connect
  -> -N and -R specify XX and reverse tunneling
  -> Listening on port 6000 and translate on port 22 (ssh port)
-Follow the tunnel from cloud vm: ssh -p 6000 root@localhost  
-Testing out port translation such that I find the files at: 






-etc/hosts to create alias 


-Setting up SSH
-Setting up Reverse Tunnelling script
-Setting up port forwarding with the right rules in the VM
-Any rules for this (0.0.0.0 thing)
'when some computer tries to connect to your vm, what restrictions do we have on its IP address, 0.0.0.0 is the wildcard'
-Chacning the config.php file 



Notes:
https://10.0.0.197/owncloud
http://X.x.x.x:6080/owncloud
admin dietpi
netstat -nlpt (sudo apt install nettools?) which programs have their ear waiting for connections


How to ssh from macbook to vm
How to ssh from raspberry pi to vm 
How to complete the tunnel from vm to go to raspberry pi

Where is the different types of data located on the pi
Where is the config file located


public key is like the lock put on the device, the private key is the key 



(Note) SSH Trouble:
I had some trouble when I downsized my vm to a XXX version. The IP address changed and potentially other things changed too. I ended up clearing known_hosts and authorized_keys files as well as all existing keys. I could generate new keys with new names ssh-keygen and utilize ssh -i cloud_vm garges@x.x.x.x. I also learned the difference between autorized_files and 





To set up the raspberry pi, I installed dietpi and used the GUI to install owncloud. The config file provided the logins to the server (admin, dietpi). 
Hooking up an external drive I had to mount it with the write owner.... [will fill this in]

Setting up a cloud VM with IP address and ssh access: ssh garges@34.125.40.187
Had to do some port forwarding set up on the vm...

Set up reverse tunnelling with a script. Also set up a tunnel for the cloud vm but had to change a couple of settings... 0.0.0.0
using netcat? -nlpt 
Had to add the cloud VM IP to the config file 



http://34.x.x.x:6080/owncloud
https://10.0.0.197/owncloud (when on the network)
