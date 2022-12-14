# SEL0337 - Projetos em Sistemas Embarcados

#### Obejtivos

<div style="text-align: justify"> 

Apresentar as plataformas de hardware compactos utilizadas em sistemas embarcados de alto nível. Apresentar e desenvolver os principais conceitos de kernel utilizados em sistemas embarcados de alto desempenho, desenvolver a habilidade em se trabalhar com os recursos disponíveis, promover a auto-suficiência no que tange à instalação e preparação de sistemas embarcados com Sistemas Operacionais com kernel Linux e desenvolvimento de pequenas aplicações. 

</div>

**Assuntos tratados na disciplina**

- Linux Embarcado
- Shell
- Single Board Computers (SBC)
- Controle de versão e repositórios de código Git/GitHub
- Python para sistemas embarcados
- Desenvolvimento de aplicações em Linux para sistemas embarcados de alto nível: GPIO, camêra/visão computacional, APIs, comunicação I2C/SPI/UART/USB, tratamento de interrupções, processos e threads.
- Customização de inicialização de sistemas operacionais com Kernel Linux em SBC: bootloaders, init system, sysemd/systemctl
- Build systems e criação de distribuições Linux customizadas: Yocto, Buildroot; Toolchain e cross-compiling, GNU C library e Rootfs

## Aula 01 (teórica)
- Apresentação da disciplina
- Sistemas embarcados 
  - Destaque, tedências e cenário brasileiro
  - Considerações sobre o hardware embarcado: SBCs, computadores em módulo, SoC, CPU ARM Cortex A, GPU e memórias.
  - Projetos em sistemas embarcados: restrições, produtos comerciais, uso de sistemas operacionais para aplicações de alto nível
  - Noções de sistemas de tempo real: soft real-time vs. hard real-time
  

## Aula 02 (teórica)
- Sistemas operacionais e Kernel Linux
  - Visão geral, tipos de kernel, padrão POSIX
  - Kernel Linux: estrutura geral, módulos, device tree e device drivers;
  - Sistemas de arquivos, formatos Fat32, ext3, ext4, ntfs, vfat, manipulação em HD com gparted, fdisk etc.
  - Processo de inicialização de S.O. (Boot), Bios, MBR, Bootloader, Kernel, init system e targets
  
 - Introdução ao ambiente Linux
   - História, evolução, ciclo de release do Kernel;
   - GNU e lincença GPL
   - Comunidade, distribuições Linux, Desktop environments
   - Principais comandos para manipulação de arquivos em terminal shell (bash)
   
## Aula 03 (teórica/prática)
- Linux embarcado
  - Arquitetura de um sistema embarcado com Linux: target(hardware), bootloader, kernel, e rootfs
  - Ambiente (host) e ferramentas de desenvolvimento, toolchain, compilação cruzada, biblioteca GNU C
  - Customização: distro pronta vs. criação do zero (processo manual) vs. automatização com build systems
  - Yocto project e Buildroot para criação de imagens customizadas para sistemas embarcados.
  
 - Explorando o hardware embarcado com Linux
   - SBCs: Raspberry Pi e alternativas (BeagleBone, Toradex, ASUS Tinkerboard, O-droid, e Labrador)
   - Configurando aplicações: gravando a imagem no cartão SD, primeiro Boot, acesso remoto via SSH e VNC, acesso via console serial (UART), periféricos e interfaces.
   - Protocolos de comunicação em SBC: I2C/SPI/UART, USB, Ethernet;
   
 - Ferramentas de codificação, repositórios e controle de versão
    - Editores de texto e configuração de IDEs
    - Shell script
    - Linguagem Python
    - Controle de versão com Git
    - Repositório de códigos com GitHub
    
## Aula 04 (prática)
 - **Prática #1** - sistemas de arquivos em terminais linux (bash), criação de arquivos, criação de diretórios, navegação entre diretórios, edição de textos/IDEs e códigos a partir do terminal utlizando Linux Debian. 
   - Comandos importantes: *cd, ls, pwd, cat, find, grep, history, echo, touch, nano, man, help*.
   - Comandos de manipulação de arquivos: *mv, rm, cp, mkdir, rmdir*.
   - [Instalação de Sistema Operacional (SO) Baseado em Linux: Debian GNU/Linux 11](https://github.com/jordeam/sel0456/blob/main/aula-02/README.md) 
- **Objetivos**: familiarização com comandos utilizados em terminais linux, a fim de realizar operações básicas em sistemas de arquivos, utilizando a linguagem de programação bash com terminal (shell)

## Aula 05 (prática)
- **Prática #2** - Introdução ao uso de instaladores de distribuições linux, como o calamares ou debian installer (raspbian), configurações iniciais após instalação do sistema operacional, configuração do usuário root, conexão a internet, ativação do SSH, ativação dos drivers da câmera.
   - Conceitos importantes: Calamares project, Ubiquity, Debian-installer, SSH, Camera legacy rasp, sudo e segurança de root.
- **Objetivos**: familiarização com ambientes de instalação de distribuições linux mais utilizados e configurações iniciais a serem feitas no Raspberry Pi OS a fim de garantir que as funções básicas estejam disponíveis para uso em atividades seguintes, assim como configurações de segurança em sistemas embarcados.


## Aula 06 (prática)
- **Prática #3** - Introdução a programação em Python, com uso de condicionais, laços de repetição, funções, conversão de tipo, importação de bibliotecas, manipulação de erros, criação de ambiente virtual e uso de GPIO no Python da Raspberry
   - Conceitos importantes: *Type casting, Timer, time, Import, Try Except, if, for, while, GPIO, Virtual environment*.
- **Objetivos**: familiarização dos conceitos básicos que tange a linguagem de programação Python, abrangendo os conceitos de importação de bibliotecas, uso de
estruturas lógicas, métodos de conversão de tipo e métodos de manipulação de erros, além disso, o uso de GPIO da rasp a fim de demonstrar uma simples aplicação de acesso aos componentes de hardware da placa a partir do Python.

## Aula 07 (prática)
- **Prática #4** - Introdução ao uso de periféricos embarcados na raspberry PI, como a modulação em largura de pulso (PWM) para controle de saídas.
   - Conceitos importantes: Modulação, Controle, Níveis analógicos, duty cycle.
- **Objetivos**: familiarização do uso de conexões externas a rasp, a partir de seus pinos de entrada e saída de uso geral, utilizando de saídas moduladas em largura de pulso.

## Aula 08 (prática)
- **Prática #5** - Introdução ao uso de periféricos embarcados na raspberry com interrupções, timers e threads a partir da programação em Python, com bibliotecas de manipulação de pinos “RPI.GPIO”, “time”, e “threading”.
   - Conceitos importantes: *GPIOs, threading,timer, polling, interrupt, contadores, parallel computing, CPU multicore*.
- **Objetivos**: familiarização do uso de conexões externas a rasp, a partir de seus pinos de entrada e saída de uso geral, além disso, do uso de temporização, a partir de delays e do uso de threads.

## Aula 09 (prática)
- **Prática #6** - Introdução ao uso de periféricos embarcados na raspberry com o uso do módulo de câmera, além da utilização do versionamento de código via Git/GitHub, e o uso de APIs para obtenção de dados da internet para o desenvolvimento de uma aplicação climática.
   - Conceitos importantes: OpenCV, PiCamera, visão computacional, Git/GitHub, APIs, IoT, I2C.
- **Objetivos**: projeto de uma base climática utilizando de um sistema de linux embarcado. Para isto, será utilizado o módulo de câmera embarcado próprio da RaspBerry Pi, além da obtenção de dados da internet. Como forma de documentação do projeto, o Git será o sistema de versionamento e o GitHub como repositório, junto ao histórico de versionamento.

## Aula 10 (prática)
** Continuação da Prática #6** 
   
## Aula 11 (prática)
- **Prática #7** - Introdução ao uso de protocolos de comunicação em sistemas com Linux embarcado, leitura de dados analógicos, uso de periféricos e comunicação serial entre sistemas embarcados distintos (SBC - microcontrolador), a partir de programação em “C” na plataforma Arduino.
   - Conceitos importantes: Leituras analógicas, I2C, programação em C, arduino, comunicação serial, controller/responder
- **Objetivos**: desenvolvimento de uma aplicação em sistema com Linux embarcado capaz de realizar leituras analógicas, utilizando a plataforma Raspberry Pi, como plataforma controladora e de alto nível, em conjunto a um microcontrolador da plataforma Arduino (dispositivo controlado no barramento I2C, que irá cumprir a função de baixo nível, responder solicitações da Rasp, fazer a leitura e enviar dados analógicos, que a Rasp. não possui conversor A/D.)

## Aula 12 (prática)
** Continuação da Prática #7**  

## Aula 13 (prática)
- **Prática #8** - Inicialização automática de aplicações em sistemas embarcados a partir de serviços no Init System, utilizando recursos do systemd
   - Conceitos importantes: Init System, systemd, SysVinit, systemctl, unit file, Boot process, inciaização de serviços durante o Boot.
- **Objetivos**: Configurar aplicações embarcadas que, por vezes, necessitam operar já na inicialização do sistema operacional ao invés de aguardar alguém logar no sistema e iniciá-la manualmente.

## Aula 14 (prática)
- **Prática #9** - Introdução a customização e criação de sistemas com Linux embarcado utilizando a plataforma Raspberry Pi: instalação do Bootloader "Das U-Boot" para carregamento do Kernel Linux do Raspbian; explorar ferramentas build para criação de distribuições customizadas com base em receitas pré-configuradas.
   - Conceitos importantes: *Boot process, bootloader, rootfs, partição, gparted, Kernel, GPU, device drivers, device tree, build system, toolchain, u-boot, cross-compiling, Buildroot, Yocto, Open-embedded, metadados, BitBake, Poky*.
- **Objetivos**: explorar as ferramentas build e recursos atualmente disponíveis para criação e customização de distribuições Linux para sistemas embarcados específicos que exigem alto grau de flexibilidade do sistema operacional. Customizar aplicações que exigem alto grau de flexibilidade não oferecido por distribuiçoes Linux "prontas", tais como tempo de Boot menor, controle de pacotes para licença comercial, requisitos de segurança, conjunto específico de aplicações, Boot via rede e sem uso de cartão micro SD. 


## Aula 15 (prática)
- **Continuação da Prática #9** 
- Encerramento do curso
- Projeto final (se houver)
  
    
## Entregas e formas de avaliação
- Implementações: programas em Python (principal linguagem adotada), shell script (bash) ou "C" (em alguns casos) desenvolvidos para cada atividade prática; montagem prática do circuito com componentes (simulador e protoboard quando houver) e interfaceamento do sistema embarcado (módulo da camera, cabo Ethernet, acesso via SSH/VNC, comunicação I2C etc) usando SBC.

- Entregas: 
  - Scripts dos programas desenvolvidos
  - Histórico de comandos bash usados no terminal (.txt, quando houver), 
  - Relatório em PDF documentando a prática com introdução, desenvolvimento e conclusão, imagens, diagramas/circuitos, programa comentado (lógica usada, comandos do terminal, bibliotecas etc.), ou link para o repositório do GitHub contendo arquivo "Readme.md" com essa explicação (quando for o caso).

- Critérios adotados na avaliação de relatórios das atividades práticas:
  1. Formatação e sequência lógica (introdução, desenvolvimento e conclusão) - 2 pontos;
  2. Envio dos arquivos/scripts – 1 ponto;
  3. Apresentação e discussão dos resultados – 3 pontos;
  4. Explanação sobre os comandos executados no Terminal Linux – 2 pontos;
  5. Item relativo à natureza de cada atividade prática* – 2 pontos;

## Dinâmica das aulas:
- **Pré-aula**: roteiro da atividade prática a ser implementada durante a aula. O roteiro será disponibilizado com antecedência para leitura prévia, possiblitando certo preparo e plenajamento do experimento (em alguns casos, haverá uma atividade "pré-lab.", como incentivo para leitura do roteiro).
- **Pós-aula**: relatório documentando a atividade prática, correlação do tema da atividade com situações práticas do coditiado, indústria, pesquisa científica, sociedade; leitura complementar, documentação no GitHub, ou uma tarefa/desafio relativo ao tema da prática.

