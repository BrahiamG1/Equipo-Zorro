# Equipo-Zorro
Repositorio del proyecto final
# Estación Meteorológica
## Diseñar una estación meteorológica capaz de procesar información como humedad, temperatura, velocidad del viento y presión atmosférica, con el fin de estimar tendencias y condiciones meteorológicas a corto plazo a partir de las variables ambientales medidas y de modelos simplificados basados en datos históricos, tales como: momentos de lluvia o de calor. 

La alimentación del sistema estará basada en luz solar, donde se implementará un controlador de carga hacia baterías de respaldo en caso de no contar con suficiente radiación solar, todo esto acompañado de una etapa de regulación y gestión de potencia que permita suministrar a cada componente los niveles de tensión y corriente requeridos y optimizar el consumo energético del sistema.

En cuanto a la adquisición de datos se pretende implementar sensores como el BME280 (capaz de medir humedad, presión y temperatura), así como el sensor Wh-sp-ws01 (capaz de medir la velocidad del viento).

La predicción se realizará mediante el procesamiento de los datos históricos obtenidos a intervalos definidos, inicialmente cada 15 minutos. A partir de estos datos se calcularán promedios y tendencias de las variables ambientales, permitiendo identificar cambios en las condiciones atmosféricas y realizar una estimación de las condiciones meteorológicas a corto plazo.

La visualización de los datos se realizará mediante pantalla LCD o un Display Pantalla Oled, donde se mostrarán los datos tras cada medición y/o predicción. Como posible ampliación, se contempla la transmisión inalámbrica de los datos mediante Bluetooth o LoRa hacia un dispositivo cercano al usuario.
## Motivación: 
La idea surge del interés por desarrollar un sistema autónomo capaz de obtener información de su entorno y tomar decisiones a partir de ella, integrando sensores ambientales, procesamiento mediante un microcontrolador y una fuente de alimentación basada en energía solar. Además, resulta especialmente interesante explorar las limitaciones y posibilidades de implementar algoritmos de predicción en un dispositivo con recursos computacionales reducidos.

# Sistema de gestión de carga
## La idea del proyecto es tener control sobre la alimentación de una carga eléctrica, con la ayuda de un capacitor y dos MOSFET, la cual va a ser monitoreada por un microcontrolador (Raspberry Pi Pico] y una pantalla, será alimentada por tres tipos de generadores eléctricos o tecnologías: solar, eólico y por una batería. Estas tecnologías pueden ir cambiando a medida que avance el proyecto, pero se mantiene la idea central. La finalidad es cargar y descargar el capacitor dependiendo de la demanda energética de la carga eléctrica, además, esta carga y descarga del capacitor se realizará con la ayuda de los MOSFETs, los cuales estarán sincronizados para evitar un corto circuito, afectando así todo el sistema. Esta monitorización se hará con la ayuda del microcontrolador. Por otro lado, se utilizará una pantalla para poder ver la demanda de corriente/voltaje de la carga y de la energía generada por las diferentes fuentes de energía. 

La motivación para realizar este proyecto son los obstáculos que este demanda; la sincronización de los MOSFETs, la gestión del voltaje de las fuentes, es decir, se usarán de forma independiente o tendrán un nodo en común, esto último añade el reto de que es necesario usar un conversor AC/DC para los generadores AC. Además, las posibles aplicaciones en otros escenarios a mayor escala.

