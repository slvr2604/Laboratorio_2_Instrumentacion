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

# IV. RESULTADOS
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
