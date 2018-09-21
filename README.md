# Configuración NodeMCU

1.  Instalar [Arduino 1.6.9](https://www.arduino.cc/en/Main/Software) (Versiones nuevas dan problemas :c)
2.  Configurar el IDE con [Arduino ESP8266 core](https://github.com/esp8266/Arduino#installing-with-boards-manager)
3.  Descargar la librería [FirebaseArduino library](https://github.com/googlesamples/firebase-arduino/archive/master.zip)
4.  Iniciar Arduino IDE
5.  "Clic" `Sketch > Include Library > Add .ZIP Library...`
6.  Elegir `firebase-arduino-master.zip` en el directorio que se descargó.
7. Añadir la librería ArduinoJson v5.13.1 (para evitar problemas).

# Demo enviando datos a firebase

Abrir la demo que viene en los ejemplos.

> File>**Examples**>Firebase Arduino>**Firebase_Demo_ESP8266**.

~~~
#include <ESP8266WiFi.h>
#include <FirebaseArduino.h>

// Set these to run example.
#define FIREBASE_HOST "proyecto"
#define FIREBASE_AUTH "clave sercreta"
#define WIFI_SSID "Wifi"
#define WIFI_PASSWORD "claveWifi"

void setup() {
  Serial.begin(9600);

  // connect to wifi.
  WiFi.begin(WIFI_SSID, WIFI_PASSWORD);
  Serial.print("connecting");
  while (WiFi.status() != WL_CONNECTED) {
    Serial.print(".");
    delay(500);
  }
  Serial.println();
  Serial.print("connected: ");
  Serial.println(WiFi.localIP());
  
  Firebase.begin(FIREBASE_HOST, FIREBASE_AUTH);
}

int n = 0;

void loop() {
  // set value
  Firebase.setFloat("number", 42.0);
  // handle error
  if (Firebase.failed()) {
      Serial.print("setting /number failed:");
      Serial.println(Firebase.error());  
      return;
  }
  delay(1000);
  
  // update value
  Firebase.setFloat("number", 43.0);
  // handle error
  if (Firebase.failed()) {
      Serial.print("setting /number failed:");
      Serial.println(Firebase.error());  
      return;
  }
  delay(1000);

  // get value 
  Serial.print("number: ");
  Serial.println(Firebase.getFloat("number"));
  delay(1000);

  // remove value
  Firebase.remove("number");
  delay(1000);

  // set string value
  Firebase.setString("message", "hello world");
  // handle error
  if (Firebase.failed()) {
      Serial.print("setting /message failed:");
      Serial.println(Firebase.error());  
      return;
  }
  delay(1000);
  
  // set bool value
  Firebase.setBool("truth", false);
  // handle error
  if (Firebase.failed()) {
      Serial.print("setting /truth failed:");
      Serial.println(Firebase.error());  
      return;
  }
  delay(1000);

  // append a new value to /logs
  String name = Firebase.pushInt("logs", n++);
  // handle error
  if (Firebase.failed()) {
      Serial.print("pushing /logs failed:");
      Serial.println(Firebase.error());  
      return;
  }
  Serial.print("pushed: /logs/");
  Serial.println(name);
  delay(1000);
}
~~~

Sólo se deben definir lo siguiente y andara todo ok!

~~~
#define FIREBASE_HOST "proyecto"
#define FIREBASE_AUTH "clave sercreta"
#define WIFI_SSID "Wifi"
#define WIFI_PASSWORD "claveWifi"
~~~