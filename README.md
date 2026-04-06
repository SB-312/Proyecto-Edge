# Proyecto-Edge (luego pensamos en el título)

# Visión del Proyecto

¿Qué problema resuelve?
En instituciones educativas y corporativas, los protocolos de limpieza suelen ser estáticos y basados en horarios, no en el uso real. Esto genera un desperdicio masivo de recursos (insumos químicos, agua, energía) y tiempo del personal de servicios generales, quienes intervienen espacios que han permanecido vacíos o con una carga de ocupación mínima.EcoSpace Edge transforma el mantenimiento reactivo en uno basado en datos, utilizando sensores de $CO_2$ para generar mapas de calor de ocupación en tiempo real. Si un aula o sala de juntas no ha superado un umbral de $CO_2$ determinado, se marca como "No requiere limpieza", optimizando la logística operativa.

Usuarios Potenciales
Gestores de Facility Management: Para optimizar la asignación de tareas del personal, Directivos de Instituciones Educativas/Empresariales: Interesados en la reducción de costos operativos y sostenibilidad, Equipos de Mantenimiento y Aseo: Para recibir rutas de trabajo optimizadas basadas en el uso real de los espacios.

# Justificación de Diseño
Para este proyecto, la implementación de un modelo de Edge Computing es crítica por las siguientes razones técnicas:

Preservación de Ancho de Banda: En un edificio con cientos de habitaciones, enviar flujos constantes de datos crudos de sensores a la nube saturaría la red local. La Raspberry Pi actúa como nodo Edge, procesando y filtrando los datos localmente.

Privacidad y Seguridad: Al procesar los datos de ocupación dentro de la red local del edificio, se mitigan los riesgos de exposición de patrones de movimiento de personas en servidores externos.

Resiliencia Operativa: El sistema de generación de mapas de calor y alertas de limpieza sigue funcionando incluso si la conexión a internet externa falla, ya que la lógica principal reside en el hardware local.

Reducción de Latencia: La visualización del dashboard local para el personal de aseo es inmediata, permitiendo decisiones en tiempo real sin depender de los tiempos de respuesta del servidor cloud.

# Arquitectura

![Diagrama](diagramaVER1.svg)

El sistema utiliza una topología de estrella para la recolección de datos, optimizando el consumo y la capacidad de procesamiento en cada etapa:

Nodos de Captura (ESP32 + MQ-135): Cada punto de monitoreo utiliza un ESP32 como unidad de control debido a su bajo costo y conectividad Wi-Fi/Bluetooth integrada. El sensor MQ-135 detecta la calidad del aire y niveles de $CO_2$, enviando las lecturas de forma inalámbrica.

Servidor Edge (Raspberry Pi 5): Centraliza la información de todos los nodos. Realiza el procesamiento pesado, genera el mapa de calor y gestiona la base de datos local. Al usar una Raspberry Pi 5, garantizamos potencia suficiente para manejar múltiples conexiones simultáneas sin cuellos de botella.

Visualización (Nube/Dashboard): La Raspberry Pi actúa como un gateway que sube únicamente la información procesada a la nube para acceso remoto.

# Presupuesto de Hardware

A continuación se detallan los costos de los componentes utilizados. Cabe destacar que para el desarrollo de este prototipo, la **Raspberry Pi 5** y los módulos **ESP32** ya se encuentran disponibles en el inventario del proyecto, por lo que el gasto de adquisición se centra en la sensórica.

| Componente | Descripción | Cantidad | Precio Unitario (COP) | Total (COP) | Estado |
| :--- | :--- | :---: | :--- | :--- | :--- |
| **Sensor MQ-135** | Sensor de calidad de aire ($CO_2$) | 3 | $12.000 | $36.000 | Adquirido |
| **ESP32 DevKit V1** | Microcontrolador con Wi-Fi/BLE | 3 | $25.000 | $75.000 | En Stock |
| **Raspberry Pi 5** | Nodo Edge de 8GB RAM | 1 | $550.000 | $550.000 | En Stock |
| **Fuente de Poder** | Adaptador 5V 5A (USB-C) | 1 | $90.000 | $90.000 | En Stock |
| **Misceláneos** | Cables, Jumpers y Protoboards | 1 | $30.000 | $30.000 | En Stock |
| **TOTAL ESTIMADO** | | | | **$781.000** | |

# Link del Jira
https://rosariolozanodm.atlassian.net/?continue=https%3A%2F%2Frosariolozanodm.atlassian.net%2Fwelcome%2Fsoftware%3FprojectId%3D10033&atlOrigin=eyJpIjoiMmY2ODU1N2NhMDY3NDc2ZTg0M2EzYzIwNjc5Y2RjZjMiLCJwIjoiamlyYS1zb2Z0d2FyZSJ9
