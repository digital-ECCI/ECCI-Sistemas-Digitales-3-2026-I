# Proyecto Final - Entrga 1: Configuración y uso de la cámara en Raspberry Pi con OpenCV

## Objetivos

* Configurar y verificar el funcionamiento de la cámara en la **Raspberry Pi**.
* Instalar y utilizar **OpenCV** para captura y procesamiento de imágenes.
* Implementar un script de Python que aplique un filtro de procesamiento de imagen asignado.


## Introducción

En esta primera entrega de proyecto se aprenderá a capturar imágenes desde la cámara de la Raspberry Pi y procesarlas en tiempo real usando la librería **OpenCV**. Cada grupo implementará un filtro de procesamiento de imagen diferente, lo que permitirá comparar distintas técnicas de visión artificial aplicadas sobre el mismo hardware.

Este tipo de procesamiento es la base de sistemas de inspección visual, como la detección de impurezas en bandas transportadoras, reconocimiento de objetos, y control de calidad industrial.

## Asignación de filtros por grupo

| Grupo | Filtro asignado |
|-------|----------------|
| 1 | Detección de contornos |
| 2 | Threshold adaptativo |
| 3 | Histograma equalizado |
| 4 | Bypass (imagen original) |
| 5 | Canny |
| 6 | Rotación y flip |
| 7 | Escala de grises |
| 8 | Filtro de color HSV |
| 9 | Dilatación y erosión |
| 10 | Blur gaussiano |
| 11 | Threshold binario |
| 12 | Laplaciano |

## Procedimiento

### 1. Verificación de la cámara

Antes de comenzar, verificar que la cámara está correctamente conectada y funciona:

```bash
rpicam-still -o test.jpg
```

Si el archivo `test.jpg` se genera correctamente, la cámara está lista.

### 2. Instalación de dependencias

```bash
sudo apt update
sudo apt install -y python3-opencv python3-picamera2
```

Verificar la instalación:

```bash
python3 -c "import cv2; print(cv2.__version__)"
```

### 3. Script en python

Cada grupo debe desarrollar un script en python (POO), el cual debe permitir el procesamiento del filtro asignado en la sección **"asignacion de filtros"**.

### 4. Implementación del filtro asignado

En horario de clase de hara la evaluación correspondiente al funcionmaiento de cada uno de filtros asignados por grupo.

### 5. Ejecución del script
El escript debe poder ejecutarse de la siguiente manera:

```bash
python3 filtro_grupoN.py
```

> Reemplazar `N` con el número del grupo.

