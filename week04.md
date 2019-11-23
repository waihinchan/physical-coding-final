# week04
#final
# review
This week i tested some basic component.
There were the LED Band, the Hall effect sensors, the capacitor, including the reed switch.

Capacitor test:
[capacitor on Vimeo](https://vimeo.com/375100965)
- - - -
Ledband test:
[ledband on Vimeo](https://vimeo.com/375102255)
Here is the model of the led band I used:[Digital RGB Addressable LED Weatherproof Strip 60 LED -1m (Adafruit Ne — Cool Components](https://coolcomponents.co.uk/collections/leds/products/digital-rgb-led-weatherproof-strip-60-led-1m-black)
This is an addressable RGB led, controlled by the  [Adafruit Neopixel Library](https://github.com/adafruit/Adafruit_NeoPixel) 
Here is the test code part:
``` cpp
#include <Adafruit_NeoPixel.h> 
#ifdef __AVR__
  #include <avr/power.h>
#endif
#define PIN 4
Adafruit_NeoPixel strip = Adafruit_NeoPixel(3, PIN);
void setup() {
  strip.begin();
  strip.show();
}
void loop() {
  colorWipe(strip.Color(0, 255, 0), 500); // Green
  strip.clear();
}
void colorWipe(uint32_t c, uint8_t wait) {
  for(uint16_t i=0; i<strip.numPixels(); i++) {
    strip.setPixelColor(i, c);
    strip.show();
    delay(wait);
  }
}
```
- - - -
After tested, i found that I can separate the led band into single led, then solder them together, this mean I can control the length of the led band. Basically in my installation, each led band will up to no more than 8 single led, I will control the space between the each led so reach to the best vision effect.

Hall effect sensors test:
[halleffectsensor on Vimeo](https://vimeo.com/375100928)
There are two type of the Hall effect sensors, one is the linear , the other is the switch. Also the switch type have two type, one is the non-latching, the other is the latching.
> The latching sensor gives out an output HIGH (5V) voltage whenever the north pole of a magnet is brought close to it. Even when the magnet is removed, the sensor still outputs a HIGH voltage and does not go LOW (0V) until the south pole of the magnet is brought close to it. These sensors that latch on to a particular state are called latched Hall effect sensors.   
> The non-latching sensor gives an output HIGH voltage whenever the north pole of a magnet is brought close to it, and switches LOW whenever the magnet is removed. I personally prefer non-latching Hall effect sensors like the US5881 for my projects.   
I will used the latching Hall effect sensors as once the magnet closed to the sensors, the switch status would been locked,  I don’t want the led turn on and off every time.
I bought some sensors from amazon, but they were all broke. I hate amazons.
I am looking for the new order from eBay next week, hope that would work.


- - - -
## next step
Waiting for the latch Hall effect sensors to build the basic minimum model.
The basic minimum model function would be when the magnet closed to the Hall effect sensors, the whale part’s led will extinguish, then one of the other creatures’ led will light on. Then the led band which at the second layer connected from the whale part to that creature part would light on.
This mean when the Hall effect sensors been triggered, there are one led turn off and another led turn off, also one led band would turn on.
- - - -
![](week04/basicmodel_schematic.png)




