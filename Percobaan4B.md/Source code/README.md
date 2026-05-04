### Program Utama Percobaan 4B.

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

  delay(50); 
}
````

### Code Program yang sudah dimodifikasi sesuai pertanyaan Percobaan 4B.

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

  pwm = map(nilaiADC,
                    0,
                    1023,
                    50,
                    200);
  pwm = constrain(pwm,
                    50,
                    200);

  analogWrite(ledPin, pwm);

  Serial.print("ADC: ");
  Serial.print(nilaiADC);
  Serial.print(" | PWM: ");
  Serial.println(pwm);

  delay(50);
}
````


### Penjelasan Fungsi Code

| Fungsi | Penjelasan |
|--------|------------|
| `#include <Arduino.h>` | Header utama Arduino framework. |
| `pinMode(pin, mode)` | Mengkonfigurasi pin sebagai `INPUT`, `OUTPUT`, atau `INPUT_PULLUP`. Pin 9 harus diset `OUTPUT` agar dapat mengalirkan arus ke LED via PWM. |
| `Serial.begin(baud)` | Menginisialisasi komunikasi serial UART dengan kecepatan yang ditentukan. Yang membantu `Serial.print()` untuk menampilkan output. |
| `analogRead(pin)` | Membaca nilai tegangan analog pada pin A0–A5 menggunakan ADC internal 10-bit. Tegangan 0V menghasilkan nilai 0 dan tegangan 5V menghasilkan nilai 1023. |
| `map(val, iMin, iMax, oMin, oMax)` | Konversi linear dari rentang input ke rentang output. Output diubah menjadi 50–200 agar LED tidak pernah benar-benar mati (pwm = 0) maupun menyala penuh (pwm = 255). |
| `constrain(val, lo, hi)` | Membatasi nilai dalam rentang `[lo, hi]`. Mencegah noise ADC di nilai ekstrem menghasilkan pwm di luar rentang 50–200 yang bisa membuat LED mati atau menyala terlalu terang. |
| `analogWrite(pin, value)` | Menghasilkan sinyal PWM 8-bit pada pin yang mendukung PWM (pin 3, 5, 6, 9, 10, 11 pada Uno). |
| `Serial.print(val)` | Mengirim data ke Serial Monitor tanpa newline yang dapat menerima berbagai tipe data: `int`, `float`, `String`, maupun `char`. |
| `Serial.println(val)` | Sama dengan `Serial.print()` dengan tambahan karakter `\r\n` di akhir baris. Sebaiknya digunakan pada bagian terakhir setiap kelompok print agar output di Serial Monitor rapi. |
| `delay(ms)` | Memblokir eksekusi selama `ms` milidetik. Nilai 50ms cukup untuk menstabilkan pembacaan ADC dan memperbarui kecerahan LED tanpa membebani loop secara berlebihan. |

