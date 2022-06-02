# Práctica 6a

El objetivo de la practica es comprender el funcionamiento del bus SPI.

El bus SPI és el mas standard en el mundo de la electrónica y automatización, con una arquitectura maestro-esclavo. La comunicación entre ellos se realiza por dos líneas independientes, una del maestro a los esclavos y otra de los esclavos a el maestro (comunicación full duplex). Es un bus síncrono.

El bus SPI requiere de:

- Una línea MOSI (master-out,slave-in) para la comunicacion del maestro al esclavo.
- Una línea MISO (master-out,slave-in) para la comunicacion del esclavo al maestro.
- Una línea SCK (clock) señal de reloj.
- Una línea SS (slave select) para seleccionar el esclavo con el que se va a realizar la comunicación.

Cuando el maestro quiere establecer comunicación con esclavo pone a LOW la línea SS correspondiente, lo que indica al esclavo que debe iniciar la comunicación.

Las ventajas son que tiene una alta velocidad de trasmisión, los dispositivos son sencillos y baratos y puede mandar secuencias de bit de cualquier tamaño.

Las desventajas son que requiere de 4 cables por cada dispositivo esclavo, solo es adecuado a cortas distáncias, no dispone de mecanismo de control y la longitud de los mensajes enviados y recibidos tiene que ser conocida por ambos dispositivos (eso ayuda a saber si lo has recibido correctamente).

## Código:
```
#include <Arduino.h>
#include <SPI.h>
#include <SD.h>
 
File myFile;
 
void setup()
{
  Serial.begin(9600);
  Serial.print("Iniciando SD ...");
  if (!SD.begin(4)) {
    Serial.println("No se pudo inicializar");
    return;
  }
  Serial.println("inicializacion exitosa");
 
  myFile = SD.open("pf32.txt");//abrimos  el archivo
  if (myFile) {
    Serial.println("pf32.txt:");
    while (myFile.available()) {
      Serial.write(myFile.read());
    }
    myFile.close(); //cerramos el archivo
  } else {
    Serial.println("Error al abrir el archivo");
  }
}
 
void loop()
{
 
}
```

## Describir la salida por el puerto serie:

Cargamos el programa en el procesador y hace que se cree un archivo dentro "pf32.txt" y hace un serial print dentro del file, eso quiere decir que lo que hay dentro del serial.println se escribe directamente en el archivo.

## Funcionamiento:

Primero hay que introducir las librerias del lector SD. Después creamos el archivo donde enviaremos el texto.

A continuacion tenemos un if que nos hará saber si se ha podido inicializar o no:

```
 if (!SD.begin(4)) {
    Serial.println("No se pudo inicializar");
    return;
  }
  Serial.println("inicializacion exitosa");
  ```

Ahora abrimos el documento que hace que podamos escribir dentro y nos saldría por pantalla. Si el archivo no se ha podido abrir nos dirá que "Error al abrir el documento":

```
myFile = SD.open("pf32.txt");//abrimos  el archivo
  if (myFile) {
    Serial.println("pf32.txt:");
    while (myFile.available()) {
      Serial.write(myFile.read());
    }
    myFile.close(); //cerramos el archivo
  } else {
    Serial.println("Error al abrir el archivo");
  }
}
```