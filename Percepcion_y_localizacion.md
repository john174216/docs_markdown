# Percepción y Localización

## 1. Objetivos
* Implementar la adquisición de datos desde sensores como cámaras, LiDAR, entre otros.
* Implementar algoritmos de fusión de sensores para combinar información redundante o complementaria.
* Generación de mapas locales o globales como mapas 3D o point clouds
* Desarrollar algoritmos para detección de obstáculos, desniveles o
características del terreno como rugosidad, inclinación y tipo de superficie.

## 2. Conceptos
* Percepción es comprender el entorno basado en mediciones
* Involucra modelos: Construcción, interpretación, mejora, expansión
* La obtención de modelos se basa en sensores
* Se usan sensores propioceptivos y exteroceptivos
* Los sensores propioceptivos miden valores internos del robot y se usan en control de bajo nivel
* Los sensores exteroceptivos miden valores del entorno del robot y se utilizan en control de alto nivel.

## 3. Tecnologías / Estado del arte
* **Sensores Propioceptivos**
    * De sensores en motores, se pueden utilizar encoders (absolutos,
incrementales) y para velocidad de giro del eje mediante tacómetros
    * Para posición se puede usar GPS, USBL, beacons de RF, sistema de captura de movimiento, barómetro.
    * Para orientación e inclinación se puede usar magnetómetros
    * Para velocidad angular se puede usar giroscopios
    * Para velocidad lineal se puede usar sensores de Efecto Doppler
    * Para aceleración lineal se pueden usar acelerómetros

* **Sensores Extereoceptivos**
    * Tambien se pueden usar los basados en tiempo de vuelo (ToF): Ultrasonido, Cámaras de ToF, Radar y LiDAR.
    * Para detectar características del entorno se pueden usar Cámaras RGB, Estéreo y catadióptricas
    * Se puede aplicar el filtro de Kalman para la estimación de velocidad y aceleración a partir de posición

* Para **Fusión Sensorial** se pueden utilizar técnicas como:
    * Combinación de LiDAR, IMU, cámaras RGB-D/estéreo y otros sensores de ultrasonido.
    * Técnicas como Filtro de Kalman Extendido, fusión basada en gráficas o Deep Sensor Fusion que permiten una percepción robusta

*Para **Percepción Semántica** se puede emplear:
    * Uso de redes neuronales (CNNs, Transformers) para detectar y clasificar elementos del entorno como personas, herramientas, pasajes o señales.
    * Aplicaciones como segmentación semántica 3D con cámaras o LiDAR.
    * Algoritmos como Mask R-CNN o YOLO.

* Para compartir datos con otras áreas, se pueden usar ciertos tipos de datos en ROS2:
    * `nav_msgs/OccupancyGrid` o `nav_msgs/MapMetaData` para mapas
    * `nav_msgs/Odometry` para odometría
    * `geometry_msgs/PoseStamped` o `PoseWithCovarianceStamped` para la posición del robot
    * `sensor_msgs/PointCloud2` para nubes de puntos

* Diferencias entre cámara Estereo y RGB-D
| Característica              | **Cámara RGB-D**                                                               | **Cámara estereoscópica**                              |
| --------------------------- | ------------------------------------------------------------------------------ | ------------------------------------------------------ |
| **Profundidad (Depth)**     | Se obtiene mediante sensores activos (infrarrojos, láser estructurado, o TOF). | Se calcula por triangulación pasiva entre dos cámaras. |
| **Iluminación necesaria**   | Funciona bien en interiores con poca luz.                                      | Requiere buena iluminación para detectar texturas.     |
| **Ejemplos**                | Intel RealSense D435, Orbbec Astra, Kinect.                                    | ZED, Stereolabs ZED2, MYNT Eye.                        |
| **Precisión en interiores** | Alta.                                                                          | Buena, pero depende del algoritmo estéreo.             |
| **Rango útil**              | Corto-mediano (0.2–5 m típico).                                                | Mayor, según separación de lentes (baseline).          |
| **Costo computacional**     | Bajo (ya entrega el depth procesado).                                          | Alto (hay que correr algoritmo estéreo en CPU/GPU).    |
| **Tipo de sensor**          | Activo.                                                                        | Pasivo (dos sensores RGB).                             |


| Característica           | OAK-D Lite (Estéreo)                              | RGB-D (como RealSense D435i)         |
| ------------------------ | ------------------------------------------------- | ------------------------------------ |
| Fuente de profundidad    | Cálculo por triangulación estéreo                 | Luz infrarroja + sensor IR           |
| Iluminación activa       | ❌ No (usa luz ambiente)                           | ✅ Sí (usa proyector infrarrojo)      |
| Precisión en distancias  | Buena, pero puede verse afectada por poca textura | Alta, incluso en escenas sin textura |
| Interiores oscuros       | Regular                                           | Muy buena                            |
| Exteriores con luz solar | Mejor (no depende de IR)                          | Puede fallar (saturación IR)         |

* **Algoritmos y frameworks**
| Herramienta / Framework   | Descripción breve                               |
| ------------------------- | ----------------------------------------------- |
| **RTAB-Map**              | SLAM RGB-D / LiDAR con capacidad semántica.     |
| **LIO-SAM**               | SLAM LiDAR + IMU para entornos 3D complejos.    |
| **Cartographer (Google)** | SLAM 2D/3D robusto, funciona con ROS2.          |
| **ORB-SLAM3**             | V-SLAM monocular, estéreo, o RGB-D.             |
| **FAST-LIO2**             | SLAM ultra rápido con LiDAR + IMU.              |
| **Kimera**                | Reconstrucción semántica + SLAM en tiempo real. |
| **OctoMap**               | Representación 3D jerárquica del entorno.       |
| **Nav2**                  | Sistema completo de navegación en ROS 2.        |

## 4. Equipos
* Cámara Intel® RealSense™ depth camera D435i - [RGB-D](https://tiendamia.com/pe/producto?amz=B0752CTSGD)
* Cámara OAK-D Lite - [Estéreo](https://arteus.pe/products/ubiquiti-locom2-nanostation-airmax-locom2-cpe-hasta-150-mbps-frecuencia-2-ghz-2412-2462-mhz-con-antena-integrada-de-8-dbi?srsltid=AfmBOoo6EFRcyCYr_JQsBFI25YdWthLGxmDeiysaxI71t-NhPGL1tSmX)
* Unitree 4D LiDAR L2 - [LiDAR](https://mtlab.pe/producto/pantalla-nextion-discovery-2-4-2-8-3-5-hmi-tactil-resistivo/?srsltid=AfmBOori3gilh8l7x1lkFazwUiYRDs4wsNZnnPL4dXag-zkqk4xFQKcK)
* FLIR Lepton (3.5 o 3.0) o Seek Thermal CompactPRO

![Comparación entre sensores](/image/sample.webp "Diferencias entre sensores")

## 5. Propuestas

* ### Primera propuesta
    *   Mediante sensor LiDAR, obtener nubes de puntos

    ![01](/public/img_percepcion/01.webp "01")
    
    *   Procesar, segmentar y agrupar nube de puntos (algoritmo RANSAC)

    ![02](/public/img_percepcion/02.webp "02")

    *   Encuadrar nube de puntos con cuadros delimitadores 3D

    ![03](/public/img_percepcion/03.webp "03")

    *   Con cámara RGB-D, se puede hacer Early Sensor Fusion
        *   Consiste en fusionar los datos sin procesar de los sensores, por ejemplo proyectar las nubes de puntos LiDAR (3D) en la imagen 2D. Luego comprobamos si las nubes de puntos pertenecen a cuadros delimitadores 2D detectados con la cámara.

        ![04](/public/img_percepcion/04.webp "04")

    *   Con cámara Estereo, se puede Late Sensor Fusion

    ![05](/public/img_percepcion/05.webp "05")

    *   Se hace IOU Matching con cualquiera de ambos enfoques (Detección de objetos)
        *   Si los cuadros delimitadores de CAMERA y LiDAR se superponen, en 2D o 3D, consideramos que ese obstáculo es el mismo.

        ![06](/public/img_percepcion/6.webp "06")

    *   Luego se puede hacer Tracking de esos objetivos con Filtro de Kalman (EKF, UKF) (Seguimiento de objetivos 3D)

        ![07](/public/img_percepcion/7.webp "07")
