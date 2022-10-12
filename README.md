# kissiekussielove
## Connecting a NodeMCU with a different NodeMCU via Adafruit IO
by Hong Zhou
last update 12 october 2022

## Required parts
- Arduino ESP8266/ESP32
- Press sensor
- micro-usb to usb-a/usb-c
- Arduino IDE

## Setting up Adafruit IO en Arduino
1. After making an account on [Adafruit IO](https://accounts.adafruit.com/) Make a new feed, give it a name: example "kussiekussielove" or make up something betters.
2. Install the library adafruit io and open the example Adafruit_06_digital.in
3. Install your push button, changed the code from D0 to 15 (ESP32) (red cable on 3.3V, black on GND and yellow won D15).
4. In config.h tab, fill out your information (WiFi and Adafruit io Key and username).
5. Upload your code press the **Boot** button on ESP32 wwhile uploading.
6. In your serial monitor you can check if it's correct.
7. Check your Adafruit feed if it has connected.
>**Note**: my code created a neww feed on adafruit called "digital" all the information displayed on that feed. There is no data in "kissiekussielove" feed.

## Sharing a feed on Adafruit IO
[step by step guide](https://learn.adafruit.com/adafruit-io-basics-feeds/sharing-a-feed)
1. Go back to adafruit IO and turn on the share modus set it on public. 
2. Invite people by filling out their email and set their acces level on read + write.

## Code 
