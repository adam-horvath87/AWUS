So. My goal is to setup an AWUS036ACH on my kali virtual machine (Base OS is Endeavour).


Step By step:

1: Join the adapter to the computer
2: To use the adapter, i must install the virtual box's extension pack (Oracle Virtualbox). Othervise the virtual machine won't see the adapter. (cuz i cant use USB 3.0, only 1.1)
3: Add the adapter to the virtual machine: Settings/USB/ --> enable usb 3.0 , On the right---> add Realtek
4: Pull out the adapter and start kali.
5: After entering to the user, join the adapter
6: Open terminal, and type: lsusb

It will show something like this:
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 001 Device 002: ID 80ee:0021 VirtualBox USB Tablet
Bus 002 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 002 Device 003: ID 0bda:8812 Realtek Semiconductor Corp. RTL8812AU 802.11a/b/g/n/ac 2T2R DB WLAN Adapter
So here is the adapter on Bus 002.

7. But before i use it, i have to install the driver for it.
So...into terminal:

#update the packets
sudo apt-get update

#upgrade the system
sudo apt-get upgrade
sudo apt-get dist-upgrade -y

#install the driver
sudo apt install dkms
sudo apt install realtek-rtl8812au-dkms -y
