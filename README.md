# Interacción Humano-Robot (Máster en Robótica e Inteligencia Artificial)

## Laboratorio 01 (semana 06/05/2024)

## Objetivos

- Comprender la estructura y desarrollo de paquetes en ROS 2.
- Familiarizarse con la interpretación y procesamiento de datos del sensor de la cámara.
- Integrar sistemas existentes en proyectos de ROS 2.
- Integrar el procesamiento de información de objetos y personas en tiempo real.

## Requisitos

- [ROS 2 Humble](https://docs.ros.org/en/humble/Installation.html)
- [Nav2](https://navigation.ros.org/)
- [Simulador RB1](https://github.com/igonzf/ros2_rb1)
- [Ultralytics YOLOv8](https://github.com/ultralytics/ultralytics)
- [yolov8_ros](https://github.com/mgonzs13/yolov8_ros)

## Enunciado

Desarrolla un paquete ROS 2 que permita reconocer el objeto al que señala una persona como parte inicial de la prueba 'Carry My Luggage' en la [RoboCup](https://github.com/RoboCupAtHome/RuleBook).

## Ejercicio 1: Creación de un paquete ROS 2

Desarrolla un paquete ROS 2 en Python denominado `pointing_ros` para implementar funcionalidades de detección de gestos. Este paquete contendrá un nodo encargado de verificar si una persona está señalando un objeto específico.

1. [**Creación del paquete**](https://docs.ros.org/en/humble/How-To-Guides/Developing-a-ROS-2-Package.html):
   - Crea un nuevo paquete ROS 2 en Python llamado `pointing_ros`.
2. **Desarrollo del nodo de detección**:
   - En el directorio `pointing_ros/pointing_ros`, crea un nodo llamado `pointing_node.py` que llevará a cabo la función de comprobar si una persona está señalando un objeto.
3. **Configuración del paquete**:
   - Añade el nodo recién creado al archivo `setup.py` como un entry point para poder ejecutarlo.

## Ejercicio 2: Integración de YOLOv8

En este ejercicio, se hará uso del paquete [`yolov8_ros`](https://github.com/mgonzs13/yolov8_ros) para obtener información sobre los objetos detectados a través de la cámara. YOLOv8 permite la detección tanto de objetos como de personas.

1. **Uso de YOLOv8**:

   - Integra el paquete `yolov8_ros` en tu workspace de ROS 2. Asegúrate de comprender cómo se estructuran los datos que publica este paquete.
     Ten en cuenta que YOLOv8 maneja modelos diferentes para la detección de personas y objetos. Para evitar conflictos, asegúrate de lanzar los nodos relacionados con estos modelos con diferentes namespaces.

2. **Integración en el nodo `pointing_node`**:
   - Implementa en el `pointing_node` los subscriptores que permitan obtener los datos proporcionados por YOLOv8. Este nodo debe ser capaz de procesar tanto la información de detección de personas como la de objetos.

## Ejercicio 3: Implementación de Pointing

En este ejercicio, utiliza los datos de las detecciones proporcionadas por YOLOv8 para determinar si una persona está apuntando a un objeto. Una vez realizadas estas comprobaciones, deberás publicar la información relevante en un topic, incluyendo los detalles de la persona que está señalando y el objeto al que está señalando.

Las comprobaciones que debes llevar a cabo son las siguientes:

1. **Obtención de puntos de referencia del brazo**:

   - Extrae los puntos que representan la posición del brazo de la persona detectada. (Puedes tener en cuenta solo un brazo)

2. **Análisis de la dirección del brazo**:

   - Determina la dirección en la que están alineados estos puntos de referencia del brazo. Esto te permite identificar la dirección en la que la persona está señalando.
   - Para facilitar el desarrollo, publica en un topic denominado `dbg_pointing` la imagen de la cámara junto con la línea que represente la dirección del brazo por medio de [OpenCV](https://www.geeksforgeeks.org/python-opencv-cv2-line-method/).

3. **Correlación con las detecciones de objetos**:
   - Comprueba si la dirección identificada del brazo coincide con alguna de las detecciones de objetos proporcionadas por YOLOv8.

Una vez completadas estas funcionalidades, publica la información obtenida en un topic denominado `pointing/detections`.
