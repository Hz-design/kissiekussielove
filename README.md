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
```
// Adafruit IO Digital Input Example
// Tutorial Link: https://learn.adafruit.com/adafruit-io-basics-digital-input
//
// Adafruit invests time and resources providing this open source code.
// Please support Adafruit and open source hardware by purchasing
// products from Adafruit!
//
// Written by Todd Treece for Adafruit Industries
// Copyright (c) 2016 Adafruit Industries
// Licensed under the MIT license.
//
// All text above must be included in any redistribution.

/************************** Configuration ***********************************/
#include <Adafruit_NeoPixel.h>
#ifdef __AVR__
#include <avr/power.h>  // Required for 16 MHz Adafruit Trinket
#endif

// Which pin on the Arduino is connected to the NeoPixels?
#define PIN D5  // On Trinket or Gemma, suggest changing this to 1

// How many NeoPixels are attached to the Arduino?
#define NUMPIXELS 14  // Popular NeoPixel ring size

Adafruit_NeoPixel pixels(NUMPIXELS, PIN, NEO_GRB + NEO_KHZ800);

// edit the config.h tab and enter your Adafruit IO credentials
// and any additional configuration needed for WiFi, cellular,
// or ethernet clients.
#include "config.h"

/************************ Example Starts Here *******************************/

// digital pin 5
#define BUTTON_PIN D0

// button state
bool current = false;
bool last = false;

// set up the 'digital' feed
AdafruitIO_Feed *digital = io.feed("digital");

// shared feed
AdafruitIO_Feed *sharedFeed = io.feed("digital", "Faynore");

void setup() {

  // set button pin as an input
  pinMode(BUTTON_PIN, INPUT);

  // start the serial connection
  Serial.begin(115200);

  // wait for serial monitor to open
  while (!Serial)
    ;

  // connect to io.adafruit.com
  Serial.println("Connecting to Adafruit IO");
  io.connect();

  // wait for a connection
  while (io.status() < AIO_CONNECTED) {
    Serial.println(".");
    delay(1000);
  }

  // we are connected
  Serial.println();
  Serial.println(io.statusText());

  pixels.begin();  // INITIALIZE NeoPixel strip object (REQUIRED)
}

void loop() {

  // io.run(); is required for all sketches.
  // it should always be present at the top of your loop
  // function. it keeps the client connected to
  // io.adafruit.com, and processes any incoming data.
  io.run();

  pixels.clear();  // Set all pixel colors to 'off'

  // grab the current state of the button.
  // we have to flip the logic because we are
  // using a pullup resistor.
  if (digitalRead(BUTTON_PIN) == LOW) {
    current = true;

    for (int i = 0; i < NUMPIXELS; i++) {  // For each pixel...
      pixels.setPixelColor(i, pixels.Color(150, 150, 150));
      pixels.show();  // Send the updated pixel colors to the hardware.
    }
  }
  // current = true;
  else {
    current = false;
    pixels.clear();
    pixels.show();
  }
  // current = false;

  // return if the value hasn't changed
  if (current == last)
    return;

  // save the current state to the 'digital' feed on adafruit io
  Serial.println("sending button -> ");
  Serial.println(current);
  digital->save(current);
  sharedFeed->save(current);

  // store last button state
  last = current;
}
```
