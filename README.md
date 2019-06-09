# FastLED + ESP8266 Web Server for Desk Light



This is a fork of [jasoncoon's esp8266 fastled webserver](https://github.com/jasoncoon/esp8266-fastled-webserver) that was adapted to control the colors of my  [WiFi Controlled Desk Lamp](https://www.thingiverse.com/thing:3676536)

[![https://youtu.be/R0lnP2zpz5U](preview.gif)](https://youtu.be/R0lnP2zpz5U)


## Important!
**
FastLED 3.2.7 & 3.2.8 DO NOT WORK
esp8266 2.5.0 and above causes compilation errors
Arduino IDE 1.8.9 and above might cause Sketch-Data-Uploading Issues
**


Hardware
--------

##### ESP8266

<img src="https://ae01.alicdn.com/kf/HTB1QYHzJKuSBuNjy1Xcq6AYjFXau/ESP8266-ESP-12-ESP12-WeMos-D1-Mini-Modul-Wemos-D1-Mini-WiFi-Entwicklung-Bord-Micro-USB.jpg" height="300px">

[Wemos D1 mini](http://s.click.aliexpress.com/e/cBDdafPw) is recommended, but any other ESP8266 variant should work too, but it might require an additional step-down converter and wouldn't fit the case.



##### Addressable Led Strip

<img src="https://ae01.alicdn.com/kf/HTB1THW.i6oIL1JjSZFyq6zFBpXa0/DC5V-WS2812B-1-mt-4-mt-5-mt-30-60-74-96-144-pixel-leds-m.jpg" height="300px" align="middle">

I would recommend buying a strip with 60 leds/m or more.

[WS2812b LED strip](http://s.click.aliexpress.com/e/SkQFQqc), make sure you choose **IP30** any other IP rating wouldn't make any sense and **WILL NOT FIT**.



##### 5v 3A Power supply

To supply the lamp you will need a 5V Power Supply with at least 3A (for ~ 100 Led Pixels), choose one with higher current if you plan to use a led strip that is more dense.



##### Analog Microphone for the Sound Reactive mode (optional)



Features
--------
* Turn the Logo on and off
* **Sound Reactive Mode**
* Adjust the brightness
* Change the display pattern
* Adjust the color
* Activate pattern autoplay


### Upcoming Features

- **Node-RED** integration
  - Simple Amazon **Alexa** integration
  - Voice control


Web App
--------

![Webinterface](https://github.com/NimmLor/esp8266-nanoleaf-webserver/blob/master/gallery/interface.jpg?raw=true)

The web app is stored in SPIFFS (on-board flash memory).



## Circuit

![circuit without Logic level converter](wiring.jpg)



Installing
-----------
The app is installed via the Arduino IDE which can be [downloaded here](https://www.arduino.cc/en/main/software). The ESP8266 boards will need to be added to the Arduino IDE which is achieved as follows. Click File > Preferences and copy and paste the URL "http://arduino.esp8266.com/stable/package_esp8266com_index.json" into the Additional Boards Manager URLs field. Click OK. Click Tools > Boards: ... > Boards Manager. Find and click on ESP8266 (using the Search function may expedite this). Click on Install. After installation, click on Close and then select your ESP8266 board from the Tools > Board: ... menu.

The app depends on the following libraries. They must either be downloaded from GitHub and placed in the Arduino 'libraries' folder, or installed as [described here](https://www.arduino.cc/en/Guide/Libraries) by using the Arduino library manager.

- [FastLED](https://github.com/FastLED/FastLED)
- [Arduino WebSockets](https://github.com/Links2004/arduinoWebSockets)

Download the app code from GitHub using the **Releases** section on [GitHub](https://github.com/NimmLor/esp8266-logo-webserver/releases) and download the ZIP file. Decompress the ZIP file in your Arduino sketch folder. Rename the folder from *esp8266-logo-webserver-master* to *esp8266-logo-webserver*

### Configuration

First enter the pin where the *Data* line is connected to, in my case it's pin D4 (GPIO02).

`#define DATA_PIN D4`

The *LED_TYPE* can be set to any FastLED compatible strip. Most common is the WS2812B strip.

If colors appear to be swapped you should change the color order. For me, red and green was swapped so i had to change the color order from *RGB* to *GRB*.

You should also set the milli-amps of your power supply to prevent power overloading. I am using the regular USB connection so i defined 1500mA.

`#define MILLI_AMPS 1500`

`#define RANDOM_AUTOPLAY_PATTERN` constant configures if the patterns in autoplay should be choosen at random, if you don't want this functionality just add '\\\' in the front.

Another **important** step is to create the **Secrets.h** file. Create a new tab (**ctrl**+**shift**+**n**) and name it *Secrets.h*, this file contains your WIFI credentials and it's structure must look like this:

```c++
// AP mode password
const char WiFiAPPSK[] = "WIFI_NAME_IF_IN_AP_MODE";

// Wi-Fi network to connect to (if not in AP mode)
char* ssid = "YOUR_WIFI_NAME";
char* password = "YOUR_WIFI_PASSWORD";
```

### Uploading

The web app needs to be uploaded to the ESP8266's SPIFFS.  You can do this within the Arduino IDE after installing the [Arduino ESP8266FS tool](http://esp8266.github.io/Arduino/versions/2.3.0/doc/filesystem.html#uploading-files-to-file-system). An alternative would be to install the [Visual Micro](https://www.visualmicro.com/) plugin for Visual Studio.

With ESP8266FS installed upload the web app using `ESP8266 Sketch Data Upload` command in the Arduino Tools menu.



## Technical

Patterns are requested by the app from the ESP8266, so as new patterns are added, they're automatically listed in the app.

The web app is stored in SPIFFS (on-board flash memory).

The web app is a single page app that uses [jQuery](https://jquery.com) and [Bootstrap](http://getbootstrap.com).  It has buttons for On/Off, a slider for brightness, a pattern selector, and a color picker (using [jQuery MiniColors](http://labs.abeautifulsite.net/jquery-minicolors)).  Event handlers for the controls are wired up, so you don't have to click a 'Send' button after making changes.  The brightness slider and the color picker use a delayed event handler, to prevent from flooding the ESP8266 web server with too many requests too quickly.

The only drawback to SPIFFS that I've found so far is uploading the files can be extremely slow, requiring several minutes, sometimes regardless of how large the files are.  It can be so slow that I've been just developing the web app and debugging locally on my desktop (with a hard-coded IP for the ESP8266), before uploading to SPIFFS and testing on the ESP8266.

### Compression

The web app files can be gzip compressed before uploading to SPIFFS by running the following command:

`gzip -r data/`

The ESP8266WebServer will automatically serve any .gz file.  The file index.htm.gz will get served as index.htm, with the content-encoding header set to gzip, so the browser knows to decompress it.  The ESP8266WebServer doesn't seem to like the Glyphicon fonts gzipped, though, so I decompress them with this command:

`gunzip -r data/fonts/`

### REST Web services

The firmware implements basic [RESTful web services](https://en.wikipedia.org/wiki/Representational_state_transfer) using the ESP8266WebServer library.  Current values are requested with HTTP GETs, and values are set with POSTs using query string parameters.  It can run in connected or standalone access point modes.
