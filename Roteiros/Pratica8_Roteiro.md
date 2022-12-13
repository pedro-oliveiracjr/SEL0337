
### **Prática 8: Inicialização automática de aplicações em sistemas embarcados a partir de serviços no Init System utilizando recursos do systemd**


#### **Resumo**

Inicialização automática de aplicações em sistemas embarcados a partir de serviços no Init System utilizando recursos do systemd; 

**Conceitos importantes:**

Init System, systemd, SysVinit, systemctl, unit file, Boot proces


#### **Objetivo**

Sistemas embarcados de produtos comerciais são específicos e possuem funcionalidades restritas. Aplicações embarcadas, por vezes, necessitam operar já na inicialização do sistema operacional ao invés de aguardar alguém logar no sistema e iniciá-la manualmente. Da mesma forma, exigem tempo de Boot menor, controle de pacotes para licença comercial, requisitos de segurança, conjunto específico de aplicações, Boot via rede e sem uso de cartão micro SD. Neste âmbito, torna-se vantajoso uma distribuição Linux customizada para a aplicação embarcada. Por outro lado, as distribuições Linux “prontas”, não possuem tal grau de flexibilidade.  Portanto, essa prática visa explorar as ferramentas build e recursos atualmente disponíveis para criação e customização de distribuições Linux para sistemas embarcados específicos que exigem alto grau de flexibilidade do sistema operacional.

**I -_Init System _e_ systemd_**

O _Init System_ é o estágio responsável por todo o processo de inicialização de um sistema operacional (O.S) com o Kernel Linux. Sendo assim, após o computador/SBC ser ligado e dar início ao processo “_boot_”, o _<span style="text-decoration:underline;">bootloader</span>_ entra em operação cumprindo a função de carregamento do kernel Linux. O _<span style="text-decoration:underline;">Init Sytem</span>_, portanto, é o processo de inicialização do sistema operacional (Debian, Ubuntu, Raspberry Pi OS etc.), que ocorre logo após o Kernel Linux ter sido carregado (Fig. 1).


![alt_text][(https://github.com/pedro-oliveiracjr/SEL0337/blob/main/Roteiros/Diagramas/P8_Boot.png?raw=true)


Fig. 1 - Processo “boot” e etapas de inicialização de um O.S.

**_Systemd_**

Podemos inferir que o “_<span style="text-decoration:underline;">system**d**</span>_” é uma ferramenta moderna que implementa o estágio “Init System” em uma “distro”(distribuição) Linux (existem outras opções: _<span style="text-decoration:underline;">system**v**, Upstart, OpenRC</span>_). O _system**d**_ é, portanto, um conjunto de programas e bibliotecas (é um [software livre](https://www.freedesktop.org/wiki/Software/systemd/)) responsáveis pela sequência correta de inicialização e desligamento do sistema operacional (&lt; _/etc/init.d_>). É usado em distros modernas, tais como <span style="text-decoration:underline;">RedHat, Suse, CentOS e Debian.</span> Abaixo, é possível observar o _systemd_ na Raspberry Pi, isto é, ao ligar/desligar a placa, note que aparece a tela abaixo com “Ok” na cor verde em cada operação no sistema operacional Raspberry Pi OS (Raspbian), indicando que o referido serviço foi inicializado (ou finalizado) com sucesso (a lista de serviços disponíveis pode ser vista em &lt; _/lib/systemd/system_>).

Fig. 2 - Inicialização de serviços com _system**d** _no boot do Raspberry Pi OS

O comando **systemctl** é o gerenciador do _systemd_, i.e., <span style="text-decoration:underline;">é uma linha de comando</span> usada para verificar o status dos serviços inicializados ou finalizados pelo_ system**d**_, operando com um gerenciador utilitário de vários serviços, conforme Fig. 3. O comando **systemctl** também exibe mensagens de erro com maiores detalhes, tais como erros de tempo de execução de serviços, erros de inicialização (&lt; s_udo systemctl status processoX >; &lt; sudo sytemctl start/stop/enable processoX _>).

Fig. 3 - Exemplo de gerenciamento de serviços com _systemd_ por meio do comando “systemctl”. 

Fonte (imagem) :[Medium/Harckernon](https://medium.com/hackernoon/a-brief-overview-and-history-of-systemd-the-linux-process-manager-ca508bee4a33)

**_Systemd_ _vs._ _systemv_**

**_SysVinit_** ou **_systemv_**  (“_system five_” / _Unix System_ V/ **SysV**) é o sistema clássico e tradicional de inicialização e gerenciamento de processos em distros Linux, sendo uma das primeiras versões comerciais do OS Unix. Em uma distro Linux, os scripts de inicialização do _<span style="text-decoration:underline;">system**v**</span>_, inicializados via shell script, invocam um “_daemon binary_” que irá, então, realizar o “fork” de um processo em background. Embora exista a flexibilidade do _shell script_, a implementação de tarefas é mais complexa no_<span style="text-decoration:underline;"> system**v**</span>_ quando se trata de paralelismo. O novo estilo de ordenação e supervisão de tarefas paralelas do _<span style="text-decoration:underline;">system**d**</span>_, tornam esse método moderno mais vantajoso em relação ao antigo _<span style="text-decoration:underline;">system**v** </span>_(Tabela 1). Ademais, o _<span style="text-decoration:underline;">system**d**</span>_ também introduziu conceito “**_<span style="text-decoration:underline;">cgroups” (grupos de controle</span>_**), que organiza os processos por grupos, seguindo uma hierarquia. Por exemplo, quando processos geram outros processos, os “processos filhos” tornam-se automaticamente membros de um “**_cgroup _**pai”, evitando confusões sobre a herança dos processos (comparativos: [systemd](https://blog.desdelinux.net/en/systemd-versus-sysvinit-systemd-shim/) vs.[ systemv](https://www.certificacaolinux.com.br/systemd-no-linux/)). 

Tabela 1 - Comparação: _systemv_ vs. _systemd_

![alt_text](https://github.com/pedro-oliveiracjr/SEL0337/blob/main/Roteiros/Diagramas/P8_sysV_SysD.png?raw=true)



**Aplicação prática do _systemd_ e controle de serviços com _systemctl_**

A motivação para uma aplicação prática embarcada utilizando recursos do systemd se baseia na necessidade de um aplicativo começar a executar sua função já durante a inicialização do OS, sem ter que aguardar alguém logar no sistema e inicializar o programa manualmente via terminal. 

Para a prática, vamos supor que uma aplicação deva inicializar imediatamente após o _power on_ da Raspberry Pi.  Neste caso, nosso serviço a ser inicializado no processo boot será um script que realiza o _blink_ de um LED. Primeiramente devemos criar um script segundo a montagem da Fig. 4, para acesso a GPIO da Rasp. Por fim, será necessário colocar esse serviço à disposição do systemd, por meio de um unit file, para que o LED comece a pisca já na inicialização do Raspberry Pi OS. 




![alt_text](https://github.com/pedro-oliveiracjr/SEL0337/blob/main/Roteiros/Diagramas/P3_BLINK_LED.png?raw=true)


Fig. 4. Montagem prática do circuito que será usado como serviço no _system**d**_

**Roteiro:**



* Faça a ligação física do circuito ilustrado na Fig. 4. e em seguida ligue a Raspberry Pi.
* Crie um diretório de trabalho (não será necessário usar ambiente virtual) e escreva um programa para realizar o _blink_ de um LED conectado à GPIO da Rasp. Desta vez, vamos fazê-lo em _bash script_, para aprendermos a manipular o acesso direto à GPIO via sistema. 
* No script, é possível observar como ocorre caminho de acesso direto à GPIO, auxiliando na compreensão de como o sistema de arquivos Linux é montado (rootfs).

    ```
#!/bin/bash

echo 18 > /sys/class/gpio/export
echo out > /sys/class/gpio/gpio18/direction

while [ 1 ]
    do
        echo 1 > /sys/class/gpio/gpio18/value
        sleep 0.2s
        echo 0 > /sys/class/gpio/gpio18/value
        sleep 0.2s
    done
```


* Salve o programa como _blink.sh_, que será o serviço que deverá entrar em operação na inicialização da Rasp. 
* Em seguida, é necessário conferir a permissão de execução (“**x”**) neste arquivo, utilizando o comando “_chmod_”, responsável por alterar permissões.

    ```
chmod +x blink.sh
```


* Execute o arquivo para checar se está funcionando. Se estiver Ok, o script irá fazer com que o LED pisque infinitamente. Digite “ctrol+c” para encerrar.

    ```
./blink.sh

# Verifique o caminho de acesso a GPIO: ls /sys/class/gpio
```


* O próximo passo é a criação de um **_unit file_**, i.e, um arquivo responsável por colocar o serviço que criamos à disposição do _system**d**._ 
* Portanto, criar um arquivo chamado “_blink.service_”, com o conteúdo abaixo. L<span style="text-decoration:underline;">embrando que o systemd executa serviços na extensão “.service”.</span>

    ```
[Unit]
Description= Blink LED
After=multi-user.target

[Service]
ExecStart=/home/sel/blink.sh
#ExecStop=
user=SEL

[Install]
WantedBy=multi-user.target
```


* Observa-se que o arquivo é composto por três principais blocos:
* **[Unit]** -  primeira seção responsável pelas informações do serviço e descrição de suas dependências. O comando **_Description_** entrega essa informação, que será escrita na tela de inicialização do systemd, quando da inicialização da Rasp., com “**OK**” na cor verde se o serviço foi inicializado com sucesso ou “**Failed**” em vermelho.
* **[Service] **-  configurações da execução do serviço que será inicializado: **_Type_** (forma como os processos/scripts serão executados); **_ExecStart_** (arquivo que será executado na inicialização); e **_ExecStop _**(arquivo que será executado ao parar o serviço). Note que foi dado “_export_” na GPIO 18, conforme consta no script _blink.sh_. Logo, seria possível a escrita de um outro script que poderia efetuar o “_unexport_” da GPIO 18 e, portanto, parando o serviço se informando em **_ExecStop_**.
* **[Install] **- descreve o comportamento da inicialização do serviço. O **_WantedBy_** informa ao _system**d** _o grupo alvo no qual o serviço que deverá ser inicializado faz parte.
* Sabemos agora, portanto, que o system**d** é composto por vários destes _unit files_ que especificam quais serviços, como devem ser inicializados, e quais são as suas dependências para operar. Em nosso exemplo, passamos esses parâmetros ao system**d**, via _unit file_ “_blink.service_”, para inicializar o_ blink.sh._
* Copie o arquivo “_blink.service_” para o diretório do padrão de serviços do _system**d**_, para que de fato seja reconhecido e esteja à disposição no estágio _init system_.

    ```
sudo cp blink.service /lib/systemd/system/
```


* Em seguida, inicialize o utilitário **_systemctl_** para testar o serviço:

    ```
sudo systemctl start blink
```


* Neste caso, o **_systemctl_** **_start_** faz com que o _system**d**_ execute o serviço informado em **ExecStart** no unit file “_blink.service_”, que em nosso caso é o script “_blink.sh_”.
* Observe também que não é necessário informar a extensão, pois o **_systemctl_ **compreende que o arquivo passado é o _blink.service_, pois estruturalmente, o_ system**d**_ executa arquivos com essa extensão.
* Para parar o serviço, utilize também o utilitário **_systemctl_**:


sudo systemctl stop blink



* O próximo e último passo é a habilitação do serviço durante o _Boot_ do OS, i.e., até o passo anterior, o serviço que criamos está funcional e à disposição do _system**d**_. No entanto, irá funcionar somente ao executar **_systemctl start_**.
* Portanto, habilite o serviço para que o LED comece a piscar já na inicialização da Rasp. usando a função “**_enable_**”.

 
sudo systemctl enable blink



* Se estiver tudo Ok, o serviço deve inicializar automaticamente durante o estágio _init system_ no _Boot _da Raspberry Pi. 
* Reinicie a Raspberry Pi para testar a funcionalidade criada.  Se necessário, desabilite a opção “S_plash Screen_” em Configurações da Rasp. (sudo raspi-config - system - splash screen) para visualizar as mensagens de inicialização. A opção _splash screen_, quando habilitada, não mostra esses detalhes.
* O serviço deve ser inicializado no estágio _init system_ e o LED deve começar a piscar. O serviço é identificado pela mensagem que foi digitada em **_Description_**, no _unit file_ “_blink.service_”, com um “OK” verde na frente, conforme resultado abaixo.

 
[    OK    ]  Started  Blink LED . .



* Para solução de problemas, utilize **_systemctl status_** e verifique mensagens de erro.

systemctl status blink.service



* Para desabilitar o serviço no Boot, basta utilizar **_unable_**. Verifique com **_systemctl statuts_**.

   
sudo systemctl enable blink
systemctl status blink.service




**Tarefa:** 

Repita o processo acima, criando e testando um novo serviço que deve ser inicializado no Boot da Raspberry Pi pelo system**d** e que também deve realizar o <span style="text-decoration:underline;">blink de um LED</span>. Contudo, use um **script em Python** para piscar o LED, ao invés do _bash script_ usado no exemplo acima. Observe que neste caso o Python (_/usr/bin/python_) também é um serviço deve ser chamado no _unit file_, pois o _script.py_ depende dele para execução em razão de suas bibliotecas. Enviar como tarefa na atividade da prática #8 atribuída no e-Disciplinas e documentar no relatório os programas elaborados como tarefa e conceitos envolvidos na prática.
