# Proyecto-Edge

## Visión del Proyecto

¿Qué problema resuelve?
En instituciones educativas y corporativas, los protocolos de limpieza suelen ser estáticos y basados en horarios, no en el uso real. Esto genera un desperdicio masivo de recursos (insumos químicos, agua, energía) y tiempo del personal de servicios generales, quienes intervienen espacios que han permanecido vacíos o con una carga de ocupación mínima.EcoSpace Edge transforma el mantenimiento reactivo en uno basado en datos, utilizando sensores de $CO_2$ para generar mapas de calor de ocupación en tiempo real. Si un aula o sala de juntas no ha superado un umbral de $CO_2$ determinado, se marca como "No requiere limpieza", optimizando la logística operativa.

Usuarios Potenciales
Gestores de Facility Management: Para optimizar la asignación de tareas del personal, Directivos de Instituciones Educativas/Empresariales: Interesados en la reducción de costos operativos y sostenibilidad, Equipos de Mantenimiento y Aseo: Para recibir rutas de trabajo optimizadas basadas en el uso real de los espacios.

## Justificación de Diseño
Para este proyecto, la implementación de un modelo de Edge Computing es crítica por las siguientes razones técnicas:

Preservación de Ancho de Banda: En un edificio con cientos de habitaciones, enviar flujos constantes de datos crudos de sensores a la nube saturaría la red local. La Raspberry Pi actúa como nodo Edge, procesando y filtrando los datos localmente.

Privacidad y Seguridad: Al procesar los datos de ocupación dentro de la red local del edificio, se mitigan los riesgos de exposición de patrones de movimiento de personas en servidores externos.

Resiliencia Operativa: El sistema de generación de mapas de calor y alertas de limpieza sigue funcionando incluso si la conexión a internet externa falla, ya que la lógica principal reside en el hardware local.

Reducción de Latencia: La visualización del dashboard local para el personal de aseo es inmediata, permitiendo decisiones en tiempo real sin depender de los tiempos de respuesta del servidor cloud.

## Arquitectura

![Diagrama](diagramaVER1.svg)

El sistema utiliza una topología de estrella para la recolección de datos, optimizando el consumo y la capacidad de procesamiento en cada etapa:

Nodos de Captura (ESP32 + MQ-135): Cada punto de monitoreo utiliza un ESP32 como unidad de control debido a su bajo costo y conectividad Wi-Fi/Bluetooth integrada. El sensor MQ-135 detecta la calidad del aire y niveles de $CO_2$, enviando las lecturas de forma inalámbrica.

Servidor Edge (Raspberry Pi 5): Centraliza la información de todos los nodos. Realiza el procesamiento pesado, genera el mapa de calor y gestiona la base de datos local. Al usar una Raspberry Pi 5, garantizamos potencia suficiente para manejar múltiples conexiones simultáneas sin cuellos de botella.

Visualización (Nube/Dashboard): La Raspberry Pi actúa como un gateway que sube únicamente la información procesada a la nube para acceso remoto.

## Presupuesto de Hardware

A continuación se detallan los costos de los componentes utilizados. Cabe destacar que para el desarrollo de este prototipo, la **Raspberry Pi 5** y los módulos **ESP32** ya se encuentran disponibles en el inventario del proyecto, por lo que el gasto de adquisición se centra en la sensórica.

| Componente | Descripción | Cantidad | Precio Unitario (COP) | Total (COP) | Estado |
| :--- | :--- | :---: | :--- | :--- | :--- |
| **Sensor MQ-135** | Sensor de calidad de aire ($CO_2$) | 3 | $12.000 | $36.000 | Adquirido |
| **ESP32 DevKit V1** | Microcontrolador con Wi-Fi/BLE | 3 | $25.000 | $75.000 | En Stock |
| **Raspberry Pi 5** | Nodo Edge de 8GB RAM | 1 | $550.000 | $550.000 | En Stock |
| **Fuente de Poder** | Adaptador 5V 5A (USB-C) | 1 | $90.000 | $90.000 | En Stock |
| **Misceláneos** | Cables, Jumpers y Protoboards | 1 | $30.000 | $30.000 | En Stock |
| **TOTAL ESTIMADO** | | | | **$781.000** | |


## Planeación y Ejecución Ágil
 
## Definicion del MVP

El Producto Mínimo Viable consistirá en el despliegue de al menos dos nodos embebidos con sensores de $CO_2$ en dos espacios cerrados distintos. Estos enviarán datos a una Raspberry Pi central, la cual procesará la información para determinar el nivel de uso mediante umbrales establecidos. El usuario final (personal de operaciones) podrá visualizar a través de una plataforma en la nube un dashboard básico con un mapa de calor simplificado (estilo semáforo) para identificar qué salas requieren limpieza prioritaria.

## Features

### 1. Hardware y Captura de Datos (Nodos y Pasarela)

Lectura de datos de $CO_2$ en tiempo real |  Must-have

El dispositivo embebido debe ser capaz de leer el sensor de $CO_2$ a intervalos regulares.

Transmisión inalámbrica de datos a la Raspberry Pi |  Must-have

Envío de las lecturas desde los nodos embebidos hacia el concentrador (Raspberry Pi) usando el protocolo que elijas (Wi-Fi, Bluetooth, LoRa, etc.).

Monitoreo de batería/estado del nodo |  Nice-to-have

Saber si un nodo se desconectó o se está quedando sin energía. No detiene la prueba de concepto inicial.

### 2. Procesamiento y Lógica (Raspberry Pi)

Algoritmo de cálculo de ocupación por umbral |  Must-have

Descripción: La Raspberry debe procesar los datos crudos de $CO_2$ ($ppm$) y determinar, bajo una regla simple (ej. si supera los $800\text{ ppm}$ durante $X$ minutos), que el espacio ha sido "altamente transitado" y requiere limpieza.

Base de datos local en la Raspberry Pi |  Must-have

Descripción: Almacenar las lecturas para poder generar el histórico del día.

Algoritmo de predicción de tráfico | Nice-to-have

Descripción: Usar Inteligencia Artificial para predecir qué días u horas se llenará más un salón. Muy avanzado para un MVP.

### 3. Visualización y Dashboard (Software / Nube)

Mapa de calor estático o por zonas en el Dashboard |  Must-have

Descripción: Una representación visual en la web donde las habitaciones cambien de color (Rojo = Requiere limpieza, Verde = Limpio/Poco uso) según los datos de la Raspberry.

Acceso remoto al Dashboard vía Nube |  Must-have

Descripción: Poder visualizar este estado desde un navegador fuera de la red local del edificio.

Interfaz Ultra-Pulida / Render 3D del edificio |  Nice-to-have

Descripción: Un mapa interactivo súper estético. Para el MVP basta con un plano 2D simple o incluso bloques de colores que representen las salas.

Sistema de alertas por Telegram / WhatsApp |  Nice-to-have

Descripción: Que le llegue un mensaje automático al personal de limpieza. Sería genial, pero para el MVP basta con que miren la pantalla del dashboard.

## Link del Cronograma
https://rosariolozanodm.atlassian.net/?continue=https%3A%2F%2Frosariolozanodm.atlassian.net%2Fwelcome%2Fsoftware%3FprojectId%3D10033&atlOrigin=eyJpIjoiMmY2ODU1N2NhMDY3NDc2ZTg0M2EzYzIwNjc5Y2RjZjMiLCJwIjoiamlyYS1zb2Z0d2FyZSJ9

## Spike Arquitectonico

1. Objetivo del Spike
El objetivo principal de este spike es mitigar el mayor riesgo técnico del proyecto: la integración y comunicación fluida entre todas las capas de la arquitectura (Hardware $\rightarrow$ Edge Computing $\rightarrow$ Cloud). Antes de proceder con el desarrollo a gran escala de las lógicas de procesamiento o el diseño estético de la interfaz, es fundamental validar que un dato generado en el nodo sensor pueda viajar con éxito hasta la plataforma en la nube.

2. Alcance y Metodología
Para este experimento se construirá un hilo de comunicación vertical de baja fidelidad, priorizando la velocidad de ejecución sobre la robustez o precisión del sistema. El flujo se dividirá en tres etapas:

Captura y Transmisión (Nivel de Nodo): 

Se utilizará un nodo embebido conectado a un sensor de CO2 (o en su defecto, un simulador de datos numéricos). Su única tarea será empaquetar una lectura básica y transmitirla de manera inalámbrica.

Recepción y Redirección (Nivel Edge):

La Raspberry Pi actuará puramente como un broker o pasarela . Recibirá el paquete de datos del nodo y, sin aplicar filtros complejos de software, lo reenviará hacia la nube a través de Internet.

Visualización Básica (Nivel Cloud): 

Se empleará una plataforma de IoT de rápido prototipado como Ubidots para reflejar el valor numérico recibido en un widget gráfico simple en tiempo real.

3. Criterios de Éxito
Este Spike se considerará completado y exitoso si se cumplen las siguientes condiciones:

Visibilidad del dato: El valor de CO2 generado en el nodo embebido se visualiza en el dashboard de la nube con un retraso (latencia) aceptable para el monitoreo de espacios.

Identificación de cuellos de botella: Se logra identificar si existen pérdidas de paquetes de datos en la comunicación inalámbrica hacia la Raspberry Pi o fallas de conexión hacia la nube.

Definición de librerías definitivas: Se descartan o aprueban las librerías de comunicación utilizadas en los dispositivos para la fase de producción del MVP.
