### Program Utama pada percobaan 4A.

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

### Code modifikasi untuk pertanyaan pada percobaan 4A.

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

  pos = map(val,
              0,
              1023,
              30,
              150);
  pos = constrain(pos,
                    30,
                    150);

  myservo.write(pos);

  Serial.print("ADC Potensio: ");
  Serial.print(val);
  Serial.print(" | Sudut Servo: ");
  Serial.println(pos);

  delay(15);
}
````
### Penjelasan Fungsi

| Fungsi | Penjelasan |
|--------|------------|
| `#include <Servo.h>` | Menambahkan library Servo bawaan Arduino IDE agar class `Servo` dan seluruh method-nya tersedia. |
| `Servo myservo` | Mendeklarasikan objek bertipe `Servo`. |
| `myservo.attach(pin)` | Menghubungkan objek servo ke pin PWM yang ditentukan. Wajib dipanggil di `setup()` sebelum servo bisa digerakkan. |
| `Serial.begin(baud)` | Menginisialisasi komunikasi serial dengan kecepatan baud rate yang ditentukan. Yang dipanggil sebelum fungsi `Serial.print()` digunakan. |
| `analogRead(pin)` | Membaca tegangan analog pada pin A0–A5 dan menghasilkan nilai integer 0–1023 yang merepresentasikan 0V–5V. |
| `map(val, iMin, iMax, oMin, oMax)` | Memetakan nilai `val` dari rentang input ke rentang output secara linear. Digunakan agar nilai ADC dapat langsung dikonversi menjadi sudut servo (30°–150°). |
| `constrain(val, lo, hi)` | Membatasi nilai agar tidak pernah kurang dari `lo` atau lebih dari `hi`. Berfungsi sebagai safety guard untuk mencegah servo bergerak ke sudut di luar batas fisiknya akibat noise ADC. |
| `myservo.write(angle)` | Menggerakkan servo ke sudut yang ditentukan. Library Servo mengkonversi sudut menjadi lebar pulsa PWM secara otomatis. |
| `Serial.print(val)` | Mengirim data ke Serial Monitor tanpa newline di akhir baris. Digunakan untuk mencetak beberapa nilai dalam satu baris yang sama. |
| `Serial.println(val)` | Sama seperti `Serial.print()` namun menambahkan karakter newline (`\r\n`) di akhir sehingga output berpindah ke baris baru. |
| `delay(ms)` | Menghentikan eksekusi selama `ms` milidetik. Nilai 15ms memberikan waktu cukup bagi servo untuk mencapai posisi baru sebelum perintah berikutnya dikirim. |


