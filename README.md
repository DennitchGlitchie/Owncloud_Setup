# Owncloud_Setup

## This project attempts to utilize a Raspberry pi to host Owncloud server and set it up for public accessutilizing a cloud VM and port translation

Resources:
https://thepi.io/how-to-set-up-a-raspberry-pi-owncloud-server/
https://medium.com/jj-innovative-results/how-to-access-a-raspberry-pi-anywhere-with-reverse-ssh-and-google-cloud-platform-59b6a89501a

(1) Gathering Hardware:
- Raspberry Pi 4 Model B
- DietPi Image https://dietpi.com/docs/install/
- 1 TB Harddrive https://www.amazon.com/dp/B07VTWX8MN?psc=1&ref=ppx_yo2_dt_b_product_details

(2) Setting up Raspberry Pi for Owncloud 
- Setting up automatic wifi by editting the config file https://dietpi.com/phpbb/viewtopic.php?p=9#p9
  - Open dietpi-wifi.txt and open it with wordpad
  - Change aWIFI_SSID[0]='MySSID' and aWIFI_KEY[0]='MyWifiKey'
  - Don't forget to set automatic wifi to be enabled? AUTO_SETUP_NET_WIFI_ENABLED=1
- Downloading the owncloud software with command dietpi-config
- Logging in on local network 10.0.0.197/owncloud admin dietpi
  - The sample data is located in /mnt/dietpi_userdata/owncloud_data/admin/files
- Setting up ssh key for raspberry pi from laptop: ssh -i owncloud_pi root@10.0.0.197
  - Use ssh key-gen to generate a key with custom name and adding the .pub file to ~/.ssh/authorized_keys
- Mounting the external drive with correct ownership: sudo mount -o umask=0,uid=111,gid=33 /dev/sdc1 /media/usb
  - lsblk to see where the drive is
  - cat /etc/group to see: "redis: x :111:www-data"
  - cat /etc/passwd to see: "www-data: x :33:33:www-data:/var/www:/usr/sbin/nologin" 
  - (": x :" should be ":x:")
  - ls -l to compare permissions 
  - Will have to remount if disconnected and if restarted
  - I have confirmed that I can remove the drive and see the files on it and then get it back there 
- Creating path in the owncloud web GUI: admin > storage (in admin section) > External Storage:local > configuration:/media/usb/

(3) Setting up VM for port translation public access
- Creating a free tier cloud vm https://cloud.google.com/free/docs/gcp-free-tier#free-tier-usage-limits
- Adding firewall rule for reverse SSH https://www.youtube.com/watch?v=lTYXKAKv-ok (he does UDP 6000-6100 but I did allow all)
- Setting up ssh key for cloud_vm from laptop: ssh -i ~/.ssh/cloud_vm garges@34.127.85.102 (laptop needs the key path)
  - Use ssh key-gen to generate a key with custom name and adding the .pub file to online interface or ~/.ssh/authorized_keys
- Setting up ssh key for cloud_vm from raspberry pi: ssh -i ~/.ssh/cloud_vm garges@34.127.85.102 (Raspberry pi needs the key path)
  - Use ssh key-gen to generate a key with custom name and adding the .pub file to ~/.ssh/authorized_keys
- Testing a reverse tunnel set up on the raspberry pi: ssh -N -R 0.0.0.0:6000:localhost:22 -i ~/.ssh/cloud_vm garges@34.127.85.102
  - 0.0.0.0 is wildcard for restrictions on IP address trying to connect
  - -N and -R specify XX and reverse tunneling
  - Listening on port 6000 and translate on port 22 (ssh port)
- Follow the tunnel from cloud vm: ssh -p 6000 root@localhost  
- Testing out port translation: ssh -N -R 0.0.0.0:6080:localhost:80 -i ~/.ssh/cloud_vm garges@34.127.85.102
  - 80 is the port of the owncloud server, 6080 is the port we will open to 
  - To install netstat: apt-get install net-tools
- Using netstat -nlpt to confirm that we have the right accessability with the ports (I am unsure to what command I used for this):
  - tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN      792/lighttpd        
  - tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      636/sshd: /usr/sbin 
  - 'when some computer tries to connect to your vm, what restrictions do we have on its IP address, 0.0.0.0 is the wildcard'
- Confirming that 34.127.85.102:6080/owncloud gives access to owncloud
  - This needs http://34.127.85.102:6080/owncloud
- Configure the "trusted_domains" setting in config/config.php: sudo nano /var/www/owncloud/config/config.php
  - 4 => '34.127.85.102', 
  - login is admin, pw is dietpi
- Creating automated script so that the reverse tunnel and port translation are automatically done
  - creating tunnel.sh script from tutorial with correct permissions
  - ssh -N -R 0.0.0.0:6000:localhost:22 -R 0.0.0.0:6080:localhost:80 -i ~/.ssh/cloud_vm garges@34.127.85.102
  - /etc/ssh/sshd_config "gateway ports" this is the file that was changed on the cloud vm "GatewayPorts clientspecified"
  - crontab -e to open the cron tab
  - */1 * * * * ~/tunnel.sh > tunnel.log 2>&1
- Setting up automatic mounting in /etc/fstab UUID=877F-16FC /media/usb vfat defaults,auto,users,rw,nofail,umask=0,uid=111,gid=33 0 0
  - Run the blkid command to find out the UUID of the USB drive:
  - https://www.shellhacks.com/raspberry-pi-mount-usb-drive-automatically/
