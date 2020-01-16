# week07

Date: Dec 24, 2019
Tags: done

## CASE

i used the play dough to build my case. I want to combine the creature and the mechanic together. Maybe in the future our creature will pollute by the electric stuff, and the "food" will be the power.

Anyway i built it. I pre-digged some hole inside the case so that i can put the hall effect sensor or other element go though it. Also i digged many big hole to make sure the light inside can penetrate the clay.

![week07/IMG_1011.heic](pic/IMG_1011.heic)

![week07/IMG_3100.heic](pic/IMG_3100.heic)

![week07/IMG_5094.heic](pic/IMG_5094.heic)

![week07/IMG_5501.heic](pic/IMG_5501.heic)

---

MULTI-TASK

One serious problem i found was the arduino can deal with different task at the same time. The problem is quite serious especially if you need serval LED or motor running at the same time.

The neopixel rely on the "delay" function to move the LED on and off. If we don't use delay, the frequency would really fast so the people can't recognize the changing.

If we only got one neopixel LED band, the arduino can deal with it because there is only one task ongoing.

In my project, i got over 5 neopixel LED band, so i need to rebuild the coding parting to make sure each band can run independently.

Here is the guide:

[Multi-tasking the Arduino - Part 1](https://learn.adafruit.com/multi-tasking-the-arduino-part-1)

here is the derived class i built from the neopixl library. By using this i can control unlimited LED band at the same time in theory.

    #include <Adafruit_NeoPixel.h>
    enum direction { FORWARD, REVERSE };
    class Mutilband : public Adafruit_NeoPixel
    { public:
    direction Direction;
    unsigned long Frq;
    unsigned long LastUdate;
    uint16_t Total;
    uint16_t Index;
    uint32_t Color1;
    Mutilband(uint16_t pixels, uint8_t pin, uint8_t type):
    Adafruit_NeoPixel(pixels,pin,type){}
    void energytrans(uint32_t color, uint8_t frq){
    Frq = frq;
    Total = numPixels();
    Index = 0;
    color = Color1;}
    void move(){
    if (Index <= Total)
    {Index++;}
    if(Index=Total){
    Index = 0;}
    }
    void update(){
    if((millis()-LastUdate) >= Frq)
    { LastUdate = millis();
    move();
    setPixelColor(Index,Color1);
    show();
    }
    }
    };
    Mutilband stick(3, 4, NEO_GRB + NEO_KHZ800);
    void setup() {
    stick.begin();
    stick.energytrans(stick.Color(0,255,0), 500);
    }
    void loop() {
    stick.update();
    }

HOW IT LOOKS LIKE:

[multi task energy transform](https://vimeo.com/user92504253/review/385221536/fe4403322f)

## BREATH LIGHT

To reach a more stunning effect, i wanted to build the breath light effect inside the dead whale body. This could make the whale looks alive.

But the original neopixel library can't not control the brightness at real time, if i need to use one more pin to control the voltage output, it would be a mess and disaster . Then i found a library calls FASTLED. This library can control the brightness of the neopixel LED band.

Also i found that the FASTLED library is quite complex, it is really hard for me to build the derived class from the FASTLED class. Then i found a library call AceRoutine. By using that, i can build a multi task terminal.(i think the concept and rule are quite the same, using the millis() to minus the freq you want. The result is the delay time.)

Here was the breathing light effect coding part, the COROUTINE_LOOP part is the breath effect.

    #include <FastLED.h>
    #include <AceRoutine.h>
    using namespace ace_routine;
    #define NUM_LEDS 10
    #define DATA_PIN 7
    CRGB leds[NUM_LEDS];
    COROUTINE(breath1) {
    COROUTINE_LOOP() {
    static int i ;
    static int j ;
    static int counter = 250;
    static int BRI = 250;
    static int bri = 0;
    static int delaytime = 5;
    for(i = BRI; i>bri; i-=1){
    fill_solid( &(leds[0]),4, CRGB(165,255,0));
    nscale8(&(leds[0]),4, i);
    FastLED.show();
    COROUTINE_DELAY(delaytime);
    }
    for(j = 0; j<BRI; j+=1){
    fill_solid( &(leds[0]),4, CRGB(165,255,0));
    nscale8(&(leds[0]),4, j);
    FastLED.show();
    COROUTINE_DELAY(delaytime);
    }
    COROUTINE_DELAY(500);
    BRI-=10;
    delaytime+=1;
    COROUTINE_DELAY(500);
    if (BRI<0) {
    fill_solid( &(leds[0]),4, CRGB::Black);
    FastLED.show();
    COROUTINE_END();
    } COROUTINE_YIELD();
    }}
    void setup() {
    FastLED.addLeds<WS2811, DATA_PIN, RGB>(leds, NUM_LEDS);
    }
    void loop() {
    CoroutineScheduler::loop();
    }

how it looks like:

[multi task energy transform](https://vimeo.com/user92504253/review/385221536/fe4403322f)
