# SecKC ESP8266 Captive Portal and Hacking Challenge

The purpose of this project was to create a small and cheap device that helps market and promote SecKC at industry events. The original concept was a device running a form of Wifi Karma, that would respond to all wifi requests and then once connected would serve a "Guerrilla Marketing" page about SecKC. In its first iteration I used a RaspberryPi Zero, running a series of scripts. However the price of the components needed made this a difficult project to "give to the people".  That is where the ESP8266 Came in. For about $5, anyone can make and deploy this device.

[![ESP8266 D1 Mini](http://i.imgur.com/MSpAXfE.jpg)](https://www.youtube.com/watch?v=ZpcRZoXQAzM)

* User connects to the device broadcasting an SSID of "Conference WiFi" (this is configurable of course)
* The captive portal helper on their phone/table/computer kicks in and presents them with a page showing a fancy SecKC logo, and beeps the ESP8266 letting the owner know that someone connected.
![](./imgs/ESP-CaptivePortal.gif)



* There is a hidden hacking game embedded. If you view source of the http://1.3.3.7/index.htm page, it will give a few clues on how to begin.

## Parts List
* [ESP8266 Module with 4MB of flash](I used the WeMos D1 Mini) (https://www.aliexpress.com/item/D1-mini-Mini-NodeMcu-4M-bytes-Lua-WIFI-Internet-of-Things-development-board-based-ESP8266-by/32642350275.html?spm=a2g0s.9042311.0.0.3Ae9zT)
* [TP4056 USB Charger Module](https://www.aliexpress.com/item/20Pcs-TP4056-5V-1A-Micro-USB-Charger-Module-18650-Lithium-Battery-Charging-Board/32776109454.html?spm=a2g0s.9042311.0.0.3Ae9zT)
*  [3.7V 500mAh 503035 Lithium Polymer LiPo Rechargeable Battery](https://www.aliexpress.com/item/Wholesale-100-pcs-3-7V-500mAh-503035-Lithium-Polymer-LiPo-Rechargeable-Battery-For-Mp3-Mp4-PAD/32734113304.html?spm=a2g0s.9042311.0.0.3Ae9zT)
* [Piezo buzzer](https://www.aliexpress.com/item/Black-Plastic-14-x-7mm-2-Pins-Passive-Piezo-Buzzer-AC1-3V-Piezo-Transducer-with-Flying/32723376720.html?ws_ab_test=searchweb0_0,searchweb201602_3_10152_10065_10151_10068_10130_5430020_10307_10137_10060_10155_10154_5370011_10056_10055_10054_10059_100031_10099_5400020_5410011_10103_10102_10052_10053_10142_10107_10050_10051_5380020_10084_10083_10080_10082_10081_10178_10110_10111_10112_5390011_10113_10114_10312_10313_10314_10315_10316_10078_10079_10073_5420011,searchweb201603_16,ppcSwitch_4&btsid=21893a7d-1b2f-4904-a0fc-4871ead676b0&algo_expid=310ddc03-9d74-4cf2-8e97-9e677fbd6706-1&algo_pvid=310ddc03-9d74-4cf2-8e97-9e677fbd6706)


## Wiring the Piezo
Connect the '+' lead of the piezo to GPIO 4 (D2 on WeMos D1 Mini) and '-' lead to Ground.
I chose GPIO 4 because I installed the long leads with the headers on the WeMos D1 Mini. The spacing from ground was perfect to just plug the buzzer in direct between G & D2.

<img src="http://i.imgur.com/f8IIkPY.jpg" width="200"><img src="http://i.imgur.com/heRqjkN.jpg" alt="Piezo Solder top" width="200"/>


## Build Firmware
I use PlatformIO to build this.  http://platformio.org/

* Install Atom editor and PlatformIO and let it update itself on first startup
* Once updated and restarted, clone the project to a folder and open the project in PlatformIO
* Next build the firmware by clicking the checkmark icon on the toolbar

![Build Firmware](https://s20.postimg.org/e9mna84pp/build_firmware.png)

## Upload Firmware and SPIFFS data
After your firmware build is successful you can upload it by clicking the arrow under the checkmark in the PlatformIO toolbar.

![Upload Firmware](https://s20.postimg.org/ue4gppiot/upload_firmware.png)

You can add/edit the files in the "www" folder to your liking. (Files in the "www" folder will be cloned and gzipped to the "data" folder when building.) Then follow the instructions below to build and upload the SPIFFS file system image to your ESP8266.

![Upload SPIFFS 1](https://s20.postimg.org/p1ymo5v4t/build_spiffs.png)

![Upload SPIFFS 2](https://s20.postimg.org/vrw3l0hy5/image.png)

**Note: Anytime you make changes to the firmware or the data you can rebuild and upload either without the need to install the other again. They reside in different areas of the flash memory.**

## Game Completion:
If you complete the game, you will get access to the SecKC-ESP8266 Console. ".
From here you can monitor all connections and see when someone connects to the device.
The console also allows you to enter commands to get more info about the status of the device, and various other 'things'.

The "debug" setting is on by default and shows you all DNS and HTTP requests made to the device. It's kind of cool to see all the sites that the apps on your phone are trying to access. Check out the screenshots of the console for a glimpse of what it looks like. Some apps are very noisy. You can toggle the "debug" off to not see that stuff.

## References
The SecKC-ESP8266 Project started from the RickRoll Project (https://github.com/idolpx/mobile-rr) and modified code to fit our needs.  That project referenced the projects below:

- PlatformIO - http://platformio.org/
-              http://docs.platformio.org/en/latest/platforms/espressif.html#uploading-files-to-file-system-spiffs
- ESP8266 Captive Portal - https://www.hackster.io/rayburne/esp8266-captive-portal-5798ff
- ESP-RADIO - https://github.com/Edzelf/Esp-radio
- ESPAsyncWebServer - https://github.com/me-no-dev/ESPAsyncWebServer
- SOFTAP - Get List of Connected Clients - http://www.esp8266.com/viewtopic.php?f=32&t=5669
- How to print free memory - http://forum.wemos.cc/topic/175/how-to-print-free-memory
- WebSocketToSerial - https://github.com/hallard/WebSocketToSerial.git
- JQueryTerminal - https://github.com/jcubic/jquery.terminal
- OTA Update - http://www.penninkhof.com/2015/12/1610-over-the-air-esp8266-programming-using-platformio/
- Piezo Beep - http://framistats.com/2015/06/07/esp8266-arduino-smtp-server-part-2/
- Simple Audio Board - http://bitcows.com/?p=19
- Tone Doesn't Work - https://www.reddit.com/r/esp8266/comments/46kw38/esp8266_calling_tone_function_does_not_work/
- ArduinoJson - https://github.com/bblanchon/ArduinoJson
- EEPROM - https://gist.github.com/dogrocker/f998dde4dbac923c47c1
- Exception Causes - https://github.com/esp8266/Arduino/blob/master/doc/exception_causes.md
- WiFi Scan- https://www.linuxpinguin.de/project/wifiscanner/
- SPIFFS - https://github.com/esp8266/Arduino/blob/master/doc/filesystem.md
-          http://blog.squix.org/2015/08/esp8266arduino-playing-around-with.html
- WiFiManager - https://github.com/tzapu/WiFiManager
- ESP-GDBStub - https://github.com/esp8266/Arduino/tree/master/libraries/GDBStub
