<center><img width=300 src="images/pirateship.png"></center>

# Pirateship for Pi
A Raspberry Pi image for making deployment of Ground Servers a breeze (in your sails). 

## Installation

__Step 1__

First you will need the software that we'll place on the SD Card for your Raspberry Pi. [Click here to download the latest Pirateship disk image for Raspberry Pi ](http://pirate.sh/latest-pirateship.img.zip).

__Step 2__

Now that you have the software, it's time to burn that software to an SD Card that you will later put in your Raspberry Pi. What you downloaded is referred to as a "Disk Image" or some times just referred to as an "Image". Disk images are entire hard drives in just one file. Here are some general instructions for burning a Disk Image to an SD Cards. 
- [Burn a Disk Image to an SD Card using Microsoft Windows](https://www.raspberrypi.org/documentation/installation/installing-images/windows.md)
- [Burn a Disk Image to an SD Card using a Mac](https://www.raspberrypi.org/documentation/installation/installing-images/mac.md)
- [Burn a Disk Image to an SD Card using a Linux Machine](https://www.raspberrypi.org/documentation/installation/installing-images/linux.md)

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

### Debugging
Is something in your `autorun.sh` not working as expected? Then it's time to dive in on the command line. We suggest connecting computers to Pis over Console Cables as opposed to finding the Pi on the network. [Adafruit has an excellent tutorial on this](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-5-using-a-console-cable/overview).  


Now that you're logged into your Pi, `sudo su` to become the root user, and run `screen -x` to enter the screen session where `autorun.sh` is executing. There you will see the output of the process. Inspect the output for any error messages. If you are building a pipe with pull and push commands, try running the pull and push commands seperately to confirm that both work. For example...

```
$ ./my-sensor-driver/pull
42
$ echo '42' | ./my-database-driver/push
You pushed the value 42 to your database
```

If your database driver is pushing data to the Internet, check to make sure the Pi is connected to the Internet.  The first command to try is `ping google.com`. If you see responses from Google then you are all set. If you see timeouts however, run `ifconfig` to see the current configuration of your network cards. If you are connected over a hardwire ethernet, pay attention to the `eth0` adapter. If you are connected over WiFi USB, pay attention to the `wlan0` adapter. What you're looking for is if your network card has been given an IP Address on the network it should be connected to. The IP Address will look something like `192.168.0.104` but may be different depending on your network configuration. If you don't have an IP Address, then the Router your Pi is connected to may be down or there may be some invalid network settings on your Pi. Try cycling the network card by running `ifdown <adapter name>; ifup <adapter name>;`. If your network settings are invalid you should see an error message, if the Router is not responding you should see a long wait.  
