# Lab06: Servidor MQTT en Raspberry Pi con IP pública (Cloudflare Tunnel) y cliente ESP32 con sensor LM75

## Objetivos

* Configurar **Cloudflare Tunnel** en la **Raspberry Pi** para exponer el dashboard de **Node-RED** públicamente.

* Configurar **Serveo** en la **Raspberry Pi** para exponer el broker **MQTT** públicamente.

* Programar el **ESP32** en MicroPython para leer datos de temperatura desde el sensor **LM75**.

* Publicar las lecturas vía **MQTT** hacia el broker remoto.

* Visualizar los datos en el dashboard de **Node-RED** accesible por Internet.

## Introducción

En este laboratorio se aprenderá a publicar datos de sensores IoT desde un **ESP32** programado con MicroPython, hacia un broker **MQTT** remoto ubicado en una **Raspberry Pi**, y acceder al dashboard de **Node-RED** desde cualquier lugar a través de una URL pública generada con **Cloudflare Tunnel**.

A diferencia de ngrok, estas herramientas son completamente gratuitas y no requieren tarjeta de crédito ni registro previo.

La arquitectura general del sistema es la siguiente:

<p align="center"> <img src="/labs/figs/lab06/arch.png" alt="Arquitectura MQTT con Cloudflare Tunnel y ESP32" width=600> </p>

## Procedimiento

### 1. Parte 1: Generar una URL pública con Cloudflare Tunnel en la Raspberry Pi

#### 1.1 Instalar cloudflared

```bash
wget https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-arm -O cloudflared
chmod +x cloudflared
sudo mv cloudflared /usr/local/bin/
cloudflared --version
```

Se debe ver algo tipo:

```
cloudflared version 2026.3.0
```

#### 1.2 Iniciar Node-RED

Antes de crear el túnel, Node-RED debe estar corriendo:

```bash
node-red
```

Dejar esta terminal abierta.

#### 1.3 Exponer Node-RED (puerto `1880`)

Abrir una segunda terminal y ejecutar:

```bash
cloudflared tunnel --url http://localhost:1880
```

Se debe ver algo tipo:

```
+--------------------------------------------------------------------------------------------+
|  Your quick Tunnel has been created! Visit it at (it may take some time to be reachable):  |
|  https://activation-chapters-participating-flooring.trycloudflare.com                      |
+--------------------------------------------------------------------------------------------+
```

Usar esa URL HTTPS pública para acceder al dashboard de Node-RED desde cualquier lugar.

> **Nota:** La URL cambia cada vez que se reinicia el túnel. Copiarla apenas aparezca y compartirla con el equipo.

#### 1.4 Exponer Mosquitto/MQTT (puerto `1883`) con Serveo

Cloudflare Tunnel no soporta túneles TCP en modo gratuito, por lo que para MQTT se usa **Serveo**, que no requiere instalación:

```bash
ssh -R 1883:localhost:1883 serveo.net
```

Se debe ver algo tipo:

```
Forwarding TCP connections from serveo.net:1883
```

Esa dirección (`serveo.net`) y puerto (`1883`) serán los que configurará el ESP32 para conectarse al broker remoto.

> **Consejo:** Mantener tres terminales abiertas: una con `node-red`, otra con `cloudflared` (Node-RED) y otra con `ssh` (MQTT).

---

### 2. Parte 2: Sensor LM75 en el microcontrolador comunicando con el broker MQTT remoto

En esta parte se configurará el microcontrolador como cliente MQTT, encargado de leer la temperatura medida por el sensor LM75 y publicarla periódicamente hacia el broker Mosquitto que se ejecuta en la Raspberry Pi.

#### 2.1 Conexión del sensor LM75

| Pin LM75 | ESP32   |
| -------- | ------- |
| VCC      | 3.3 V   |
| GND      | GND     |
| SDA      | GPIO 21 |
| SCL      | GPIO 22 |

<p align="center"> <img src="/labs/figs/lab06/sensor-esp32.png" alt="Conexión LM75 con ESP32" width=400> </p>

#### 2.2 Programar el microcontrolador

Crear el código en el microcontrolador usando Thonny u otra herramienta. Este programa realiza las siguientes acciones:

* Se conecta a la red Wi-Fi.
* Establece comunicación I²C con el LM75.
* Lee la temperatura cada 5 s.
* Publica los valores en un topic MQTT (`in/micro/sensor/temperatura`).

En la configuración del broker MQTT usar los datos que entregó Serveo:

```python
mqtt_server = "serveo.net"
mqtt_port   = 1883
```

#### 2.3 Verificación del envío de datos

* Abrir **Node-RED** desde la URL pública HTTPS que generó Cloudflare Tunnel.
* Crear un nodo `mqtt in` configurado con:
  * Servidor: `serveo.net`
  * Puerto: `1883`
  * Topic: `in/micro/sensor/temperatura`
* Conectar ese nodo a un nodo `thermometer` para visualizar la temperatura. Si no lo tienen instalado, hacerlo desde el `Palette` como se explicó en el [lab04](/labs/04_lab04/README.md).
* Verificar que los datos del sensor aparecen en el dashboard.
