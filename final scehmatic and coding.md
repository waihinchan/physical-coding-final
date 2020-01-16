# final scehmatic and coding

Date: Dec 25, 2019
Tags: ongoing

video link:

[https://vimeo.com/385229334](https://vimeo.com/385229334)

    int readpin1 = 2;
    int readpin2 = 3;
    int readpin3 = 4;
    int readpin4 = 5;
    int analogPin = A1;
    int val = 0;
    int noise = 0;
    int speakerPin = 10;
    bool mon1 = true;
    bool mon2 = true;
    bool mon3 = true;
    bool mon4 = true;
    void setup() {
    Serial.begin(115200);
    pinMode(2,INPUT);
    pinMode(3,INPUT);
    pinMode(4,INPUT);
    pinMode(5,INPUT);
    pinMode(speakerPin, OUTPUT);
    }
    void loop() {
    int sound = val*3-noise;
    tone(speakerPin,800-noise-val/2,1000);
    delay(1000);
    val = analogRead(analogPin);
    if(digitalRead(2)==HIGH&&mon1){
    noise+=50;
    mon1 = false;
    }
    if(digitalRead(3)==HIGH&&mon2){
    noise+=50;
    mon2 = false;
    }
    if(digitalRead(4)==HIGH&&mon3){
    noise+=50;
    mon3 = false
    ; }
    if(digitalRead(5)==HIGH&&mon4){
    noise+=50;
    mon4 = false;
    }
    }