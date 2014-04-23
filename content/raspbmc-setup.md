Title: RaspBMC setup
Date: 2010-11-03 10:20
Category: diy, tutorial
Tags: raspberrypi, diy, tutorial
Author: David Maranhao
Summary: Setting up a Raspberry Pi powered home media center with RaspBMC

~

### Installing raspbmc



### Setting up wifi

I've finally gotten around to purchasing a wireless adapter for my raspberry pi and it turned out that set up wasn't as bad as I expected.  For about $6 on eBay, I got a generic 802.11n/g/b USB Wifi adapter (uses RalinkTBD chipset)

(image goes here)

Before buying an adapter make sure it's verified to work with the raspberry pi [here](http://elinux.org/RPi_VerifiedPeripherals#USB_WiFi_Adapters "Optional Title")

	$ ls date+%h | wc -l


Install usb adapter and boot raspbmc

Start an SSH session with the raspberry pi

	$ ssh pi@192.168.2.211
	$ pi@192.168.2.211's password: raspberry

Check that your adapter was detected using the lsub command

	pi@raspbmc:~$ lsusb
	
	Bus 001 Device 002: ID 0424:9512 Standard Microsystems Corp. 
	Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
	Bus 001 Device 003: ID 0424:ec00 Standard Microsystems Corp. 
	Bus 001 Device 004: ID 148f:5370 Ralink Technology, Corp. RT5370 Wireless Adapter
	Bus 001 Device 005: ID 046d:c316 Logitech, Inc. HID-Compliant Keyboard

The firmware-ralink package is needed and can be installed through apt-get

	pi@raspbmc:~$ sudo apt-get update
	pi@raspbmc:~$ sudo apt-get install firmware-ralink

Confirm wlan0 exists

	pi@raspbmc:~$ ifconfig


### Remote control
#### Android XBMC remote
The [Official XBMC Remote by Team-XBMC](http://play.google.com/store/apps/details?id=org.xbmc.android.remote&hl=en) is an android app that turns your phone into full feature remote control for XBMC.  Super useful, easy to set up and free.. this app is great! 

##### Here's how to set things up
* Download and install [Official XBMC Remote by Team-XBMC](http://play.google.com/store/apps/details?id=org.xbmc.android.remote&hl=en)
* Start the app on your android device and enter Settings->Manage XBMC Hosts
* Add host
	Name of this instance:
	Host or IP address:
	HTTP API port:
	Username:
	Password:
	EventServer port:
	

#### IR remote
TBD





https://play.google.com/store/apps/details?id=org.leetzone.android.yatsewidgetfree&feature=related_apps
https://play.google.com/store/apps/details?id=ch.berard.xbmcremotebeta

