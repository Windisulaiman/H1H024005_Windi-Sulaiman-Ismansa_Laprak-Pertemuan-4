````cpp
[#include <Servo.h> // library untuk servo motor

Servo myservo; // membuat objek servo

const int potensioPin = A0;  // pin analog input potensiometer
const int servoPin = 9;      // pin digital PWM untuk servo

int pos = 0;  // menyimpan sudut servo (0–90 derajat)
int val = 0;  // menyimpan nilai ADC dari potensiometer (0–1023)

void setup() {

  // Hubungkan servo ke pin yang sudah ditentukan
  myservo.attach(servoPin);

  // Aktifkan komunikasi serial untuk monitoring
  Serial.begin(9600);

}

void loop() {

  // Baca nilai dari potensiometer (rentang 0–1023)
  val = analogRead(potensioPin);

  // Ubah nilai ADC menjadi sudut servo (0–90 derajat)
  pos = map(val,
             0,    // nilai minimum ADC
             1023, // nilai maksimum ADC
             0,    // sudut minimum servo
             90);  // sudut maksimum servo (dibatasi 90°)

  // Gerakkan servo sesuai hasil mapping
  myservo.write(pos);

  // Tampilkan data ADC dan sudut servo ke Serial Monitor
  Serial.print("ADC Potensio: ");
  Serial.print(val);

  Serial.print(" | Sudut Servo: ");
  Serial.println(pos);

  // Delay untuk memberi waktu servo bergerak stabil
  delay(15); // 15ms cukup untuk servo SG90 bergerak antar posisi
}()
````
````cpp
#include <Servo.h>

Servo myservo;

const int potensioPin = A0;
const int servoPin    = 9;

int pos = 0;
int val = 0;

void setup() {
  myservo.attach(servoPin);
  Serial.begin(9600);
}

void loop() {
  val = analogRead(potensioPin);

  pos = map(val, 0, 1023, 30, 150);
  pos = constrain(pos, 30, 150);

  myservo.write(pos);

  Serial.print("ADC Potensio: ");
  Serial.print(val);
  Serial.print(" | Sudut Servo: ");
  Serial.println(pos);

  delay(15);
}
````
