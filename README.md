# Owncloud_Setup

This project attempts to utilize a Raspberry pi to host Owncloud server and set it up for public accessutilizing a cloud VM and port translation

Resources:
https://thepi.io/how-to-set-up-a-raspberry-pi-owncloud-server/
https://medium.com/jj-innovative-results/how-to-access-a-raspberry-pi-anywhere-with-reverse-ssh-and-google-cloud-platform-59b6a89501a

To set up the raspberry pi, I installed dietpi and used the GUI to install owncloud. The config file provided the logins to the server (admin, dietpi). 
Hooking up an external drive I had to mount it with the write owner.... [will fill this in]

Setting up a cloud VM with IP address and ssh access: ssh garges@34.125.40.187
Had to do some port forwarding set up on the vm...

Set up reverse tunnelling with a script. Also set up a tunnel for the cloud vm but had to change a couple of settings... 0.0.0.0
using netcat? -nlpt 
Had to add the cloud VM IP to the config file 



http://34.x.x.x:6080/owncloud
https://10.0.0.197/owncloud (when on the network)
