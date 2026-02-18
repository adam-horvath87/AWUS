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

If its not working, i cant see the adapter in the iwconfig list...(cuz 6.18.9 - currently the newest...rolling releases...god damn... - kernel version doesn't have working driver support for this adapter...so I needed and older version of the system) 
So i can see the in the usb list with lsusb, but i cannot see under iwconfig...
└─$ iwconfig
lo        no wireless extensions.

eth0      no wireless extensions.

I had to download an original kali iso from their website. The version number is 6.12.25 (right now)
I can't just simpli update, upgrade the system (if i do, it will refreshes to 6.18.9, which is not good), i had to find the older systems headers files, and install them manually...So, here are them:
https://old.kali.org/kali/pool/main/l/linux/

I needed 3 files from here...
linux-headers-6.12.25-amd64_6.12.25-1kali1_amd64.deb                                                      
linux-headers-6.12.25-common_6.12.25-1kali1_all.deb                                                       
linux-kbuild-6.12.25_6.12.25-1kali1_amd64.deb    

lets install them:
sudo apt install ./linux-kbuild-6.12.25_6.12.25-1kali1_amd64.deb \
                 ./linux-headers-6.12.25-common_6.12.25-1kali1_all.deb \
                 ./linux-headers-6.12.25-amd64_6.12.25-1kali1_amd64.deb

Let's check if its good:
ls -l /lib/modules/$(uname -r)/build

it will show something like this:
lrwxrwxrwx 1 root root 40 2025 apr   30 /lib/modules/6.12.25-amd64/build -> ../../../src/linux-headers-6.12.25-amd64

So it's installed..next move is download this github repo:
git clone https://github.com/aircrack-ng/rtl8812au.git

then entering the directory:
cd rtl8812au 

and run this command: #installing the driver which i downloaded 
make
make install

Two more commands left to turn on the adapter:
sudo depmod -a
sudo modprobe 88XXau

And here it is...its working:
└─$ iwconfig
lo        no wireless extensions.

eth0      no wireless extensions.

wlan0     unassociated  ESSID:""  Nickname:"<WIFI@REALTEK>"
          Mode:Managed  Frequency=2.412 GHz  Access Point: Not-Associated   
          Sensitivity:0/0  
          Retry:off   RTS thr:off   Fragment thr:off
          Power Management:off
          Link Quality:0  Signal level:0  Noise level:0
          Rx invalid nwid:0  Rx invalid crypt:0  Rx invalid frag:0
          Tx excessive retries:0  Invalid misc:0   Missed beacon:0



