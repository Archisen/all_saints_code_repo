---
title: Sample Post
layout: post
post-image: "https://www.howtogeek.com/wp-content/uploads/2015/07/img_559f4011b3368.png"
description: A post outlining the goals and scope of my first IoT Project; a distributed sensor array.
tags:
- sample
- post
- test
---

My first project is to develop an IoT sensor array. I've toyed around with nrf24l01 RF modules for a little while and have different sensors laying around. Creating some sort of distributed sensor array will be a great way to look at the different tools on the my rasberry pi which from here on I'll call saint Pi I. In my previous post I set St. Pi up as an IoT server. If you'd like to follow along go back and [take a look][TAL1]. This post will cover the components, goal, and plan for the Project.

# Project scope
At a high level the system will be made to store, visualize, and use incoming data from a group of distributed sensors. These sensors will be attatched to non internet connected [mcu's][MCU1]. These mcu's will all comunicate to a central internet gateway using 2.4GHz RF messages. That gateway will then tranmit the data to the Pi using MQTT messages. From this point the IoT stack will take over. The MQTT messages will be routed through Nodered to InfluxDB and from there we'll send it Graphana to create some visualizations.

 I'll be using 3 Arduino nano's for the non internet connected mcu's. These 3 nano's will use nrf24l01 modules to talk to an esp32. The esp32 will act as the gatway between the arduino's and the web.  

[TAL1]: https://archisen.github.io/all_saints_code_repo/blog/rpi-iot-overview
[MCU1]: https://en.wikipedia.org/wiki/Microcontroller 
[E32_MQTT]: https://docs.espressif.com/projects/esp-idf/en/latest/esp32/api-reference/protocols/mqtt.html
[NRD_MQTT]:https://cookbook.nodered.org/mqtt/connect-to-broker