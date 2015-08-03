<center><img width=300 src="images/pirateship.png"></center>

# Pirateship for Pi
A Raspberry Pi image for making deployment of Ground Servers a breeze in your sails. 

Download the latest Pirateship disk image for Raspberry Pi [[zip](http://pirate.sh/latest-pirateship.img.zip)] [[gz](http://pirate.sh/latest-pirateship.img.gz)]

To install, download the latest image from the link above and then follow the directions to burn that disk image onto an SD Card. 
- [Windows](https://www.raspberrypi.org/documentation/installation/installing-images/windows.md)
- [Mac](https://www.raspberrypi.org/documentation/installation/installing-images/mac.md)
- [Linux](https://www.raspberrypi.org/documentation/installation/installing-images/linux.md)

## Features
### Run code on boot off of a USB Drive with `autorun.sh`
Have some code you want to run on boot without having to mess with init.d/systemd/etc? Create a file named `autorun.sh` on a USB Drive, plug it into your Pi running Pirateship for Pi, and you're good to go.

### Run code on boot ONCE
Maybe you have some dependencies you want downloaded/installed before your `autorun.sh` starts running. Place that code in `autorunonce.sh`. When Pirateship for Pi finishes running that code, it will move that code to `autoranonce.sh` so that it doesn't run on the next boot by accident. 

### Connect to WiFi 
To connect a Pi running Pirateship to a WiFi network, make a text file named `autorunonce.sh` with the following snippet in it, edit it by adding your own wifi name and password, put the file on a USB Drive, plug the USB drive into your Pi running Pirateship, and then power on your Pi. 
```
pirateship default
pirateship adapter <wifi name> <wifi password> WPA
ifdown wlan0
ifup wlan0
```

... or if you don't have a WiFi password...

```
pirateship default
pirateship adapter <wifi name> "" none
ifdown wlan0
ifup wlan0
```

That's all you need to do to reset your connection, configure for your network, and then cycle the WiFi dongle. From there on out those settings will be saved and the connection will be reestablished between boots. 
