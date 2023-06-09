---
title: "MicroPython"
date: 2022-01-15
draft: false
banner: 'tech-note.jpg'
summary: "Can we program microcontrollers using python? Let us learn how to do it."
math: false
categories: ['beginner']
tags: ['programming','python','ESP32']
---

Micropython is a simple interpreter written is C which can be run on tiny microcontroller boards. This enables us to write python code directly for the microcontroller. Infact we even get a python prompt (called REPL) where we can type commands directly. This makes it a very easy platform to learn programming microcontrollers. Though micropython runs on different platforms, let us just focus on ESP8266 (which is supported extensively) and ESP32.

## Setup

The setup is relatively easy. The first step is to get the python tool called esptool which is used to flash the micropython binary to the ESP chip.

{{< highlight go  >}}
pip install esptool 
{{< / highlight >}}

If micropython is flashed the first time on the device, it is recommended to first erase the flash. This is down by

{{< highlight go  >}}
esptool.py --port <port> erase_flash
{{< / highlight >}}

Note the port should be whatever port the device mounts on. Typically its `/dev/ttyUSB0` for Linux and `/dev/tty.wchusbserial1410` or something similar for Mac. On Windows it is `COM1` or similar.

Next, go to the micropython [downloads](http://micropython.org/download) page and download the image corresponding to the board. In our case it is either firmware for ESP8266 or ESP32.

To flash this code to the ESP board use the following command

{{< highlight go >}}
esptool.py --port <port> --baud 460800 write_flash --flash_size=detect 0 esp8266-2016-05-03-v1.8.bin
{{< / highlight >}}

If you are flashing to ESP32 boards using the following instead*

{{< highlight go >}}
esptool.py --port <port> --baud 460800 write_flash --flash_size=detect 0x1000 esp32-idf4-20191220-v1.12.bin
{{< / highlight >}}

*we just changed the starting address.

## Testing
The easiest way to get to the python prompt is to login via the serial port using the following command on a Mac

{{< highlight go  >}}
screen /dev/tty.wchusbserial1410 115200
{{< / highlight >}}

However we can also login using wifi. On the ESP8266 builds the wifi access point is setup automatically and shows up as `MicroPython-xxxxxx` where the x’s are replaced with part of the MAC address of the device. The wifi password is `micropythoN`.

Before logging in we need to first setup the webrepl which is done running the following command via serial.

{{< highlight go  >}}
import webrepl_setup
{{< / highlight >}}

Next, follow this link http://micropython.org/webrepl/ on your browser. This loads the webrepl interface. Connect to the wifi access point (which is something similar to `MicroPython-xxxxxx`). Now press the Connect button. This should connect to the device over wifi and give you a python prompt.


