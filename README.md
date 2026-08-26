# Laboratorio_2_Instrumentacion
Segunda entrega de laboratorio instrumentación biomédica y biosensores BMED C. Grupo conformado por: Alissia Montealegre Quintero, Raul Alexander Peñuela Jimenez.





# Estimación del nivel de estrés basada en la respuesta galvánica cutánea (GSR)  

## Resumen

En este proyecto se desarrolló un prototipo portátil para la adquisición y monitoreo de la respuesta galvánica de la piel (GSR) mediante un microcontrolador ESP32. La señal fue adquirida de forma no invasiva utilizando electrodos adhesivos de gel ubicados en la región palmar de la mano. Como parte de la validación inicial del sistema, se empleó un potenciómetro para simular variaciones de la señal antes de realizar las mediciones con electrodos. La adquisición se realizó mediante el conversor analógico-digital de 12 bits del ESP32 y se aplicó un promedio de lecturas consecutivas para reducir fluctuaciones. Asimismo, se implementó comunicación inalámbrica mediante WiFi y una interfaz en MATLAB para la visualización de la señal en tiempo real. Los resultados permitieron comprobar el funcionamiento del sistema de adquisición y establecer una base para la calibración y el análisis de la señal GSR bajo diferentes condiciones de medición.

#### Palabras clave: respuesta galvánica de la piel, GSR, ESP32, adquisición de señales fisiológicas, electrodos de gel, WiFi, MATLAB.

---

# I. Introducción

Las señales fisiológicas proporcionan información que permite estudiar los cambios en el organismo ante diferentes condiciones. la conductancia eléctrica de la piel asociada principalmente con la actividad de las glándulas sudoríparas. La respuesta está relacionada con la actividad del sistema nervioso autónomo y, por lo tanto, su registro puede utilizarse para observar las modificaciones fisiológicas producidas en respuesta a ciertos estímulos.

Para registrar la señal GSR es necesario que la piel esté en contacto adecuado con el sistema de medición y que exista una etapa que convierta la variación eléctrica en datos que puedan ser procesados ​​y visualizados. En aplicaciones con movilidad, estas mediciones pueden ser más difíciles si las tareas se realizan con equipos de laboratorio y conexiones físicas. Por Esto resulta necesario pensar en sistemas que concentren la adquisición y el procesamiento en equipos de menor tamaño y que permitan enviar la información sin depender de una conexión directa a un ordenador.

El El objetivo de este trabajo fue desarrollar un sistema portátil basado en el ESP32 que permitiera la adquisición y transmisión de la señal GSR. La La medición se realizó por medio de electrodos de gel adhesivos ubicados en la región palmar.de la mano, mientras que el microcontrolador se encargó de leer la señal y convertirla en datos digitales. Para aumente la estabilidad de las mediciones se utiliza el promedio de lecturas consecutivas. El ESP32 viene con WiFi integrado, lo cual permitió enviar la información a un dispositivo externo para poder visualizarla posteriormente.

El sistema se verificó inicialmente con un potenciómetro, el cual se empleó para producir variaciones controladas y comprobar la lectura analógica y la transmisión de los datos. Esta fase comprobada una vez, incorporaron los electrodos para la adquisición de la señal directamente sobrela piel. Los Los datos obtenidos se vincularon posteriormente con MATLAB para su representación gráfica en tiempo real. De este modo se estudió el funcionamiento de un prototipo portátil capaz de adquirir, procesar, transmitir y visualizar la respuesta galvánica de la piel.

---

# II. Marco Teórico

## A. Respuesta Galvánica (GSR)

La Respuesta Galvánica de la Piel (GSR, por sus siglas en inglés) es una señal fisiológica que refleja los cambios en las propiedades eléctricas de la piel debido a las variaciones en la actividad de las glándulas sudoríparas. habilidad de la piel para conducir corriente eléctrica, produciendo cambios en la resistencia o conductancia medida entre dos puntos de contacto.

La actividad de las glándulas sudoríparas está regulada principalmente por el sistema nervioso autónomo, por lo que la señal GSR puede presentar cambios ante diferentes estímulos fisiológicos o ambientales. Debido a esta característica, la GSR es ampliamente utilizada en estudios relacionados con respuestas fisiológicas, interacción humano-máquina y monitoreo no invasivo.

La señal GSR permite identificar variaciones en la actividad fisiológica, aunque sus cambios no pueden asociarse por sí solos con un diagnóstico médico o con una emoción determinada. La interpretación de la señal está condicionada por factores como la calibración del sistema, las condiciones en las que se realiza la medición y las características propias de cada persona.

---

## B. Medición de la respuesta galvánica de la piel

La adquisición de la señal GSR se realiza mediante electrodos superficiales colocados directamente sobre la piel. Los electrodos actúan como punto de contacto entre el tejido y el circuito de medición, permitiendo captar las variaciones eléctricas de la piel para su posterior adquisición y procesamiento.

Al ubicar dos electrodos sobre la superficie de la piel, es posible registrar variaciones en su resistencia eléctrica asociadas con cambios en la humedad superficial. Cuando aumenta la actividad de las glándulas sudoríparas, la conductividad de la piel tiende a incrementarse y, en consecuencia, su resistencia disminuye. Esta variación modifica la señal eléctrica captada por el sistema de adquisición.

La calidad de la medición depende de factores como:

- Ubicación de los electrodos,
- Presión ejercida sobre la piel,
- Calidad del contacto,
- Movimiento del usuario,
- Condiciones ambientales.

Por lo anterior, la señal adquirida requiere un procesamiento que reduzca las variaciones no deseadas y facilite su análisis e interpretación.

---

## C. Electrodos adhesivos de gel 

Los electrodos adhesivos de gel permiten establecer el contacto eléctrico entre la piel y el circuito de medición. El material conductor que contienen reduce la impedancia presente en la interfaz piel-electrodo, lo que facilita la captación de las variaciones eléctricas generadas en la superficie de la piel.

En la adquisición de señales fisiológicas, este tipo de electrodos resulta práctico debido a su facilidad de colocación y a que mantiene un contacto estable con la piel durante mediciones de corta duración. En el sistema desarrollado, los electrodos se utilizaron para captar los cambios asociados con las variaciones de conductividad de la piel y transmitirlos hacia la etapa de adquisición.

---

## D. Conversión Analógica-Digital (ADC)

La señal captada por los electrodos corresponde inicialmente a una señal analógica, debido a que las variaciones eléctricas de la piel se presentan de forma continua. Para que pueda ser procesada por un sistema digital, esta señal debe pasar por una etapa de conversión analógico-digital (ADC), encargada de transformar la información eléctrica en datos que puedan ser interpretados por el microcontrolador.

La conversión analógico-digital permite representar las variaciones de la señal mediante valores digitales que posteriormente pueden ser almacenados, procesados o transmitidos. Sin embargo, estos valores constituyen una representación de la señal eléctrica adquirida y no corresponden directamente a magnitudes fisiológicas como la resistencia o la conductancia de la piel. Para obtener estas magnitudes es necesario establecer una relación entre la señal eléctrica medida y la variable de interés mediante la caracterización y calibración del sistema de adquisición.

---

## E. Microcontrolador ESP32

El ESP32 es un microcontrolador utilizado en sistemas embebidos debido a que integra recursos de procesamiento, entradas y salidas digitales, conversión analógico-digital y comunicación inalámbrica. Entre sus características se encuentran la conectividad WiFi y Bluetooth, lo que permite intercambiar información con otros dispositivos sin necesidad de una conexión física.

En el sistema desarrollado, el ESP32 se utiliza como unidad central para la adquisición y transmisión de la señal GSR. La señal proveniente de los electrodos ingresa al microcontrolador a través de una entrada analógica, donde es convertida a datos digitales para su procesamiento. A partir de estos datos, el dispositivo puede realizar operaciones básicas sobre la señal y enviarla de forma inalámbrica hacia un equipo externo. La integración de estas funciones permite utilizar el ESP32 en sistemas de adquisición de señales fisiológicas que requieren un dispositivo de tamaño reducido y comunicación inalámbrica.

---

## F. Procesamiento y filtrado de señales

Durante la adquisición de señales fisiológicas pueden aparecer variaciones que no hacen parte de la respuesta que se quiere medir. En la GSR, estas variaciones pueden deberse al ruido eléctrico, al movimiento de la persona o a cambios en el contacto entre los electrodos y la piel. Como resultado, los valores registrados pueden presentar fluctuaciones que dificultan la observación de los cambios de la señal.

Una forma de reducir estas fluctuaciones es calcular el promedio de varias muestras tomadas de manera consecutiva. El valor obtenido representa las mediciones utilizadas en el cálculo y reduce el efecto de cambios puntuales entre muestras. De esta manera, la señal resultante presenta menos variaciones rápidas y puede observarse con mayor facilidad.

El número de muestras utilizadas influye en el nivel de suavizado. Si se emplean más muestras, la señal presenta menos fluctuaciones, pero los cambios rápidos también pueden verse reducidos. Si se utilizan menos muestras, se conservan mejor las variaciones de la señal, aunque el ruido puede tener mayor presencia. Por esta razón, la cantidad de muestras debe seleccionarse de acuerdo con el comportamiento de la señal que se desea registrar.

---

## G. Comunicación inalámbrica mediante Wifi

La comunicación inalámbrica permite enviar los datos adquiridos por un sistema electrónico sin utilizar cables entre el dispositivo de medición y el equipo que recibe la información. En sistemas de adquisición fisiológica, esta característica resulta útil cuando se necesita que la persona pueda moverse durante la medición o cuando se busca separar físicamente el dispositivo de adquisición del equipo utilizado para visualizar los datos.

El ESP32 incorpora conectividad WiFi, con la cual puede intercambiar información con otros dispositivos a través de una red inalámbrica. Dependiendo de la configuración, el microcontrolador puede conectarse a una red existente o establecer su propia red para permitir la comunicación con un dispositivo externo. Los datos adquiridos pueden enviarse mediante esta conexión hacia un computador u otro equipo encargado de recibirlos y procesarlos.

En el sistema desarrollado, la comunicación WiFi se utiliza para transmitir los valores obtenidos durante la adquisición de la señal GSR. El uso de esta conexión evita depender de un cable USB durante la visualización y permite que el equipo de adquisición funcione de manera independiente del computador. Además, la transmisión inalámbrica facilita la integración del microcontrolador con una interfaz de visualización, en este caso MATLAB, donde los datos pueden recibirse y representarse durante la medición.

La comunicación inalámbrica no modifica la señal fisiológica adquirida, sino que constituye el medio utilizado para transportar los datos desde el sistema de adquisición hasta el dispositivo de visualización. Por esta razón, su funcionamiento debe considerarse junto con aspectos como la velocidad de transmisión, la estabilidad de la conexión y el tiempo requerido para enviar y recibir los datos, especialmente cuando se busca representar la señal en tiempo real.

---

## H. Matlab

MATLAB es un entorno de programación orientado al cálculo numérico, el análisis de datos y la representación gráfica. Su estructura permite trabajar con matrices, vectores y conjuntos de datos, además de incorporar funciones para realizar operaciones matemáticas y procesar información proveniente de diferentes sistemas de adquisición.

En el análisis de señales, MATLAB permite representar los datos en función del tiempo y aplicar operaciones de procesamiento como filtrado, suavizado, análisis estadístico y transformaciones matemáticas. Estas herramientas facilitan la identificación de cambios en la amplitud, tendencias y variaciones presentes en una señal.

---

# III. Metodología

Para el desarrollo del prototipo de monitoreo de respuesta galvánica de la piel (GSR) se realizó una metodología experimental dividida en diferentes etapas, iniciando con la validación del sistema de adquisición mediante una señal simulada y posteriormente integrando la medición con electrodos reales. El procedimiento realizado se describe a continuación.

---

## 1. Diseño e implementación del sistema de adquisición

Inicialmente, se configuró el sistema electrónico utilizando un ESP32 como unidad de adquisición y procesamiento. La señal se conectó a una de las entradas analógicas de la placa, específicamente al pin D34, encargado de recibir las variaciones de voltaje provenientes del circuito de medición.

El ESP32 se seleccionó por las funciones de adquisición analógica, procesamiento digital y comunicación inalámbrica mediante WiFi que integra en una misma plataforma, lo que permitió disponer de un sistema compacto y portátil.

---

## 2. Simulación inicial mediante potenciómetro 

Antes de realizar las mediciones sobre la piel, se realizó una prueba inicial utilizando un potenciómetro como elemento de resistencia variable. Con este componente fue posible modificar manualmente la señal de entrada y observar la respuesta del sistema ante diferentes valores.

Durante esta etapa se verificó:
- La correcta lectura de la entrada analógica del ESP32.
- La variación de los valores obtenidos por el conversor analógico-digital.
- El funcionamiento del procesamiento de la señal.
- La comunicación entre el dispositivo y la interfaz de visualización.

  Esta prueba permitió comprobar el funcionamiento del montaje antes de conectar los electrodos y realizar la adquisición de la señal GSR.

  ---
## 3. Integración de electrodos para adquisición de señal GSR

Una vez comprobado el funcionamiento del sistema mediante la señal simulada, se reemplazó el potenciómetro por electrodos adhesivos de gel para realizar la medición directamente sobre la piel.

Los electrodos se colocaron en la región palmar de la mano, estableciendo el contacto necesario para captar las variaciones eléctricas asociadas con los cambios en la conductividad de la piel. Durante la medición se observó el comportamiento de los valores registrados y la estabilidad de la señal bajo las condiciones establecidas para la prueba.

---

## 4. Adquisición y procesamiento de la señal

La señal analógica obtenida mediante los electrodos fue ingresada al conversor analógico-digital (ADC) del ESP32 para transformarla en datos digitales. Las lecturas obtenidas presentaron pequeñas variaciones durante la medición, asociadas principalmente al ruido y al movimiento del usuario.

Para disminuir estas fluctuaciones, se calculó el promedio de varias lecturas consecutivas. Este procedimiento permitió suavizar la señal y reducir los cambios rápidos presentes en los datos adquiridos. La señal procesada se utilizó posteriormente para su visualización y análisis.

---

## 5. Desarrollo de comunicación inalámbrica

Una vez comprobada la adquisición de la señal, se configuró la conexión WiFi del ESP32 para transmitir los datos de manera inalámbrica. El microcontrolador se programó para crear una red propia identificada como GSR_ESP32, a la cual puede conectarse un dispositivo externo sin utilizar una conexión física.

También se configuró una ruta de comunicación para enviar únicamente el valor numérico correspondiente a la señal adquirida. Esta estructura permitió establecer el intercambio de datos entre el ESP32 y MATLAB para su posterior visualización.

---

## 6. Desarrollo de interfaz de visualización

Se desarrolló una interfaz web alojada en el ESP32 para mostrar los datos obtenidos durante la medición. En ella se presenta:

- Valor actual de la señal GSR.
- Clasificación de la respuesta obtenida según los rangos establecidos.
- Estado de funcionamiento del sistema.

La interfaz permite consultar los datos durante la adquisición y facilita el seguimiento de los cambios registrados en la señal durante las pruebas.

---

## 7. Visualización y análisis en MATLAB

Finalmente, se estableció la comunicación entre el ESP32 y MATLAB a través de la red WiFi generada por el microcontrolador. MATLAB realiza solicitudes periódicas al dispositivo para obtener los valores registrados durante la adquisición y almacenarlos para su posterior representación.

Los datos recibidos se organizan en función del tiempo y se muestran mediante una gráfica que se actualiza durante la medición. Esta visualización permite seguir los cambios de la señal GSR y comparar su comportamiento bajo las diferentes condiciones evaluadas.

---

# IV. RESULTADOS

<p align="center">
<img width="739" height="1600" alt="Visualización de la señal obtenida mediante simulación con potenciómetro" src="https://github.com/user-attachments/assets/e5ed2f06-c919-4276-94cc-ed2e48970cec" />
</p>

<p align="center">
  <sub>Figura 1. Visualización de la señal obtenida mediante simulación con potenciómetro. Tomado de: Elaboración propia.</sub>
</p>

La Figura 1. corresponde a la primera etapa experimental realizada con el potenciómetro como elemento de variación de la señal de entrada. En esta prueba se observó el comportamiento de la lectura obtenida por el ESP32 ante cambios en la resistencia del potenciómetro, evidenciando la variación de los valores registrados por el sistema de adquisición.

En la imagen se observa un valor de 3856 unidades ADC, correspondiente a una señal cercana al límite superior del rango de lectura del conversor analógico-digital del ESP32. Debido a que el microcontrolador cuenta con una resolución de 12 bits, la señal adquirida se representa en una escala entre 0 y 4095 niveles digitales, por lo que este valor indica una condición de entrada elevada dentro del rango disponible.

Esta medición permitió verificar que el sistema respondía correctamente ante variaciones de la señal de entrada y que la información adquirida podía ser visualizada mediante la interfaz desarrollada. La lectura obtenida corresponde únicamente a la representación digital de la señal medida por el ADC, por lo que no representa directamente un valor fisiológico de conductancia o resistencia de la piel.

# Código Arduino – ESP32

## Importación de librerías

Inicialmente se importan las librerías necesarias para la comunicación inalámbrica y la creación del servidor web.

```cpp
#include <WiFi.h>
#include <WebServer.h>
```

### Función de cada librería

| Librería  | Función                                                    |
| --------- | ---------------------------------------------------------- |
| WiFi      | Permite configurar y utilizar la conexión WiFi del ESP32   |
| WebServer | Permite crear un servidor HTTP para enviar y recibir datos |

---

## Configuración del sensor y WiFi

Se define el pin utilizado para adquirir la señal GSR y los parámetros de la red inalámbrica.

```cpp
#define GSR_PIN 34

const char* ssid = "GSR_ESP32";
const char* password = "12345678";

WebServer server(80);
```

### Parámetros utilizados

| Parámetro  | Valor     | Descripción                                      |
| ---------- | --------- | ------------------------------------------------ |
| `GSR_PIN`  | 34        | Entrada analógica donde se conecta el sensor GSR |
| `ssid`     | GSR_ESP32 | Nombre de la red WiFi creada por el ESP32        |
| `password` | 12345678  | Contraseña de la red                             |
| Puerto     | 80        | Puerto utilizado para la comunicación HTTP       |

El ESP32 funciona como un punto de acceso WiFi, permitiendo que el computador se conecte directamente al dispositivo.

---

## Adquisición de la señal GSR

Para obtener una medición más estable se realizan 20 lecturas consecutivas del sensor y posteriormente se calcula su promedio.

```cpp
int leerGSR(){

  long suma = 0;

  for(int i = 0; i < 20; i++){

    suma += analogRead(GSR_PIN);
    delay(5);

  }

  return suma / 20;
}
```

### Promedio de las mediciones

```cpp
return suma / 20;
```

El valor final corresponde al promedio de las 20 muestras adquiridas:

$$
GSR_{promedio} =
\frac{1}{20}
\sum_{i=1}^{20}GSR_i
$$

Este procedimiento permite reducir parcialmente las variaciones rápidas y obtener una señal más estable.

---

## Clasificación de la señal

Se establecen tres niveles de respuesta dependiendo del valor ADC obtenido.

```cpp
String obtenerNivel(int valor){

  if(valor < 1800){

    return "BAJO";

  }

  else if(valor < 2600){

    return "MODERADO";

  }

  else{

    return "ALTO";

  }
}
```

### Umbrales utilizados

| Valor ADC     | Nivel    |
| ------------- | -------- |
| `< 1800`      | BAJO     |
| `1800 – 2599` | MODERADO |
| `≥ 2600`      | ALTO     |

Estos valores corresponden a los umbrales definidos en el código. La guía propone establecer los umbrales a partir de los valores máximo y mínimo obtenidos durante la experimentación.

---

## Configuración del ESP32

La función `setup()` se ejecuta una vez al iniciar el dispositivo.

```cpp
void setup(){

  Serial.begin(115200);

  WiFi.softAP(ssid, password);

  Serial.println("Red WiFi creada");
  Serial.print("IP del ESP32: ");
  Serial.println(WiFi.softAPIP());
```

### Comunicación serial

```cpp
Serial.begin(115200);
```

Permite visualizar información del ESP32 mediante el monitor serial utilizando una velocidad de 115200 baudios.

### Creación de la red WiFi

```cpp
WiFi.softAP(ssid, password);
```

Configura el ESP32 como un **Access Point**, creando una red inalámbrica independiente.

La dirección IP del dispositivo se muestra mediante:

```cpp
WiFi.softAPIP()
```

En este proyecto se utiliza:

```text
192.168.4.1
```

---

## Comunicación con MATLAB

Se crea una ruta HTTP denominada `/valor` para que MATLAB pueda solicitar la medición actual de GSR.

```cpp
server.on("/valor", [](){

  int valor = leerGSR();

  server.send(200, "text/plain", String(valor));

});
```

Cuando MATLAB realiza una solicitud a:

```text
http://192.168.4.1/valor
```

el ESP32:

1. Lee la señal GSR.
2. Calcula el promedio de las 20 muestras.
3. Convierte el resultado a texto.
4. Envía el valor mediante HTTP.

Por ejemplo:

```text
2350
```

---

## Interfaz web

Además de enviar los datos a MATLAB, el ESP32 genera una interfaz web para visualizar el valor de GSR y su clasificación.

```cpp
server.on("/", [](){

  int valor = leerGSR();
  String nivel = obtenerNivel(valor);

  String color;

  if(nivel == "BAJO"){

    color = "green";

  }

  else if(nivel == "MODERADO"){

    color = "orange";

  }

  else{

    color = "red";

  }
```

Se asigna un color dependiendo del nivel:

| Nivel    | Color   |
| -------- | ------- |
| BAJO     | Verde   |
| MODERADO | Naranja |
| ALTO     | Rojo    |

La página se actualiza automáticamente cada segundo mediante:

```html
<meta http-equiv='refresh' content='1'>
```

Esto permite visualizar continuamente el valor de la señal y el nivel calculado.

---

## Inicio del servidor

Una vez configuradas las rutas, se inicia el servidor web.

```cpp
server.begin();
```

Posteriormente, dentro de `loop()`, se atienden las solicitudes recibidas.

```cpp
void loop(){

  server.handleClient();

}
```

La función `server.handleClient()` permite que el ESP32 responda continuamente a las solicitudes realizadas por MATLAB o por un navegador.

---

# Código MATLAB

## Limpieza del entorno

Inicialmente se limpia el espacio de trabajo y se cierran las figuras anteriores.

```matlab
clc;
clear;
close all;
```

### Función de cada instrucción

| Instrucción | Función                          |
| ----------- | -------------------------------- |
| `clc`       | Limpia la ventana de comandos    |
| `clear`     | Elimina las variables existentes |
| `close all` | Cierra las figuras abiertas      |

---

## Dirección del ESP32

Se define la dirección utilizada para solicitar los datos GSR.

```matlab
url = "http://192.168.4.1/valor";
```

Esta dirección corresponde al endpoint `/valor` creado en el servidor del ESP32.

La comunicación se realiza mediante HTTP.

```text
MATLAB → Solicitud HTTP → ESP32
MATLAB ← Valor GSR ← ESP32
```

---

## Configuración de la gráfica

Se crea una gráfica inicialmente vacía para posteriormente actualizarla durante la adquisición.

```matlab
figure;

h = plot(NaN,NaN,'LineWidth',2);

grid on;

xlabel("Tiempo (s)");
ylabel("Valor GSR (ADC)");
title("Señal GSR en tiempo real");
```

### Parámetros de la gráfica

| Elemento    | Descripción                  |
| ----------- | ---------------------------- |
| Eje X       | Tiempo en segundos           |
| Eje Y       | Valor GSR en ADC             |
| `grid on`   | Activa la cuadrícula         |
| `LineWidth` | Define el grosor de la señal |
| `title`     | Título de la gráfica         |

La gráfica comienza vacía utilizando:

```matlab
plot(NaN,NaN)
```

El identificador `h` permite actualizar posteriormente la misma gráfica.

---

## Variables de adquisición

Se crean los vectores utilizados para almacenar los datos.

```matlab
datosGSR = [];
tiempo = [];

inicio = tic;
```

### Variables utilizadas

| Variable   | Función                               |
| ---------- | ------------------------------------- |
| `datosGSR` | Almacena los valores de GSR           |
| `tiempo`   | Almacena el instante de cada medición |
| `inicio`   | Permite medir el tiempo transcurrido  |

---

## Adquisición en tiempo real

La adquisición se mantiene activa mediante un ciclo infinito.

```matlab
while true

    try

        respuesta = webread(url);
```

La función `webread()` realiza una solicitud al ESP32 y recibe el valor actual de GSR.

---

## Conversión del dato

El ESP32 envía el valor como texto, por lo que MATLAB debe convertirlo a un número.

```matlab
valor = str2double(respuesta);
```

Por ejemplo:

```text
"2350" → 2350
```

Esto permite utilizar posteriormente el valor para realizar cálculos y graficarlo.

---

## Registro del tiempo

Se determina cuánto tiempo ha transcurrido desde el inicio de la adquisición.

```matlab
t = toc(inicio);
```

El valor obtenido corresponde al tiempo actual de la medición en segundos.

---

## Almacenamiento de los datos

Cada nueva medición se agrega a los vectores correspondientes.

```matlab
tiempo = [tiempo t];
datosGSR = [datosGSR valor];
```

De esta manera se construye progresivamente la señal:

```text
tiempo  → [0.2, 0.4, 0.6, 0.8, ...]
GSR     → [2100, 2150, 2180, 2250, ...]
```

Cada valor de `datosGSR` queda asociado con su respectivo instante en `tiempo`.

---

## Actualización de la gráfica

La gráfica se actualiza utilizando los nuevos datos adquiridos.

```matlab
set(h, ...
    'XData', tiempo, ...
    'YData', datosGSR);
```

Esto permite visualizar la evolución de la señal sin crear una nueva gráfica en cada medición.

Posteriormente:

```matlab
drawnow;
```

actualiza inmediatamente la ventana gráfica.

---

## Ventana de visualización

Para evitar que la gráfica se extienda indefinidamente, se muestran únicamente los últimos 30 segundos.

```matlab
if t > 30
    xlim([t-30 t]);
end
```

Por ejemplo, cuando han transcurrido 50 segundos, la gráfica mostrará aproximadamente desde los 20 hasta los 50 segundos.

Los datos anteriores continúan almacenados en los vectores.

---

## Manejo de errores

La comunicación con el ESP32 se encuentra protegida mediante una estructura `try-catch`.

```matlab
catch error

    disp("No se pudo leer el ESP32");
    disp(error.message);

end
```

Si MATLAB pierde la comunicación con el ESP32, se muestra un mensaje indicando que no fue posible realizar la lectura.

Esto evita que el programa se cierre inmediatamente ante un error de comunicación.

---

## Intervalo de adquisición

Finalmente se utiliza:

```matlab
pause(0.2);
```

Esto introduce una pausa de 0.2 segundos entre solicitudes.

La frecuencia aproximada de consulta es:

$$
f = \frac{1}{0.2} = 5\ Hz
$$

Por lo tanto, MATLAB realiza aproximadamente cinco solicitudes por segundo.

Este valor corresponde a la frecuencia de consulta de MATLAB y no a la frecuencia de adquisición de las 20 muestras realizadas internamente por el ESP32.

---

## Flujo completo del sistema

```text
Sensor GSR
    ↓
Entrada analógica ESP32
    ↓
20 muestras
    ↓
Promedio
    ↓
Valor GSR
    ↓
Servidor HTTP
    ↓
WiFi
    ↓
MATLAB
    ↓
Conversión del dato
    ↓
Almacenamiento
    ↓
Gráfica en tiempo real
```

De esta manera, el ESP32 se encarga de la **adquisición y transmisión inalámbrica**, mientras que MATLAB realiza el **almacenamiento y visualización** de la señal GSR.

Esta implementación corresponde a la etapa de transmisión inalámbrica planteada en la guía de laboratorio.
