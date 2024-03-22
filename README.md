# fuse_pibox

Hardware Build Guide
Step 1: Gather Parts and Hardware
![Screenshot 2024-03-22 at 2 41 21 PM](https://github.com/mkohler99/fuse_pibox/assets/7109569/d6f8377b-ada8-4126-960d-3a26dea70e0c)

![Screenshot 2024-03-22 at 2 39 10 PM](https://github.com/mkohler99/fuse_pibox/assets/7109569/d4ddf6dc-713b-4467-b817-4fbdae6d125a)

Step 2: Slide the 3d printed shell onto the metal base

![Screenshot 2024-03-22 at 2 39 24 PM](https://github.com/mkohler99/fuse_pibox/assets/7109569/db4fd4f8-3452-4f24-a407-94098eab368a)

Install Screws around the side to hold the shell onto the metal base


![Screenshot 2024-03-22 at 2 39 17 PM](https://github.com/mkohler99/fuse_pibox/assets/7109569/cf5c4ebd-1d07-4ec8-9ee5-206c9db3e163)

4: Install the ethernet connector
![Screenshot 2024-03-22 at 2 41 13 PM](https://github.com/mkohler99/fuse_pibox/assets/7109569/3094577f-87e4-4675-911e-0d82a8a58a4d)

5: Install the USB Connectors
![Screenshot 2024-03-22 at 2 41 07 PM](https://github.com/mkohler99/fuse_pibox/assets/7109569/1a450faa-9850-491a-aa2b-f13803ec075c)

6:Install the Midi connector with pigtail and the USB extensions
![Screenshot 2024-03-22 at 2 41 00 PM](https://github.com/mkohler99/fuse_pibox/assets/7109569/44036d71-9790-472b-a8ca-682043e0f777)

7: Plug in the electroncis stack
![Screenshot 2024-03-22 at 2 40 45 PM](https://github.com/mkohler99/fuse_pibox/assets/7109569/6eec72b7-a0bf-4429-969e-b7e369224b74)

8: Place the electronics module onto the 4 posts in the metal base and attatch the USB midi interface board
![Screenshot 2024-03-22 at 2 40 38 PM](https://github.com/mkohler99/fuse_pibox/assets/7109569/30609499-c48b-40cf-9c19-9597c574113d)

9: screw in the cover for the midi board
![Screenshot 2024-03-22 at 2 40 32 PM](https://github.com/mkohler99/fuse_pibox/assets/7109569/6d4f9fe4-d7ec-4ee5-859f-744dfd1dcb33)

10:Add the usb power connector to the top shield
![Screenshot 2024-03-22 at 2 40 26 PM](https://github.com/mkohler99/fuse_pibox/assets/7109569/56242c62-96af-41c4-bbfe-9336958ff851)

11:AAttatch the display wire and zip tie in place. additionally zip tie in the usb cables
![Screenshot 2024-03-22 at 2 40 18 PM](https://github.com/mkohler99/fuse_pibox/assets/7109569/f5138576-02df-47bc-ad09-51e2ac1c7894)

12:Install the fan
![Screenshot 2024-03-22 at 2 40 06 PM](https://github.com/mkohler99/fuse_pibox/assets/7109569/6471664c-809c-4033-8bfb-2cb012cebb51)

13: Zip tie the fan cable
![Screenshot 2024-03-22 at 2 39 50 PM](https://github.com/mkohler99/fuse_pibox/assets/7109569/c25b75b5-7c53-4903-b8a1-7df7af0bca15)

14: Screw the fan into the case with the standard pc fan screws
![Screenshot 2024-03-22 at 2 39 57 PM](https://github.com/mkohler99/fuse_pibox/assets/7109569/8732bfbd-ae09-4733-b358-0541a6ea55e2)

15: Install the display board
![Screenshot 2024-03-22 at 2 39 43 PM](https://github.com/mkohler99/fuse_pibox/assets/7109569/8dacd42f-82f6-4cc2-9892-dc89f1690eb0)

16: Install the acrylic cover
![Screenshot 2024-03-22 at 2 39 31 PM](https://github.com/mkohler99/fuse_pibox/assets/7109569/0d4a8f39-9699-4f60-81e4-35a16435e4bc)


Run bringup Tests:
Final Validation TEST:
Both Fans spin
Nodered Midi Input
Nodered Neopixel Control
Companion Display
Read /proc/device-tree/hat vendor


Software Config: 
PI SETUP

Use pi baker, setup user and password and enable ssh - device is headless

-Raspbian Bullseye Legacy 64 Bit Lite:
Username fuse, password fuse1234
Hostname - make unique


NODERED

Install pimorni unicorn hat

curl -sS https://get.pimoroni.com/unicornhat | bash

Install ALSA   sudo apt-get install libasound2-dev

Install node red   bash <(curl -sL https://raw.githubusercontent.com/node-red/linux-installers/master/deb/update-nodejs-and-nodered)

Answer questions

Autostart service on boot sudo systemctl enable nodered.service

Reboot



From within node red 
Install midi
Install osc
Install neopixel

Install test program.





COMPANION:

Install companion satellite.

Sudo su

curl https://raw.githubusercontent.com/bitfocus/companion-satellite/main/pi-image/install.sh | bash

This doesn’t actually install satellite, it instead installs the subsystem

Then run satellite-update

Choose the latest version of stable

Config satellite to test server 
nano /home/satellite/satellite-config.json
Edit so Remote IP is at 10.10.10.126

Reboot


How to flash the custom FUSE PI HAT
How to flash a PI Hat.

1.) Connect only the hat, you may not connect through a stackable POE HAT, so you have to power the PI with USB C. I of course accidentally blocked this port with the case design 

2.) Add a jumper over the WRITE pin header to enable the EEPROM Flash

3.) SSH into the PI. 

4.) Navigate to the ~/utils/eeptools folder, this is the eep utils from raspberry pi.

5.) Enable i2c bus 9 with this command ‘sudo dtoverlay i2c-gpio i2c_gpio_sda=0 i2c_gpio_scl=1 bus=9’ (OPTIONAL, the script does this)

6.) Detect the EEPROM with ‘i2cdetect -y 9’ you should see a device enumerated at address 50, thats the EEPROM  (OPTIONAL ,only needed for troubleshooting) 

7.) Run this command to flash the device ‘sudo ./eepflash.sh -w -t=24c32 -f=fusehat.eep’ it should say it completed successfully

8.) Reboot and remove the write jumper

9.) Navigate to /proc/device-tree/hat and ‘cat vendor’ it should read ‘FUSE Technical Group’

10.) run ‘raspi-gpio get’ and verify the state of pins that are set in the HAT settings: For example ‘GPIO 18: level=0 fsel=2 alt=5 func=PWM0_0 pull=DOWN’ this means GPIO 18 (the pixel string) is set to be a PWM (alt 5) output 







