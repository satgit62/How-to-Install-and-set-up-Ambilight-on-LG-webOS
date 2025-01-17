# How-to-Install-and-set-up-Ambilight-on-LG-webOS

Installation instructions and settings for PicCap (hyperion-webos) and HyperHDR/Hyperion

The prerequisite for Ambilight is an LG TV device that has been successfully rooted and the Homebrew Channel installed. You can find out whether your device can be rooted at https://cani.rootmy.tv/ by entering your LG model type.

# Attention!
On newer TVs there is no official way for capturing DRM-protected content like from Netflix or Amazon. This restriction doesn't take place for content comming from an HDMI input.
So currently as a workaround you can play your media using your PC, Apple TV, FireTV-Stick or Chromecast and still enjoy your LEDs (Ambilight). On my old webOS 3.9 and 4.x it also works via internal apps.

After a successful rooting with one of the known methods (faultmanager-autoroot, DejaVuln or mvpd-autoroot) PicCap (hyperion-webos) and the mostly used HyperHDR or Hyperion, LEDs control/processing software from Homebrew Channel must be installed and configured. 

Note:
The settings for Hyperion.NG users are similar to HyperHDR, with the difference that there is no “HDR to SDR tone mapping” in Hyperion. To achieve some functions in contrast to HyperHDR, the setting level in Hyperion must be set to Expert.

# Homebrew Channel settings:
1. switch off unprotected and vulnerable Telnet.
2. switch on SSH. (Username for this = root and password = alpine) 
3. switch on Block System Updates
4. switch off failsafe mode 
5. reboot system to apply (perform reboot)
6. ![Homebrew Channel Settings](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/e7323b74-5cca-4db1-80a0-ae7ab149c2ef)

# PicCap

Install PicCap from Homebrew Channel and restart.

# Manual installation of apps without Homebrew Channel.

As an alternative to the direct installation from Homebrew Channels, the webOS Device Manager from GitHub can also be used for the installation. 
See: https://github.com/webosbrew/dev-manager-desktop
Important! Under Add Device Connection Mode, select Use SSH Server by Homebrew Channel. (IP address of your LG television, port = 22, Username for this = root and password = alpine) Username for this = root and password = alpine)
Not to be confused with the developer mode app.
Please do not install a developer mode app on a rooted device. 

![webOS Device Manager Connection Mode](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/38aac118-389d-4234-87d3-d2aad02f84b4)

Alternatively, you can install packages (.ipk) via SSH installation:

```
curl -k -L -o /tmp/app.ipk ‘<URL goes here>’ luna-send-pub -w 15000 -i ‘luna://com.webos.appInstallService/dev/install’ ‘{’id‘: “com.ares.defaultName”, “ipkUrl”:’/tmp/app.ipk‘, “subscribe”:true}’
```
  
After the restart, open PicCap and go directly to the Logs menu and wait until the service has been given root rights (Elevated Services).

![PicCap-log](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/a54c1768-5583-48b6-b6c6-29f92c5c54ec)

Usually, after a restart PicCap sets itself to the automatic default settings.
These are, among others, the IP address of HyperHDR/Hyperion, internal host is default 127.0.0.1 and port 19400, Hyperion priority 150, the resolution, maximum FPS, video capture backend, graphical capture backend as well as the autostart and VSync.

Note: Depending on the webOS version, the video and graphical backend is different and should be set correctly instead of automatically if required. See: https://github.com/webosbrew/hyperion-webos#hyperion-webos.

You have to test for yourself which resolution your device harmonises with PicCap.

# Attention! Some devices absolutely need 360x180.
 
The most common resolutions for PicCap are:

```
128x72
160x90
192x108
256x144
360x180
```
Note! If you have a device with low CPU/memory performance, I recommend using a lower resolution (256 × 144) at 30 fps instead of 60 fps and reducing the Hyperion priority from 150 to 100 if the device allows it. You can also disable the 'Graphical Capture Backend' to reduce delay and save device resources.

![PicCap Einstellungen](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/21a951b7-f75d-41fd-b496-15d1245bd35b)
If everything is configured correctly and the connection to HyperHDR/Hyperion is established, you will see the following in the bottom bar under “State: Getting status info. I Receiver:Connected” with the respective UI and video backend as well as the selected frame rate.

You can use the “top” command in Terminal/SSH to see what memory and CPU load “hyperion-webos” and “hyperhdr” are using.

![CPU and Memory load](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/e4c8876d-d774-4cd7-a252-7808227d90e0)

# Advanced Settings

Depending on which backend your TV model uses for recording, you must set the correct QUIRK option. See: https://github.com/webosbrew/hyperion-webos#quirks.

![PicCap advanced settings](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/20f1dc05-f15a-4c72-b1d4-6779967e5176)

By switching on the "QUIRK_DILE_VT_DUMP_LOCATION_2" options, the delay of the LEDs is also reduced to a minimum.

# NOTE:
For newer devices with the "QUIRK_DILE_VT_DUMP_LOCATION_2" option switched on (e.g. C3, G3 and newer), make sure that the Quick Media Switching option under Settings --> General --> External devices --> HDMI settings is switched off. Otherwise, the LEDs will be faster than the picture when the media is running via HDMI players. This was noted and reported by user @Meg_the_Face.

![Quick Media Switching1](https://github.com/user-attachments/assets/b47bcbcb-2582-4961-a535-7cf5ab83f0a2)

![Quick Media Switching](https://github.com/user-attachments/assets/0c7a3af1-cbe2-41cf-862b-32bf646ed917)


# HyperHDR/Hyperion.NG

HyperHDR is a fork of Hyperion. To realize Ambilight on LG TVs, you need either Hyperion.NG or HyperHDR in addition to PicCap. Both Hyperion versions are similar in structure and operation, and each has its own special functions. And as always, there are advantages and disadvantages. It also depends on which LED controller you want to use. If you want to use LED controllers such as HyperSerial, HyperSerialPico on RP2040 or HyperSerial with WLED on ESP32 and attach importance to HDR content, you should use HyperHDR. For LED controllers with WLED firmware on ESP32, you can use Hyperion.NG. Hyperion.NG has the advantage over Hyperion.HDR because it supports more controllers and can also control all Smart RGB luminaires/lamp registered in Home Assistant. Both solutions support most common RGB/W LEDs. However, since there are also exotic RGB lighting, you can find out more in advance on the GitHub page.

HyperHDR: https://github.com/awawa-dev/HyperHDR
Hyperion.NG: https://github.com/hyperion-project/hyperion.ng

Install and start HyperHDR or Hyperion.NG via the Homebrew Channel or using the webOS Device Manager. 
Switch on Autostart and start the daemon service. Reboot the TV.
Since HyperHDR is a fork of Hyperion and has a similar structure, I will use HyperHDR for the examples here. Please use either HyperHDR or Hyperion.NG.

Once the HyperHDR daemon service is successfully started, open a browser of your choice (Chrome, Mozilla Firefox or Microsoft Edge) on your PC, tablet or cell phone and access the web interface of HyperHDR or Hyperion.NG under the IP address of your TV with port 8090 (e.g. 192.168.xxx.x:8090) to make the necessary settings.

![HyperHDR Web Configuration](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/aabea194-8a85-43f9-9e59-b69270235baa)

HyperHDR LED Controller type settings for ESP32 controller with “WLED” firmware:

First go to LED hardware and make the settings under LED controller.

# Controller udpraw Protocol for WLED

You can select WLED under LED controller, but in conjunction with some ESP versions, the LEDs were not switched off when the TV was switched off. 
So I prefer here as controller type: udpraw, RGB byte order: RGB, update time 0, target IP: IP address of your ESP/WLED and port: 19446.

![udp raw](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/77f1c58c-c5c2-47c8-bb57-6eda23c6328a)

# Controller Type wled
The controller type: wled also has an autodiscover function when you set the first time.

![Controller type WLED](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/242735b1-4c10-454a-84c3-f0fcc2456088)

# HyperHDR LED controller type settings for RP2040-USB controller with “HyperSerialPico” or “HyperSerial” ESP32 Generic/S2 Mini firmware

First, go to LED Hardware and select “adalight” under “Controller Type”.
Select your RP2040 device under “Output path”. For example, ttyACM0. 
Select the option “High speed serial AWA protocol with data integrity check”, with a baud rate of, 2000000.
In addition, “Esp8266/ESP32/Rp2040 handshake” and “Force HyperSerial detection (ignore board ProductId/VendorId)” must be selected.

![adalight-controller](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/bc77629c-0a73-4f94-a4dd-d6dd4ab189e0)

It is only necessary to select the “White channel calibration (RGBW only)” option if you are using RGBW LEDs with 4 channels (SK6812RGBW).
The white channel of the “Neutral RGBW” LEDs does not come close to the color obtained by mixing RGB LEDs. It is slightly yellow, so you may need to reduce the blue/white component to boost the blue channel. “Cold RGBW” LEDs are usually better balanced. So on my SK6812RGBW neutral white, I reduced the “blue/white aspect” to 180 to boost the blue channel and create a reasonable white balance.

![White channel calibration RGBW only](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/710579ab-d440-46e4-ba85-b2f56dc3261b)

# Controller Type Home Assistant Hyperion.NG

RGB LED lamps/bulbs that are registered under Home Assistant can also be set as an instance here. To do this, a token must be created in Home Assistant and entered here. After saving, your homeassistant lamps are available here under Home. You can decide where the lamp is by using the LED position layout assistant.

![LED-Controller HA](https://github.com/user-attachments/assets/89099c4f-4595-4f9c-b0c2-c9c813a0cfc9)

![LED-Controller Home Assistant](https://github.com/user-attachments/assets/3d4ea236-f9d2-4666-a0d4-5ce34706da94)

https://github.com/satgit62/hyperion.ng-webos-loader

# Controller Type Home Assistant HyperHDR

![HyperHDR Home Assistant1](https://github.com/user-attachments/assets/d6da57e3-ec0c-444b-b46e-dea1f66d5799)

![HyperHDR-Home Assistant1](https://github.com/user-attachments/assets/d9e825bc-a10e-437f-a3cb-0eac2790a731)

For the Home Assistant settings, follow the instructions on GitHub: https://github.com/awawa-dev/HyperHDR/pull/1014

https://github.com/satgit62/hyperhdr-webos-loader/releases/tag/latest

# Controller Type Philips Hue 

Philips Hue configuration see also:

HyperHDR https://github.com/awawa-dev/HyperHDR/discussions/512 

Hyperion https://docs.hyperion-project.org/de/user/leddevices/network/philipshue.html

# Other LED Controllers

I would like to mention at this point that there are several ESP controllers with HyperSerial drivers from awawa-dev in GitHub, which can be installed on both ESP8266 and ESP32. For details and separate settings, please see:  https://github.com/awawa-dev/HyperSerialEsp8266 and https://github.com/awawa-dev/HyperSerialESP32/

Attention! There are also ESP32 with CH341 or CP210x which allow higher data rates via HyperSerial, but the kernel driver is not included in LG firmware.
So you have to install it on the device first, depending on the kernel architecture.
Download and instructions can be found at: https://github.com/throwaway96/webos-kernel-drivers

Kompatibler HyperSerialPico-Controller:
https://github.com/awawa-dev/HyperHDR/discussions/561
Kompatibler WLED-Controller:
https://kno.wled.ge/basics/compatible-controllers/



# LED Layout

In the second step, we go to LED layout.
Here you have to create the LED geometry of your TV, enter the exact number of LEDs top, bottom, left and right as well as the input position. (This is the first LED in configuration)
It is also possible to create a three-sided Ambilight, or a four-sided one with a foot gap. The layout is viewed from the front.
On my devices, I glued the LEDs from the front, bottom left and followed the clockwise direction so that the end of the LED stripe stopped in proximity to the beginning of the LED stripe. Thus, I could feed power to both the beginning and end of the LEDs with only a single AWG 18 silicone two-conductor wire from the power supply.

![HyperHDR Classic Layout](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/bcdc1c0f-71ff-4d22-b774-0a78e1f7ad29)


# Layout advanced Settings
The settings are important for:

* define how deep horizontally and vertically the image content is to be scanned/considered and transferred to the LEDs. (Horizontal/Vertical LED depth)

![Layout Advanced Settings](https://github.com/user-attachments/assets/8ad09184-1e4f-448b-a5e6-84ffb9b5f416)

  
* Specify whether there are gaps in the corners (Edge Gap/ Missing LEDs)

![Layout Advanced Settings Edge Gap ](https://github.com/user-attachments/assets/6c33b0e4-2a84-4c16-872f-fb4619e6ef07)


* Specify whether the LED strip overlaps in the corners (Overlap)

![Layout Advanced Settings Overlap](https://github.com/user-attachments/assets/050b0547-d7f2-49d6-92de-c43fafe2183a)


* Specify whether the LED strip has not been laid straight and parallel to each other. (Point Top Left, Point Top Right, Point Bottom Right and Point Bottom Left)

![Layout Advanced Settings Point](https://github.com/user-attachments/assets/7186b6c6-8b62-49a1-98fa-4e66c9e94e78)

The actual position of the LEDs is determined by these layout specifications. 

# Effects

In the next step, we turn to the menu effects (effects) and ensure that the boat effects and background effect remain switched off. Do not check the relevant boxes. Otherwise, you will have unwanted “flashing orgies” when starting the TV.

![Effects](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/af77184a-ac00-445f-9d36-f4e0a32322d9)

The next step is only important for HyperHDR users! (This function is not available in Hyperion.NG)

# HDR to SDR tone mapping

For the correct HDR global detection function, the “HDR to SDR tone mapping” option must be activated under Network Services, Flatbuffer Server.

![HDR to SDR tone mapping](https://github.com/user-attachments/assets/0fe30f83-9228-4f1a-abc3-6cb77efd288c)


# Image Processing

Under image processing, you have the choice between “Classic HyperHDR calibration” and not “Classic HyperHDR calibration”.
When using WLED, the “Classic HyperHDR calibration” is suitable, as the saturation can regulate the color intensity.

![HyperHDR Classic HyperHDR calibration](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/6aae5f47-fd2f-41cb-a5c2-bc2ed59f7f23)

When using HyperSerial/HyperSerialPico, the non-“Classic HyperHDR calibration” is suitable, as you can control the brightness here. Since HyperSerial driver comes with a balanced color saturation, the missing saturation control is negligible in this case.

![Color channel adjustments No Classic HyperHDR calibration](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/0f6f66a8-c78c-45bd-b52e-1643fb1a435e)

# Live Calibration

The color and gamma values depend on the LED stripe type used. The adjustment should be made under the “Remote Control” menu in the live calibration menu, so that the changes are noticed immediately. As the values set in Live Calibration are not permanent, you should enter the values in the Image Processing menu and save them permanently.

![HyperHDR Live calibration](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/d6600e06-f0db-4a9f-a406-355293bf1b20)

# Smoothing

To avoid flickering and unsteadiness in LEDs, the next step is to activate and save “smoothing” under Image processing.

![HyperHDR Smoothing](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/310818e6-f476-4820-b43e-f8ae7da18c51)

# LED Visualization:

A live image captured by PicCap must be visible in the HyperHDR LED visualization menu. No image is visible when DRM content is played via internal apps such as NETFLIX. 
If you want to watch DRM-protected content such as NETFLIX, Disney & Co., you must use an external HDMI player such as Apple TV or FireTV, as the DRM restrictions do not apply via the TV's HDMI inputs.

![HyperHDR LED Visualization](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/382bd569-6bc2-4a0c-9fdb-d3f6494911ad)

# Remote Control

Under HyperHDR/Hyperion remote control menu, you could monitor all processes and see whether data from PicCap is arriving at the HyperHDR “Flatbuffers” under source selection. Smoothing and black bar detection can also be switched on and off.
Under LED device, the LEDs can be switched off and on as required.

Note: the following only applies to HyperHDR.

There you can also switch HDR Global on or off if required. However, HyperHDR recognizes when a source provides HDR content and switches HDR Global on automatically and switches it off again for SD video sources.

The automatic activation/deactivation of the HDR Global switch, provided that “HDR to SDR tone mapping” is selected under Network Services, Flatbuffer, only applies when using a single LUT table. With multiple LUTs the tone mapping must always be on.

HyperHDR:
![HyperHDR Remote Control](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/cc5cc695-3846-4cf5-9b3a-c528c5860451)

Hyperion.NG:
![Remote Control Hyperion](https://github.com/user-attachments/assets/2d63ae73-8924-4fa2-948c-e17154808f03)

# Logs

You can view whether the LED controller has been recognized correctly under HyperHDR Logs. For the USB connection also under webOS Device Manager Debug, “dmesg”.

HyperserialPico Log:

![HyperSerialPico](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/a8f9e0ea-d95b-4426-9fde-e00385b33aac)

# LG USB Debug dmesg Log:

![webos Manager Debug dmesg](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/ecd21d64-1efe-4643-933c-59115530ee1b)

# WLED Log:

![WLED Logs](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/3c608c7f-685b-4dd5-8745-c70776dcb175)

# General Settings

You can manage the instances in the “LED hardware instance management” general settings menu. Use different LED hardware at the same time. Each instance runs independently of the other, which enables different LED layouts and calibration settings. This is important if, for example, you are using hardware from other manufacturers, such as Philips Hue lamps and Stripes. 

Under “Import/export configuration” you can save your HyperHDR settings and restore them as required.

The log level debug can be set under the “Logging” menu.

![General settings](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/3441e160-91d8-4275-b736-356acdd6b109)

# Important note when using WLED/ESP32 Controller

WLED has limited its automatic brightness limiter to 850mA in the LED standard setting for safety reasons.

If your power supply and cabling match the current requirements of your LED type and number, you must deactivate the automatic brightness limiter or adjust your power supply accordingly. Otherwise, your LEDs will have little or no luminosity.
For example: Your power supply delivers max 5V 10 Ampere so you can limit to 9000 mA.

![WLED Automatic brightness limiter](https://github.com/user-attachments/assets/8e696b56-7e30-4389-83bb-0137ee6d6650)

If you also want to have the maximum brightness when switching on, you must set the default brightness under LED settings. 0 = off, 255 = max. brightness.

![WLED default brightness](https://github.com/user-attachments/assets/54f2fe4c-bef1-4bea-9f33-24eaf25af5ef)

It is also important to make further settings depending on the LED type used, RGB or RGBW. For example, when using four-channel LEDs such as SK6812 RGBW instead of three-channel RGB LEDs such as the WS2812b. 
In the LED settings under “White management”--> “White Balance correction”, under “Calculate white channel automatically from RGB”, select “Dual” to actually use the white channel of the LEDs.

![WLED Withe Balance](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/e669d876-a8e2-4dec-9dc1-7675877c46ff)

# Important note if you are using the ESP32 WiFi Controller and the connection is interrupted

If the connection between WLED and router is interrupted, select under WiFi Settings --> Disable WiFi sleep to prevent the ESP from switching off its WiFi. In this case, the ESP32 can consume more power, but the connection remains active.

![WI-FI](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/e1578c44-f8a6-465a-921f-73cbe776966d)

# Quinled-dig-uno-v3-digital-led-controller

For those who prefer WLED firmware because of all the extras, but have had a bad experience over WiFi because of the long delay, then I recommend the ESP32 variant with built-in LAN connection. 
"QuinLED Dig Uno v3 DIGITAL LED controller", which is also available with LAN and acrylic housing. Or "ABC! WLED Controller Board V43 (5-24V)", "Ethernet Adapter for WLED Controller" and ‘Housing for WLED Boards’. The controllers are supplied with WLED.
Just google: 
“QuinLED Dig Uno v3 DIGITAL LED controller”, which is also available with LAN and acrylic housing. Or “ABC! WLED Controller Board V43 (5-24V)", ‘Ethernet Adapter for WLED Controller’ and ‘Housing for WLED Boards’. The controllers are supplied with WLED.

![quinled-dig-uno-v3-digital-led-controller](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/66fc94af-7ecf-4a95-aca9-0390492be823)
![UNO V3](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/4a7c293d-b0b8-4895-93e8-e56d52f9c32f)

![QuinLED Dig Uno v3 DIGITAL LED controller](https://github.com/user-attachments/assets/d4f27dc0-1a5d-48d1-ae05-02af5c73de28)

# ABC! WLED Controller Board+Ethernet_Adapter

![ABC! WLED Controller Board+Ethernet_Adapter](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/5badd444-1380-4cbf-84b7-a49c69877e37)

# ABC! WLED Controller ESP32:

![ABC! WLED Controller V41 ESP32](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/3d5f4588-11ac-408b-a0aa-060fcb1f2417)

# Cod.m Controller:
![cod m Controller](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/9005e953-ae30-4981-bed4-0fb36a019257)

# Level Shifter

For those who do not use a ready-made LED controller with a built-in level shifter, I strongly recommend integrating a level shifter into the circuit. Since most LEDs need to be supplied with 5 volts, but the logic of the controller can only handle 3.2 volts, the direct DATA line should be connected via a level shifter. For testing purposes, you can temporarily install a 470 Ohm resistor on the DATA line and a 1000µF electrolytic capacitor on the 5 Volt+ and GND at the input of the LEDs. 

![level shifter1](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/650c56c7-7c51-476a-a265-a1902e0c1f8b)

# Example Connection of various LED controllers

Install HyperSerialPico firmware on controller:
Unzip the firmware folder. (https://github.com/awawa-dev/HyperSerialPico/releases/) Take the appropriate file for your LED strip and transfer it to the controller in DFU mode.
 
1. Put your Pico board into DFU mode:

  If your Pico board has only one button (boot) then press & hold it and connect the board to the USB port. Then you can release the button.
  If your Pico board has two buttons, connect it to the USB port. Then press & hold boot and reset buttons, then release reset and next release boot button.

2. In the system file explorer you should find new drive (e.g. called RPI-RP2 drive) exposed by the Pico board. Drag & drop (or copy) the selected firmware to this drive. The Pico will reset automaticly after the upload and after few seconds it will be ready to use by HyperHDR as a serial port device using Adalight driver.

# Adafruit ItsyBitsy RP2040  mit Level Shifter using GPIO14(OUTPUT_DATA_PIN) on output 5.
For devices larger than 65 inch televisions, a third power injection for the LEDs in the center is required.

![Ambilight Adafruit ItsyBitsy](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/76f4c91f-6420-4157-9b0f-7efdc4396b12)

![ItsyBitsy RP2040](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/8a70a8f3-c196-4e63-9925-8fd1e48caf9c)

# PLASMA 2040 mit Level Shifter

If you use SK6812 with only one segment, you only need DATA. If you are using two segments, you must also use CLK. The firmware (HyperSerialPico) must be compiled correctly for this.

For ws281x/sk6812 LED strips:

Compile with set(OUTPUT_DATA_PIN 15) for data line if single LED segment is used.
Output is DA connector.
Compile with set(OUTPUT_DATA_PIN 14) for data line if two LED segments are used.
Output is: CL (first segment) and DA (second segment) connector.

For SPI LED strips, spi1 interface must be used:

set(OUTPUT_SPI_DATA_PIN 15)  
set(OUTPUT_SPI_CLOCK_PIN 14)  
set(SPI_INTERFACE spi1)

Output is: CL (clock) and DA (data) connector.

![Plasma2040](https://github.com/user-attachments/assets/1ea38359-e95c-468f-a58a-872f4550cc53)


# RP2040 Adalight Feather Scorpio mit Level Shifter Ausgang GPIO 16.
For devices larger than 65 inch televisions, a third power injection for the LEDs in the center is required.

![Adafruit Feather RP2040 Scorpio1](https://github.com/user-attachments/assets/e640cb4e-0a4d-4ca1-982f-61f3e77e1066)

![LED 4 Seitige Aufbau HyperSerialPico](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/70693991-ed28-4208-94d0-e0c9541de007)
![Adafruit Feather Scorpio Pinout](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/8d661a6d-1521-48a4-83e0-5c4e370307b1)

# ESP 8266 Wemos D1 Mini with level shifter:

![Level Shifter](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/179cb14f-6244-40e7-987e-8d7b2d589d50)

# ESP32 with level shifter:

![ESP32 S2 Mini Lolin](https://github.com/user-attachments/assets/ccad4b26-c770-44cd-92d3-ec945ca9e380)

![WLED+Netztteil und Level Shifter](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/591438fb-5e3d-4086-bf6a-c1018d293832)

# RP2040 LED-Controller:

Important! For the RP2040 controller with HyperSerialPico, please look for the correct DATA line output. Depending on the type, with built-in level shifter or without, there are different GPIO assignments for DATA.
The right place to go for the firmware is: https://github.com/awawa-dev/HyperSerialPico, https://github.com/awawa-dev/HyperSerialPico/releases and for the description of the compatible hardware and pin output is: https://github.com/awawa-dev/HyperHDR/discussions/561

# ESP32 Self-built WLED controller with LAN interface (WT32-ETH01) and level shifter:

If you prefer to buy and set up the LAN-ESP32 “WT32-ETH01” yourself, you should also bear in mind that a TTL-to-USB adapter is required for the software flashing. In addition, a level shifter should definitely be integrated to prevent the ESP from being destroyed or experiencing unwanted flashes or effects.
 
For the flash process, you must connect the TX from TTL to USB adapter with the RX0 from the ESP and the RX from TTL to USB adapter with the TX0 from the ESP, i.e. cross over. GND to GND. If the USB->TTL adapter offers a choice between 5V and 3.3 volts, then 5V should be selected. To start the flash with the WLED software, IO0 (next to RX0) must be connected to GND.
![wt32prog](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/60327c76-ad0b-4c93-ae6a-42e48f8b1f45)

![ESP32 LAN WTH0](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/b7c9fcc4-c95c-4eeb-bf31-84c83ac56626)

For the flash process with WLED, please use the online flash at: https://install.wled.me/
![WLED Installer](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/dae9a498-e149-4c33-89ca-ef9df9995ebc)

WLED Ethernet setup:
Go to "Config" and then to "WiFi Setup". At the bottom of this page select the Ethernet type you use. (WT32-ETH01) Then click on "Save & Connect".

![WiFi Setup](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/bc7f0104-ea8e-4884-b37a-d9cb8f218db5)


# Power Supply

Example of power supply units that can be used:
Switching power supply “MEAN WELL Well 1439455 LRS-75-5 AC power supply Module” (Please note that a 3-pole mains connection cable is also required here) or closed switching power supply “5V Power Supply 5 Volt 15A 75W Adapter 100V~240V AC to DC Converter 5 Vdc 15 Amp Power Transformer for LED Pixel Strip Light”
“ALITOVE Netzteil 5V 15A Universal Adapter 5 Volt Netzteil 75W” and the screw-on adapter to match “Female And Male DC Connectors 2.5×5.5 mm Power Plug Adapter Jacks Sockets Connector For Signal Color LED Strip CCTV Camera”

For a helpful calculation of the required power supply rating, cable cross-section, fuse and other parameters depending on the number and type of LEDs, you should definitely consult https://wled-calculator.github.io/.

![Power Calculater](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/0c84d678-f2cc-4fc1-b341-30941c44b030)

Important note!

# PicCap (webos-hyperion) update

After a few years, it was discovered that PicCap provides the wrong algorithm for the color calculation for HyperHDR and therefore the LED colors do not match the TV picture. 
The data is captured with FMT_ABGR, which is actually ARGB. ABGRToARGB, now the bytes are actually BGR, ARGBToRGB24, which in turn is changed to get RGB again.

Among other things, the new generation of devices was also taken into account and the so-called “libvtcapture_backend” and the older “libdile_vt_backend” were adapted.
Devices of the LX SoCs such as C1, C2, C3, G1, G2, G3 and A series.

Because the PicCap developer is currently not active, you have to make the changes manually and update the backends under the “hyperion-webos” yourself. 

Thanks to @sundermann for the new backend.

# Note:
To exchange/replace the backend files, you can use both the webOS Dev Manager and FileZilla. See pictures.

![webOS Dev Manager](https://github.com/user-attachments/assets/a9139a0c-3723-4a8c-94bd-7b3ce4166ea2)

![webOS Dev Manager Backends](https://github.com/user-attachments/assets/1bc2a1bb-cf7f-4f8e-93bd-87377376e734)

![FileZilla](https://github.com/user-attachments/assets/dc9135c6-178c-4c63-8ffc-d4490ae1dee1)

# Updated backends NV12 from @sundermann December 2024. 

Download:

https://github.com/webosbrew/hyperion-webos/actions/runs/12244511211/artifacts/2296363294

If you don't have a GitHub account, I've added the hyperion_webos_Release.zip file at the top of the main.  

Installation instructions:

1. call up PicCap under Apps and stop the service.
2. unzip the hyperion_webos_Release.zip and copy the content to `/media/developer/apps/usr/palm/services/org.webosbrew.piccap.service/` on the TV. (Replace the hyperion-webos and the backends in the folder)
3. restart the TV and run PicCap to get root. When this is done, you will see in the bottom right of the status bar whether the receiver is “connected” to the respective UI and video backends.

Now you may have to reset the color and gamma settings you made before, as the color calculation is now correct. Depending on the type of LEDs used, readjust if necessary.

You can check the actual hexadecimal color values in HyperHDR Live LED visualization with the browser, other tools, color pipette and check if they correspond to reality. Calculation from hexadecimal to decimal can be found in: https://www.farb-tabelle.de/de/farbtabelle.htm

The color correction I achieved with it can be seen in the “EBU Color bars results”. This corresponds to 99.9 percent of reality!

The file “EBU Color bars results” with the correct resolution Download: https://en.m.wikipedia.org/wiki/File:EBU_Colorbars_HD.svg#file

![EBU Color bars color results](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/d25d733a-4a07-44d3-b02a-26959ef32895)

![EBU_Colorbars_HD svg](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/794d28a5-1e95-450e-8ed0-39e9ca3b23ba)

![Farbpipette](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/27131ff3-4809-45d2-853f-c6b9dabf3dc9)

![Led-Visualisierung_Farbpipette](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/8307f8a9-f39c-4cdf-8920-27c7d10f273f)

# NOTE

For those who are always not satisfied with the colour rendering of their LEDs, the ultimate colour matching should be done using different LUT's for SDR, HDR and Dolby Vision.
See: https://github.com/satgit62/Ultimate-HyperHDR-Ambilight-fine-tuning-experience-for-LG-webOS-with-new-LUT-calibration-

























































