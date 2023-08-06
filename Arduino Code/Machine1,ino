#include <stdio.h>
#include <math.h>
#include <SPI.h>
#include <Wire.h>
//-----1
//#include <Timer.h>
#include <Arduino.h>
#include <ESP8266WiFi.h>
#include <WiFiClientSecure.h>
#include <ESP8266WiFiMulti.h> //丟資料給某網站的程式庫
#include <ESP8266HTTPClient.h>
#include <SoftwareSerial.h>
//-----2
#include <MQUnifiedsensor.h>
#include <DHT.h>
#include <PMS.h>
//-----3
SoftwareSerial pmsSerial(4, 5);  //RX,TX。接到PMS3003的TX,RX
DHT dht(D6, DHT11);//DHT11
//
#ifndef STASSID
#define STASSID ssidname
#define STAPSK password
#endif
//Definitions
#define placa "ESP8266"
#define Voltage_Resolution 5
#define pin A0 //Analog input 0 of your arduino
#define type "MQ-131" //MQ131
#define ADC_Bit_Resolution 10 // For arduino UNO/MEGA/NANO
#define RatioMQ131CleanAir 15 //RS / R0 = 15 ppm
//#define calibration_button 13 //Pin to calibrate your sensor

//Declare Sensor
MQUnifiedsensor MQ131(placa, Voltage_Resolution, ADC_Bit_Resolution, pin, type);

const char* ssid = "";
const char* password = "";
String serverName = "http://210.60.151.33/Machine1.php";
String apiKeyValue = "tPmAT5Ab3j7F9";

#define SEALEVELPRESSURE_HPA (1013.25)
PMS pms(pmsSerial);
PMS::DATA dat;

unsigned long time_1 = 0;


void setup() {
  //Init the serial port communication - to debug the library
  Serial.begin(115200); //Init serial port
  // SSD1306_SWITCHCAPVCC = generate display voltage from 3.3V internally
  if (!display.begin(SSD1306_SWITCHCAPVCC, 0x3C)) {
    Serial.println(F("SSD1306 allocation failed"));
    for (;;); // Don't proceed, loop forever
  }
  display.clearDisplay();

  dht.begin();
  pmsSerial.begin(9600);
  Wire.begin(2, 0);
  starttime = millis();

  //Set math model to calculate the PPM concentration and the value of constants
  MQ131.setRegressionMethod(1); //_PPM =  a*ratio^b
  MQ131.setA(23.943); MQ131.setB(-1.11); // Configure the equation to to calculate O3 concentration
  MQ131.init();

  Serial.print("Calibrating please wait.");
  float calcR0 = 0;
  for (int i = 1; i <= 10; i ++)
  {
    MQ131.update(); // Update data, the arduino will read the voltage from the analog pin
    calcR0 += MQ131.calibrate(RatioMQ131CleanAir);
    Serial.print(".");
  }
  MQ131.setR0(calcR0 / 10);
  Serial.println("  done!.");

  if (isinf(calcR0)) {
    Serial.println("Warning: Conection issue, R0 is infinite (Open circuit detected) please check your wiring and supply");
    while (1);
  }
  if (calcR0 == 0) {
    Serial.println("Warning: Conection issue found, R0 is zero (Analog pin shorts to ground) please check your wiring and supply");
    while (1);
  }

  MQ131.serialDebug(true);
  Serial.println("Ignore Ratio = RS/R0, for this example we will use readSensorR0Rs, the ratio calculated will be R0/Rs. Thanks :)");

  //wifi
  WiFi.mode(WIFI_STA);
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.println(".");
  }
  Serial.println("");
  Serial.print("Connected to WiFi network with IP Address: ");
  Serial.println(WiFi.localIP());

  Serial.println("Timer set to 5 seconds (timerDelay variable), it will take 5 ");

}

void loop() {
  if (millis() > time_1 + INTERVAL_MESSAGE) {    //60000ms
    time_1 = millis();
    MQ131.update(); // Update data, the arduino will read the voltage from the analog pin
    MQ131.readSensorR0Rs(); // Sensor will read PPM concentration using the model, a and b values set previously or from the setup
    MQ131.serialDebug(); // Will print the table on the serial port

    //DHT
    Serial.print("Temperature: ");
    Serial.print(dht.readTemperature());
    Serial.println(" *C ");


    while (!pms.read(dat));
    Serial.print("PM 1.0 : ");
    Serial.print(dat.PM_AE_UG_1_0);
    Serial.println("(ug/m3)");
    Serial.print("PM 2.5 : ");
    Serial.print(dat.PM_AE_UG_2_5);
    Serial.println("(ug/m3)");
    Serial.print("PM 10.0 : ");
    Serial.print(dat.PM_AE_UG_10_0);
    Serial.println("(ug/m3)");
    Serial.println();

    //連線PHP伺服器
    if (WiFi.status() == WL_CONNECTED) {
      WiFiClient client;
      HTTPClient http;

      // Your Domain name with URL path or IP address with path
      http.begin(client, serverName);

      // Specify content-type header
      http.addHeader("Content-Type", "application/x-www-form-urlencoded");

      // Prepare your HTTP POST request data
      /*
        if ( concentrationPM10_ugm3 >3000000){
        namestr="---";
        Serial.print("str: ");
        Serial.println(namestr);
        }*/
      String httpRequestData = "api_key=" + apiKeyValue + "&name=" + "Mark2" + "&OC=" + dht.readTemperature() + "&O3(PPB)=" + MQ131.getO3(PPB) + "&O3(UG)=" + MQ131.getO3(UG_M3)  + "&PM25(UG)=" + dat.PM_AE_UG_2_5;
      Serial.print("httpRequestData: ");
      Serial.println(httpRequestData);

      int httpResponseCode = http.POST(httpRequestData);

      if (httpResponseCode > 0) {
        Serial.print("HTTP Response code: ");
        Serial.println(httpResponseCode);
      }
      else {
        Serial.print("Error code: ");
        Serial.println(httpResponseCode);
      }
      // Free resources
      http.end();
    }
    else {
      Serial.println("WiFi Disconnected");
    }
  }
}
