# FAQ & Troubleshooting

## Issues while compiling

- **Compiling shows `typedef bool error`**
  - You have esp8266 2.5.x installed in combination with an old version of fastled, install esp8266 2.4.2 or upgrade FastLED to 3.2.9
- **Compiling shows tons of errors**
  - You might have FastLED 3.2.7 installed, install 3.2.6 or update to 3.2.9
- **My LEDs just light up white**
  - You might have FastLED 3.2.8 installed, install 3.2.6 or update to 3.2.9
- **Sketch Data Upload does not work**
  - Try to install Arduino IDE 1.8.7 or 1.8.8
  - Don't install hourly or beta builds of the Arduino IDE
- **The Arduino IDE doesn't show the other files and says that files weren't found**
  - The Arduino IDE requires that the filename of the .ino file has the same name as the folder it is in, if the names do not match, then the .ino file will be moved into a new folder, to fix it move the file up to the other files and make sure the folder name is the same as the one of the file
- **Secrets.h file not found**
  - Create the Secrets.h file according to the instructions

## Issues while uploading

- **The Arduino doesn't boot when the LEDs are attached**
  - The LEDs might be wired in the wrong direction, the arrow must point away from the esp8266
- **The Sketch-Data-Upload says "esptool.exe not found"**
  - Check if you are using Arduino IDE version 1.8.7 or 1.8.8, newer version might throw this error

## Issues with the Webinterface

- **The website just shows "not found :/"**
  - Check if you have uploaded the Sketch-Data
  - Check if the Sketch-Data-Upload was successful
- **The website doesn't load**
  - Check if you have entered the correct IP-Address
  - You have to be in the same network as the device

## Issues with the LEDs

- **The LEDs flicker a lot**
  - You might have bad soldering spots
  - Try to use a logic level shifter to shift the supplied data signal to 5V
- **Not all of my LEDs turn on**
  - Check if you have correct values for `LINE_COUNT` and `LEDS_PER_LINE`
  - Check the wiring for errors

- **My LEDs just light up white**
  - You might have FastLED 3.2.8 installed, install 3.2.6 or update to 3.2.9



## FAQ

- **Can I connect to the esp8266 from outside of my local network??**
  - Yes, you can do this by creating a port forwarding rule on your router
- **Can I use the NodeMCU for this project?**
  - Yes, but it is way harder to position it into the base of the lamp
- **Can I use other LED-Strip types for this project?**
  - Yes, any FastLED compatible LED-Strip can be used
  - Just change the `LED_TYPE` 
  - A list of all compatible LED-Strips can be found [here](<https://github.com/FastLED/FastLED/wiki/Chipset-reference>).
- **What power supply should I choose?**
  - 100x WS2812 LEDs draw around 3A, I would recommend 5V 3A for 60LEDs/m















