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

* Diferencias entre las camaras que se tienen

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

* **Datasets Públicos**

| Nombre del Dataset | Descripción | Aplicaciones | Formato de Etiquetas | Acceso |
|--------------------|-------------|---------------|-----------------------|--------|
| **DsLMF+** | 138,004 imágenes con categorías como personal, cascos, comportamiento y maquinaria. | Detección de anomalías y monitoreo de seguridad en minas subterráneas. | YOLO, COCO | [Figshare](https://springernature.figshare.com/articles/dataset/An_open_dataset_for_intelligent_recognition_and_classification_of_abnormal_condition_in_longwall_mining/22654945?backTo=%2Fcollections%2FDsLMF_An_open_dataset_for_intelligent_recognition_of_abnormal_condition_in_underground_longwall_mining_face%2F6307599&file=40215118) |
| **DsDPM 66** | 105,096 imágenes de operaciones de perforación, con etiquetas como tuberías, taladros y EPP. | Monitoreo inteligente y detección de anomalías en perforación. | YOLO, COCO | [1:Mineros, cascos, EPPs](https://figshare.com/articles/dataset/_b_An_Open_Paradigm_Dataset_for_Intelligent_Monitoring_of_Underground_Drilling_Scenarios_in_Coal_Mines_b_/26135107/1) [2:Equipos de perforación, Interacción de personal](https://figshare.com/articles/dataset/_b_An_Open_Paradigm_Dataset_for_Intelligent_Monitoring_of_Underground_Drilling_Scenarios_in_Coal_Mines_b_/26135008/1)|
| **MineCareerDB** | Imágenes de alta resolución enfocadas en visión computacional en minería. | Clasificación y detección de equipos/situaciones en minería. | No especificado | [Link01](https://data.mendeley.com/datasets/c5s76mj4bm/5) [Link02](https://www.sciencedirect.com/science/article/pii/S2352340924009387)|
| **SH17 Dataset** | 8,099 imágenes con 75,994 instancias de 17 clases de EPP. | Detección de EPP en entornos industriales (incluye minería). | No especificado | [Kaggle](https://www.kaggle.com/datasets/mugheesahmad/sh17-dataset-for-ppe-detection) |

![Dataset EPPs](/public/img_percepcion/datasets.gif "Dataset EPPs")

## 4. Equipos
* Cámara Intel® RealSense™ depth camera D435i - [RGB-D](https://www.intelrealsense.com/depth-camera-d435i/)
* Cámara OAK-D Lite - [Estéreo](https://shop.luxonis.com/products/oak-d-lite-1)
* Unitree 4D LiDAR L2 - [LiDAR](https://www.unitree.com/L2)

![Comparación entre sensores](/public/sensor_fusion_3.webp "Diferencias entre sensores")

* FLIR Lepton (3.5 o 3.0) o Seek Thermal CompactPRO

| Modelo              | Resolución     | Interfaz     | Compatibilidad      | Bibliotecas              | Radiometría | Precio Estimado | Usos Típicos                                  |
|---------------------|----------------|--------------|----------------------|---------------------------|--------------|------------------|------------------------------------------------|
| **FLIR Lepton 3.5** | 160×120        | SPI + I2C    | ✅ Pi / ✅ Jetson     | `pylepton`, `libuvc`, OpenCV | Sí           | 250–350 USD | Robótica, visión térmica embebida, detección de temperatura |
| **FLIR Boson**      | 320×256 – 640×512 | USB / MIPI / UART | ⚠️ Pi* / ✅ Jetson | SDK oficial FLIR, ROS, OpenCV | Sí        | >1000 USD | Drones, inspección industrial |
| **Seek Compact PRO**| 320×240        | USB          | ⚠️ Pi** / ✅ Jetson   | `libseek`, ROS no oficial | Sí           | 300 USD   | Proyectos móviles, robótica, vigilancia térmica  |
| **AMG8833**         | 8×8            | I2C          | ✅ Pi / ✅ Jetson     | Adafruit Python Library   | No           | (35 USD)      | Detección de presencia, IoT, patrones térmicos básicos |
| **MLX90640**        | 32×24          | I2C          | ✅ Pi / ✅ Jetson     | Python, C++, OpenCV       | Sí           | 60–80 USD   | Prototipos térmicos, medición sin contacto, robótica ligera |

* **FLIR Lepton 3.5, 160×120 56º radiométrica**
https://www.apliter.com/producto/flir-lepton-3-5-160x120-56o-radiometrica/
    * Resolución térmica:
        * Lepton 2.5: 80 × 60
        * Lepton 3.5: 160 × 120 (radiométrica)
    * Interfaz: SPI + I2C (requiere adaptador PureThermal o breakout board)
    * Compatibilidad:  bibliotecas como pylepton

* **LiDAR Unitree L2**
https://nfmrobotics.com/
https://www.robotshop.com/es/products/lidar-4d-unitree-l2
    * Detecta objetos a 30 m de distancia con 64.000 puntos/seg
    * Ofrece un campo de visión ultra amplio de 360° x 96°

* **Seek CompactPRO**
https://phonetronic.pe/camara-termica/camara-termica-seek-compact-pro-iphone/?srsltid=AfmBOor4s9WTu0_KHbNttEOaZDugpKbhASN9cpNxkMO3xXVJImdB-Z3t
    * Resolución térmica: 320 × 240
    * Interfaz: USB‑C
    * Compatibilidad: drivers UVC

* **Seek Compact**
https://tiendamia.com/pe/producto?amz=B00NYWABAA
    * Resolución térmica: 206 × 156
    * Interfaz: USB‑C (UVC compatible con Linux)
    * Compatibilidad: vía libuvc o libseek

* **Pure Thermal Mini USB interface board para FLIR Lepton**
https://www.apliter.com/producto/pure-thermal-mini-usb-interface-board-para-flir-lepton/


## 5. Propuestas de funcionalidades

* ### Tracking de objetos
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

* ### Detección de víctimas
    * Se puede usar el algoritmo de YOLO con su funcionalidad de Estimación de Pose para detectar personas desmayadas por inhalación de gases tóxicos. Esto mediante detección de persona + estimación de pose.
    * Se puede utilizar la cámara térmica para inferir estado de la persona y confirmar que el "objeto" detectado sigue emitiendo calor corporal.

| Herramienta / Framework            | Función                                      | Compatibilidad | Detalle                                                 |
| ---------------------------------- | ----------------------------------------------- | --------------------------------- | ----------------------------------------------------- |
| **YOLO + Keypoint head**           | Detección + puntos clave del cuerpo humano      | Jetson (con TensorRT)           | Nueva variante YOLOv8 permite pose estimation directa |
| **OpenPose**                       | Estimación de pose por keypoints (muy completo) | Jetson (pesado), PC          | Preciso, pero pesado para embebidos                   |
| **MediaPipe Pose**                 | Pose rápida y eficiente (Google)                | Jetson Nano / Pi                | Liviano, buena integración                            |
| **Lightweight OpenPose (Pytorch)** | Red ligera de pose estimation                   | Jetson Nano                     | Equilibrio entre precisión y velocidad                |

![Pose Estimation](/public/img_percepcion/pose_estimation.jpg "Pose Estimation")