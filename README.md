# Equipo-Zorro
Repositorio del proyecto final

# Proyecto: Ideas
## Estación Meteorológica
Diseñar una estación meteorológica capaz de procesar información como humedad, temperatura, velocidad del viento y presión atmosférica, con el fin de estimar tendencias y condiciones meteorológicas a corto plazo a partir de las variables ambientales medidas y de modelos simplificados basados en datos históricos, tales como: momentos de lluvia o de calor. 

La alimentación del sistema estará basada en luz solar, donde se implementará un controlador de carga hacia baterías de respaldo en caso de no contar con suficiente radiación solar, todo esto acompañado de una etapa de regulación y gestión de potencia que permita suministrar a cada componente los niveles de tensión y corriente requeridos y optimizar el consumo energético del sistema.

En cuanto a la adquisición de datos se pretende implementar sensores como el BME280 (capaz de medir humedad, presión y temperatura), así como el sensor Wh-sp-ws01 (capaz de medir la velocidad del viento).

La predicción se realizará mediante el procesamiento de los datos históricos obtenidos a intervalos definidos, inicialmente cada 15 minutos. A partir de estos datos se calcularán promedios y tendencias de las variables ambientales, permitiendo identificar cambios en las condiciones atmosféricas y realizar una estimación de las condiciones meteorológicas a corto plazo.

La visualización de los datos se realizará mediante pantalla LCD o un Display Pantalla Oled, donde se mostrarán los datos tras cada medición y/o predicción. Como posible ampliación, se contempla la transmisión inalámbrica de los datos mediante Bluetooth o LoRa hacia un dispositivo cercano al usuario.
## Motivación: 
La idea surge del interés por desarrollar un sistema autónomo capaz de obtener información de su entorno y tomar decisiones a partir de ella, integrando sensores ambientales, procesamiento mediante un microcontrolador y una fuente de alimentación basada en energía solar. Además, resulta especialmente interesante explorar las limitaciones y posibilidades de implementar algoritmos de predicción en un dispositivo con recursos computacionales reducidos.

