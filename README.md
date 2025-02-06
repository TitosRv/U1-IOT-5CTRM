# Unidad 1 - IOT - 5 Cuatrimestre
## Parte Teórica (30 Puntos)
### Ejercicios de NetAcad Python Fundamentals 2
#### Modulo 1
![image](https://github.com/user-attachments/assets/0c8f6367-db9b-4af7-87c8-ac50b443fa6d)

#### Modulo 2
![image](https://github.com/user-attachments/assets/556b28e6-b112-4959-a928-1f0765f00a28)

#### Modulo 3
![image](https://github.com/user-attachments/assets/272ed3af-8d88-456b-9c91-133d298064f6)

#### Modulo 4
![image](https://github.com/user-attachments/assets/61f42f82-4c1f-423c-9e00-6366e12005eb)

#### Examen Final
![image](https://github.com/user-attachments/assets/3ce35986-5c8d-4a76-bd7b-41f9c0921be8)


## Parte Práctica (60 Puntos)
### Ejercicio 1: Almacenamiento de Datos (15 Puntos)
- *Diagrama de Conexión*:
  ![image](https://github.com/user-attachments/assets/bf273df3-f1d5-4380-a5cd-2c7c2850e42b)
- *Diagrama de NODE-RED*:
  ![image](https://github.com/user-attachments/assets/0abc5f9c-fdc0-4bac-b832-16c31e2852f2)

- *Código Documentado*:
#import para acceso a red
import network
#Para usar protocolo MQTT
from umqtt.simple import MQTTClient

#Importamos modulos necesarios
from machine import Pin
from time import sleep
from hcsr04 import HCSR04

#Propiedades para conectar a un cliente MQTT
MQTT_BROKER = "broker.emqx.io"
MQTT_USER = ""
MQTT_PASSWORD = ""
MQTT_CLIENT_ID = ""
MQTT_TOPIC = "utng/sensors"
# MQTT_TOPIC_PUBLISH = "CAMBIAR_POR_TU_TOPICO"

MQTT_PORT = 1883

#Creo el objeto que me controlará el sensor
sensor=HCSR04(trigger_pin=15, echo_pin=4, echo_timeout_us=24000)

#Declaro el pin led
led = Pin(2, Pin.OUT)
led.value(0)

#Función para conectar a WiFi
def conectar_wifi():
    print("Conectando...", end="")
    sta_if = network.WLAN(network.STA_IF)
    sta_if.active(True)
    sta_if.connect('Wokwi-GUEST', '')
    while not sta_if.isconnected():
        print(".", end="")
        sleep(0.3)
    print("WiFi Conectada!")

#Funcion para subscribir al broker, topic
def subscribir():
    client = MQTTClient(MQTT_CLIENT_ID,
    MQTT_BROKER, port=MQTT_PORT,
    user=MQTT_USER,
    password=MQTT_PASSWORD,
    keepalive=0)
    client.set_callback(llegada_mensaje)
    client.connect()
    client.subscribe(MQTT_TOPIC)
    print("Conectado a %s, en el topico %s"%(MQTT_BROKER, MQTT_TOPIC))
    return client

#Funcion encargada de encender un led cuando un mensaje se lo diga
def llegada_mensaje(topic, msg):
    print("Mensaje:", msg)
    if msg == b'true':
        led.value(1)
    if msg == b'false':
        led.value(0)

#Conectar a wifi
conectar_wifi()
#Subscripción a un broker mqtt
client = subscribir()

distancia_anterior = 0
#Ciclo infinito
while True:
    client.check_msg()
    distancia=int(sensor.distance_cm())
    if distancia != distancia_anterior:
        print(f"La distancia es {distancia} cms.")
        client.publish(MQTT_TOPIC, str(distancia))
    distancia_anterior = distancia
    sleep(2).
    
- *Video de Funcionamiento*:
  

### Ejercicio 2: Control de Actuadores (15 Puntos)
- *Diagrama de Conexión*: [Ver diagrama](/proyectos/ejercicio_2_control_actuadores/diagramas)
- *Código Documentado*: [Ver código](/proyectos/ejercicio_2_control_actuadores/codigo)
- *Video de Funcionamiento*: [Ver video](/proyectos/ejercicio_2_control_actuadores/videos)

### Ejercicios en Clase: Videos Demostrativos (10 Puntos)
- *CRUD en PostgreSQL*: [Ver video](/proyectos/ejercicios_clase/videos)
- *Instalaciones y Configuraciones Básicas*: [Ver video](/proyectos/ejercicios_clase/videos)
- *LED y Botón con Raspberry Pi*: [Ver video](/proyectos/ejercicios_clase/videos)
- *Conexión MQTT en Node-RED*: [Ver video](/proyectos/ejercicios_clase/videos)

### Ejercicios de Soldadura (20 Puntos)
#### Ejercicio 3: Circuito Funcional en Placa Fenólica (10 Puntos)
- *Demostración al Docente*: [Ver fotografía](/proyectos/soldadura/ejercicio_3_circuito_fenolica)
- *Fotografía en Repositorio*: [Ver fotografía](/proyectos/soldadura/ejercicio_3_circuito_fenolica)

#### Ejercicio 4: Figura 2D o 3D Soldada (10 Puntos)
- *Exposición al Docente*: [Ver fotografía](/proyectos/soldadura/ejercicio_4_figura_soldada)
- *Fotografía en Repositorio*: [Ver fotografía](/proyectos/soldadura/ejercicio_4_figura_soldada)

## Parte Socioafectiva (10 Puntos)
- *Puntualidad en la Entrega de Evidencias*: [Ver evidencias](#)
- *Colaboración Activa en Clase*: [Ver evidencias](#)
- *Organización y Claridad en el Repositorio*: [Ver repositorio](#)

## Herramientas y Tecnologías Utilizadas
- *Hardware*: Raspberry Pi, ESP32, Sensores (DHT11, LDR, etc.), Actuadores (LEDs, servomotores, etc.)
- *Software*: Arduino IDE, MicroPython, Node-RED, PostgreSQL, Mosquitto MQTT
- *Herramientas*: Protoboard, Cautín, Placa Fenólica, Multímetro

## Contribuciones
Este proyecto fue realizado por Carlos Benito Ramirez Vazquez como parte de la evaluación ordinaria de la asignatura *Aplicaciones de IoT*.
