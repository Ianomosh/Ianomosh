#include "DHT.h"

#define DHTPIN 2     // Pin where the DHT11 sensor is connected
#define DHTTYPE DHT11   // DHT 11 
DHT dht(DHTPIN, DHTTYPE);

int relayPin = 8;   // Relay controlling the fan
float temperatureThreshold = 30.0;  // Temperature threshold for fan (e.g., 30°C)

void setup() {
  Serial.begin(9600);
  dht.begin();
  
  pinMode(relayPin, OUTPUT);
  digitalWrite(relayPin, HIGH);  // Start with the relay off
}

void loop() {
  // Reading temperature
  float temp = dht.readTemperature();

  // Check if the reading failed
  if (isnan(temp)) {
    Serial.println("Failed to read from DHT sensor!");
    return;
  }

  // Print temperature to the Serial Monitor
  Serial.print("Temperature: ");
  Serial.print(temp);
  Serial.println(" °C");

  // If temperature exceeds threshold, turn on fan (relay on)
  if (temp > temperatureThreshold) {
    digitalWrite(relayPin, LOW);   // Turn the relay ON
    Serial.println("Fan ON");
  } else {
    digitalWrite(relayPin, HIGH);  // Turn the relay OFF
    Serial.println("Fan OFF");
  }

  delay(2000);  // Wait 2 seconds before reading again
}
