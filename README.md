# LL120 Arduino without Commander Pro or Lighting Hub

# Fan details
Each LL fan consists of 16 LEDs - 12 on the outer loop and 4 on the inner loop
The 4 inner LEDs are layed out as side facing LEDs
The fan uses WS2811 LEDs (I think as I managed to get them to work with that arrangement type)
These LEDs use the GRB color order
Each of the LEDs draws 60mA of current (20mA per color channel in the LED) at full brightness showing white meaning one fan will draw a max current of 960mA
A USB 2.0 header can output 500mA of current which is almost half of what is needed however I have managed to run 1 fan fine on a USB 2.0 header - Though for one fan using a USB 3.0 header would be better as it can provide upto 900mA of current
If you want to run 2 or more fans then you will need to power the LEDs another way or buy a Corsair RGB Fan Hub (Commander Pro is still not requried)

The pin order of the RGB connect on the fan with the flat side facing up is [Ground | Data in | Data out | 5v] (Inversed if you have to clip facing up)

<p>
  <img src="https://github.com/Aw4k3/LL120-Arduino-without-Commander-Pro-or-Lighting-Hub/blob/master/LL120%20Fan%20LED%20order.png" width="400" height="400">
  <img src="https://github.com/Aw4k3/LL120-Arduino-without-Commander-Pro-or-Lighting-Hub/blob/master/LL120%20RGB%20Connecter%20Pin%20order.png" width="400" height="287">
</p>

# Getting the RGB to work
To get this working you will need to first connect your arduino board to your fans RGB wire
With the flat side of the connect facing up, the pin order is [Ground | Data in | Data out | 5v]
If you are only using one fan the Data out can be ignored, However if you are using more than one fan, connect the data out of the first fan to the data in of the next
Now connect your Fans 5v, Ground and Data in to the 5v, Ground and any GPIO pin on your board (in my case i used a WeMos D1 board)
Now connect your board to your computer and open the Arduino IDE
If you don't already have the "FastLED" Library installed, go to [Sketch > Include Library > Manage Libraries] and install the "FastLED" library
Now open the LL120 Sketch, then set your board and COM port in the Tools tab
Finally, just upload the code

# Code base
```c++
//#define FASTLED_ESP8266_RAW_PIN_ORDER
#include <FastLED.h>

#define NUM_LEDS 16 // This will be the number of fans you have times 16 (16 LEDs per fan)
#define DATA_PIN D8
CRGB leds[NUM_LEDS];

void setup() {
  delay(2000);
  FastLED.addLeds<WS2811, DATA_PIN, GBR>(leds, NUM_LEDS);
}

void loop() {
  // Your lighting code here
  FastLED.show();
}
```
Uncomment #define FASTLED_ESP8266_RAW_PIN_ORDER if you get a pin error when using a NodeMCU board
