# Version 0.1
# Document Started on date 03-12-2024

# Document explains the step to Install Ubuntu Server 22.04 on Oracle Cloud with LXQt Desktop and configure Free RDP connection
# Original source - YouTube https://youtu.be/onjz8AYMqwo?si=bg9j-7N20UZM_SS4

# Image Needed - Ubuntu arm64 image with minimum of 2vcpu and 12Gb memory
# Already configured VCN wiuth Public Subnet and Internet Gateway

# 1. Provision the VM First and make sure ssh is accessible over port 22 allowed through Public Subnet
apt update
apt upgrade
apt install vim
apt install lxqt sddm xrdp


# Approx 2Gb will be downloaded and updated

# Check the following 
systemctl status xrdp
systemtl enable --now xrdp

# Open port 3389 for your public IP on Public Security List
No	136.158.58.12/32 (your Public IP)	TCP	All	3389		TCP traffic for ports: 3389

# Allow port 3389 on Ubuntu Machine as well
vim /etc/iptables/rules.v4
# duplicate the rule of ssh port 22 and add 3389 port in place of port 22 and save the file
# To reload the IPtables Rule
iptables-restore < /etc/iptables/rules.v4

# Now go to /home/ubuntu
vim .xsession
# add 
startlxqt
# and save the file

# set the password of user ubuntu

passwd ubuntu
# type your password 2 times and a complex password

# reboot the Ubuntu Node

# Once the node will come back


# try remote desktop with public ip and take ubuntu as per user and provide its password

# After login 
# go to Start > Windows Manager > Windows Manager Tweaks > Compositor > uncheck enable display compositing 

# Reboot the node. you can choose not to reboot at before step and reboot after completion of these steps.

# Install iputils-ping Package: Now, install the iputils-ping package with this command:
apt install iputils-ping

# To install standalone .deb package
sudo dpkg -i code_1.87.1-1709684532_arm64.deb

