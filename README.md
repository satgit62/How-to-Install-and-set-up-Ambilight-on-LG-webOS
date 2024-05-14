# How-to-Install-and-set-up-Ambilight-on-LG-webOS

Installation instructions and settings for PicCap (hyperion-webos) and HyperHDR/Hyperion

Prerequisite for Ambilight is an LG TV device that is successfully rooted and installed Homebrew Channel. https://gist.github.com/throwaway96/e811b0f7cc2a705a5a476a8dfa45e09f

Attention! PicCap is not able to record DRM-protected video content from TVs' own apps. However, the DRM restrictions do not affect the TV's HDMI inputs, so NETFLIX & Co. can be processed and enjoyed with Ambilight.


After a successful rooting with one of the known methods (Enabling debug on LG webOS by modifying NVM, RootMyTV, crashd, or DejaVuln) PicCap (hyperion-webos) and the mostly used Hyperion or HyperHDR, LEDs control/processing software from Homebrew Channel must be installed and configured. 

First, you should pay attention to the following points in Homebrew Channel: 
1. switch off unprotected and vulnerable Telnet.
2. switch on SSH. (Username for this = root and password = alpine) 
3. switch on Block System Updates
4. switch off failsafe mode 
5. reboot system to apply (perform reboot)
6. ![Homebrew Channel Settings](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/e7323b74-5cca-4db1-80a0-ae7ab149c2ef)

PicCap:

Install PicCap from Homebrew Channel and restart.
As an alternative to the direct installation from Homebrew Channels, the webOS Device Manager from GitHub can also be used for the installation. 
See: https://github.com/webosbrew/dev-manager-desktop
Important! Under Add Device Connection Mode, select Use SSH Server by Homebrew Channel. (Username for this = root and password = alpine)

![webOS Device Manager Connection Mode](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/38aac118-389d-4234-87d3-d2aad02f84b4)

After the restart, open PicCap and go directly to the Logs menu and wait until the service has been given root rights (Elevated Services).

![PicCap-log](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/a54c1768-5583-48b6-b6c6-29f92c5c54ec)

Usually, after a restart PicCap sets itself to the automatic default settings.
These are, among others, the IP address of HyperHDR/Hyperion, internal host is default 127.0.0.1 and port 19400, Hyperion priority 150, the resolution, maximum FPS, video capture backend, graphical capture backend as well as the autostart and VSync.
Note: Depending on the webOS version, the video and graphical backend is different and should be set correctly instead of automatically if required. See: https://github.com/webosbrew/hyperion-webos#hyperion-webos.
Remark! If you own a device with a weak CPU/memory, I recommend choosing a smaller resolution (256 × 144) and reducing the Hyperion priority from 150 to 100. You can also do without the “Graphical capture backend” to reduce the delay and save the device resources.

![PicCap Einstellungen](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/21a951b7-f75d-41fd-b496-15d1245bd35b)
If everything is configured correctly and the connection to HyperHDR/Hyperion is established, you will see the following in the bottom bar under “State: Getting status info. I Receiver:Connected” with the respective UI and video backend as well as the selected frame rate.

You can use the “top” command in Terminal/SSH to see what memory and CPU load “hyperion-webos” and “hyperhdr” are using.

![CPU and Memory load](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/e4c8876d-d774-4cd7-a252-7808227d90e0)

Under Advanced Settings, you may need to set the correct QUIRK. See: https://github.com/webosbrew/hyperion-webos#quirks.

![PicCap advanced settings](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/20f1dc05-f15a-4c72-b1d4-6779967e5176)

HyperHDR:

Install and start HyperHDR or Hyperion.NG via the Homebrew Channel or using the webOS Device Manager. 
Switch on Autostart and start the daemon service. Reboot the TV.
Since HyperHDR is a fork of Hyperion and has a similar structure, I will use HyperHDR for the examples here. Please use either HyperHDR or Hyperion.NG.

Once the HyperHDR daemon service is successfully started, open a browser of your choice (Chrome, Mozilla Firefox or Microsoft Edge) on your PC, tablet or cell phone and access the web interface of HyperHDR or Hyperion.NG under the IP address of your TV with port 8090 (e.g. 192.168.xxx.x:8090) to make the necessary settings.

![HyperHDR Web Configuration](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/aabea194-8a85-43f9-9e59-b69270235baa)

HyperHDR LED Controller type settings for ESP32 controller with “WLED” firmware:

First go to LED hardware and make the settings under LED controller. 
You can choose WLED under LED-controller, but some bugs speak against it.
So I prefer here as controller type: udpraw, RGB byte order: RGB, update time 0, target IP: IP address of your ESP/WLED and port: 19446.

![udp raw](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/7cdaa010-40e2-4185-a8a1-2ce7e547bd6d)

HyperHDR LED Controller type settings for RP2040-USB controller with “HyperSerialPico” firmware:

First, go to LED Hardware and select “adalight” under “Controller Type”.
Select your RP2040 device under “Output path”. For example, ttyACM0. 
Select the option “High speed serial AWA protocol with data integrity check”, with a baud rate of, 2000000.
In addition, “Esp8266/ESP32/Rp2040 handshake” and “Force HyperSerial detection (ignore board ProductId/VendorId)” must be selected.

![adalight-controller](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/bc77629c-0a73-4f94-a4dd-d6dd4ab189e0)

It is only necessary to select the “White channel calibration (RGBW only)” option if you are using RGBW LEDs with 4 channels (SK6812RGBW).
The white channel of the “Neutral RGBW” LEDs does not come close to the color obtained by mixing RGB LEDs. It is slightly yellow, so you may need to reduce the blue/white component to boost the blue channel. “Cold RGBW” LEDs are usually better balanced. So on my SK6812RGBW neutral white, I reduced the “blue/white aspect” to 180 to boost the blue channel and create a reasonable white balance.

![White channel calibration RGBW only](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/710579ab-d440-46e4-ba85-b2f56dc3261b)

Other LED controllers:

I would like to mention at this point that there are several ESP controllers with HyperSerial drivers from awawa-dev in GitHub, which can be installed on both ESP8266 and ESP32. For details and separate settings, please see:  https://github.com/awawa-dev/HyperSerialEsp8266 and https://github.com/awawa-dev/HyperSerialESP32/

LED layout:

In the second step, we go to LED layout.
Here you have to create the LED geometry of your TV, enter the exact number of LEDs top, bottom, left and right as well as the input position. (This is the first LED in configuration)
It is also possible to create a three-sided Ambilight, or a four-sided one with a foot gap. The layout is viewed from the front.
On my devices, I glued the LEDs from the front, bottom left and followed the clockwise direction so that the end of the LED stripe stopped in proximity to the beginning of the LED stripe. Thus, I could feed power to both the beginning and end of the LEDs with only a single AWG 18 silicone two-conductor wire from the power supply.

![HyperHDR Classic Layout](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/bcdc1c0f-71ff-4d22-b774-0a78e1f7ad29)

Effects:

In the next step, we turn to the menu effects (effects) and ensure that the boat effects and background effect remain switched off. Do not check the relevant boxes. Otherwise, you will have unwanted “flashing orgies” when starting the TV.

![Effects](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/af77184a-ac00-445f-9d36-f4e0a32322d9)

Image processing:

Under image processing, you have the choice between “Classic HyperHDR calibration” and not “Classic HyperHDR calibration”.
When using WLED, the “Classic HyperHDR calibration” is suitable, as the saturation can regulate the color intensity.

![HyperHDR Classic HyperHDR calibration](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/6aae5f47-fd2f-41cb-a5c2-bc2ed59f7f23)

When using HyperSerial/HyperSerialPico, the non-“Classic HyperHDR calibration” is suitable, as you can control the brightness here. Since HyperSerial driver comes with a balanced color saturation, the missing saturation control is negligible in this case.

![Color channel adjustments No Classic HyperHDR calibration](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/0f6f66a8-c78c-45bd-b52e-1643fb1a435e)

Live Calibration:

The color and gamma values depend on the LED stripe type used. The adjustment should be made under the “Remote Control” menu in the live calibration menu, so that the changes are noticed immediately. As the values set in Live Calibration are not permanent, you should enter the values in the Image Processing menu and save them permanently.

![HyperHDR Live calibration](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/d6600e06-f0db-4a9f-a406-355293bf1b20)

Smoothing:

To avoid flickering and unsteadiness in LEDs, the next step is to activate and save “smoothing” under Image processing.

![HyperHDR Smoothing](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/310818e6-f476-4820-b43e-f8ae7da18c51)

LED visualization:

A live image captured by PicCap must be visible in the HyperHDR LED visualization menu. No image is visible when DRM content is played via internal apps such as NETFLIX. 
If you want to watch DRM-protected content such as NETFLIX, Disney & Co., you must use an external HDMI player such as Apple TV or FireTV, as the DRM restrictions do not apply via the TV's HDMI inputs.

![HyperHDR LED Visualization](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/382bd569-6bc2-4a0c-9fdb-d3f6494911ad)

Remote Control:

Under HyperHDR/Hyperion remote control menu, you could monitor all processes and see whether data from PicCap is arriving at the HyperHDR “Flatbuffers” under source selection. There you can also switch HDR Global on or off if required. However, HyperHDR recognizes when a source provides HDR content and switches HDR Global on automatically and switches it off again for SD video sources.
Under LED device, the LEDs can be switched off and on as required.

![HyperHDR Remote Control](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/cc5cc695-3846-4cf5-9b3a-c528c5860451)

Important note when using ESP32 Controller:

When using ESP32 LED controller with WLED firmware, it is necessary to make further settings under WLED depending on the LED type, RGB or RGBW used. For example, when using four channel LEDs such as SK6812RGBW. 
In the LED settings under “White management”--> “White Balance correction”, under “Calculate white channel automatically from RGB”, select “Dual” to actually use the white channel of the LEDs.

![WLED Withe Balance](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/e669d876-a8e2-4dec-9dc1-7675877c46ff)

Important note if you are using the ESP32 WiFi Controller and the connection is interrupted:

If the connection between WLED and router is interrupted, select under WiFi Settings --> Disable WiFi sleep to prevent the ESP from switching off its WiFi. In this case, the ESP32 can consume more power, but the connection remains active.

![WI-FI](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/e1578c44-f8a6-465a-921f-73cbe776966d)

For those who prefer WLED firmware because of all the extras, but have had a bad experience over WiFi because of the long delay, then I recommend the ESP32 variant with built-in LAN connection. 
"QuinLED Dig Uno v3 DIGITAL LED controller", which is also available with LAN and acrylic housing. Or "ABC! WLED Controller Board V43 (5-24V)", "Ethernet Adapter for WLED Controller" and ‘Housing for WLED Boards’. The controllers are supplied with WLED.
Just google: 
“QuinLED Dig Uno v3 DIGITAL LED controller”, which is also available with LAN and acrylic housing. Or “ABC! WLED Controller Board V43 (5-24V)", ‘Ethernet Adapter for WLED Controller’ and ‘Housing for WLED Boards’. The controllers are supplied with WLED.

quinled-dig-uno-v3-digital-led-controller:

![quinled-dig-uno-v3-digital-led-controller](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/66fc94af-7ecf-4a95-aca9-0390492be823)

ABC! WLED Controller Board+Ethernet_Adapter:

![ABC! WLED Controller Board+Ethernet_Adapter](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/5badd444-1380-4cbf-84b7-a49c69877e37)

cod.m Controller:
![cod m Controller](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/9005e953-ae30-4981-bed4-0fb36a019257)

Level Shifter:

For those who do not use a ready-made LED controller with a built-in level shifter, I strongly recommend integrating a level shifter into the circuit. Since most LEDs need to be supplied with 5 volts, but the logic of the controller can only handle 3.2 volts, the direct DATA line should be connected via a level shifter. For testing purposes, you can temporarily install a 470 Ohm resistor on the DATA line and a 1000µF electrolytic capacitor on the 5 Volt+ and GND at the input of the LEDs. 

![level shifter1](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/650c56c7-7c51-476a-a265-a1902e0c1f8b)

Example Connection of various LED controllers:

![LED 4 Seitige Aufbau HyperSerialPico](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/70693991-ed28-4208-94d0-e0c9541de007)

![ABC! WLED Controller V41 ESP32](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/3d5f4588-11ac-408b-a0aa-060fcb1f2417)

![Level Shifter](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/179cb14f-6244-40e7-987e-8d7b2d589d50)

![WLED+Netztteil und Level Shifter](https://github.com/satgit62/How-to-Install-and-set-up-Ambilight-on-LG-webOS/assets/68075993/591438fb-5e3d-4086-bf6a-c1018d293832)



















































