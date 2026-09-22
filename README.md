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

# Meteoro

Meteoro
consiste en una estación meteorológica capaz de llevar un estadístico de las
variables ambientales tales como: temperatura, presión atmosférica, humedad, presencia
de lluvia, intensidad de radiación UV y velocidad del viento. El sistema
contará además con un sistema de alimentación basado en energía solar, buscando
que pueda operar de manera autónoma durante periodos prolongados.

Entre
los estadísticos obtenidos se contemplan valores promedios, máximos y mínimos
diarios, varianza y desviación estándar de las variables medidas. Esto con el
fin de construir un histórico del comportamiento de las condiciones ambientales
de la zona y analizar sus variaciones a lo largo del tiempo.

Respecto
a la idea inicial, el principal cambio consiste en que ya no se busca realizar
una predicción de clima a corto plazo, sino caracterizar estadísticamente el
comportamiento de las variables ambientales mediante la recopilación y
almacenamiento de datos históricos. Este cambio se realiza teniendo en cuenta
las limitaciones físicas y naturales asociadas a la predicción meteorológica.
Especialmente en zonas tropicales, donde las condiciones pueden presentar
variaciones difíciles de modelar.

Adicionalmente,
se incorpora una **unidad
microSD** para almacenar de manera persistente las mediciones
realizadas durante los diferentes intervalos de adquisición. De esta manera,
los datos históricos se conservan incluso después de una interrupción de
alimentación o un reinicio del sistema. También se integra un módulo **RTC (Real Time Clock)**,
encargado de mantener la fecha y hora del sistema para asociar una marca
temporal a cada medición. Además, se aprovechará la salida de interrupción del
RTC para generar eventos que permitan despertar el microcontrolador desde un
estado de bajo consumo y realizar las adquisiciones programadas.

Otro
cambio importante esta en la visualización de los datos. En lugar de utilizar
un display OLED local, la información será enviada a una plataforma IoT de tipo
Low-Code, como **Node-RED**, para permitir la visualización remota de las
,ediciones mediante gráficas y otros elementos de monitoreo. Esta decisión se
toma considerando que la estación estará ubicada en un lugar donde no se tendrá
acceso constante al sistema para consultar un display local.

En
cuanto al sensado, se incorporan dos nuevas variables ambientales. Para la
medición de radiación UV se utilizará el sensor **GUVA-S12SD**, mientras que para la detección
de lluvia o humedad superficial se utilizará un sensor de lluvia basado en **LM393**, con salida
analógica y digital. El anemómetro también se modifica respecto a la propuesta
inicial, seleccionando el modelo **ZTS3000FSJT**
debido a su disponibilidad nacional y menor costo. Este sensor presenta una
salida analógica de 0 a 5 V y requiere una alimentación entre 10 y 30 V, por lo
que será necesario implementar el acondicionamiento de señal correspondiente
para conectarlo de manera segura al ADC de 3.3 V del microcontrolador. El **BME280**
se instalará dentro de una garita meteorológica que permita la circulación de
aire y, al mismo tiempo, reduzca la incidencia directa de la radiación solar y
la exposición a la lluvia.

Finalmente, se modifica el sistema de
alimentación para incorporar generación de energía mediante un panel solar. Se
plantea utilizar un panel de aproximadamente 12 V junto con un controlador de
carga adecuado para un paquete de tres celdas LiPo de 3.7 V conectadas en
serie, obteniendo un voltaje nominal de 11.1 V. Esta configuración permite
disponer de una fuente de tensión adecuada para el anemómetro, mientras que un
convertidor DC-DC proporcionará los 3.3 V requeridos por la Raspberry Pi Pico y
los demás sensores. El sistema de alimentación deberá además gestionar el
almacenamiento de energía y el funcionamiento de la estación durante los
periodos en los que la radiación solar sea insuficiente.

# 1.    Interpretación y delimitación del proyecto

#  

# Descripción operativa

El
sistema **Meteoro**
consiste en una estación meteorológica autónoma orientada al registro y
caracterización estadística de variables ambientales. El sistema realizará
mediciones de temperatura, humedad relativa y presión atmosférica mediante un
sensor BME280, intensidad de radiación UV mediante un sensor GUVA-S12SD,
presencia de lluvia mediante un sensor de detección de lluvia basado en LM393 y
velocidad del viento mediante un anemómetro ZTS3000FSJT.

Normalmente,
el sistema permanecerá en un estado de bajo consumo. Un módulo RTC (Real Time
Clock) será utilizado para mantener la fecha y hora del sistema y generar una
interrupción que despierte al microcontrolador en intervalos de tiempo
determinados. Inicialmente se plantea un intervalo de adquisición de 15
minutos.

Cada
vez que el sistema sea despertado, se realizará la adquisición de las variables
ambientales. Posteriormente, los datos serán procesados para obtener los
estadísticos definidos para el proyecto y se almacenarán de forma persistente
en una memoria microSD junto con la fecha y hora de la medición. Finalmente, el
sistema transmitirá los datos a una plataforma IoT para permitir su
visualización y monitoreo remoto.

Una
vez finalizado el ciclo de adquisición, almacenamiento y transmisión, el
sistema volverá al estado de bajo consumo hasta que se produzca el siguiente
evento de activación.

La
alimentación del sistema será proporcionada mediante un sistema de generación y
almacenamiento de energía solar, compuesto por un panel solar, un controlador
de carga y un banco de baterías. Se utilizarán etapas de regulación y
conversión de tensión para proporcionar los niveles de alimentación requeridos
por el microcontrolador, los sensores y el anemómetro.

**Alcance**

El
proyecto comprende el diseño e implementación de una estación capaz de:

·        
Medir
temperatura, humedad relativa y presión atmosférica.

·        
Medir
la intensidad de radiación UV.

·        
Detectar
la presencia de lluvia.

·        
Medir
la velocidad del viento.

·        
Registrar
las mediciones junto con su fecha y hora.

·        
Almacenar
los datos de manera persistente en una memoria microSD.

·        
Calcular
estadísticas de las variables medidas, incluyendo valores promedio, máximos,
mínimos, varianza y desviación estándar.

·        
Transmitir
los datos mediante una conexión inalámbrica hacia una plataforma IoT para su
visualización remota.

·        
Operar
mediante alimentación proveniente de energía solar y almacenamiento en
baterías.

·        
Implementar
estados de bajo consumo entre los períodos de adquisición para reducir el
consumo energético.

El
proyecto se limita a la **caracterización
y seguimiento estadístico de las condiciones ambientales locales**.
No se contempla como objetivo realizar predicciones meteorológicas de corto o
largo plazo ni realizar mediciones meteorológicas con precisión profesional o
certificada.

Tampoco
se contempla el desarrollo de una plataforma web propia, ya que la
visualización de los datos se realizará mediante una plataforma IoT existente.

**Actores**

Los
principales actores y elementos que interactúan con el sistema son:

·        
**Condiciones ambientales:** proporcionan los
fenómenos físicos que serán medidos, principalmente temperatura, humedad,
presión, radiación UV, lluvia y viento.

·        
**Sensores:** convierten las
variables ambientales en señales que pueden ser procesadas por el sistema.

·        
**Raspberry Pi Pico:** controla el proceso de
adquisición, procesamiento, almacenamiento y comunicación.

·        
**RTC:** proporciona la referencia temporal y
genera los eventos de activación periódica.

·        
**Memoria microSD:** almacena
persistentemente el histórico de mediciones.

·        
**Sistema de alimentación:** proporciona y
administra la energía necesaria para el funcionamiento de la estación.

·        
**Plataforma IoT:** recibe y presenta los
datos transmitidos por la estación.

·        
**Usuario:** consulta y analiza remotamente la
información recopilada por el sistema.

**Entradas**

Las
principales entradas del sistema son:

·        
Temperatura
ambiental.

·        
Humedad
relativa.

·        
Presión
atmosférica.

·        
Intensidad
de radiación UV.

·        
Presencia
de lluvia.

·        
Velocidad
del viento.

·        
Fecha
y hora proporcionadas por el RTC.

·        
Energía
proporcionada por el sistema solar y las baterías.

·        
Configuraciones
del sistema, como el intervalo de adquisición.

**Salidas**

Las
principales salidas del sistema son:

·        
Registros
históricos almacenados en la memoria microSD.

·        
Estadísticos
calculados a partir de las mediciones.

·        
Datos
transmitidos a la plataforma IoT.

·        
Gráficas
y visualizaciones de las variables ambientales en la plataforma IoT.

·        
Información
de estado del sistema, como funcionamiento normal o condiciones de batería
baja, si se implementa esta función.

**Modos de operación**

Inicialmente
se consideran los siguientes modos:

**Modo de reposo:** el microcontrolador y los periféricos que no
sean necesarios permanecerán en un estado de bajo consumo mientras se espera el
siguiente evento de adquisición.

**Modo de adquisición:** el sistema se despierta mediante la
interrupción del RTC y activa o consulta los sensores para obtener las
variables ambientales.

**Modo de procesamiento y almacenamiento:** las mediciones son
procesadas, se calculan los estadísticos correspondientes y los datos son
almacenados en la memoria microSD junto con su fecha y hora.

**Modo de comunicación:** el sistema establece
la comunicación con la plataforma IoT y transmite los datos obtenidos.

**Modo de bajo consumo o contingencia:** en condiciones de
energía limitada, el sistema podrá reducir sus actividades o frecuencia de
adquisición para prolongar el tiempo de funcionamiento con la energía
disponible.

**Restricciones**

·        
El
sistema debe operar con un consumo energético reducido debido a su alimentación
mediante energía solar y baterías.

·        
El
anemómetro requiere una alimentación superior a la tensión utilizada por el
microcontrolador, por lo que será necesario implementar una etapa de conversión
de tensión.

·        
La
salida analógica del anemómetro es de 0 a 5 V, mientras que las entradas
analógicas del microcontrolador trabajan con niveles de 3.3 V, por lo que será
necesario implementar acondicionamiento de señal.

·        
La
estación estará expuesta a condiciones ambientales, por lo que los sensores
deberán contar con una protección física adecuada.

·        
El
BME280 deberá instalarse dentro de una garita meteorológica que permita
circulación de aire y reduzca la incidencia directa de la radiación solar.

·        
La
disponibilidad de energía dependerá de las condiciones de radiación solar y de
la capacidad de almacenamiento de las baterías.

·        
La
comunicación inalámbrica dependerá de la disponibilidad de la red utilizada.

·        
El
sensor de lluvia utilizado permite detectar presencia de agua, pero no
proporciona una medición normalizada de precipitación acumulada en milímetros.

·        
El
sistema no está destinado a proporcionar mediciones meteorológicas certificadas
ni a sustituir una estación meteorológica profesional.

**Supuestos**

·        
Se
dispone de suficiente radiación solar promedio para recuperar la energía
consumida por el sistema durante el período de operación.

·        
La
capacidad de las baterías será suficiente para mantener el sistema funcionando
durante períodos sin generación solar.

·        
Los
sensores utilizados presentan una precisión suficiente para realizar la
caracterización estadística propuesta.

·        
La
comunicación inalámbrica estará disponible durante los períodos en los que se
requiera transmitir información.

·        
La
memoria microSD tendrá capacidad suficiente para conservar el histórico de
mediciones durante el período de operación.

·        
El
intervalo inicial de 15 minutos será suficiente para caracterizar las
variaciones ambientales de interés.

·        
Los
sensores permanecerán dentro de sus rangos de operación especificados durante
el funcionamiento normal.

·        
El
sistema podrá recuperar su funcionamiento después de una interrupción temporal
de alimentación.

**Preguntas pendientes**

·        
¿El
intervalo de adquisición de 15 minutos es adecuado para representar las
variaciones de las variables ambientales o será necesario utilizar diferentes
intervalos para determinadas variables?

·        
¿Qué
estrategia de adquisición debe utilizarse para la velocidad del viento,
considerando que puede presentar variaciones más rápidas que las demás
variables?

·        
¿Qué
nivel de precisión y estabilidad pueden obtenerse realmente con los sensores
seleccionados en condiciones ambientales reales?

·        
¿Cuál
será el consumo energético total del sistema en los modos de reposo,
adquisición, almacenamiento y transmisión?

·        
¿Qué
capacidad de batería y qué potencia del panel solar son necesarias para
garantizar la autonomía deseada?

·        
¿Qué
estrategia se utilizará cuando la batería alcance un nivel bajo de carga?

·        
¿Qué
formato y estructura tendrá el archivo almacenado en la microSD para facilitar
posteriormente el análisis de los datos?

·        
¿Qué
umbral o condición permitirá identificar un evento significativo de viento y,
si se implementa esta función, utilizarlo para modificar temporalmente la
frecuencia de adquisición?

·        
¿Qué
estrategia se utilizará para conservar los datos cuando no exista conexión con
la plataforma IoT?

# 2. Especificación de requisitos

#  

# 2.1 Requisitos funcionales

|      |
| ---- |

**ID**

|      |
| ---- |

**Requisito**

|      |
| ---- |

**Fuente**

|      |
| ---- |

**Criterio de verificabilidad**

|     |
| --- |

RF-01

|     |
| --- |

El
&#x20; sistema deberá medir la temperatura ambiental mediante el BME280.

|     |
| --- |

Propuesta
&#x20; del proyecto

|     |
| --- |

Se
&#x20; realizará una medición y se comparará con un instrumento de referencia,
&#x20; verificando que el valor obtenido se encuentre dentro del error establecido.

|     |
| --- |

RF-02

|     |
| --- |

El
&#x20; sistema deberá medir la humedad relativa mediante el BME280.

|     |
| --- |

Propuesta
&#x20; del proyecto

|     |
| --- |

Se
&#x20; comparará la medición con un instrumento de referencia bajo las mismas
&#x20; condiciones ambientales.

|     |
| --- |

RF-03

|     |
| --- |

El
&#x20; sistema deberá medir la presión atmosférica mediante el BME280.

|     |
| --- |

Propuesta
&#x20; del proyecto

|     |
| --- |

Se
&#x20; verificará que el sistema obtenga y registre un valor de presión en las
&#x20; unidades definidas.

|     |
| --- |

RF-04

|     |
| --- |

El
&#x20; sistema deberá medir la intensidad de radiación UV mediante el sensor
&#x20; GUVA-S12SD.

|     |
| --- |

Retroalimentación
&#x20; del profesor / propuesta modificada

|     |
| --- |

Se
&#x20; expondrá el sensor a diferentes niveles de radiación UV y se verificará una
&#x20; variación correspondiente en su lectura.

|     |
| --- |

RF-05

|     |
| --- |

El
&#x20; sistema deberá detectar la presencia de lluvia mediante el sensor basado en
&#x20; LM393.

|     |
| --- |

Retroalimentación
&#x20; del profesor / propuesta modificada

|     |
| --- |

Se
&#x20; aplicará agua sobre el sensor y se verificará el cambio de la salida respecto
&#x20; a una condición seca.

|     |
| --- |

RF-06

|     |
| --- |

El
&#x20; sistema deberá medir la velocidad del viento mediante el anemómetro
&#x20; ZTS3000FSJT.

|     |
| --- |

Cambio
&#x20; de diseño / disponibilidad y costo

|     |
| --- |

Se
&#x20; aplicarán diferentes condiciones de flujo de aire y se verificará que la
&#x20; salida del sensor cambie de acuerdo con la velocidad del viento.

|     |
| --- |

RF-07

|     |
| --- |

El
&#x20; sistema deberá adquirir las variables ambientales en intervalos de tiempo
&#x20; configurables, utilizando inicialmente un intervalo de 15 minutos.

|     |
| --- |

Propuesta
&#x20; del proyecto

|     |
| --- |

Se
&#x20; verificará mediante registros de tiempo que las adquisiciones se produzcan en
&#x20; los intervalos establecidos.

|     |
| --- |

RF-08

|     |
| --- |

El
&#x20; sistema deberá mantener la fecha y hora de las mediciones mediante un módulo
&#x20; RTC.

|     |
| --- |

Retroalimentación
&#x20; del profesor / propuesta modificada

|     |
| --- |

Se
&#x20; comparará la fecha y hora registrada por el sistema con la hora de
&#x20; referencia.

|     |
| --- |

RF-09

|     |
| --- |

El
&#x20; sistema deberá utilizar la señal de interrupción del RTC para iniciar un
&#x20; ciclo de adquisición.

|     |
| --- |

Diseño
&#x20; del sistema

|     |
| --- |

Se
&#x20; observará que el sistema permanezca en bajo consumo y se active cuando ocurra
&#x20; la alarma programada del RTC.

|     |
| --- |

RF-10

|     |
| --- |

El
&#x20; sistema deberá almacenar persistentemente las mediciones en una memoria
&#x20; microSD.

|     |
| --- |

Retroalimentación
&#x20; del profesor

|     |
| --- |

Se
&#x20; verificará la creación y actualización del archivo de datos y la conservación
&#x20; de registros después de reiniciar o interrumpir la alimentación.

|     |
| --- |

RF-11

|     |
| --- |

Cada
&#x20; registro almacenado deberá incluir la fecha y hora asociadas a la medición.

|     |
| --- |

Retroalimentación
&#x20; del profesor / diseño

|     |
| --- |

Se
&#x20; inspeccionará el archivo almacenado y se comprobará que cada registro
&#x20; contenga una marca temporal válida.

|     |
| --- |

RF-12

|     |
| --- |

El
&#x20; sistema deberá calcular estadísticas de las variables ambientales
&#x20; registradas.

|     |
| --- |

Propuesta
&#x20; del proyecto

|     |
| --- |

Se
&#x20; compararán los resultados calculados por el sistema con los resultados
&#x20; obtenidos externamente a partir del mismo conjunto de datos.

|     |
| --- |

RF-13

|     |
| --- |

El
&#x20; sistema deberá obtener como mínimo valores promedio, máximo y mínimo de las
&#x20; variables medidas.

|     |
| --- |

Propuesta
&#x20; del proyecto

|     |
| --- |

Se
&#x20; suministrará un conjunto conocido de datos y se verificará que los resultados
&#x20; coincidan con los cálculos de referencia.

|     |
| --- |

RF-14

|     |
| --- |

El
&#x20; sistema deberá obtener varianza y desviación estándar de las variables para
&#x20; los períodos definidos.

|     |
| --- |

Propuesta
&#x20; del proyecto

|     |
| --- |

Se
&#x20; utilizará un conjunto de datos conocido y se compararán los resultados del
&#x20; sistema con un cálculo de referencia.

|     |
| --- |

RF-15

|     |
| --- |

El
&#x20; sistema deberá transmitir los datos hacia una plataforma IoT.

|     |
| --- |

Retroalimentación
&#x20; del profesor / propuesta modificada

|     |
| --- |

Se
&#x20; generará una medición y se verificará su recepción en la plataforma IoT.

|     |
| --- |

RF-16

|     |
| --- |

La
&#x20; plataforma IoT deberá permitir visualizar las variables registradas mediante
&#x20; elementos gráficos.

|     |
| --- |

Propuesta
&#x20; modificada

|     |
| --- |

Se
&#x20; comprobará la actualización de las gráficas o indicadores después de recibir
&#x20; nuevos datos.

|     |
| --- |

RF-17

|     |
| --- |

El
&#x20; sistema deberá almacenar localmente los datos cuando la comunicación con la
&#x20; plataforma IoT no esté disponible.

|     |
| --- |

Diseño
&#x20; / robustez del sistema

|     |
| --- |

Se
&#x20; desconectará la comunicación, se generarán mediciones y posteriormente se
&#x20; verificará que los datos permanezcan en la microSD.

|     |
| --- |

RF-18

|     |
| --- |

El
&#x20; sistema deberá regresar al estado de bajo consumo después de finalizar un
&#x20; ciclo de adquisición.

|     |
| --- |

Objetivo
&#x20; de bajo consumo / diseño

|     |
| --- |

Se
&#x20; medirá el consumo o se observará el estado de operación del sistema después
&#x20; de finalizar el ciclo.

##  

## **2.2 Requisitos no funcionales**

|      |
| ---- |

**ID**

|      |
| ---- |

**Requisito**

|      |
| ---- |

**Fuente**

|      |
| ---- |

**Criterio de verificabilidad**

|     |
| --- |

RNF-01

|     |
| --- |

El
&#x20; sistema deberá utilizar alimentación basada en energía solar y almacenamiento
&#x20; en baterías.

|     |
| --- |

Retroalimentación
&#x20; del profesor / propuesta

|     |
| --- |

Se
&#x20; demostrará el funcionamiento del sistema mediante el subsistema solar y la
&#x20; batería.

|     |
| --- |

RNF-02

|     |
| --- |

El
&#x20; sistema deberá minimizar el consumo energético durante los períodos en los
&#x20; que no se estén realizando mediciones.

|     |
| --- |

Objetivo
&#x20; del proyecto

|     |
| --- |

Se
&#x20; medirá el consumo en reposo y durante los principales estados de operación.

|     |
| --- |

RNF-03

|     |
| --- |

Las
&#x20; señales analógicas deberán encontrarse dentro del rango admisible por las
&#x20; entradas del microcontrolador.

|     |
| --- |

Restricciones
&#x20; eléctricas de los componentes

|     |
| --- |

Se
&#x20; medirán las tensiones máximas de las señales antes de conectarlas al ADC.

|     |
| --- |

RNF-04

|     |
| --- |

La
&#x20; salida de 0–5 V del anemómetro deberá ser acondicionada antes de conectarse
&#x20; al ADC de 3.3 V.

|     |
| --- |

Hoja
&#x20; de datos del ZTS3000FSJT / diseño

|     |
| --- |

Se
&#x20; medirá la tensión de entrada al ADC y se comprobará que no supere el límite
&#x20; establecido.

|     |
| --- |

RNF-05

|     |
| --- |

El
&#x20; sistema deberá conservar la información histórica ante reinicios o
&#x20; interrupciones temporales de alimentación.

|     |
| --- |

Retroalimentación
&#x20; del profesor

|     |
| --- |

Se
&#x20; interrumpirá la alimentación y posteriormente se verificará la integridad de
&#x20; los registros almacenados.

|     |
| --- |

RNF-06

|     |
| --- |

El
&#x20; sistema deberá permitir identificar el estado de funcionamiento de sus
&#x20; principales subsistemas.

|     |
| --- |

Diseño
&#x20; del sistema

|     |
| --- |

Se
&#x20; verificará mediante indicadores, registros o mensajes de diagnóstico.

|     |
| --- |

RNF-07

|     |
| --- |

Los
&#x20; datos almacenados deberán utilizar un formato que facilite su posterior
&#x20; análisis.

|     |
| --- |

Diseño
&#x20; del sistema

|     |
| --- |

Se
&#x20; verificará que los archivos puedan ser importados y procesados mediante una
&#x20; herramienta externa como Python o Excel.

|     |
| --- |

RNF-08

|     |
| --- |

El
&#x20; sistema deberá poder funcionar sin una conexión permanente con la plataforma
&#x20; IoT.

|     |
| --- |

Requisito
&#x20; de operación remota / robustez

|     |
| --- |

Se
&#x20; desconectará la red y se verificará que la adquisición y almacenamiento local
&#x20; continúen.

|     |
| --- |

RNF-9

|     |
| --- |

Los
&#x20; componentes deberán operar dentro de sus rangos de alimentación
&#x20; especificados.

|     |
| --- |

Hojas
&#x20; de datos

|     |
| --- |

Se
&#x20; medirán las tensiones de alimentación durante el funcionamiento.

|     |
| --- |

RNF-10

|     |
| --- |

El
&#x20; diseño deberá permitir realizar mantenimiento o reemplazo de sensores y
&#x20; almacenamiento sin modificar completamente el sistema.

|     |
| --- |

Diseño
&#x20; del sistema

|     |
| --- |

Se
&#x20; inspeccionará físicamente el montaje y las conexiones.

##  

## **2.3 Requisitos ambientales**

Estos
requisitos corresponden a las condiciones del entorno en las que se espera que
opere el sistema.

|      |
| ---- |

**ID**

|      |
| ---- |

**Requisito ambiental**

|      |
| ---- |

**Fuente**

|      |
| ---- |

**Criterio de verificabilidad**

|     |
| --- |

RA-01

|     |
| --- |

El
&#x20; BME280 deberá instalarse dentro de una garita meteorológica que permita
&#x20; circulación de aire y reduzca la exposición directa al Sol y a la lluvia.

|     |
| --- |

Retroalimentación
&#x20; del profesor

|     |
| --- |

Inspección
&#x20; física del montaje.

|     |
| --- |

RA-02

|     |
| --- |

Los
&#x20; sensores expuestos deberán contar con protección física frente a lluvia
&#x20; directa cuando su principio de funcionamiento no requiera exposición al agua.

|     |
| --- |

Condiciones
&#x20; de operación / diseño

|     |
| --- |

Inspección
&#x20; del montaje.

|     |
| --- |

RA-03

|     |
| --- |

El
&#x20; sistema deberá poder operar dentro de los rangos de temperatura especificados
&#x20; por los fabricantes de los componentes.

|     |
| --- |

Hojas
&#x20; de datos

|     |
| --- |

Comparación
&#x20; entre las condiciones de operación y los rangos especificados.

|     |
| --- |

RA-04

|     |
| --- |

Los
&#x20; componentes electrónicos principales deberán estar protegidos frente a
&#x20; humedad y contacto accidental con agua.

|     |
| --- |

Condiciones
&#x20; ambientales / diseño

|     |
| --- |

Inspección
&#x20; del encapsulado y montaje.

|     |
| --- |

RA-05

|     |
| --- |

El
&#x20; sistema de generación solar deberá estar ubicado de manera que pueda recibir
&#x20; radiación solar suficiente para realizar la recarga.

|     |
| --- |

Diseño
&#x20; energético

|     |
| --- |

Prueba
&#x20; de generación y medición de tensión/corriente del sistema solar.

|     |
| --- |

RA-06

|     |
| --- |

El
&#x20; montaje deberá proporcionar estabilidad mecánica suficiente para mantener la
&#x20; orientación y posición de los sensores.

|     |
| --- |

Condiciones
&#x20; de instalación

|     |
| --- |

Inspección
&#x20; física y prueba de estabilidad.

|     |
| --- |

RA-07

|     |
| --- |

El
&#x20; anemómetro deberá instalarse de forma que tenga exposición suficiente al
&#x20; flujo de aire y no quede obstruido por elementos cercanos.

|     |
| --- |

Principio
&#x20; de medición del sensor

|     |
| --- |

Inspección
&#x20; del montaje y prueba de respuesta al flujo de aire.

##  

## **2.4 Requisitos normativos y de seguridad**

Los requisitos
normativos deben considerarse de acuerdo con el contexto final de instalación
del prototipo. En Colombia, el RETIE vigente fue modificado mediante la
Resolución 40284 del 23 de junio de 2026 y establece requisitos relacionados
con instalaciones eléctricas y productos objeto del reglamento. La
aplicabilidad concreta dependerá de si el prototipo se considera una
instalación eléctrica sujeta al reglamento o únicamente un equipo electrónico
de laboratorio.

|      |
| ---- |

**ID**

|      |
| ---- |

**Requisito normativo / de seguridad**

|      |
| ---- |

**Fuente**

|      |
| ---- |

**Criterio de verificabilidad**

|     |
| --- |

RN-01

|     |
| --- |

El
&#x20; sistema de alimentación deberá diseñarse evitando conexiones directas o
&#x20; condiciones de carga que puedan generar riesgos para las baterías.

|     |
| --- |

Seguridad
&#x20; eléctrica / fabricante de baterías

|     |
| --- |

Inspección
&#x20; del circuito de carga y verificación del uso de un controlador adecuado.

|     |
| --- |

RN-02

|     |
| --- |

El
&#x20; paquete de baterías deberá contar con un sistema de carga y protección
&#x20; compatible con la configuración utilizada.

|     |
| --- |

Seguridad
&#x20; de baterías / fabricante

|     |
| --- |

Inspección
&#x20; del cargador/BMS y verificación de sus especificaciones.

|     |
| --- |

RN-03

|     |
| --- |

Las
&#x20; conexiones eléctricas deberán estar aisladas y protegidas frente a
&#x20; cortocircuitos accidentales.

|     |
| --- |

Seguridad
&#x20; eléctrica / RETIE como referencia aplicable

|     |
| --- |

Inspección
&#x20; física y prueba de continuidad/aislamiento cuando corresponda.

|     |
| --- |

RN-04

|     |
| --- |

Los
&#x20; conductores y conectores deberán ser adecuados para las corrientes y
&#x20; tensiones presentes en el sistema.

|     |
| --- |

Seguridad
&#x20; eléctrica / RETIE

|     |
| --- |

Revisión
&#x20; de especificaciones y calibre/capacidad de los conductores.

|     |
| --- |

RN-05

|     |
| --- |

El
&#x20; sistema no deberá presentar partes eléctricas expuestas que puedan producir
&#x20; contacto accidental durante la operación normal.

|     |
| --- |

Seguridad
&#x20; eléctrica

|     |
| --- |

Inspección
&#x20; física del prototipo.

|     |
| --- |

RN-06

|     |
| --- |

En
&#x20; caso de utilizar conectividad inalámbrica mediante Pico W, se deberá utilizar
&#x20; la interfaz inalámbrica integrada dentro de las condiciones de operación
&#x20; permitidas para el dispositivo.

|     |
| --- |

Diseño
&#x20; / regulación de espectro

|     |
| --- |

Verificación
&#x20; del módulo utilizado y configuración de operación.

#  

# 3. Planificación de la verificación

La
verificación se realizará mediante pruebas individuales de los principales
subsistemas y una prueba integral del sistema. Cada prueba tendrá un objetivo,
condiciones de ejecución, procedimiento y resultado esperado.

## **3.1 Especificación de pruebas**

### **PR-01 — Adquisición de variables ambientales**

**Requisitos:** RF-01, RF-02, RF-03,
RF-04, RF-05, RF-06.

**Objetivo:** comprobar que los
sensores proporcionan lecturas que pueden ser adquiridas correctamente por la
Raspberry Pi Pico.

**Procedimiento:**

1.   
Encender
el sistema.

2.   
Inicializar
los sensores.

3.   
Leer
cada variable.

4.   
Mostrar
temporalmente las lecturas mediante el puerto de depuración.

5.   
Modificar
las condiciones ambientales cuando sea posible.

6.   
Verificar
que las lecturas respondan al cambio.

**Resultado
esperado:**
todos los sensores entregan valores válidos y responden ante cambios de las
variables medidas.

### **PR-02 — Activación mediante RTC**

**Requisitos:** RF-07, RF-08, RF-09.

**Objetivo:** verificar que el RTC
mantiene la referencia temporal y puede generar el evento de activación.

**Procedimiento:**

1.   
Configurar
el RTC con una hora conocida.

2.   
Programar
una alarma con un intervalo corto durante la prueba.

3.   
Colocar
el microcontrolador en el modo de bajo consumo utilizado.

4.   
Esperar
la activación.

5.   
Registrar
el momento en que se produce el despertar.

6.   
Repetir
varias veces.

**Resultado
esperado:**
el microcontrolador permanece en bajo consumo y se despierta aproximadamente en
el instante programado.

### **PR-03 — Almacenamiento en microSD**

**Requisitos:** RF-10, RF-11, RF-17,
RNF-05, RNF-07.

**Objetivo:** comprobar que las
mediciones pueden almacenarse persistentemente.

**Procedimiento:**

1.   
Insertar
una microSD.

2.   
Iniciar
el sistema.

3.   
Realizar
varias adquisiciones.

4.   
Verificar
la creación del archivo.

5.   
Revisar
que cada registro contenga fecha, hora y variables.

6.   
Reiniciar
el sistema.

7.   
Realizar
nuevas mediciones.

8.   
Verificar
que los nuevos datos se agreguen al histórico.

**Resultado
esperado:**
los datos anteriores permanecen y las nuevas mediciones se agregan sin
sobrescribir el histórico.

### **PR-04 — Recuperación después de pérdida de alimentación**

**Requisitos:** RF-10, RF-17, RNF-05.

**Objetivo:** verificar la
persistencia de la información ante una interrupción de alimentación.

**Procedimiento:**

1.   
Realizar
varias mediciones.

2.   
Interrumpir
deliberadamente la alimentación.

3.   
Volver
a alimentar el sistema.

4.   
Inicializar
nuevamente la estación.

5.   
Revisar
la microSD.

**Resultado
esperado:**
los registros realizados antes de la interrupción permanecen disponibles y el
sistema puede continuar almacenando nuevos registros.

### **PR-05 — Cálculo de estadísticos**

**Requisitos:** RF-12, RF-13, RF-14.

**Objetivo:** verificar la exactitud
del procesamiento estadístico.

**Procedimiento:**

1.   
Crear
un conjunto pequeño de datos conocido.

2.   
Introducirlo
en el algoritmo o utilizar mediciones almacenadas.

3.   
Calcular
externamente promedio, máximo, mínimo, varianza y desviación estándar.

4.   
Comparar
los resultados con los obtenidos por el sistema.

**Resultado
esperado:**
los resultados coinciden dentro del error numérico esperado.

### **PR-06 — Transmisión a plataforma IoT**

**Requisitos:** RF-15, RF-16.

**Objetivo:** verificar que los
datos puedan visualizarse remotamente.

**Procedimiento:**

1.   
Conectar
el sistema a la red.

2.   
Realizar
una medición.

3.   
Transmitir
los datos.

4.   
Acceder
a la plataforma IoT.

5.   
Verificar
la recepción y actualización de las variables.

**Resultado
esperado:**
las mediciones aparecen correctamente en la plataforma y las gráficas se
actualizan.

### **PR-07 — Operación sin conexión**

**Requisitos:** RF-17, RNF-08.

**Objetivo:** verificar que una
pérdida de conectividad no detenga la adquisición.

**Procedimiento:**

1.   
Poner
el sistema en funcionamiento normal.

2.   
Interrumpir
la conexión de red.

3.   
Realizar
varias adquisiciones.

4.   
Verificar
que los datos se almacenen localmente.

5.   
Restablecer
la conexión.

6.   
Comprobar
el comportamiento definido para los datos pendientes de transmisión.

**Resultado
esperado:**
la estación continúa midiendo y almacenando datos, aunque no exista conexión
temporal con la plataforma.

### **PR-08 — Consumo energético**

**Requisitos:** RNF-01, RNF-02, RNF-9.

**Objetivo:** caracterizar el
consumo del sistema en sus principales estados.

**Procedimiento:**

1.   
Medir
corriente durante el estado de bajo consumo.

2.   
Medir
corriente durante adquisición.

3.   
Medir
corriente durante escritura en microSD.

4.   
Medir
corriente durante transmisión Wi-Fi.

5.   
Comparar
los consumos.

**Resultado
esperado:**
el estado de reposo presenta un consumo significativamente menor que los
estados activos.

### **PR-09 — Sistema solar y batería**

**Requisitos:** RNF-01, RA-05, RN-01,
RN-02.

**Objetivo:** verificar la capacidad
del sistema de alimentación para proporcionar energía al prototipo.

**Procedimiento:**

1.   
Medir
tensión del panel solar.

2.   
Verificar
la operación del controlador de carga.

3.   
Medir
tensión del banco de baterías.

4.   
Alimentar
el sistema desde la batería.

5.   
Verificar
que el sistema permanezca operativo.

6.   
Simular
o realizar un período de baja generación solar.

**Resultado
esperado:**
el sistema de alimentación proporciona las tensiones requeridas y permite el
funcionamiento del sistema sin conexión a una fuente externa.

### **PR-10 — Acondicionamiento del anemómetro**

**Requisitos:** RF-06, RNF-03, RNF-04.

**Objetivo:** comprobar que la señal
de 0–5 V del ZTS3000FSJT puede ser adquirida de forma segura por el ADC del
Pico.

**Procedimiento:**

1.   
Medir
la salida del anemómetro.

2.   
Medir
la señal después del divisor/acondicionamiento.

3.   
Aplicar
diferentes velocidades de flujo de aire.

4.   
Registrar
la lectura del ADC.

5.   
Comparar
la respuesta con la señal de entrada.

**Resultado
esperado:**
la señal entregada al ADC permanece dentro del rango permitido y la lectura
aumenta/disminuye de acuerdo con la salida del anemómetro.

### **PR-11 — Protección y montaje ambiental**

**Requisitos:** RA-01, RA-02, RA-03,
RA-04, RA-06, RA-07.

**Objetivo:** comprobar que el
montaje físico es adecuado para las condiciones ambientales previstas.

**Procedimiento:**

1.   
Inspeccionar
la ubicación de los sensores.

2.   
Verificar
la presencia y ventilación de la garita del BME280.

3.   
Verificar
la protección de los circuitos electrónicos.

4.   
Verificar
la estabilidad mecánica.

5.   
Comprobar
que el anemómetro no quede obstruido.

**Resultado
esperado:**
los sensores se encuentran protegidos o expuestos según corresponda a su
principio de medición y el montaje es mecánicamente estable.

# 3.2 Matriz inicial de trazabilidad requisito-prueba

|      |
| ---- |

**Requisito**

|      |
| ---- |

**Prueba**

|     |
| --- |

RF-01

|     |
| --- |

PR-01

|     |
| --- |

RF-02

|     |
| --- |

PR-01

|     |
| --- |

RF-03

|     |
| --- |

PR-01

|     |
| --- |

RF-04

|     |
| --- |

PR-01

|     |
| --- |

RF-05

|     |
| --- |

PR-01

|     |
| --- |

RF-06

|     |
| --- |

PR-01,
&#x20; PR-10

|     |
| --- |

RF-07

|     |
| --- |

PR-02

|     |
| --- |

RF-08

|     |
| --- |

PR-02,
&#x20; PR-03

|     |
| --- |

RF-09

|     |
| --- |

PR-02

|     |
| --- |

RF-10

|     |
| --- |

PR-03,
&#x20; PR-04

|     |
| --- |

RF-11

|     |
| --- |

PR-03

|     |
| --- |

RF-12

|     |
| --- |

PR-05

|     |
| --- |

RF-13

|     |
| --- |

PR-05

|     |
| --- |

RF-14

|     |
| --- |

PR-05

|     |
| --- |

RF-15

|     |
| --- |

PR-06

|     |
| --- |

RF-16

|     |
| --- |

PR-06

|     |
| --- |

RF-17

|     |
| --- |

PR-03,
&#x20; PR-07

|     |
| --- |

RF-18

|     |
| --- |

PR-02,
&#x20; PR-08

|     |
| --- |

RNF-01

|     |
| --- |

Inspección
&#x20; del prototipo

|     |
| --- |

RNF-02

|     |
| --- |

PR-09

|     |
| --- |

RNF-03

|     |
| --- |

PR-08

|     |
| --- |

RNF-04

|     |
| --- |

PR-10

|     |
| --- |

RNF-05

|     |
| --- |

PR-10

|     |
| --- |

RNF-06

|     |
| --- |

PR-04

|     |
| --- |

RNF-07

|     |
| --- |

PR-01,
&#x20; PR-07, PR-08

|     |
| --- |

RNF-08

|     |
| --- |

PR-03

|     |
| --- |

RNF-09

|     |
| --- |

PR-07

|     |
| --- |

RNF-10

|     |
| --- |

PR-08,
&#x20; PR-09, PR-10

|     |
| --- |

RNF-11

|     |
| --- |

Inspección
&#x20; del prototipo

|     |
| --- |

RA-01

|     |
| --- |

PR-11

|     |
| --- |

RA-02

|     |
| --- |

PR-11

|     |
| --- |

RA-03

|     |
| --- |

PR-11

|     |
| --- |

RA-04

|     |
| --- |

PR-11

|     |
| --- |

RA-05

|     |
| --- |

PR-09

|     |
| --- |

RA-06

|     |
| --- |

PR-11

|     |
| --- |

RA-07

|     |
| --- |

PR-11

|     |
| --- |

RN-01

|     |
| --- |

PR-09
&#x20; / inspección

|     |
| --- |

RN-02

|     |
| --- |

PR-09
&#x20; / inspección

|     |
| --- |

RN-03

|     |
| --- |

Inspección

|     |
| --- |

RN-04

|     |
| --- |

Inspección

|     |
| --- |

RN-05

|     |
| --- |

Inspección

|     |
| --- |

RN-06

|     |
| --- |

Inspección
&#x20; / configuración

# 3.3 Demostración integral durante la sustentación

La
demostración final se organizará de manera que el funcionamiento del sistema
pueda observarse de principio a fin, evitando depender únicamente de una
explicación teórica.

Primero se
presentará físicamente la estación, mostrando el panel solar, el sistema de
almacenamiento y regulación de energía, la Raspberry Pi Pico, la memoria
microSD y los sensores. Se explicará brevemente la función de cada subsistema y
el flujo general de información.

Posteriormente
se mostrará el funcionamiento de una adquisición. Se provocarán cambios
controlados en algunas variables, por ejemplo, modificando la temperatura del
BME280, exponiendo el sensor UV a una fuente de radiación UV apropiada,
humedeciendo el sensor de lluvia y generando un flujo de aire sobre el
anemómetro. Se verificará que las lecturas cambien y sean procesadas por el microcontrolador.

A continuación,
se demostrará el funcionamiento del RTC. El sistema se colocará en el modo de
bajo consumo y se mostrará cómo el evento generado por el RTC provoca la
activación del sistema. Después de despertar, se realizará una nueva
adquisición.

Se demostrará
también el almacenamiento local. Se mostrará el archivo de la microSD con los
registros de fecha, hora y variables medidas. Se podrá reiniciar o desconectar
temporalmente el sistema y posteriormente comprobar que los registros
históricos permanecen disponibles.

Después se
mostrará la plataforma IoT. Se realizará una nueva medición y se comprobará que
los datos aparecen remotamente y que las gráficas se actualizan.

Finalmente se
demostrará el comportamiento energético. Se mostrará la alimentación mediante
batería y, si las condiciones de la sustentación lo permiten, la contribución
del panel solar. También se podrá mostrar la diferencia de consumo entre el
estado activo y el estado de bajo consumo mediante un instrumento de medición.

La
demostración seguirá, por tanto, el flujo:

**condición
ambiental → adquisición → procesamiento → almacenamiento → transmisión →
visualización → bajo consumo → nueva activación.**
