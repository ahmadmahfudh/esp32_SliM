

















#define BLYNK_TEMPLATE_ID "TMPL6qolzS8L6"
#define BLYNK_TEMPLATE_NAME "SLiM"
#define BLYNK_AUTH_TOKEN "fljS8nOZPmG0uk2Rm1Fu0oXEL0mL6iTQ"
#define BLYNK_PRINT Serial

#include <WiFiClient.h>
#include <WiFi.h>
#include <BlynkSimpleEsp32.h>

// HardwareSerial percent(18, 19);

char auth[] = "fljS8nOZPmG0uk2Rm1Fu0oXEL0mL6iTQ"; // Ganti dengan token autentikasi Blynk Anda
char ssid[] = "4G-UFI-FF6"; // Ganti dengan SSID WiFi Anda
char pass[] = "1234567890"; // Ganti dengan kata sandi WiFi Anda

void setup() {
  Serial.begin(9600);
  Serial2.begin(9600, SERIAL_8N1, 32, 33);
  // percent.begin(9600, SERIAL_8N1, 18, 19);
  Blynk.begin(auth, ssid, pass, "blynk.cloud", 80); // Mulai koneksi WiFi dan Blynk
  
  // Tunggu sampai koneksi Blynk berhasil
  while (Blynk.connect() == false) {}

  Serial.println("Connected to Blynk");

  // Tampilkan informasi jaringan WiFi
  Serial.print("Connected to WiFi network: ");
  Serial.println(WiFi.SSID());
  Serial.print("IP address: ");
  Serial.println(WiFi.localIP());
}

void loop() {
  // Jalankan fungsi Blynk
  Blynk.run();

  // Memeriksa apakah ada data yang tersedia di Serial2
  if (Serial2.available()) {
    // Membaca data sampai newline
    String dataString = Serial2.readStringUntil(',');
    String datastringpercent = Serial2.readStringUntil('\n');
    
    // Konversi String ke integer
    int dataInteger = dataString.toInt();
    int percentValue = datastringpercent.toInt();
    
    // Menampilkan data di serial monitor
    Serial.println(dataInteger);
    Serial.println(percentValue);
    
    // Menulis data ke Virtual Pin Blynk
    Blynk.virtualWrite(V1, dataInteger);
    Blynk.virtualWrite(V0, dataString);
    
    // Menggunakan nilai integer untuk logika kondisi
    if (dataInteger >= 0 && dataInteger <= 2) {
      Blynk.logEvent("code_1");
    } else if (dataInteger >= 2 && dataInteger <= 4) {
      Blynk.logEvent("code_2");
    } else if (dataInteger >= 4 && dataInteger <= 6) {
      Blynk.logEvent("code_3");
    }
    
    // Menulis data persen ke Virtual Pin Blynk
    Blynk.virtualWrite(V2, percentValue);
    
    // Menggunakan nilai integer untuk logika kondisi
    if (percentValue >= 20 && percentValue <= 50) {
      Blynk.logEvent("moisture_1");
    } else if (percentValue >= 51 && percentValue <= 70) {
      Blynk.logEvent("moisture_2");
    } else if (percentValue >= 71 && percentValue <= 100) {
      Blynk.logEvent("moisture_3");
    }
  }
}
