#include "HX711.h"

const int LoadCellDoutPin = 2;
const int LoadCellSckPin = 3;

HX711 scale;

void setup() {
  Serial.begin(9600);
  scale.begin(LoadCellDoutPin, LoadCellSckPin);
}

void loop() {

  if (scale.is_ready()) {
    Serial.println("Tempatkan objek dengan berat yang diketahui di timbangan.");
    delay(5000); 
    
    long tareValue = scale.tare(); 
    Serial.print("Nilai Tare: ");
    Serial.println(tareValue);

    Serial.println("Sekarang, letakkan objek dengan berat yang diketahui di timbangan.");
    delay(5000);
    
    float knownWeight = 100.0; 
    float scaleFactor = scale.get_units(10) / knownWeight; 
    scale.set_scale(scaleFactor); 
    
    Serial.println("Kalibrasi selesai!");
    Serial.print("Faktor Skala: ");
    Serial.println(scaleFactor);
  } else {
    Serial.println("Error: Tidak dapat mengakses sensor HX711.");
  }

  delay(1000); 
}