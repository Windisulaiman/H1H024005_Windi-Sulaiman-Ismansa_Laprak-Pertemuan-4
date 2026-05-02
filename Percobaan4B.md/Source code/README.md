````cpp
#include <Arduino.h> 

const int potPin = A0;  // pin analog potensiometer
const int ledPin = 9;   // pin digital PWM untuk LED

int nilaiADC = 0;  // menyimpan hasil baca ADC (0–1023)
int pwm = 0;       // menyimpan nilai PWM hasil konversi (0–255)

void setup() {

  pinMode(ledPin, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  nilaiADC = analogRead(potPin);
  pwm = map(nilaiADC,
            0,    // nilai minimum ADC
            1023, // nilai maksimum ADC
            0,    // PWM minimum (LED mati)
            255); // PWM maksimum (LED full brightness)

  analogWrite(ledPin, pwm);

  Serial.print("ADC: ");
  Serial.print(nilaiADC);

  Serial.print(" | PWM: ");
  Serial.println(pwm);

  delay(50); // 50ms cukup stabil untuk pembacaan potensiometer
}()
````

````cpp
#include <Arduino.h>

const int potPin = A0;
const int ledPin = 9;

int nilaiADC = 0;
int pwm      = 0;

void setup() {
  pinMode(ledPin, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  nilaiADC = analogRead(potPin);

  pwm = map(nilaiADC, 0, 1023, 50, 200);
  pwm = constrain(pwm, 50, 200);

  analogWrite(ledPin, pwm);

  Serial.print("ADC: ");
  Serial.print(nilaiADC);
  Serial.print(" | PWM: ");
  Serial.println(pwm);

  delay(50);
}
````

