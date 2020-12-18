# CoronaMeter



Malzemeler :

<li>Arduino Nano </li>
<li>Gy-906 Temassız Kızılötesi Sıcaklık Sensörü</li>
<li>Pulse Nabız Ölçer Sensörü</li>
<li>SD1306 128x64 Oled Ekran</li>
<li>5V Lazer Modülü</li>
<li>12x12 Push Buton</li>
<li>5V Buzzer</li>
<li>40Pin Dişi Header</li>
<li>40Pin Erkek Header</li>
<li>Delikli Plaket</li>

<br>Devre Şeması :


![](https://github.com/mcelik7/CoronaMeter/blob/main/images/NABIZVESICAKLIK.png)

<br>Kodlar :

```c++
//Ehm.Müh.Muhammed Emin Çelik//
//15-11-2020//

#define USE_ARDUINO_INTERRUPTS true    // Set-up low-level interrupts for most acurate BPM math.
#include <PulseSensorPlayground.h>     // Includes the PulseSensorPlayground Library.
#include <Adafruit_GFX.h>
#include <Wire.h>
#include <Adafruit_MLX90614.h>
#include <Adafruit_SSD1306.h>

const int buzzer = 4;
const int PulseWire = A0;       // PulseSensor PURPLE WIRE connected to ANALOG PIN 0
const int LED13 = 13;          // The on-board Arduino LED, close to PIN 13.
int Threshold = 700;           // Determine which Signal to "count as a beat" and which to ignore.
int a=0;
const int Laser_Pin=5;  //Laser Pin
int buttonState = 0; 
const int buttonPin = 2;     // the number of the pushbutton pin

PulseSensorPlayground pulseSensor;  // Creates an instance of the PulseSensorPlayground object called "pulseSensor"


#define SCREEN_WIDTH 128 // OLED display width, in pixels
#define SCREEN_HEIGHT 64 // OLED display height, in pixels



// Declaration for an SSD1306 display connected to I2C (SDA, SCL pins)
Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, -1);

Adafruit_MLX90614 mlx = Adafruit_MLX90614();
```

```c++
static const unsigned char PROGMEM logo2_bmp[] =
{ 0x03, 0xC0, 0xF0, 0x06, 0x71, 0x8C, 0x0C, 0x1B, 0x06, 0x18, 0x0E, 0x02, 0x10, 0x0C, 0x03, 0x10,              //Logo2 and Logo3 are two bmp pictures that display on the OLED if called
0x04, 0x01, 0x10, 0x04, 0x01, 0x10, 0x40, 0x01, 0x10, 0x40, 0x01, 0x10, 0xC0, 0x03, 0x08, 0x88,
0x02, 0x08, 0xB8, 0x04, 0xFF, 0x37, 0x08, 0x01, 0x30, 0x18, 0x01, 0x90, 0x30, 0x00, 0xC0, 0x60,
0x00, 0x60, 0xC0, 0x00, 0x31, 0x80, 0x00, 0x1B, 0x00, 0x00, 0x0E, 0x00, 0x00, 0x04, 0x00,  };

static const unsigned char PROGMEM logo3_bmp[] =
{ 0x01, 0xF0, 0x0F, 0x80, 0x06, 0x1C, 0x38, 0x60, 0x18, 0x06, 0x60, 0x18, 0x10, 0x01, 0x80, 0x08,
0x20, 0x01, 0x80, 0x04, 0x40, 0x00, 0x00, 0x02, 0x40, 0x00, 0x00, 0x02, 0xC0, 0x00, 0x08, 0x03,
0x80, 0x00, 0x08, 0x01, 0x80, 0x00, 0x18, 0x01, 0x80, 0x00, 0x1C, 0x01, 0x80, 0x00, 0x14, 0x00,
0x80, 0x00, 0x14, 0x00, 0x80, 0x00, 0x14, 0x00, 0x40, 0x10, 0x12, 0x00, 0x40, 0x10, 0x12, 0x00,
0x7E, 0x1F, 0x23, 0xFE, 0x03, 0x31, 0xA0, 0x04, 0x01, 0xA0, 0xA0, 0x0C, 0x00, 0xA0, 0xA0, 0x08,
0x00, 0x60, 0xE0, 0x10, 0x00, 0x20, 0x60, 0x20, 0x06, 0x00, 0x40, 0x60, 0x03, 0x00, 0x40, 0xC0,
0x01, 0x80, 0x01, 0x80, 0x00, 0xC0, 0x03, 0x00, 0x00, 0x60, 0x06, 0x00, 0x00, 0x30, 0x0C, 0x00,
0x00, 0x08, 0x10, 0x00, 0x00, 0x06, 0x60, 0x00, 0x00, 0x03, 0xC0, 0x00, 0x00, 0x01, 0x80, 0x00  };
```

```c++
void setup() {
  pulseSensor.analogInput(PulseWire);   
  pulseSensor.blinkOnPulse(LED13);       //auto-magically blink Arduino's LED with heartbeat.
  pulseSensor.setThreshold(Threshold);   

  Serial.begin(9600);
  Serial.println("Adafruit MLX90614 test"); 
  pinMode(buzzer,OUTPUT);
  pinMode(Laser_Pin,OUTPUT);
  pinMode(buttonPin, INPUT_PULLUP);
  
  if (pulseSensor.begin()) {
    Serial.println("We created a pulseSensor Object !");  //This prints one time at Arduino power-up,  or on Arduino reset.  
  }
  delay(100);
  if(!display.begin(SSD1306_SWITCHCAPVCC, 0x3C)) { // Address 0x3D for 128x64
    Serial.println(F("SSD1306 allocation failed"));
    for(;;);
  }
  

  display.clearDisplay();
 
  display.setTextSize(1);
  display.setTextColor(WHITE);
  display.setCursor(20, 20);
  display.println("CORONAMETRE yi");
   display.setCursor(20, 40);
  display.println("Sabiyleyin . .");
  display.display();
  delay(5000);
  display.clearDisplay();
  
  display.setCursor(20, 20);
  display.println("ve Mavi Butona");
    display.setCursor(40, 40);
  display.println("Dokunun !");
  display.display();
  delay(5000);
  display.clearDisplay();

  mlx.begin(); 
  
}
```

```c++
void loop() {
 
 int myBPM = pulseSensor.getBeatsPerMinute(); 
 int Signal=analogRead(PulseWire);
 buttonState=digitalRead(buttonPin);
 
 if(buttonState==0 || a==1){
  a=1;
  if (pulseSensor.sawStartOfBeat() || mlx.readObjectTempC() >0 ){
        if(mlx.readObjectTempC() > 39.5 || myBPM > 100 ){
               
    display.clearDisplay();
    display.setTextSize(1);  //Size 2 means each pixel is 12 width and 16 high
    display.setCursor(50, 20);
    display.print("CORONA");
    display.setCursor(50, 40);
    display.print("ALARM");
    display.display(); 
    digitalWrite(buzzer, HIGH);
    delay(5000);

  }
    else if(mlx.readObjectTempC() < 39.5 && myBPM < 100){
    display.clearDisplay();
    digitalWrite(Laser_Pin, HIGH);
    digitalWrite(buzzer, LOW);
    display.setTextSize(1.8);  //Size 2 means each pixel is 12 width and 16 high
    display.setCursor(15, 10);
    display.print("TEMP=");
    display.setCursor(50, 10);
    display.print(mlx.readObjectTempC(),1);
    display.setCursor(80, 10);
    display.print((char)247);
    display.setCursor(85, 10);
    display.print("C");
    display.setTextSize(1.8);  //Size 2 means each pixel is 12 width and 16 high
    display.setCursor(15, 36);
    display.print("BPM = ");
    display.setCursor(50, 36);
    display.print(myBPM);
    
    if(Signal<700){
      display.drawBitmap(95, 35, logo2_bmp, 24, 21, WHITE);
     }
  else{
      display.drawBitmap(90, 30, logo3_bmp, 32, 32, WHITE);
  }
    display.display();               // Print the value inside of myBPM. 
   
  delay(20);                    // considered best practice in a simple sketch.
  }
  }
 }
 digitalWrite(Laser_Pin, LOW);
}
```
