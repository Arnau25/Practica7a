# Practica 7a Reproducción desde memoria interna

### Código 
````
#include "Arduino.h" 
#include "FS.h"
#include "HTTPClient.h"
#include "SPIFFS.h"
#include "SD.h"
#include "AudioGeneratorAAC.h"
#include "AudioOutputI2S.h"
#include "AudioFileSourcePROGMEM.h"
#include "sampleaac.h"



AudioFileSourcePROGMEM *in;
AudioGeneratorAAC *aac;
AudioOutputI2S *out;

void setup(){
  Serial.begin(9600);

  in = new AudioFileSourcePROGMEM(sampleaac, sizeof(sampleaac));
  aac = new AudioGeneratorAAC();
  out = new AudioOutputI2S();
  out -> SetGain(0.125);
  out -> SetPinout(26,25,22);
  aac->begin(in, out);
}

void loop(){
  if (aac->isRunning()) {
    aac->loop();
  } else {
    aac -> stop();
    Serial.printf("Sound Generator\n");
    delay(1000);
  }
}
````
### Funcionamiento
Para esta practica hemos tenido que añadir unas librerias extra:
````
#include "Arduino.h" 
#include "FS.h"
#include "HTTPClient.h"
#include "SPIFFS.h"
#include "SD.h"
#include "AudioGeneratorAAC.h"
#include "AudioOutputI2S.h"
#include "AudioFileSourcePROGMEM.h"
#include "sampleaac.h"
````
Seguidamente declararemos tres punteros, el AudioFileSourcePROGMEM el cual lee una matriz PROGREM, el AudioGeneratorAAC el cual se encarga de reproducir un archivo AAC utilizando el decodificador AAC de punto fijo Helix y por último el AudioOutputI2 que es el que envia señales estéreo o mono a cualquier frecuencia establecida.
````
AudioFileSourcePROGMEM *in;
AudioGeneratorAAC *aac;
AudioOutputI2S *out;
````

Por lo que hace el void setup(), lo iniciamos con una comunicación en serie a una velocidad de 115200 bauds. Declararemos el puntero in como una variable que llevará nuestro fichero de audio.
Seguidamente empezamos la comunicación entre decodificador y la placa utilizaremos el puntero aac. Y finalmente para las diferentes configuraciones utilizaremos el puntero out.

````
void setup(){
  Serial.begin(9600);

  in = new AudioFileSourcePROGMEM(sampleaac, sizeof(sampleaac));
  aac = new AudioGeneratorAAC();
  out = new AudioOutputI2S();
  out -> SetGain(0.125);
  out -> SetPinout(26,25,22);
  aac->begin(in, out);
}
````

En cuanto al void loop(), mediante un condicional miraremos si la variable Running es true o no lo es para saber que no haya ningun error, si se cumple se llamará a la función loop(). En caso de que fuera false se cerraria inmediatamente con la funcion stop().
````
void loop(){
  if (aac->isRunning()) {
    aac->loop();
  } else {
    aac -> stop();
    Serial.printf("Sound Generator\n");
    delay(1000);
  }
}
````
