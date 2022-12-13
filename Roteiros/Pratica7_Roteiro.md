### Prática 7: Introdução a protocolos de comunicação em sistemas embarcados


#### Resumo

Introdução ao uso de protocolos de comunicação em sistemas com Linux embarcado, leitura de dados analógicos, uso de periféricos  e comunicação serial entre sistemas embarcados distintos (SBC - microcontrolador), a partir de programação em “C” na plataforma Arduino.

**Conceitos importantes:**

Leituras analógicas, I2C, programação em C, arduino, comunicação serial, controller/responder


#### Objetivo

O objetivo desta prática é o desenvolvimento de uma aplicação em sistema com Linux embarcado capaz de realizar <span style="text-decoration:underline;">leituras analógicas</span>, utilizando a plataforma Raspberry Pi em conjunto a um microcontrolador da plataforma <span style="text-decoration:underline;">Arduino</span>.

Para a realização deste projeto, o Arduino deve ser responsável por realizar a leitura analógica do sinal desejado, e se comunicar com a Raspberry PI a partir do <span style="text-decoration:underline;">protocolo de comunicação I2C</span>. Da mesma forma, a Raspberry deve estar preparada para receber os dados via I2C e mostrá-los ao usuário. Dessa forma, torna-se possível utilizar a Raspberry PI não somente como um sistema que adquire seus sinais, mas também como um sistema capaz de se comunicar com sistemas mais simples que possam realizar a <span style="text-decoration:underline;">aquisição dos dados</span>.

**Raspberry Pi e Arduino trabalhando em conjunto**

A Raspberry Pi já é bem conhecida como uma plataforma versátil com diversas funcionalidades de alto nível, tais como c<span style="text-decoration:underline;">onexão HDMI, Wi-Fi, Bluetooth, USB, Ethernet, possui arquitetura ARM Cortex A (com 64 bits, 1 GB de RAM, 1.4 GHz, e GPU)</span>, <span style="text-decoration:underline;">com conexões de I/O digitais para periféricos (permitindo display, LEDs, PWM, botões etc.) e diversas interfaces (módulo câmera, comunicação serial, acesso remoto)</span>.  Entretanto, sendo uma <span style="text-decoration:underline;">SBC que roda um sistema operacional com kernel Linux</span>, sabemos também que ela possui suas <span style="text-decoration:underline;">limitações</span> quando se trata de aplicações práticas de “<span style="text-decoration:underline;">tempo real</span>”, i.e., nos casos em que se exige do sistema embarcado uma <span style="text-decoration:underline;">temporização precisa e o tempo de resposta é crítico</span> (“_hard real-time_”). Ademais, a Raspberry Pi por si só também <span style="text-decoration:underline;">não é capaz de lidar com dados analógicos</span>. Por essa razão, em determinadas aplicações se faz necessário o uso dela em conjunto a um microcontrolador (que por sua vez possui <span style="text-decoration:underline;">hardware de tempo real e conversor A/D</span>). 

A plataforma [Arduino](https://www.arduino.cc) se apresenta como uma solução alternativa para atender o propósito descrito acima. Para integrar Raspberry Pi e Arduino em única aplicação embarcada, é necessário estabelecer uma <span style="text-decoration:underline;">comunicação de dados </span>entre ambos por meio de um <span style="text-decoration:underline;">protocolo</span> de comunicação (neste caso, a comunicação serial). Em redes de computadores, **comunicação serial** é o processo de enviar dados um [bit](https://pt.wikipedia.org/wiki/Bit) de cada vez, sequencialmente, em um [canal de comunicação](https://pt.wikipedia.org/wiki/Canal_de_comunica%C3%A7%C3%A3o) ou [barramento](https://pt.wikipedia.org/wiki/Barramento) (exemplos: RS-232, USB, Ethernet). Na **[comunicação paralela](https://pt.wikipedia.org/wiki/Comunica%C3%A7%C3%A3o_paralela)**, ao contrário, todos os bits são enviados de uma só vez. A comunicação serial é bastante comum entre dispositivos embarcados, utilizando os seguintes protocolos **I2C, SPI** e** UART** ([aqui um comparativo](https://www.robocore.net/tutoriais/comparacao-entre-protocolos-de-comunicacao-serial.html) e [terminologias](https://www.totalphase.com/blog/2021/12/i2c-vs-spi-vs-uart-introduction-and-comparison-similarities-differences/)). 

Para esta prática, vamos explorar os recursos do <span style="text-decoration:underline;">protocolo I2C</span> para estabelecer uma comunicação entre a <span style="text-decoration:underline;">Raspberry Pi 3B+ e o Arduino (será usado o modelo Uno com microcontrolador ATmega328P da família de 8 bits AVR)</span>. Dessa forma, a Raspberry Pi será a plataforma de alto nível que ocupa a função de placa “**<span style="text-decoration:underline;">controladora</span>**” que irá r<span style="text-decoration:underline;">equisitar informações</span> (no caso, dados analógicos) do Arduino que, por sua vez, na comunicação I2C, será o <span style="text-decoration:underline;">dispositivo** “controlado”**</span>. Ao contrário da Raspberry Pi, o Arduino irá ocupar a figura da plataforma de mais “baixo nível” e mais adequada no quesito tempo real (evidentemente, no caso do Arduino Uno, essa função será cumprida parcialmente, tendo vista suas limitações e sua maior aplicação em projetos didáticos, mas vamos assumir dessa forma neste projeto, sabendo que existem microcontroladores mais modernos e com arquiteturas mais adequadas para cumprir essa função). 

O I2C - _Inter-Integrated Circuit Bus _é um protocolo de dois cabos que habilita a comunicação serial entre dois ou mais dispositivos. Um circuito I2C consiste de um barramento “_controller_” (dispositivo controlador) e um ou mais barramentos “_responder_” (dispositivos controlados). Os dois cabos de comunicação funcionam da seguinte forma:


![alt_text](https://github.com/pedro-oliveiracjr/SEL0337/blob/main/Roteiros/Diagramas/P7_I2C.png?raw=true)


Fig. 1 Conceito da comunicação serial com protocolo I2C. Adaptado de: https://electrosome.com/i2c/


**O problema da comunicação I2C entre Raspberry Pi e Arduino**

Apesar de ambas plataformas suportarem I2C, a interface entre elas é um desafio em razão de não operarem no mesmo nível lógico de tensão: a Raspberry Pi opera com 3.3 V, ao passo que o Arduino Uno (assim como diversos outros modelos) opera com 5 V. Importa destacar na Fig. 1 acima, o uso de resistores pull-up, os quais adequam os níveis lógicos e de clock ao nível de tensão de referência (VCC) . Cumpre também frisar que na comunicação I2C o nível lógico de tensão é determinado pelo dispositivo controlador. Isto posto, o seguinte cuidado deve ser tomado: (i) u<span style="text-decoration:underline;">ma saída 3.3 V da Raspberry Pi pode ser conectada à uma entrada 5V do Arduino</span>; (ii) u<span style="text-decoration:underline;">ma saída 5 V do Arduino **NÃO** deve ser conectada à um entrada 3.3 V da Raspberry Pi. </span>A solução mais adequada para este problema é usar um conversor de nível lógico bidirecional 3.3V - 5V (_Bidirectional Logic Level Converter _ou_ “level-shifter”_). É um circuito que recebe e converte sinais de tensão de nível lógico 5V para 3.3V e vice-versa, fornecendo uma comunicação segura entre os dispositivos. Conforme Fig. 2, temos:



* **Lado “_low voltage”_ (Raspberry Pi)** - 4 canais de conversão de níveis lógicos (LV1, LV2, LV3 e LV4), um canal para alimentação, denominado “LV” e GND. **<span style="text-decoration:underline;">Os pinos SDA e SCL da Rasp. (GPIO 2 e GPIO3) devem se conectar à LV1 e LV2, ou LV3 e LV4</span>**, respectivamente. 
* **Lado “high voltage” (Arduino) - ** 4 canais de conversão de níveis lógicos (HV1, HV2, HV3 e HV4), um canal para alimentação, denominado “HV” e GND. **<span style="text-decoration:underline;">Os pinos SDA e SCL do Arduino (A4 e A5) devem se conectar à HV1 e HV2, ou HV3 e HV4</span>**, respectivamente. 


![alt_text](https://github.com/pedro-oliveiracjr/SEL0337/blob/main/Roteiros/Diagramas/P7_RASP_ARDUINO.png?raw=true)


Fig. 2 - Level-shifter entre Raspberry Pi e Arduino para comunicação I2C

**Aplicação**

Portanto, para a atividade prática, deve-se executar um programa escrito a partir da IDE do Arduino, que seja capaz de realizar uma ação ao receber um dado, e capaz de devolver informações ao receber uma requisição, ambos via I2C, para isto, o módulo [Wire](https://www.arduino.cc/reference/en/language/functions/communication/wire/) deve ser utilizado. Para a visualização do recebimento de pacotes no Arduino, deve-se realizar a mudança de estado em um LED, e enviar pelo monitor serial uma mensagem de funcionamento.

Para a obtenção dos dados, deve-se conectar a um dos pinos analógicos do Arduino a um potenciômetro, como segue na na Fig. 3. Dessa forma, o pino central conectado a porta analógica poderá realizar leituras analógicas, excursionando o valor recebido de 0 a 5V entre 0 e 1023, dado que o conversor A/D do sistema possui resolução de 10 bits.  

Uma vez configurado o I2C e a leitura analógica para o Arduino, deve-se realizar a configuração para a Raspberry, para isto deve-se utilizar o módulo SMBus, para gerar um script em Python capaz de receber e enviar informações pelo protocolo I2C a partir dos pinos corretos, obtidos pelo pinout da Raspberry.

Note que a ligação (Fig. 2, Fig. 3 e Tabela 1) do level-shifter deve respeitar os pinos impressos em sua PCB, conectando o pino de 5V ao HV, e o ground no GND pelo lado do Arduino, e os pinos de 3.3V em LV e ground em GND pelo lado da Raspberry. Os pinos de SDA e SCL do I2C devem estar nos outros pinos LVx e HVx, além disso atente-se em não inverter SDA e SCL entre as duas placas. A Fig. 4 apresenta a montagem prática.

Atente-se às ligações utilizadas, e lembre-se que não necessariamente todos os fios estarão na mesma ordem, devido ao fato de que há mais opções de pinos a serem utilizados para a mesma tarefa. No entanto, algumas funções não podem ser implementadas com qualquer conjunto de pinos, como o I2C. 

Tabela 1 - Mapeamento da conexão física entre Raspberry Pi e Arduino 

*A conexão pode ser feita em qualquer pino GND da rasp., não necessariamente no n° 9.


![alt_text](https://github.com/pedro-oliveiracjr/SEL0337/blob/main/Roteiros/Diagramas/montagemSketch_bb.jpg?raw=true)


Fig. 3 - Esquema de ligação completo do projeto ([download da imagem em HD aqu](https://drive.google.com/file/d/15g2liKFBkuUPWhWXkMyq9GnwQNsBDrun/view?usp=sharing)i)



![alt_text](https://github.com/pedro-oliveiracjr/SEL0337/blob/main/Roteiros/Diagramas/c2bcc299-b85f-4a3c-a1a1-fd5353876118.jpg?raw=true)


Fig. 4- Montagem final

**Motivação**

Protocolos de comunicação são recursos capazes de tornar sistemas embarcados uma ferramenta de análise e envio de dados via rede, além de poder controlar diversos tipos de sensores ou outros sistemas embarcados conjuntamente. Conforme discorrido, além do I2C, há outros protocolos, como o Ethernet, SPI, CAN, UART, etc. Todos estes sistemas e protocolos são utilizados não somente em pequenas aplicações, mas também em produtos como, por exemplo, as [redes CAN](https://www.embarcados.com.br/wp-content/uploads/filebase/eventos/tdc_2016_trilha_embarcados/TDC2016-Rede-CAN-Conceitos-e-Aplicacoes.pdf) existentes em todos os sensores e núcleos de processamento em veículos modernos, ou o protocolo [SPI](https://embarcados.com.br/spi-parte-1/) para controle de arquivos em cartões de memória e acesso a memórias externas ao microcontrolador. Destacam-se ainda, as aplicações básicas de UART e serial vistas em práticas anteriores, e o acesso a ethernet, permitindo a conexão com inúmeros sistemas.

Importa também ressaltar o uso de leituras analógicas nesta prática, a qual possui grande importância para sistemas embarcados, tendo em vista que, em geral, as variáveis de interesse dos problemas reais não possuem discretização, sendo assim necessário o uso de conversores A/D para a conversão de sistemas analógicos para digitais discretizados. Uma forma de conexão pode ser o uso de módulos de conversores A/D ou, por exemplo, como visto nesta prática, uma rede de microcontroladores com módulos internos de conversão A/D implementados capazes de se comunicar com o sistema operacional principal, onde podem ser analisados os dados obtidos, por meio de um interfaceamento multiplataforma que cumpre diferentes funções (alto nível e baixo nível).


#### Roteiro



* Instale a IDE do arduino na Raspberry PI, atualmente em sua versão 2.0, ela possui suporte a sistemas operacionais Linux, utilizando de sua versão AppImage.
* Caso possua algum problema para executar o AppImage, ou falte alguma permissão, o execute via terminal com o comando: “ sudo ./arduino-ide_2.0.2_Linux_64bit.AppImage”
* A placa Arduino deve ser conectada à Rasp via USB conforme Fig. 5. Se houver problema de acesso ao Port durante a compilação, deve-se garantir acesso utilizando o seguinte comando: sudo chmod a+rw /dev/ttyACM0
* Crie e execute um programa em C capaz de realizar a medição analógica do valor do potenciômetro
* Faça as adaptações necessárias para que este programa seja capaz de enviar essa informação pelo canal I2C, a partir da biblioteca Wire.h
* Note que o I2C possui resolução de 10 bits, e o arduino de 8 bits, portanto, atente-se ao tipo da variável ao salvar o valor lido, a fim de evitar que se obtenha perda de informação, para verificar, observe se a variável está excursionando entre 0 e 1023(10 bits), e não entre 0 e 255 (8 bits)
* Para o envio dos 10 bits, deve-se dividir o inteiro em dois bytes, o arduino possui uma função denominada “highByte” e “lowByte”, utilize-a na chamada da função de envio de bytes pelo I2C.
* Desconecte a placa da energia e realize as conexões necessárias entre os componentes na protoboard e ambas placas para habilitar a conexão física I2C e a leitura analógica de potenciômetro conectado ao Arduino.
* Verifique se as ligações estão corretas antes de energizar o circuito, lembre-se que a operação está sendo feita entre dois sistemas com níveis de tensão distintos, o que pode ocasionar defeitos na Raspberry PI caso a tensão de 5V do Arduino acesse os pinos de 3.3V da Raspberry PI
* Para verificar a conexão I2C, pode-se executar o seguinte comando no terminal da Raspberry: sudo i2cdetect -y 1
* Observe na tabela gerada se há um dispositivo conectado no endereço definido pelo seu programa do Arduino para o I2C
* Faça um script em Python capaz de receber os dados do I2C: o módulo responsável por esta comunicação é o SMBus
* Selecione o mesmo endereço configurado no Arduino
* A partir de uma entrada de teclado, envie um comando para o arduino, que envie 0 ou 1, e um terceiro valor caso o valor inserido seja diferente dos dois primeiros
* Para o Arduino, faça com que um LED (o Arduino já possui um LED em sua placa, pelo nome LED_BUILTIN) acenda ou apague a partir do valor 0 ou 1 recebido.
* Junte com o comando que envia por I2C a cada requisição o valor analógico do pino conectado ao potenciômetro.
* Dado que os valores serão recebidos em dois bytes separados, realize bit shift para obter o valor original na Rasp.
* Mostre no terminal serial do Arduino o valor enviado, e no terminal do Python o valor recebido e compare os valores, eles devem ser exatamente iguais, caso possua alguma diferença reveja as estruturas utilizadas, a fim de determinar a fonte de erro.
* Enviar na tarefa os scripts em Python (Rasp) e em “C” (Arduino). Para documentação dos conceitos da prática no relatório, explique os comandos utilizados para ambos os programas, e os comandos utilizados para as etapas descritas no roteiro, mostrando prints das imagens dos dois terminais com o mesmo valor enviado e recebido. 
* Bom projeto! e em caso de dúvidas contate o professor ou monitor da disciplina.


Fig. 5 - Utilização da IDE  para escrever e fazer uploads de programas para o Arduino a partir da Raspberry Pi

**Referências**



* [I2C Between Arduino & Raspberry Pi](https://dronebotworkshop.com/i2c-arduino-raspberry-pi/)
* [Comunicação I2C entre Raspberry Pi e Arduino](https://www.aranacorp.com/pt/i2c-comunicacao-entre-raspberry-pi-e-arduino/)

**Tutoriais **


```

# Conceitos relativos às funções em Python para controlar outro dispositivo e realizar leituras usando I2C

# Mais detalhes: documentação SMbus https://buildmedia.readthedocs.org/media/pdf/smbus2/latest/smbus2.pdf

Para acessar o barramento I2C na Rasp usando o módulo SMBus:

import smbus # ou from smbus import SMBus

# Criar objeto de classe SMBus para acessar I2C
     #<Object name> = smbus.SMBus(I2C port no.) 
     # ou <Object name> = SMBus(I2C port no.) 
     
     # I2C port no : I2C port no. i.e. 0 or 1
     #Exemplo:
Bus = smbus.SMBus(1) #ou - Bus = SMBus(1)
#=======================================================================
# Agora é possível acessar a classe SMBus com objeto "bus"
# <bus.write_byte_data(Device Address, Register Address, Value)>
   # Função usada para escrever dados no registrador solicitado:
   #Device Address : 7-bit or 10-bit device address
   #Register Address : Registrador de endereço necessário para escrita 
   #Value : valor necessário para escrita no registrador
   #Exemplo:
 
Bus.write_byte_data(0x68, 0x01, 0x07)
#=======================================================================
#bus.write_i2c_block_data(Device Address, Register Address, [value1, value2,....])
  # Função usada para escrita de bloco de 32 bytes.
  #Device Address : 7-bit or 10-bit device address
  #Register Address : Registrador de endereço necessário para escrita 
  #Value1 Value2... : escrita de blocos de bytes no endereço solicitado
  #Exemplo:

Bus.write_i2c_block_data(0x68, 0x00, [0, 1, 2, 3, 4, 5]) # escrita de 6 bytes no endereço "0"

#======================================================================= 
# bus.read_byte_data(Device Address, Register Address)
  #Função para leitura de bytes do registrador
  #Device Address : 7-bit or 10-bit device address
   #Register Address : endereço do registrado requisitado para leitura    de dados
  #Exemplo:
 
Bus.read_byte_data (0x68, 0x01)

#======================================================================= 
#Bus.read_i2c_block_data(Device Address, Register Address, block of bytes)
  #função para leitura de um bloco de 32 bytes
  # Device Address - 7-bit or 10-bit device address
  # Register Address - " "
  #Block of Bytes  	- N° de bytes no endereço requisitado
  #Exemple:

Bus.read_i2c_block_data(0x68, 0x00, 8) #  o valor retornado é uma lista de 6 bytes
```



```
#  Tutorial: Raspberry Pi controlando o Arduino 
# sudo raspi-config - interface options - habilitar I2C
# sudo i2cdetect -y 1 - verifica o barramento I2C - ao realizar a conexão I2C física com Arduino o endereço ocupado deve aparecer 


from smbus import SMBus
 
addr = 0x8 # bus address
bus = SMBus(1) #  /dev/ic2-1
 
flag = True
 
print ("Digite 1 para ON ou 0 para OFF")
while flag:
 
	ledstate = input(">>>>   ")
 
	if ledstate == "1":
		bus.write_byte(addr, 0x1) 
	elif ledstate == "0":
		bus.write_byte(addr, 0x0)
	else:
		flag = False

#  Os valores de entrada acima, enviam 1 ou 0 para o barramento I2C - ou 0 em caso de valor de entrada diferente de 0 ou 1.

# Até aqui, o código acima, se testado, deve ser capaz de controlar o LED do Arduino, apagando (quando enviado 0) ou ligando (quando enviado 1).  Para que o Arduino recebe o controle, deve ser feito o upload de um programa em C, via IDE. O tutorial encontra-se abaixo.


# A partir daqui, conforme solicitado no roteiro da prática, a linha abaixo deve ser  configurada para a leitura de dois bytes - referente aos valores analógicos que serão lidos do potenciômetro conectado ao Arduino - na resolução de 10 bits 

<Bus.read_i2c_block_data(endereço do arduino, registrador, conjunto de bytes)>

# por fim, realizar a conversão da saída acima, de 0 e 1023 (10 bits) recebidos do Arduino, para imprimir no terminal da Rasp o valor entre 0 e 255 (8 bits) 

```



```
// Tutorial - Arduino como dispositivo controlado na comunicação I2C tendo a Raspberry Pi como controladora

// código em C a ser compilado na IDE do Arduino e feito upload na placa

// biblioteca Wire  para  I2C
#include <Wire.h>
 
// Controlar o LED da própria placa Arduino
const int ledPin = LED_BUILTIN;
 
void setup() {
  // adicionando endereço no barramento I2C com dispositivo controlado
  Wire.begin(0x8);
  
  //Reporar "receiveEvent" quando receber dados          
  Wire.onReceive(receiveEvent);
  
  // Define o pino do LED como saída e o desliga 
  pinMode(ledPin, OUTPUT);
  digitalWrite(ledPin, LOW);
}
 
// Função executada sempre que dados são recebidos do controlador (Raspberry Pi)
void receiveEvent(int howMany) {
  while (Wire.available()) { // loop
    char c = Wire.read(); // recebe o byte como char
    digitalWrite(ledPin, c);
  }
}
void loop() {
  delay(100);
}
//  até aqui, o código acima, se testado, deve atender às solicitações da Rasp, ligando ou desligando o LED do Arduino.

// A partir daqui, o script deve ser configurado visando a leitura analógica do potênciometro, que será solicitada na Rasp

// void setup () {
// o parâmetro <Serial.begin(9600)> define a taxa de comunicação serial - 9600 bits por segundo, no caso
//a função < Wire.onRequest(requestEvents)> é chamada quando o Arduino receber solicitações da Rasp
//  }

// void requestEvent () {
 // a função: <analogRead(analog_pin)> realiza leitura do pino analogico que se deseja receber dados 
// <Wire.write(highByte(n)); > e  < Wire.write(lowByte(n));>  são funções usadas na chamada da função de envio de bytes pelo I2C por highByte e lowByte

// }

// void receiveEvent(int number) {
// sem alterações

//}

// Lembrar sempre de, ao longo dos blocos, imprimir na tela os valores com < Serial.println>

// ref. 
// https://www.aranacorp.com/pt/i2c-comunicacao-entre-raspberry-pi-e-arduino/
//https://docs.particle.io/reference/device-os/api/wire-i2c/onreceive/

```


$ ls output/images/ imx6dl-colibri-ipe.dtb 

rootfs.tar u-boot.bin rootfs.ext2 sdcard.img u-boot.img rootfs.ext4 SPL zImage
