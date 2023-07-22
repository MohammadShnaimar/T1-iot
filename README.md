![image](https://github.com/MohammadShnaimar/T1-iot/assets/139280577/8c66cb1f-d19e-4f54-906c-a4eab2ae6f5b)



#include <WiFi.h>
#include <HTTPClient.h>

const char* ssid = "Wokwi-GUEST";
const char* password = "";

const String url = "https://raw.githubusercontent.com/MohammadShnaimar/T1-iot/main/S.html";
String payload = "";
//https://raw.githubusercontent.com/MohammadShnaimar/T1-iot/main/F.html
//https://raw.githubusercontent.com/MohammadShnaimar/T1-iot/main/T.html
//https://raw.githubusercontent.com/MohammadShnaimar/T1-iot/main/R.html
//can use anyone to test
// Define LED pins
const int ledPinF1 = 25;  // LED for letter F
const int ledPinF2 = 26;  // LED for letter R
const int ledPinS1 = 27;  // LED for letter S
const int ledPinT1 = 33;  // LED for letter T

void setup() {
  Serial.begin(115200);
  WiFi.begin(ssid, password);
  pinMode(ledPinF1, OUTPUT);
  pinMode(ledPinF2, OUTPUT);
  pinMode(ledPinS1, OUTPUT);
  pinMode(ledPinT1, OUTPUT);

  Serial.print("Connecting to WiFi");
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }

  Serial.print("OK! IP=");
  Serial.println(WiFi.localIP());

  Serial.print("Fetching " + url + "... ");
}

void turnOffAllLEDs() {
  digitalWrite(ledPinF1, LOW);
  digitalWrite(ledPinF2, LOW);
  digitalWrite(ledPinS1, LOW);
  digitalWrite(ledPinT1, LOW);
}

void loop() {
  HTTPClient http;
  http.begin(url);
  int httpResponseCode = http.GET();
  if (httpResponseCode > 0) {
    Serial.print("HTTP ");
    Serial.println(httpResponseCode);
    payload = http.getString();
    Serial.println();
    Serial.println(payload);
    
    if (payload == "F") {
      // Turn on the LEDs for the letter "F"
      digitalWrite(ledPinF1, HIGH);
      digitalWrite(ledPinF2, LOW);
      digitalWrite(ledPinS1, LOW);
      digitalWrite(ledPinT1, LOW);
    } else if (payload == "S") {
      // Turn on the LED for the letter "S"
      digitalWrite(ledPinF1, LOW);
      digitalWrite(ledPinF2, LOW);
      digitalWrite(ledPinS1, HIGH);
      digitalWrite(ledPinT1, LOW);
      } else if (payload == "R") {
      // Turn on the LED for the letter "R"
      digitalWrite(ledPinF1, LOW);
      digitalWrite(ledPinF2, HIGH);
      digitalWrite(ledPinS1, LOW);
      digitalWrite(ledPinT1, LOW);
    } else if (payload == "T") {
      // Turn on the LED for the letter "T"
      digitalWrite(ledPinF1, LOW);
      digitalWrite(ledPinF2, LOW);
      digitalWrite(ledPinS1, LOW);
      digitalWrite(ledPinT1, HIGH);
    } else {
      // Turn off all LEDs for unrecognized letters or other responses
      turnOffAllLEDs();
    }
  } else {
    Serial.print("Error code: ");
    Serial.println(httpResponseCode);
    Serial.println(":-(");
  }
  
  http.end();
  delay(1000); // Wait for 1 second before making the next HTTP request
}
