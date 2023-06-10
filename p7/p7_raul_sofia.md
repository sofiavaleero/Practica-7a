# PRACTICA 7: I2S

## CODI DE LA PRACTICA

```cpp
#include <Arduino.h>
#include "AudioGeneratorAAC.h"
#include "AudioOutputI2S.h"
#include "AudioFileSourcePROGMEM.h"

AudioFileSourcePROGMEM *in;
AudioGeneratorAAC *aac;
AudioOutputI2S *out;

void setup(){
  Serial.begin(115200);

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

```

# FUNCIONAMENT 

L'objectiu d'aquesta pràctica és comprendre l'ús i el funcionament del bus I2S (Inter-Integrated Circuit Sound), que s'utilitza per a la transmissió de senyals d'àudio digital. Aquest bus permet enviar senyals d'àudio en format PCM (modulació per codi d'impulsos) stereo.

El funcionament del bus I2S es basa en una connexió sincronitzada entre dispositius i consta de tres canals principals:

SCK (o BCLK): Aquest canal transmet el senyal de rellotge que sincronitza la comunicació entre els dispositius.

WS (Word Select o Frame Select): Aquest canal indica si es transmeten dades per al canal d'àudio esquerra o dret. S'utilitza per a la multiplexació de dades.

SD (Serial Data): Aquest canal transmet les dades d'àudio en si, tant per al canal esquerra com per al canal dret.

Primerament definim els punters:
1. A una font d'arxiu d'audio emmagatzemadaa la memoria de programa
2. A un generador d'audio AAC
3. A la sortidad d'audio I2S

En el setup inicialitzem les funcions d'entrada, de sortida i AAC que hem definit 

Finalment en el loop mira si el generador es troba en funcionament, si és així s'executa el bucle de reproducció del generador d'audio AAC, sinó es deté. 
