# Comunicación y Teleoperabilidad

## 1. Objetivos
* Implementar un sistema de comunicación que permita la conexión inalámbrica del robot cuadrúpedo hacia una estación base.
* Planificar una infraestructura de comunicación robusta para el envío de datos, trasmisión de video y envío de comandos de control.
* Desarrollar interfaces graficas y aplicaciones para gestión de la información obtenida para su integración con los procesos de la empresa
* Desarrollar/integrar interfaces de control para la teleoperación del robot.

## 2. Partes
* **Comunicación** 
    * Transmisión y recepción de datos entre el robot y otros sistemas (estación base y nube) a través de canales inalámbricos. _**Importancia:** Asegurar un flujo de datos confiable y en tiempo real que permita operación, monitoreo y supervisión del cuadrúpedo._

* **Teleoperabilidad**
    * Capacidad de controlar y supervisar el robot a distancia mediante interfaces de usuario, con retroalimentación de sensores. _**Importancia:** Permitir la intervención humana en tiempo real cuando el modo autónomo no sea viable._

* **Canales de comunicación**
    * Medios inalámbricos (Wi-Fi, LTE, 5G, LoRa, ZigBee) por los que se transmiten los datos. _**Importancia:** Seleccionar e implementar la infraestructura de red adecuada según el entorno (a cielo abierto, subterráneo, etc.)._

* **Protocolos de comunicación**
    * Conjunto de reglas para el intercambio de información entre sistemas. Ej.: TCP/IP, UDP, MQTT, HTTP, WebSockets. _**Importancia:** Garantizar interoperabilidad, latencia baja y fiabilidad según el tipo de datos._

* **Topología de red**
    * Estructura de la red de comunicación (punto a punto, cliente-servidor, malla, etc.). _**Importancia:** Diseñar la arquitectura más conveniente para la misión del robot y su contexto operativo._

## 3. Posible integración con otras áreas
* **Percepción y Localización:** Envío de imágenes obtenidas, mapas y posición en tiempo real.
* **Navegación:** Recepción de comandos de destino o misiones remotas.
* **Control:** Transmisión de parámetros de control o comandos manuales.
* **Diagnóstico (Adicional):** Envío de logs, estado de batería, fallos, temperatura, entre otros.

## 4. Tecnologías / Estado del arte

| Marca / Plataforma       | Modelo(s) Representativos           | Bandas de Frecuencia       | Codificación de Video       | Tipo de Red              | Características Clave                                                                                     | Precio Aproximado (USD) |
|--------------------------|-------------------------------------|----------------------------|-----------------------------|--------------------------|------------------------------------------------------------------------------------------------------------|--------------------------|
| **Rajant**               | BreadCrumb DX2, DX4, KM3            | 900 MHz, 2.4 GHz, 5 GHz    | Compatible con H.265        | Mesh Dinámica            | Red Mesh autosanable, tolerante a fallos, ideal para vehículos móviles.                                   | $1,200 – $2,500          |
| **FreeWave Technologies**| ZumLink IQ 900, ZumEdge             | 900 MHz, 2.4 GHz, sub-GHz  | Compatible con H.265 (externo) | Punto a Punto / Multipunto | Radios industriales, integrables con cámaras IP o SBC (Jetson, etc.), cifrado AES, Edge Computing opcional | $1,000 – $3,000          |
| **Radwin**               | FiberinMotion, Vehicular CPE        | 4.9 GHz – 6 GHz (licenciado) | Nativo para tráfico H.265/H.264 | Punto a Punto / Móvil    | Ideal para vehículos en movimiento, QoS garantizada, baja latencia.                                        | $1,500 – $3,500          |
| **Cambium Networks**     | PMP 450i, ePMP Force 425/3000       | 2.4 GHz, 5 GHz, bandas licenciadas | Soporta cámaras IP (H.265)  | Punto a Punto / Multipunto | QoS avanzada, multicast de video, gestión remota, industrial y resistente.                                 | $800 – $2,000            |
| **GE MDS Orbit**         | MCR-4G, MCR-900, MCR-WiFi           | 900 MHz, 2.4 GHz, LTE      | Cámaras externas (H.265)    | Multiprotocolo (RF, LTE, WiFi) | Alta disponibilidad, redundancia de enlaces, integración SCADA e IoT industrial.                          | $1,500 – $4,000          |


## Notas

**Codificación H.265:** *En todos los casos, se asume que el video proviene de una cámara IP o módulo de procesamiento (como Jetson Nano, Raspberry Pi, etc.) que codifique el video antes de enviarlo.*

**Bandas licenciadas:** *Algunos modelos como Radwin requieren licencia de frecuencia (regulada por el MTC).*

**QoS (Calidad de Servicio):** *Importante para garantizar video fluido y baja latencia en transmisión de datos críticos y para control remoto.*

## 5. Propuestas

* ### Primera propuesta
    * Dado que además de enviar datos de sensores, también se tiene que enviar salida de video y otros datos de mayor complejidad, la red más adecuada sería la que se basa en las bandas de WiFi, especialmente la de 2.4 GHz. 
    * Por ello, se propone implementar equipos y antenas para usar la banda de 2.4 GHz en una infraestructura robusta que asegure la calidad de la transmisión de datos. 
    * De manera general, el sistema consistiría en integrar una antena de 2.4 GHz en el cuadrúpedo para enviar los datos hacia una modulo receptor con antena integrada ubicado en un lugar de monitoreo (denominado “Estación base”). 
    * Cuando se reciba y envíe información desde la Estación Base, se podrá conectar con interfaces físicas de control y visualización de datos; asimismo, también se podrá subir dicha información a un servidor web desde el cual integrar otras interfaces gráficas en apps o software usado por la empresa.
    ![Imagen de propuesta 01](/public/diagrama_comunicacion_cuadrupedo/propuesta01.PNG "Propuesta 01")

* ### Segunda propuesta
    * Se propone hacer uso de la infraestructura de comunicación existente en el interior de la mina.
    * Según la reunión con la empresa minera, y en base a experiencias comentadas en otros [articulos](https://www.furukawalatam.com/es/conexion-furukawa-detalles/fibra-optica-y-mineria-transformacion-digital-hacia-la-industria-40), se podría tomar la fibra óptica como referencia de tecnología de comunicación a utilizar.
    * Por lo tanto, se propone que utilizando un adaptador USB 2.4 GHz se pueda hacer la conexión a un ONT tipo router. 
    * Mediante el ONT más cercano a la ubicación del cuadrúpedo, se enviarían los datos y salida de video; así como recibir comandos de control.
    * Si se consigue contar con acceso a la red de la mina, tendrían que tomarse en cuenta aspectos como IP estática, DHCP y en general implementar una vía de comunicación específica con el cuadrúpedo.
    * Se podría reconsiderar el uso de los dispositivos Ubiquiti NanoStation Loco M2 a modo de Backhaul para ampliar la cobertura, e incluso si el ONT solo tiene salida Ethernet disponible.  
    * El dispositivo de control HMI tambien se conectaría a la red interna de la empresa.
    * El servidor (backend) para recepción de datos, se ejecutaría en un equipo tambien conectado a la red interna de la empresa.
    ![Imagen de propuesta 02](/public/diagrama_comunicacion_cuadrupedo/propuesta02.PNG "Propuesta 02")

## 6. Equipos
* **Alfa AWUS036ACH** - S/. 100 - [link](https://tiendamia.com/pe/producto?amz=B0752CTSGD)
* **Ubiquiti NanoStation Loco M2** - S/. 240 - [link](https://arteus.pe/products/ubiquiti-locom2-nanostation-airmax-locom2-cpe-hasta-150-mbps-frecuencia-2-ghz-2412-2462-mhz-con-antena-integrada-de-8-dbi?srsltid=AfmBOoo6EFRcyCYr_JQsBFI25YdWthLGxmDeiysaxI71t-NhPGL1tSmX)
* **Pantalla TFT Nextion** - S/. 180 [link](https://mtlab.pe/producto/pantalla-nextion-discovery-2-4-2-8-3-5-hmi-tactil-resistivo/?srsltid=AfmBOori3gilh8l7x1lkFazwUiYRDs4wsNZnnPL4dXag-zkqk4xFQKcK)
* **Joystick**
* **Herramientas:** Python, Node.js, React

* **Ubiquiti NanoStation M2** (S/. 545)
https://lleva.pe/airmax-nanostation-m2-ubiquiti

* **Ubiquiti NanoStation Loco M2** (S/. 247)
https://arteus.pe/products/ubiquiti-locom2-nanostation-airmax-locom2-cpe-hasta-150-mbps-frecuencia-2-ghz-2412-2462-mhz-con-antena-integrada-de-8-dbi?srsltid=AfmBOopETa1jNi7TNut8Qswk5keWUhFzW2THgbDKEx3O-MaFyrpTl-kv

* **Radwin 2000**
https://store.nexus.com.pe/producto?codigo=RW-2954-D200

* **Radwin 5000 HPMP HSU 505 SFF**
https://wirelessunits.com/radwin-5000-hpmp-hsu-505-sff-series-subscriber-unit-radio-17-dbi-integrated-antenna-5-xghz-5mbps/

* **Cisco IW3702 (S/.968 - Reacondicinado)**
https://www.networkhardwares.com/es-pe/products/cisco-iw3702-4e-b-k9-cisco-industrial-wrls-ap-3702-4port-rf-on-top-b-reg-domain-iw3702-4e-b-k9-cisco-iw3702-4e-b-k9-cisco-industrial-wrls-ap-3702-4port-rf-on-top-b-reg-domain-iw3702-4e-b-k9?srsltid=AfmBOooOTKJT-DFOX6C5wvzQKCcGG3f6hLrmBtxZf3M9gDuca-zfKZo8

* **Alfa AWUS036ACH (S/.105)**
https://tiendamia.com/pe/producto?amz=B0752CTSGD

* **TP-Link CPE210**
https://www.tp-link.com/es/business-networking/pharos-cpe/cpe210/
    * Velocidad hasta 54 Mbps (802.11g), conexión externa de 12 dBi.
    * Ideal para enlaces cortos de alta penetración (hasta 15 km).
    * Soporta PoE, resistente a condiciones exteriores

* **TL-WA5210G (S/. 178)**
https://cuscoinformatico.com/access-point-tp-link-tl-wa5210g
    * Doble polarización MIMO 2×2, hasta 300 Mbps.
    * Enlaces exteriores de 5 + km, soporta modos AP/cliente/bridge/repetidor.

* **EnGenius ENS202**
https://www.engeniustech.com/span/engenius-products/outdoor-wireless-bridge-ens202/
    * Outdoor CPE de nivel profesional (300 Mbps 11n).

| Equipo                            | Proveedor                       | Precio aprox.       | Notas                                                                             |
| --------------------------------- | ------------------------------- | ------------------- | --------------------------------------------------------------------------------- |
| Radwin 5000 HSU 505 SFF           | WirelessUnits.com               | USD 336             | Con antena externa por N-type ([wirelessunits.com][1], [networkhardwares.com][2]) |
| Radwin 5000 HSU 505 (integrado)   | GNS Wireless                    | USD 473             | 13 dBi integrado                                                                  |
| Radwin 5000 (usado)               | eBay (WirelessUnits/Ezbusiness) | Desde USD 95        | Usado                                                                             |
| Cisco IW3702‑2E (reacondicionado) | eBay                            | USD 200–300 + envío | Cliente industrial Wi‑Fi                                                          |
| Cisco IW3702‑4E (reacondicionado) | NetworkHardwares.com            | USD 241             | Dual banda industrial                                                             |

[1]: https://wirelessunits.com/radwin-5000-hpmp-hsu-505-sff-series-subscriber-unit-radio-connectorized-2x-n-type-2-4ghz-5mbps/?utm_source=chatgpt.com "RADWIN 5000 HPMP HSU 505 SFF Series Subscriber Unit Radio ..."
[2]: https://www.networkhardwares.com/es/products/cisco-iw3702-4e-b-k9-cisco-industrial-wrls-ap-3702-4port-rf-on-top-b-reg-domain-iw3702-4e-b-k9-cisco-iw3702-4e-b-k9-cisco-industrial-wrls-ap-3702-4port-rf-on-top-b-reg-domain-iw3702-4e-b-k9?srsltid=AfmBOorGazxc93AeEIWCQx-vF_9Lvdd8GW_LQ3q-1c-77ba8GTWZ6FK-&utm_source=chatgpt.com "Cisco Industrial Inalámbrico AP 3702 4 Puertos RF Superior B ..."


## 7. Anexos
### Ubiquiti Nanostation Loco M2
NanoStation Loco M2 es un equipo de exterior compacto, incluye una antena MIMO de doble polaridad para la banda de 2.4 GHz. La velocidad de transferencia real del equipo es de hasta 150 Mbps (18.75 MBps).
* #### Características
    * 2X2 MIMO / 2.4GHz
    * 23dBm, Antena integrada 8dBi
    * 150+Mbps
    * Inyector PoE PoE 24v incluido
    * Un puerto Ethernet 10/100 Mbps
    * Anchos de canal ajustable de 5 hasta 40 MHz
    * Seguridad: WEP, WPA, WPA2 y MAC ACL
    * Señalización propietaria: airMAX (TDMA)

    ![Ubiquiti 01](/public/ubiquiti_M2/materiales.png "Ubiquiti 01")
    ![Ubiquiti 02](/public/ubiquiti_M2/esquema.png "Ubiquiti 02")
    ![Ubiquiti 03](/public/ubiquiti_M2/dimensiones.png "Ubiquiti 03")
    ![Ubiquiti 04](/public/ubiquiti_M2/configuracion.png "Ubiquiti 04")

### Servidor
* **BACKEND**
    * Flask
    * Python
    * Docker
    * PostgresSQL
    * MongoDB
    * Node.js

* **FRONTEND**
    * React.js
    * CSS
    * TailwindCSS
    * AJAX
    * Redis
    * HTTP/MQTT/WebSockets

### Comunicación
* LAN
* DHCP
* UDP
* QoS
* IP Dinámico
