# Biblioteca HelloDrum Arduino
[![arduino-library-badge](https://www.ardu-badge.com/badge/Hello%20Drum.svg?)](https://www.ardu-badge.com/Hello%20Drum)
Esta é uma biblioteca para fazer E-Drum com Arduino.
**Ver.0.7.7(11/1/2020) Trabalho em andamento.**

## Descrição

Esta é uma biblioteca para fazer E-Drum com Arduino.
Ao usá-lo com a biblioteca Arduino MIDI, você pode criar E-drum.

Página do projeto:[https://open-e-drums.com/](https://open-e-drums.com/)
Blog:[https://open-e-drums.tumblr.com/](https://open-e-drums.tumblr.com/)
YouTube:[https://www.youtube.com/channel/UCNCDcIO26xL_NhI04QY-v4A](https://www.youtube.com/channel/UCNCDcIO26xL_NhI04QY-v4A)
Modelos 3D de Pad: [https://www.thingiverse.com/RyoKosaka/designs](https://www.thingiverse.com/RyoKosaka/designs)

**Este software é uma versão alfa e não é compatível.
Use por sua conta e risco.**

## Recursos

- Pad piezo simples, pad piezo duplo, prato de 2 zonas, prato de 3 zonas
- Compatível com os pads de 2 zonas da Roland (série PD)
- Compatível com almofadas de 3 zonas da YAMAHA (Série XP)
- Compatível com os pratos de 3 zonas da YAMAHA (PCY135/PCY155) e os pratos de 2 zonas da Roland (CY12C/CY5/CY8)
- Compatível com controladores de chimbal do tipo SoftPot, FSR e Optical (TCRT5000) e chimbal da Roland (VH10/VH11)
- Detecção com MUX (4051 e 4067)
- Modo de configuração com LCD ou OLED
- Modo de configuração com escudo do teclado LCD (DFRobot, HiLetgo)
- Sensibilidade, limiar, tempo de varredura, tempo de máscara, número da nota, curva de velocidade podem ser definidos com cada pad
- Salvar valores de configuração com EEPROM
- Funciona com placas ESP32 e Teensy e AVR como UNO e MEGA.
- Funciona com [Biblioteca MIDI](https://github.com/FortySevenEffects/arduino_midi_library), [Biblioteca BLE-MIDI](https://github.com/lathoub/Arduino-BLE-MIDI), [biblioteca USB-MIDI ](https://github.com/lathoub/Arduino-USBMIDI).

## Como usar
- **Instalar**
Use o Library Manager do Arduino para instalar a biblioteca. Pesquise por "hellodrum".
Se você usa MIDI, instale também a [Biblioteca MIDI](https://github.com/FortySevenEffects/arduino_midi_library). Ou [Biblioteca BLE-MIDI](https://github.com/lathoub/Arduino-BLE-MIDI) ou [biblioteca USB-MIDI](https://github.com/lathoub/Arduino-USBMIDI).

<img src="https://open-e-drums.com/images/ide.png" width="800px">

- **Inicializar EEPROM**
Se quiser usar a EEPROM para armazenar as configurações, você precisará inicializar a EEPROM.
Por favor, escreva o código de exemplo, exemplo > EEPROM > InitializeEEPROM > InitializeEEPROM.ino, para o seu Arduino. Uma vez escrito, a inicialização está completa.

- **Codificação**
    ```cpp
     #include <hellodrum.h>
     #include <MIDI.h>
     MIDI_CREATE_DEFAULT_INSTANCE();

     //Por favor nomeie seu piezo.
     //O piezo chamado snare é conectado ao pino A0
     HelloDrum caixa(0);

     //Configuração
     byte SNARE[6] = {
       80, //sensibilidade
       10, //limiar
       20, //scantime
       20, //masktime
       38, //nota
       1 //tipo de curva
     };

     void setup()
     {
         MIDI.begin(10);
         snare.setCurve(SNARE[5]); //Define a curva de velocidade
     }

     laço vazio()
     {
         //De detecção
         snare.singlePiezo(SNARE[0], SNARE[1], SNARE[2], SNARE[3]); //(sensibilidade, limite, scantime, masktime)

         //Enviando sinais MIDI
         if (snare.hit == true) {
             MIDI.sendNoteOn(SNARE[4], caixa.velocity, 10); //(nota, velocidade, canal)
             MIDI.sendNoteOff(SNARE[4], 0, 10);
         }
     }
     ```

- **Codificação (MUX)**:
    ```cpp
     #include <hellodrum.h>
     #include <MIDI.h>
     MIDI_CREATE_DEFAULT_INSTANCE();

     //Define os pinos MUX
     HelloDrumMUX_4051 mux(2,3,4,0);//D2, D3, D4, A0
    
     //Por favor nomeie seu piezo.
     //O piezo chamado snare está conectado ao pino MUX 0
     HelloDrum caixa(0);

     //Configuração
     byte SNARE[6] = {
       80, //sensibilidade
       10, //limiar
       20, //scantime
       20, //masktime
       38, //nota
       1 //tipo de curva
     };

     void setup()
     {
         MIDI.begin(10);
         snare.setCurve(SNARE[5]); //Define a curva de velocidade
     }

     laço vazio()
     {
         //varrendo todos os pinos do mux
         mux.scan();

         //De detecção
         snare.singlePiezoMUX(SNARE[0], SNARE[1], SNARE[2], SNARE[3]); //(sensibilidade, limite, scantime, masktime)

         //Enviando sinais MIDI
         if (snare.hit == true) {
             MIDI.sendNoteOn(SNARE[4], caixa.velocity, 10); //(nota, velocidade, canal)
             MIDI.sendNoteOff(SNARE[4], 0, 10);
         }
     }
     ```

     [Verifique a instrução.md](https://github.com/RyoKosaka/HelloDrum-arduino-Library/blob/master/docs/instruction.md) para obter mais informações sobre codificação.

## Usando a Biblioteca MIDI do Arduino

[Biblioteca MIDI do Arduino FortySevenEffects](https://github.com/FortySevenEffects/arduino_midi_library)
Existem três maneiras de se comunicar com um PC usando MIDI com um arduino.

1. Reescrever o chip USB do arduino (somente UNO, MEGA)
     - https://www.arduino.cc/en/Hacking/DFUProgramming8U2
     - http://morecatlab.akiba.coocan.jp/lab/index.php/aruino/midi-firmware-for-arduino-uno-moco/?lang=en
2. Usando Hairless MIDI (maneira mais fácil)

## Usando a Biblioteca MIDI do Arduino

[Biblioteca MIDI do Arduino FortySevenEffects](https://github.com/FortySevenEffects/arduino_midi_library)
Existem três maneiras de se comunicar com um PC usando MIDI com um arduino.

1. Reescrever o chip USB do arduino (somente UNO, MEGA)
     - https://www.arduino.cc/en/Hacking/DFUProgramming8U2
     - http://morecatlab.akiba.coocan.jp/lab/index.php/aruino/midi-firmware-for-arduino-uno-moco/?lang=en
2. Usando Hairless MIDI (maneira mais fácil)
     - https://projectgus.github.io/hairless-midiserial/
     - https://open-e-drums.tumblr.com/post/171304647319/using-hairless-midi
3. Usando um terminal MIDI e um cabo MIDI-USB
     - https://www.arduino.cc/en/Tutorial/Midi
     - https://open-e-drums.tumblr.com/post/171168448524/using-midi-socket-with-arduino


```cpp
#include <hellodrum.h>
#include <MIDI.h>
MIDI_CREATE_DEFAULT_INSTANCE();

//...
```

## Usando a biblioteca USB-MIDI

[lathoub Arduino-USBMIDI](https://github.com/lathoub/Arduino-USBMIDI)
Se você estiver usando atmega32u4 (Arduino Leonardo, Arduino Micro, Arduino Pro Micro...), você pode usar a biblioteca USB-MIDI. Nenhum software adicional é necessário, o 32u4 é reconhecido como um dispositivo MIDI.

```cpp
#include <hellodrum.h>
#include <USB-MIDI.h>
USBMIDI_CREATE_DEFAULT_INSTANCE();

//...
```

## Usando a biblioteca BLE-MIDI com ESP32

[lathoub Arduino-BLE-MIDI](https://github.com/lathoub/Arduino-BLE-MIDI)
É muito fácil usar BLE-MIDI com ESP32.
Você pode encontrar um dispositivo chamado "BLE-MIDI".

```cpp
#include <hellodrum.h>
#include <BLEMIDI_Transport.h>
#include <hardware/BLEMIDI_ESP32.h>
BLEMIDI_CREATE_DEFAULT_INSTANCE();

//...
```
Verifique a documentação da KORG para obter instruções sobre como conectar.
[Guia de conexão MIDI Bluetooth KORG](https://cdn.korg.com/us/support/download/files/7c456d3daad3b027197b3fda1f87dce7.pdf?response-content-disposition=inline%3Bfilename%3DBluetooth_MIDI_SettingG_E1.pdf&response-content-type=application% 2Fpdf% 3B)

## Método de detecção e valores de configuração
**Certifique-se de verificar aqui primeiro se não estiver funcionando corretamente.**
[Verifique sensing.md](https://github.com/RyoKosaka/HelloDrum-arduino-Library/blob/master/docs/sensing.md) para métodos de detecção.
[Verifique setting.md](https://github.com/RyoKosaka/HelloDrum-arduino-Library/blob/master/docs/setting.md) para definir os valores.

## O circuito
[Verifique circuit.md](https://github.com/RyoKosaka/HelloDrum-arduino-Library/blob/master/docs/circuit.md) para saber como conectar o bloco e a placa.

## Almofadas

Os dados STL de pads de 6 polegadas a 12 polegadas, controladores de chimbal (<https://www.thingiverse.com/RyoKosaka/designs>)

## Histórico de Lançamentos

* 0.7.7
    - Correção de bug para ESP32
    - Adicionar e atualizar códigos de amostra sobre BLE-MIDI, USB-MIDI
* 0.7.6
    - Correção de bug para LCD e botões
    - Adicionar e atualizar códigos de amostra
* 0,7,5
    - Correção de bug para ESP32
    - Correção de bug para hihatControl()
    - Atualizar códigos de amostra
    - Adicionar modo pullup para lidar com pinos flutuantes (Beta)
    - Adicionar modo de depuração
* 0.7.4
    - Adicionar função de curva de velocidade
    - FSR() e TCRT5000() integrados em hihatControl()
    - Atualizar circuitos
    - Adicionar imagens de circuito
    - Atualizar o algoritmo de detecção
    - Adicionar figura de detecção
    - Atualize o código de amostra
    - Organizar o código-fonte
* 0.7.3
    - Atualizar tipo de variáveis
    - Adicionar função de botão para escudo do teclado LCD
    - Adicione o código de amostra "lcdShield.ino" para a proteção do teclado LCD
* 0.7.2
    - Atualize o código de amostra
    - Adicionar função de botão
    - Adicionar código de amostra para Teensy
* 0.7.1
    - Está disponível detecção com MUX de 16 canais (4067)
    - Atualize o código de amostra
    - Organizar pastas e arquivos
    - Adicionar biblioteca.propriedades
    - Teensy3.2 foi testado
* 0.7.0
    - Detecção melhorada
    - Sensor piezo duplo disponível (teste)
    - ESP32 EEPROM disponível
    - Modo de configuração com I2C LCD ou I2C OLED disponível
    - Adicionar código de exemplo
* 0.6.0
    - A detecção com MUX (4051) está disponível
    - Adicionar código de amostra BLE MIDI com ESP32
    - Hihat Contorller com FSR está disponível
* 0.5.0
    - Modo de configuração disponível
    - A função de exibição por LCD está disponível
    - A função de salvar os itens de configuração por EEPROM está disponível
    - Detecção aprimorada do controlador de chimbal TCRT 5000
* 0.1.0
    - Trabalho em progresso

## Pendência
- rimGain
- retriggerCancelar
- Codificador rotativo

## Autor

[@tnctrekit](https://twitter.com/tnctrekit)
[Funciona](https://ryokosaka.com)

## Licença

[MIT](http://opensource.org/licenses/mit-license.php)
