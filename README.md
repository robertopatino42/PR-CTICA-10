# PR-CTICA-10

INTRODUCCIÓN

El ejercicio consiste únicamente en hacer conexion a un servidor publico y encendidio de un led.

Descripción La Esp32 la utilizamos en un entorno de adquision de datos, lo cual en esta practica solo ocuparemos un led; Esta practica se usara un simulador llamado WOKWI, junto con NodeRed.

Material Necesario Para realizar esta practica necesitas lo siguiente: -Abrir Node.js -Abrir WOKWI colocar; Tarjeta ESP 32 Led Resistencia

INSTRUCCIONES

1- Abrir la terminal de programación en WOKWI y colocar la siguente codigo:

#include <WiFi.h>
#include <PubSubClient.h>

const char* ssid = "Wokwi-GUEST";
const char* password = "";
const char* mqttServer = "44.195.202.69";
const int mqttPort = 1883;
const char* mqttUser = "diegojm";
const char* mqttPassword = "1234";
const char* mqttTopic = "diegoled";

WiFiClient espClient;
PubSubClient client(espClient);

int ledPin = 13; // Pin del LED

void setup() {
  pinMode(ledPin, OUTPUT);
  Serial.begin(115200);
  setup_wifi();
  client.setServer(mqttServer, mqttPort);
  client.setCallback(callback);
}

void loop() {
  if (!client.connected()) {
    reconnect();
  }
  client.loop();
}

void setup_wifi() {
  delay(10);
  Serial.println();
  Serial.print("Conectando a: ");
  Serial.println(ssid);
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
  Serial.println("");
  Serial.println("Conectado a la red WiFi");
}

void reconnect() {
  while (!client.connected()) {
    Serial.print("Intentando conexión MQTT...");
    if (client.connect("ESP32Client", mqttUser, mqttPassword)) {
      Serial.println("Conectado");
      client.subscribe(mqttTopic);
    } else {
      Serial.print("Error de conexión, rc=");
      Serial.print(client.state());
      Serial.println(" Intentando de nuevo en 5 segundos");
      delay(5000);
    }
  }
}

void callback(char* topic, byte* payload, unsigned int length) {
  Serial.print("Mensaje recibido: [");
  Serial.print(topic);
  Serial.print("] ");
  for (int i = 0; i < length; i++) {
    Serial.print((char)payload[i]);
  }
  Serial.println();

  if (strcmp(topic, mqttTopic) == 0) {
    if ((char)payload[0] == '1') {
      digitalWrite(ledPin, HIGH);
    } else {
      digitalWrite(ledPin, LOW);
    }
  }

  2- Instalar librerías.

  ![Captura de pantalla 2024-01-27 013514](https://github.com/robertopatino42/PR-CTICA-10/assets/153964688/6e6fd812-d04e-47bb-82e5-eb67a2955e83)

  3- Realizar las conexiones entre el Led y la tarjeta ESP32

  ![Captura de pantalla 2024-01-27 013835](https://github.com/robertopatino42/PR-CTICA-10/assets/153964688/9518e73d-e100-4187-be35-da1cf4e2b2c6)

  4- En Node red colocaremos los siquientes bloques que se requieren y nos aseguraremos de tener el mismo IP que en el codigo de WOKWI ademas de colocar correctamente el nombre del topico

  5- Hacemos correr la programacion en WOKWI asegurandonos que se conecto de manera correcta. Finalmente el NODE-RED y la ESP32 dentro de WOKWI arrojaran los resultados deseados:

  ![Captura de pantalla 2024-01-27 013835](https://github.com/robertopatino42/PR-CTICA-10/assets/153964688/6e7e292f-df06-469b-8131-f9370a789f03)

  

}
