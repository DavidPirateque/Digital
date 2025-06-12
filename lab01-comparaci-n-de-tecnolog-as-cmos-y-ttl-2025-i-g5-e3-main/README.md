[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-2e0aaae1b6195c2367325f4f02e2d04e9abb55f0b24a779b69b11b9e10269abc.svg)](https://classroom.github.com/online_ide?assignment_repo_id=19366815&assignment_repo_type=AssignmentRepo)
# Lab01 - Comparación de tecnologías CMOS vs TTL

## Integrantes 


[Gabriel Santiago Rodriguez Torres](https://github.com/santiago85319)

[Luis Allberto Mendoza Rojas](https://github.com/lmendozar2001)

[cristian stiven hoyos peralta](https://github.com/hoyoscristian)



### SIMULACIONES Y ANÁLISIS TEÓRICO

Trabajo previo

2. Circuitos equivalentes:

En primer lugar, se buscó la hoja de datos del dispositivo TTL propuesto en la guía práctica. Asimismo, se buscó el datasheet del dispositivo SN74LS04 de marca ONE/UBC; allí se pudieron evidenciar algunos datos claves de su construcción y sus características. Asimismo, se realizo el montaje del circuito en el entorno simulado de LTspice como se muestra en la siguiente imagen.


<p align="center">
  <img src= imagenes/CircuitoequivalenteTTL.png alt="Circuito equivalente TTL" width="600">
</p>

En la misma línea, el funcionamiento lógico cuando la entrada (A) está baja (nivel lógico bajo “0”); el transistor de entrada (Q1) está en corto (apagado), por lo cual no conduce corriente. Lo anterior, permite que la corriente fluya a través de una resistencia de pull-up y active el transistor Q2. También, este transistor (Q2) conduce; saturando el transistor Q3 y conectando la salida a tierra (nivel lógico bajo). Ahora bien, cuando la entrada (A) está alta (nivel lógico “1”); el transistor de entrada (Q1) entra en saturación (encendido) y conduce corriente. De acuerdo a lo anterior, se produce una desviación de la corriente de la base de Q2, apagándose; por lo que, a su vez se apaga el Q3. Y por último, la salida se conecta a VCC; por medio de una resistencia de pull-up, generando un nivel lógico alto (1).

En segundo lugar, el circuito equivalente de un inversor CMOS (como el CD4069) se basa en un par de transistores MOS complementarios; los cuales son:  un MOSFET de canal N (NMOS) y un MOSFET de canal P (PMOS), configurados en carga activa. Tambien se llevo acabo el montaje del circuito en el entorno simulado de LTspice como se muestra en la siguiente imagen.


<p align="center">
  <img src= imagenes/CircuitoequivalentedecompuertaCMOS.png alt="Circuito equivalente CMOS" width="600">
</p>

El funcionamiento lógico, como en el circuito anterior, se divide en dos partes. Primero, cuando la entrada (A) está baja (nivel lógico bajo “0”): el PMOS está encendido, conectando la salida a VDD y generando un nivel lógico alto (1); luego, el NMOS está apagado, evitando la conexión a tierra de la salida. Segundo, cuando la entrada (A) está alta (nivel lógico bajo “1”): el NMOS está encendido, conectando la salida a tierra (GND) y generando un nivel lógico bajo; de la misma forma,el PMOS está apagado, desconectando VDD de la salida.

3. Importación y configuración de modelos SPICE:

En este apartado, se importaron los modelos de las compuertas para LTspice siguiendo las recomendaciones expuestas en la guia de la practica. Sin embargo, se opto por no modificar los simbolos de las compuertas por efectos practicos. Asimismo, en la siguiente imagen se evidencia el aspecto de las compuertas en el entorno simulado de LTspice.


<p align="center">
  <img src= imagenes/DiseñodelascompuertaspropuestasenLTspice.png alt="Diseñode las compuertas propuestas en LTspice" width="600">
</p>

4. Simulación del comportamiento ante señales de entrada:

Para el primer caso, se implementó el circuito con una compuerta SN74LS04 en el entorno de simulación LTspice; en el cual, se utilizó una señal cuadrada de 100 kHz. Después,se analizaron los resultados de la simulación para determinar los parámetros exigidos en la guía. Además, se hicieron variaciones en la frecuencia en el rango especificado; sin embargo se van exponer dos muy distintas para analizar que fectos tiene en la compuerta; una es a 100kHz y la otra es 1kHz. 


<p align="center">
  <img src= imagenes/SimulacióndevoutyvindelacompuertaSN74LS04con100kHz.png alt="Simulación de vout y vin de la compuerta SN74LS04 con 100kHz" width="600">
</p>

<p align="center">
  <img src= imagenes/SimulacióndevoutyvindelacompuertaSN74LS04a1kHz.png alt="Simulación de vout y vin de la compuerta SN74LS04 a 1kHz" width="600">
</p>



Para señales de entrada con frecuencias bajas en comparación con señales con frecuencias altas se presenta un fenómeno interesante. Por ejemplo, con una señal de entrada con una frecuencia de un 1kHz, la compuerta TTL se demora mucho más con un valor lógico “1” que con un valor lógico “0”. Sin embargo, para una frecuencia de 100kHz pasa totalmente lo contrario; debido a que se demora más tiempo en un valor lógico “0” que en el valor lógico “1”. Ahora bien, en estas dos condiciones el tiempo que tarda en “0” es aproximadamente 6us. Sin embargo cuando están en un nivel lógico “1” se demora mucho más en regresar al “0” a una frecuencia baja que a una frecuencia alta. Ya que,  para el caso de 1kHz se demora cerca de 52.25mV aproximadamente; por otro lado a una frecuencia alta se demora cerca de 4us, lo cual nos da una diferencia muy alta.

5. Funciones de transferencia de las compuertas:

Parámetros clave obtenidos en la simulación

1) VIH (Voltaje de entrada alto): Es el nivel mínimo de entrada necesario para que la salida comience a caer significativamente desde VOH, el cual es aproximadamente: VIH ≈ 2.47V. 

2) VIL (Voltaje de entrada bajo): Es el nivel máximo de entrada para que la salida no suba significativamente desde VOL, el cual es aproximadamente: VIL ≈ 0.8V .

3) VOH (Voltaje de salida alto): Es el nivel lógico alto garantizado de la salida, el cual es aproximadamente: VOH ≈ 3.4V (con carga típica de 400 Ω).

4) VOL (Voltaje de salida bajo): Nivel lógico bajo garantizado de la salida, el cual es aproximadamente: VOL ≈ 0.2V.


<p align="center">
  <img src= imagenes/FuncióndetransferenciadelacompuertaSN74LS04.png alt="Función de transferencia de la compuerta SN74LS04" width="600">
</p>

Para el segundo caso, se implementó el circuito con una compuerta CD4069 en el entorno de simulación LTspice; para el cual, se utilizó una señal cuadrada de 100 kHz. Después,se analizaron los resultados de la simulación para determinar los parámetros exigidos. Además, se hicieron variaciones en la frecuencia en el rango especificado; sin embargo se van exponer dos muy distantes se van a utilizar los valores de frecuencia que se usaron para la compuerta SN74LS04 y así analizar qué efectos tiene en la compuerta.



<p align="center">
  <img src= imagenes/SimulacióndevoutyvindeCD4069100kHz.png alt="Simulación de vout y vin de CD4069 100kHz" width="600">
</p>


<p align="center">
  <img src= imagenes/SimulacióndevoutyvindeCD40691kHz.png alt="Simulación de vout y vin de CD4069 1kHz" width="600">
</p>

Para señales de entrada con frecuencias bajas en comparación con señales con frecuencias altas se presenta un fenómeno totalmente contrario a la compuerta SN74LS04.
Ya que, con una señal de entrada con una frecuencia de un 1kHz y de 100kHz, la compuerta CMOS se demora aproximadamente lo mismo en estado lógico “0”. Además, se debe tener en cuenta que esta compuerta trabaja entre 3 y 15 voltios de alimentación; se decidió optar por alimentar la compuerta con 15V.
 
Parámetros clave obtenidos en la simulación

1) VIH (Voltaje de entrada alto): Es el nivel mínimo de entrada necesario para que la salida comience a caer significativamente desde VOH, el cual es aproximadamente: VIH ≈ 10.5V. 

2) VIL (Voltaje de entrada bajo): Es el nivel máximo de entrada para que la salida no suba significativamente desde VOL, el cual es aproximadamente: VIL ≈ 4.5V .

3) VOH (Voltaje de salida alto): Es el nivel lógico alto garantizado de la salida, el cual es aproximadamente: VOH ≈ 15 V (con carga típica de 400 Ω).

4) VOL (Voltaje de salida bajo): Nivel lógico bajo garantizado de la salida, el cual es aproximadamente: VOL ≈ 0V.


<p align="center">
  <img src= imagenes/FuncióndetransferenciadelacompuertaCMOS.png alt="Función de transferencia de la compuerta CMOS" width="600">
</p>

6. Estimación de tiempos de conmutación:

En primer lugar, para realizar la medición de los tiempos de subida (tr), bajada (tf) y retardo (tPLH, tPHL y tP) para las compuertas propuestas; se utilizaron las ecuaciones o comandos que se dieron en clase de laboratorio. Además, se realizó una tabla en la cual los datos se encuentran clasificados.


<p align="center">
  <img src= imagenes/TiemposdelacompuertaTTL.png alt="Tiempos de la compuerta TTL" width="600">
</p>

<p align="center">
  <img src= imagenes/TiemposdelacompuertaCMOS.png alt="Tiempos de la compuerta CMOS" width="600">
</p>

### Tabla de comparación de los tiempos requeridos de las compuertas propuestas:

####  Comparación de Tiempos Característicos (Datasheet, Simulación y Experimental)

| Compuerta     | Fuente       | `tr` (ns)    | `tf` (ns)    | `tPLH` (ns)       | `tPHL` (ns)       |
|---------------|--------------|--------------|--------------|-------------------|-------------------|
| **CD4069UBC** | Datasheet    | ≤ 20        | ≤ 20        | 50 (typ 25) | 50 (typ 25) |
|               | Simulación (spice de la profe)  | 92     | 55.8     | 36.4       | 153        |
|               | Experimental | 17.3     | 5.58    | 32       | 37         |
| **SN74LS04**  | Datasheet    | ≤ 15         | ≤ 15         | 4 – 15 (typ 9)    | 4 – 15 (typ 9)    |
|               | Simulación(spice de la profe)   | 8.61           | 16.5      | 3.94      | 4.08       |
|               | Experimental | 9.72         | 10.43         | 8.67       | 9.23        |




7. ##  Estimación Teórica del Consumo de Potencia

###  Ecuaciones Utilizadas

#### CMOS:
Potencia dinámica (dominante en CMOS):
P_dinámica = N × C_L × V_DD² × f

- N = 5 compuertas  
- C_L = 10 pF = 10 × 10⁻¹² F  
- V_DD = 15 V

#### TTL (alimentado a 5 V):

Potencia estática dominante:
P_estática = N × I_cc × V_CC

- N = 5 compuertas  
- I_cc = 4 mA  
- V_CC = 5 V

###  Tabla Comparativa de Potencia Estimada

| Frecuencia (MHz) | P_dinámica, CMOS (mW) | P_total, TTL(mW) |
|------------------|----------------------------------------|----------------------------------|
| 0.1              | 0.001125                               | 100                              |
| 1                | 0.01125                                | 100                              |
| 5                | 0.05625                                | 100                              |
| 10               | 0.1125                                 | 100                              |

> En tecnología CMOS, el consumo de potencia aumenta con la frecuencia de operación, siendo casi nulo en reposo. En cambio, en TTL predomina la potencia estática, generando un consumo constante incluso cuando no hay conmutación.







### COMPORTAMIENTO EXPERIMENTAL
#### Comportamiento del CMOS CD4069  
---

#####  Transición Ascendente (de nivel bajo a nivel alto lógico)
La siguiente figura muestra una captura de osciloscopio donde se visualiza claramente la inversión de la señal, así como el retardo característico entre la entrada y la salida:

<p align="center">
  <img src= imagenes/CD4069Subida.png alt="Captura de osciloscopio - CD4069Subida" width="600">
</p>


#####  Señal de entrada (amarilla)
La señal de entrada presenta una forma de onda cuadrada con transiciones rápidas entre los niveles lógicos bajo y alto. Este tipo de señal es común en sistemas digitales y resulta adecuada para evaluar el comportamiento de dispositivos lógicos como el inversor CD4069.

---

#####   Señal de salida (azul)
La salida presenta un **retardo respecto a la entrada**, característica típica de los inversores construidos con tecnología CMOS. Este desfase confirma la función de inversión lógica del CD4069:

- Cuando la entrada está en **nivel alto (lógico "1")**, la salida pasa a **nivel bajo (lógico "0")**.  
- Cuando la entrada está en **nivel bajo (lógico "0")**, la salida pasa a **nivel alto (lógico "1")**.

Los niveles de voltaje observados están dentro del rango esperado para dispositivos CMOS:

- **Nivel lógico bajo:** < 1 V  
- **Nivel lógico alto:** = 5 V 

---

#####   Transición y tiempo de propagación
La señal de salida responde con rapidez a los cambios en la entrada, aunque con un pequeño **tiempo de propagación**, característico de la operación interna de los inversores CMOS. Este retardo se produce una vez que la entrada supera el umbral de conmutación correspondiente al nivel lógico alto.

El comportamiento observado indica una **respuesta rápida y estable**, lo que evidencia la eficiencia del CD4069 al trabajar con señales digitales.

---

#####  Rango de operación
El CD4069 puede operar con una amplia gama de tensiones de alimentación, típicamente entre **3 V y 15 V**, lo que le permite adaptarse a diversas aplicaciones electrónicas.

En aplicaciones el CD4069 se destaca por:

- Alta fiabilidad  
- Bajo consumo de energía  
- Buena compatibilidad con distintos niveles de señal  
- Desempeño consistente como inversor lógico
---
##### Interpretación y análisis

- La **inversión lógica** observada, así como los **niveles de ruido**, se encuentran dentro de los márgenes esperados para este tipo de circuito, lo que valida el correcto funcionamiento del CD4069 como inversor CMOS.

- La forma de onda de salida presenta **tiempos de transición descendente**, influenciados por las características internas de conmutación del CD4069. Estos tiempos pueden variar dependiendo de la carga y la configuración del circuito.

- Se confirma que los **tiempos de propagación y la velocidad de conmutación** están condicionados por la **carga conectada** al inversor y por la **tensión de alimentación** aplicada. Estos factores deben tenerse en cuenta en el diseño de sistemas digitales de alta precisión.

- Este análisis es fundamental para validar el comportamiento del CD4069 en **aplicaciones digitales**, especialmente en aquellas que requieren:
  - Conmutación rápida
  - Alta inmunidad al ruido
  - Estabilidad en señales lógicas

---


####  Transición Descendente (de nivel Alto a nivel bajo lógico)
La siguiente figura muestra una captura de osciloscopio donde se visualiza claramente la inversión de la señal, así como el retardo característico entre la entrada y la salida:

<p align="center">
  <img src= imagenes/CD4069Bajada.png alt="Captura de osciloscopio - CD4069Bajada" width="600">
</p>

---
##### Comportamiento de la señal de entrada  (amarilla)

La señal amarilla representa la entrada al inversor CD4069:

- Muestra una transición clara desde un nivel alto (estado lógico "1") hacia un nivel bajo (estado lógico "0").

##### Comportamiento de la señal de salida (azul)

La señal azul corresponde a la salida del CD4069:

- Refleja la inversión lógica esperada en un inversor CMOS:
  - Cuando la entrada desciende a nivel bajo (lógico "0"), la salida asciende a nivel alto (lógico "1").
- Este comportamiento valida el funcionamiento correcto del CD4069 como inversor.

##### Forma de onda de salida (CH2)

- Se observa una ligera demora entre el cambio de la entrada y la respuesta en la salida, atribuible al tiempo de propagación del dispositivo.

##### Dependencia de los tiempos de propagación

El análisis gráfico revela que los tiempos de propagación y las transiciones de la señal están influenciados por múltiples factores, entre ellos:

- La carga conectada al dispositivo.
- La tensión de alimentación utilizada.

Estos aspectos son especialmente relevantes en el diseño de sistemas digitales donde se manejan señales rápidas o que son sensibles al ruido, ya que impactan directamente en el rendimiento del inversor y la estabilidad de la señal.

---




### Comportamiento  del Oscilador Basado en Anillo
---
#### Descripción de la Señal Observada


- La forma de onda observada en el osciloscopio corresponde a una **señal periódica senosoidal**.
- Aunque no se trata de una señal perfectamente cuadrada ni sinusoidal, su comportamiento es estable y repetitivo, lo que confirma el funcionamiento correcto del oscilador.


<p align="center">
  <img src= imagenes/CMOSOscilador.png alt="Captura de osciloscopio - CMOSOscilador" width="600">
</p>

---


#### Análisis del Oscilador en Anillo

El circuito implementado corresponde a un **oscilador en anillo**, un tipo de generador de señal compuesto por un **número impar de inversores** (NOT) conectados en cascada. La **salida del último inversor** se retroalimenta directamente a la **entrada del primero**, generando oscilaciones periódicas sin necesidad de una señal de entrada externa.

---

#### Principio de Funcionamiento

- El oscilador en anillo se basa en la **retroalimentación negativa con retardo**, donde el cambio de estado tarda en propagarse por todas las compuertas hasta volver a la entrada inicial.
- Al tratarse de un número impar de inversores, la señal se invierte en cada etapa, lo que garantiza una realimentación inestable que da lugar a una oscilación continua.
- Se utilizó el circuito integrado **CD4069UBC**, un inversor CMOS sin búfer, ideal para operar en modo lineal y permitir la auto-oscilación.

---



#### Características Técnicas de la Señal

| Parámetro            | Valor Aproximado                  |
|----------------------|------------------------------------|
| **Forma de onda**    | senosoidal oscilatoria     |
| **Amplitud máxima**  | ~1.57 V (nivel lógico alto - VOH) |
| **Amplitud mínima**  | ~0 V (nivel lógico bajo - VOL)    |
| **Periodo**          | Variable según número de inversores |
| **Frecuencia**       | Inversa del periodo total          |

---

#### Efectos del Aumento de Compuertas

| Número de Inversores | Tiempo Total de Retardo | Frecuencia de Oscilación | Forma de Onda                   | Observaciones                     |
|----------------------|------------------------|--------------------------|--------------------------------|----------------------------------|
| 3                    | Bajo                   | Alta                     | Onda con transiciones rápidas  | Frecuencia mayor, señal más cuadrada, pero menos estable. |
| 5                    | Moderado               | Media                    | Onda más suavizada, senosoidal | Mejor estabilidad y señal más limpia.            |
| 7 o más              | Alto                   | Baja                     | Onda aún más suavizada, baja frecuencia | Oscilación más lenta aunque con mayor influencia de ruido y distorsión. |

- Al **aumentar el número de inversores**, el retardo total se incrementa, lo que reduce la frecuencia de la señal de salida.
- El **aumento del retardo** suaviza las transiciones, lo que genera una señal con forma más cercana a una onda sinusoidal pero con menor frecuencia.
- Cuando se utilizan pocas compuertas (ejemplo 3 compuertas), la frecuencia es alta, pero la señal puede presentar **mayores distorsiones** y menor estabilidad debido a efectos de ruido.
- Al usar más compuertas (ejemplo 5 o 7 compuertas), la frecuencia disminuye, y la señal tiende a ser más estable, pero puede aparecer una mayor distorsión por capacitancias y retardos acumulados.

---


#### La parte experimental confirma que el oscilador en anillo diseñado:

- Genera una señal **oscilatoria estable sin necesidad de señal externa**.
- Presenta una forma de onda característica de este tipo de osciladores, con una **respuesta dinámica influenciada por el número de etapas y las propiedades físicas de los componentes**.
- Es completamente funcional tanto con **3 como con 5 inversores**, y se observaron oscilaciones incluso con **6 inversores**, lo cual es posible en configuraciones prácticas al operar en la región lineal del inversor.

Este tipo de oscilador es una excelente opción para generar señales de prueba o como base para circuitos digitales sincronizados, especialmente en aplicaciones educativas y de prototipado rápido.

---
### Comportamiento  de la compuerta TTL SN74LS04.
---
#### TTL - SUBIDA
---

<p align="center">
  <img src= imagenes/TTLSubida.png alt="Captura de osciloscopio - TTLSubida" width="600">
</p>

---
El 74LS04 es un inversor lógico TTL ampliamente utilizado en sistemas digitales debido a su alta velocidad de conmutación, bajo consumo de energía y confiabilidad al manejar señales lógicas. A continuación, se presenta un análisis detallado de su comportamiento en condiciones de prueba típicas, centrado en la transición ascendente (bajo a alto).

#### Observaciones Experimentales

Señales Observadas

Amarilla : Entrada al inversor.

Azul : Salida del inversor.

La observación muestra un desfase característico entre ambas señales, evidenciando el comportamiento inversor de la compuerta TTL. Cuando la entrada (canal amarillo) realiza una transición de bajo a alto, la salida (canal azul) responde con una transición correspondiente de alto a bajo, y viceversa.

#### Niveles de Tensión

En la salida : Valor máximo observado: 4.14 V - debido a nuestro circuito equivalente

Este nivel confirma la salida lógica alta del inversor, acorde con las especificaciones típicas del 74LS04.

#### Voltajes Medios (Valor Promedio de la Señal)

Entrada : 507.8 mV

Salida : -742.5 mV

El valor negativo observado en el voltaje medio de la salida indica una dominancia del nivel bajo, lo que es coherente con la operación de un inversor que responde rápidamente a las transiciones de entrada. La inversión lógica queda confirmada por la diferencia entre los valores medios de ambas señales.

#### Comportamiento en las Transiciones

Durante la transición ascendente:

La salida (azul) inicia en estado bajo y cambia rápidamente a estado alto. Este comportamiento evidencia tiempos de subida y bajada muy breves, característicos de la tecnología TTL. La forma de onda es abrupta, con flancos pronunciados, lo que confirma la capacidad del 74LS04 para operar con rapidez en circuitos digitales.

---

#### TTL - BAJADA

---

<p align="center">
  <img src= imagenes/TTLBajada.png alt="Captura de osciloscopio - TTLBajada" width="600">
</p>

---


#### Análisis de la función de transferencia

 La gráfica también muestra la relación entre la señal de entrada (amarillo) y la
 salida invertida (azul). En este caso, la señal de entrada comienza en un estado
 alto y realiza la transición hacia un estado bajo, mientras que la salida efectúa la inversión
 correspondiente.
 #### Mediciones de voltaje:
 Canal 1 (Entrada): 617.2 mV

 Canal 2 (Salida): 4.14 V

 Los voltajes medios registrados son:

 Canal 1: 515.6 mV

 Canal 2: −381.6 mV
 
 #### Análisis de transición:
 
Comparado con la primera gráfica, el voltaje medio en la señal azul está más cerca del centro
 del rango lógico bajo, lo que indica que el circuito responde de manera adecuada a los
 cambios en la señal de entrada.

La salida (azul) se mantiene en un nivel lógico bajo (0 V) mientras la entrada (amarillo)
 se encuentra en un nivel lógico alto (4.14 V). Este comportamiento es característico de un
 inversor.

El cambio de estado de la salida es rápido y definido, validando el desempeño esperado del
 circuito TTL.

---

### Referencias

------
[1] Cornell University. (2010). EE 2301 Experiment 3: TTL and CMOS Logic [PDF]. Disponible en: http://www.lns.cornell.edu/~ib38/teaching/p360/lectures/wk09/l26/EE2301Exp3F10.pdf
