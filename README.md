	STEPS IN ADDING WIFI RTL2870 USB FUNCTIONALITY TO KERNEL

1)	Build the default configuration
	make ARCH=arm omap3_devkit8500_defconfig

2)	Customize the Kernel enabling the Ralint WiFi as module. Also enable the staging drivers

3)	Compile the kernel.
	make ARCH=arm uImage

4)	Compile the drivers for ARM architecture. The compiled drivers, kernel configuration and uImage, wireless-tools binaries are placed in this directory.

5)	Compile the wireless-tools with the cross compilation toolchain.

6)	Copy the uImage to /media/LABEL1

	cp uImage /media/LABEL1

7)	Copy the driver to /lib/firmware

	cp rt5370sta.ko /media/LABEL2/lib/firmware

	cp RT2870STA.dat /etc/Wireless/RT2870STA/

8)	Copy wireless_tools binaries to sdcard.

	cd wireless_tools.29

	cp ifrename iwconfig iwevent iwgetid iwlist iwpriv iwspy /media/LABEL2/sbin

	cp libiw.so.29 /media/LABEL2/lib

	cd ..

9)	Unmount the sd card and insert it into Devkit8500.

	umount /media/LABEL1
	umount /media/LABEL2

10)	Boot Devkit8500

11)	After the ramdisk image is loaded, load the driver for the WiFi module

	insmod /lib/firmware/rt5370sta.ko

12)	Than perform the WiFi configuration as follows

	ifconfig ra0 up
	iwlist ra0 scan
	iwconfig ra0 mode managed
	iwconfig ra0 essid "<SSID WLAN>"

13)	If dhcp is configured run it else assign static IP

	ifconfig ra0 192.168.1.20 netmask 255.255.255.0
	route add default gw 192.168.1.1 ra0
	echo "nameserver 192.168.1.1" > /etc/resolv.conf

14)	Ping the google server

	ping google.com.pk


15)	If ping is successful, the Wifi is successfully configured.
