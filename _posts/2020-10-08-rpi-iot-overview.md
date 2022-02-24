---
title: Raspberry Pi IoT Server
layout: post
post-image: "https://www.distek.com/blog/wp-content/uploads/2016/10/raspberry-pi-logo.png"
description: An explaination of my proccess for setting up a raspberry pi based IoT server. 
tags:
- IoT
- Raspberry Pi
- Docker
- Graphana
- Andreas Spiess
---


This post will act as an overview of how I set up my raspberry pi IoT server. I am basing this project off of Andreas Spiess' youtube channel [video #295][AP video 295]. In future posts I wil actually be using and exploring each of the server components. This post is just meant to show how to get the server up and running. As this is one of my first blog posts on this page it'll be fun to try and develop a wrtiting while developing style. 

# Project  Guide
As stated above this project will be following [video #295][AP video 295] of Andreas Spiess' YouTube channel looking at the using the raspberry pi as an IoT server using Docker, a Mosquito, nodeRed, InfluxDB, and Grafana. The order of this post will be as follows:
1. Raspberry Pi setup and prerequisites
2. IoT Stack Setup
3. Close

Now that we have a map of where where going let's jump into the setup. I'm excited to see where this goes. 

# Raspberry Pi setup and prerequisites
For this project I'll be using a Raspberry Pi 4 I bought back in april. To setup my pi as an IoT server there are a few changes I made that Andreas doesn't go over in [video #295][AP video 295]. First instead of booting raspbian from an SD card I wanted to use a usb connected SSD or flash drive. To do this I followed a guide by *Chris Barnatt* with *Explaining Computers* titled [Raspberry Pi 4 8GB & USB Boot][usb boot EC]. At minute 12:57 he explains the proccess for updating the firmware on the pi to allow for usb boot. When the video was made the firmware was still in beta, however as of writing this firmware allowing for usb boot is apart of the official release. if youre buying a raspberry pi after this wrtiting it's likely that the firmware already supports usb booting. 

### Booting From SSD
Before going through the proccess of updating the firmware lets check to see if the Pi already has the correct firmware loaded. To do this type:
```
vcgencmd bootloader_version
```

If the release date is any time after june 2020 you can skip the next part.

To set up my Pi do boot from my SSD I used a [120GB Kexin SSD][Kexin SSD], this solution will also work with a flash drive. I first loaded raspbian onto an SD card and booted my Pi. Once my Pi was setup I opened the terminal and entered the commands:

```
sudo apt update
sudo apt full-upgrade
```

From there I opened the file explorer and navigated to the desired firmware. I found this file at:
```
/lib/firmware/raspberrypi/bootloader/stable
```

I would suggest selecting thew most current version. Once you've found the file you'll be using return to the console and enter:
```
sudo rpi-eeprom-update -d -f [/path_to_file/ file]
``` 

once this opperation is complete I did a `sudo reboot`. Once the reboot finished I checked the firmware version to make sure that the change went through. When the change completed it was time to copy raspbian onto the SSD. To do this I pluged the SSD into the Pi. I went to the start menu and selected accessories -> SD Card Copier. I selected my SD card as the copy from device, my SSD as the copy to device, and pressed start the opperation. Once the opperation completed it was finished. I shut down the Pi and removed the SD card.  

### Setting a static IP
My next step was setting the Pi up with a static IP. To do this I followed a guide from Nemeen Shah's website titled [How to Assign Static IP Address to Raspberry Pi][static IP tutorial]. The guide is pretty proccess is very straight forward. 

#### Step 1: Find current IP & gateway

to find current IP I returned to the terminal and typed: 
```
ifconfig
```

To find the network gateway I typed:
```
sudo route -n
```

#### Step 2: Set static Ip & gateway
Once I knew my IP and Gateway I was ready to set my static IP & gateway. To do so I had to edit *dhcpcd.conf*. To do so I typed:
```
sudo nano /etc/dhcpcd.conf
```

Once in the file I had to type the following lines:
```
interface wlan0
static ip_address=xxx.xxx.x.xx  //this is your static IP address
static routers=xxx.xxx.x.x  //this is your gateway IP address
static domain_name_servers= 8.8.8.8 8.8.4.4 //Googles dns
```

In the last line you don't have to use Google's DNS, you can choose any that suites your needs. 

My raspberry Pi is now setup to be an effective platform to build my IoT stack off of. Now I can jump into seting up the IoT stack.

# IoT Stack Setup

To set up IoT stack I followed the Instructions found on the [IoT Stack wiki][ISW]. Again this wiki was pretty straight forward and easy to follow. Bellow I have listed the ports used to connect to graphana, node-red, & portainer. 


| Container | Port |
|-----------|------|
|  Nodered  | 1880 |
| Portainer | 9000 |
| Graphana  | 3000 |
| Mosquitto | 1883 |

To connect to each container go into your web browser & type:

```
https://<Your IP>:<container-Port>
```

If each container's page shows up you should be setup for the most part. Lastly, however, we must check influxDB. to do so we must return to the terminal and type:
```
Docker exec -it influxdb /bin/bash
```
This will open up a docker prompt. You can use this prompt to execute different docker commands. In our case we'll want to want to enter our influx container and take a look at the databases we have which can be done simply by typing.
```
influx
show databases
```
# Close

With that we've set up our raspberry pi as an IoT server. from here all thats left to do is think of a project to take on to utilize these different tools.

[AP video 295]:https://www.youtube.com/watch?v=a6mjt8tWUws&t=77s
[usb boot EC]:https://www.youtube.com/watch?v=2zrwjGcyM5s 
[Kexin SSD]:https://www.amazon.com/KEXIN-120GB-Portable-External-SSD/dp/B08D2ZC4T1/ref=sr_1_2?dchild=1&keywords=kexin+ssd&qid=1602357576&refinements=p_n_feature_three_browse-bin%3A14027457011&rnid=6797515011&s=pc&sr=1-2 
[static IP tutorial]: https://nematicslab.com/how-to-assign-static-ip-address-to-raspberry-pi/
[ISW]: https://sensorsiot.github.io/IOTstack/Getting-Started/




