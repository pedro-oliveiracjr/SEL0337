<!-- Output copied to clipboard! -->

<!-----

Yay, no errors, warnings, or alerts!

Conversion time: 0.528 seconds.


Using this Markdown file:

1. Paste this output into your source file.
2. See the notes and action items below regarding this conversion run.
3. Check the rendered output (headings, lists, code blocks, tables) for proper
   formatting and use a linkchecker before you publish this page.

Conversion notes:

* Docs to Markdown version 1.0β34
* Tue Dec 13 2022 08:27:42 GMT-0800 (PST)
* Source doc: Roteiro_pratica-2
----->



### Prática 2: Introdução a instalação de sistema operacional em sistemas embarcados

NOME:									 Nº USP: 

NOME:									 Nº USP: 


#### Resumo

Introdução ao uso de instaladores de distribuições linux, como o calamares ou debian installer(uma versão modificada dele é utilizada no instalador do raspbian), configurações iniciais após instalação do sistema operacional, configuração do usuário root, conexão a internet, ativação do SSH, ativação dos drivers da câmera. 

**Conceitos importantes:**

_Calamares project, Ubiquity, Debian-installer, SSH, Camera legacy rasp, sudo e segurança de root._


#### Objetivo

O objetivo desta prática é a familiarização com ambientes de instalação de distribuições linux mais utilizados, como é o caso do debian-installer, responsável pela instalação do Debian e de alguns projetos derivados dele, como o Raspberry Pi OS (antigo “Raspbian”). Além disso, a familiarização com estes sistemas para uso de outros ambientes como o [calamares](https://diolinux.com.br/tecnologia/os-segredos-do-instalador-calamares.html), responsável pela instalação de grande quantidade de distribuições linux conhecidas, desenvolvida por um brasilerio da Universidade Federal de Minas Gerais, e o Ubiquity, responsável pela instalação do Ubuntu e alguns derivados.

Ademais, a prática envolve algumas configurações iniciais a serem feitas no Raspberry Pi OS a fim de garantir que as funções básicas estejam disponíveis para uso em atividades seguintes, assim como configurações de segurança.


#### Aplicação

Portanto, a partir dos conceitos apresentados acima, a prática consiste em realizar a instalação do sistema operacional Raspberry Pi OS, a partir do instalador fornecido pela Raspberry Pi Foundation, denominado “raspberry pi imager”, e a partir dele, realizar as configurações do instalador debian.

Uma vez instalado o sistema operacional, deve-se atentar a realizar a alteração do acesso root padrão antes de conectá-lo a internet, impedindo que a placa possa ser acessada remotamente por usuários não autorizados na rede. Com isso, a conexão à internet pode ser feita de forma segura, habilitando a internet wireless da placa, e seus subsistemas importantes para atividades seguintes como o SSH, VNC e o driver de câmera legacy.

Após todas as configurações e instalações, pode-se atualizar os repositórios a fim de garantir que o sistema esteja inteiro atualizado com os padrões do repositório oficial do Debian, visando garantir a performance e segurança do sistema.


#### Motivação

A motivação para conhecer os conceitos básicos da instalação de sistemas operacionais embarcados advém do fato de que os processos utilizados para a instalação de um sistema operacional embarcado são os mesmos para a instalação de uma distribuição comum, e em geral, todos os instaladores seguem processos semelhantes. Desta forma, conhecendo-se o processo para uma plataforma, pode-se generalizar os conceitos com maior facilidade para o uso de outras distribuições em projetos que necessitem de sistemas operacionais baseados em distribuições Linux (Kernel). Além disso, as configurações de segurança e inicialização são protocolos padrão que devem ser seguidos em qualquer sistema a ser utilizado conectado à internet, a fim de impedir que usuários da rede possam acessar seu sistema em projetos mais complexos que os utilizados em aula. 


#### Roteiro

Roteiro a ser seguido para execução da prática:



* Antes de iniciar o processo, deve-se observar se o instalador “Raspberry Pi imager” já está instalado no computador do laboratório.
* Uma vez instalado, deve ser feito o download da distribuição com interface gráfica, e instalação no cartão SD.
* Nas configurações do imager, já é possível ser feita a alteração do usuário, porém, esta não é possível ainda alterar senha de root, por este motivo, não deve-se conectá-lo à rede antes de ser feita esta configuração, a fim de garantir a segurança do dispositivo.
* Desta forma, ao final da instalação, pode-se conectar o cartão SD a Raspberry Pi, já tendo acesso a interface gráfica.
* Conforme discorrido, deve-se como primeira atividade alterar o usuário root, para o nome de usuário e senha iguais aos dos computadores da bancada, garantindo que usuários da rede não possam acessá-la remotamente.
* Com o usuário root configurado, pode-se conectá-la à rede Wi-Fi do laboratório (LabMicros).
* Uma vez instalados e configurados os parâmetros básicos do sistema, pode-se acessar o utilitário de configuração do sistema a fim de ativar algumas funções que serão úteis em práticas seguintes, como o acesso a câmera legacy, o cliente SSH, e o  suporte a VNC, além de visualizar outras configurações possíveis de se ativar, como I2C, por exemplo, etc.
* Com todas as opções necessárias habilitadas, pode-se instalar o utilitário de terminal _neofetch_, com ele pode-se observar alguns parâmetros do hardware e do sistema operacional instalados.
* Salve um print da imagem dele (output do terminal após a execução do comando “neofetch”) para permitir a consulta posterior (no relatório os parâmetros mostrados pelo _neofetch _deverão ser explicados_ _a partir da teoria vista - * <span style="text-decoration:underline;">as instruções para elaboração do relatório serão fornecidas em brev</span>e). Faça o mesmo procedimento para os parâmetros mostrados no terminal quando executado o comando “**pintout**”.
* Com todos os procedimentos realizados, pode-se atualizar os pacotes a partir do gerenciador de pacotes no terminal.
* Verifique o endereço “IP” da Rasp.
* Gere um histórico de texto dos comandos utilizados no terminal.
* Após conectar a Raspberry e configurar todos os pacotes e configurações já executadas, faça o acesso remoto à rasp via “SSH” (security socket shell) a partir do PC (pode no Linux Debian do PC via acesso direto “SSH” - e no Windows a partir do Putty).
* Busque dentro dos diretórios da Rasp o arquivo de texto salvo com histórico de comandos do terminal. Copie este arquivo para o diretório atual a partir do terminal Linux Debian do PC. Salve essas informações para seu relatório (lembrando que na tarefa deverá ser enviado o arquivo “.txt” referente aos comandos executados).
* Faça o acesso remoto à Rasp via VNC a partir do PC (Debian ou Windows). O acesso remoto via VNC à rasp pode também ser feito a partir do seu smartphone (opcional). 
* Para a conexão remota (SSH direto ou VNC) acontecer e os passos anteriores funcionarem, o computador host (PC ou smartphone) e o computador remoto (Rasp) devem estar conectados à mesma rede - seja cabeada (ethernet) ou Wi-Fi (LabMicros). 
* Por fim, execute um comando para limpar o histórico terminal.
* Ainda não será necessário a elaboração e envio de um relatório relacionado à presente atividade prática., sendo necessário agora apenas o envio do arquivo “.txt” com histórico salvo dos comandos do terminal da Rasp usados na atividade.
