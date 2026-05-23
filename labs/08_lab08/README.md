# Proyecto Final - Entrega Final: Reconocimiento de impurezas en banda transportadora
 
## Objetivos
 
* Integrar la cámara de la **Raspberry Pi** con un sistema de detección de impurezas en tiempo real sobre una banda transportadora.
* Implementar una solución de reconocimiento visual utilizando la técnica que el grupo considere más adecuada.
* Implementar la lógica de toma de decisión para detener la banda al detectar una impureza.
* Construir una interfaz de monitoreo en **Node-RED** que muestre el video en vivo y el estado del sistema.
* Exponer la interfaz públicamente mediante **Cloudflare Tunnel** o **ngrok** y compartir el enlace.
## Introducción
 
En esta entrega final se integran todos los elementos trabajados durante el curso: visión artificial, procesamiento de imágenes, control de hardware y visualización remota con **Node-RED**.
 
El sistema debe ser capaz de inspeccionar en tiempo real el contenido de una banda transportadora, identificar elementos que no correspondan al producto (impurezas), tomar una decisión de descarte y reflejar todo esto en una interfaz web accesible desde cualquier lugar.
 
Cada grupo es libre de elegir la técnica de reconocimiento que considere más adecuada, ya sea mediante filtros de procesamiento de imagen (HSV, Canny, contornos, etc.) o mediante el entrenamiento de una red neuronal.



## Requisitos del sistema
 
| Componente | Descripción |
|------------|-------------|
| Raspberry Pi | Con cámara conectada y funcionando |
| Cámara | Raspberry Pi Camera REV 1.3 o compatible |
| ESP32 | Encargado del control del motor de la banda transportadora |
| Banda transportadora | Con motor controlado por el ESP32 |
| Interfaz (Node-RED) | Instalado y corriendo en la Raspberry Pi |
| Cloudflare Tunnel o ngrok | Para exponer la interfaz públicamente |
 
## Entregables
 
Cada grupo debe presentar en la evaluación los siguientes elementos:
 
### 1. Script de detección
 
Script en Python desarrollado mediante **POO**, que realice:
 
* Captura de frames en tiempo real desde la cámara.
* Análisis de cada frame para detectar la presencia de impurezas.
* Publicación del resultado y del frame procesado vía **MQTT** hacia Node-RED.
* Envío de la señal de control al **ESP32** para detener o reanudar la banda según el resultado del análisis.
La técnica de detección es libre: filtros de procesamiento de imagen, red neuronal entrenada, o cualquier otra aproximación que el grupo justifique.
 
### 2. Firmware del ESP32
 
Código en el **ESP32** que:
 
* Reciba la señal de control desde la Raspberry Pi vía **MQTT**.
* Controle el motor de la banda transportadora en función de la señal recibida (detener / reanudar).


### 3. Interfaz
 
Dashboard construido en **Node-RED** o a elección del grupo que cumpla:
 
* Video en vivo con las anotaciones del sistema (frame procesado).
* Estado actual de la banda: **EN MARCHA** / **DETENIDA**.
* Indicador visual de impureza detectada.


### 4. URL pública activa (Opcional)
 
La interfaz accesible desde cualquier red mediante **Cloudflare Tunnel** o **ngrok**. La URL debe ser compartida con el docente antes de iniciar la exposición y mantenerse activa durante toda la evaluación.
 
### 5. Demostración en vivo
 
Durante la exposición el grupo debe demostrar el funcionamiento completo del sistema:
 
* Pasar el producto correcto por la banda y verificar que continúa en marcha.
* Pasar una impureza y verificar que la banda se detiene y la interfaz lo refleja.


### 6. Sustentación
 
Explicación ante el grupo de:
 
* La técnica de detección elegida y el por qué de esa elección.
* Las dificultades encontradas y cómo fueron resueltas.
* Las limitaciones del sistema implementado.



### 7. Entregables

1. Código fuente: Subir al repositorio todos las fuentes empleadas por el grupo en el sistema de reconocimiento de impurezas (ejemplo: python, C/C++, json, javascript, etc) 

2. Documentación: En el respectivo ```README.md``` de Github Classroom escribir una documentación técnica describiendo el procedimiento llevado a cabo en el proyecto y los resultados obtenidos o un archivo PDF en su lugar.

 
