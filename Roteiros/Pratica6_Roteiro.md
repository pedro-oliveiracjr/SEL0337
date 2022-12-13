
### Prática 6: Introdução a interfaces de visão computacional, sistemas de versionamento de arquivos e APIs públicas.


#### Resumo

Introdução ao uso de periféricos embarcados na raspberry com o uso do módulo de câmera, além da utilização do versionamento de código via Git/GitHub, e o uso de APIs para obtenção de dados da internet para o desenvolvimento de uma aplicação climática. 

**Conceitos importantes:**

OpenCV, PiCamera, visão computacional, Git/GitHub, APIs, IoT, REST, JSON.


#### Objetivo

O objetivo desta prática é o projeto de uma base climática utilizando de um sistema de linux embarcado. Para isto, será utilizado o módulo de câmera embarcado próprio da RaspBerry Pi, além da obtenção de dados da internet. Como forma de documentação do projeto, o Git será o sistema de versionamento e o GitHub como repositório, junto do histórico de versionamento.

Para realização do projeto, será utilizado implementações em Python capazes de controlar a câmera embarcada.Nesta prática, é recomendado o uso do módulo PiCamera do Python, uma vez que já está presente no sistema, e que a instalação da OpenCV não é trivial na rasp. Além disso, para obtenção dos dados da internet, será utilizado uma API pública, desenvolvida pela Oracle, e o módulo requests do Python para obter os seus dados.

Ambas as implementações podem ser encontradas na internet, a partir do site oficial da PI Foundation, na forma de projetos completos, desenvolvidos para atividades educacionais.

O uso do sistema de versionamento de código GIT e o sistema de armazenamento destas versões e documentação GitHub é de extrema importância para os projetos profissionais desenvolvidos por grandes empresas, grupos de software livre, e como forma de se manter um perfil atualizado com as habilidades profissionais e pessoais. Além disso, serve como forma de distribuição de softwares, e pode ser usado para projetos em que possua um número grande de pessoas e corporações atuando em um mesmo projeto, uma vez que o sistema é capaz de cuidar das inconsistências, e reunir alterações no código feitas em partes distintas do projeto, sem que uma parte interfira na outra.

Os repositórios em GitHub também servem como forma de documentar projetos grandes, a fim de garantir que outras pessoas possam implementar a sua solução em outros projetos, garantindo que o seu projeto possa ajudar mais pessoas, e seja encontrado de forma fácil.


![alt_text](https://github.com/pedro-oliveiracjr/SEL0337/blob/main/Roteiros/Diagramas/P6_GIT_API_CAM.png?raw=true)


Fig. 1- Ideia do projeto com módulo da câmera, estação climática e documentação com Git/GitHub


#### Aplicação

Portanto, para a prática, deve-se executar um script em python, que inicialmente, seja capaz de obter dados da câmera, seguindo roteiro e tutorial disponibilizado. A partir disto, deve-se implementar alguma modificação na imagem, e escrever na própria imagem o n° USP da dupla que está executando o projeto, utilizando um preview de imagem para verificar a posição da câmera, e a realização de uma foto e um vídeo curto (±5 segundos), permitindo a visualização do ambiente (dentro do contexto de uma base climática, permitiria a identificação da presença e intensidade solar).

Em seguida, da mesma forma, deve-se implementar um script capaz de coletar dados de uma base climática a partir da API fornecida pelo roteiro. Para tanto, deve-se acessar a URL dada pelo módulo _requests_, junto da ID que foi fornecida, de forma a obter a estação mais próxima em funcionamento, e obter seus dados climáticos.

Uma vez obtidos os dados climáticos, deve-se gerar um script único que seja capaz de executar as duas implementações anteriores, (neste caso, será somente necessário juntar os dois códigos), de forma que se obtenha os dados climáticos e as imagens do ambiente (caso deseje, pode-se utilizar o módulo json para extrair algum dado climático mais relevante e colocar na própria imagem). Coloque como saída do programa a impressão dos dados obtidos da base climática, e salve as imagens e vídeos gerados.


#### Motivação

Bases climáticas estão espalhadas por todo o planeta, seja em centros de estudo, pontos de medição urbanos, sondas espaciais, e projetos de conservação do meio ambiente. Em todos eles é necessário que os dados coletados do clima possam ser facilmente recuperados, para que os operadores possam fazer suas análises, que podem ir desde uma previsão do clima, indicando a possibilidade de chuva, ou forte incidência solar, ou como uma análise do índice de aquecimento global. Para todas estas aplicações, bases climáticas simples que utilizam de sistemas embarcados podem ser aplicadas. Até mesmo a própria base climática que será utilizada neste projeto, foi construída a partir de um sistema embarcado fornecido pela Oracle para obtenção dos dados em sua API.

Para esta prática, são utilizados dados climáticos de outro sistema (da internet) e de forma didática, ainda assim, porém, possui aplicação de utilidade. Dessa forma, uma vez que a API somente fornece os dados em forma de texto, esta base climática poderia ser uma adição ao projeto, mostrando uma imagem de como está o ambiente ao qual a medição foi feita. Além disso, pode-se criar a própria API, utilizando do mesmo módulo em Python, em que o sistema colete por conta própria os dados climáticos a sua volta, a partir do uso de sensores conectados em si, e forneça os seus dados junto das imagens para que o público possa acessar. Ou, ainda, que pesquisas possam ser feitas a respeito do clima da região, ressaltando a importância de sistemas embarcados capazes de se conectar a internet, e capazes de interagir com sistemas operacionais embarcados na forma de um projeto simples de análise ambiental.


#### Roteiro



* Atente-se à conexão da placa: a câmera deve ser conectada <span style="text-decoration:underline;">enquanto a placa está desligada</span>. No slot escrito câmera na placa, entre o conector ethernet e o HDMI. <span style="text-decoration:underline;">Verifique se a posição dos conectores do cabo flat e dos pinos internos do conector estão de acordo, evitando curtos circuitos na câmera</span> ([tutorial de como conectar](https://projects-static.raspberrypi.org/projects/getting-started-with-picamera/dbf2d9575be4756f79e4293a047a8a531d340710/en/images/connect-camera.gif): [https://abrir.link/mY6WK](https://abrir.link/mY6WK)).
* A partir do terminal, abra as configurações da rasp, como visto na prática 2, e habilite a câmera legada, e a conexão I2C, necessárias para a comunicação com a câmera.
* Importe os módulos _PiCamera, requests_ e _json_, além do módulo _pprint_. Caso algum deles não esteja instalado, utilize o instalador de pacotes python como visto em práticas anteriores.
* Para testar o funcionamento da câmera, pode-se utilizar o comando de terminal _raspistill_. Nos tutoriais, cujos links são compartilhados abaixo, encontram-se os parâmetros que devem ser passados para uso do comando.
* Implemente o script em python de acesso a câmera. Maiores informações de como controlar a câmera podem ser encontradas no site do[ projeto da PI foundation](https://projects.raspberrypi.org/en/projects/getting-started-with-picamera/7): [https://abrir.link/FGYzd](https://abrir.link/FGYzd)
* Acesse via navegador a API de clima (Oracle), a partir do [link](https://apex.oracle.com/pls/apex/raspberrypi/weatherstation/getallstations): [https://abrir.link/z2om3](https://abrir.link/z2om3)
* O tutorial da API de clima está no link a seguir [Weather station](https://projects.raspberrypi.org/en/projects/fetching-the-weather/7): [https://abrir.link/U9W6R](https://abrir.link/U9W6R)
* Implemente o projeto de obtenção de dados climáticos. Note que o tutorial é mais complexo do que o exigido, uma vez que se calcula qual a estação climática mais próxima, porém, visto que a mais próxima está desativada, a obtenção da ID da estação mais próxima não retorna uma estação com dados. Por este motivo, siga os passos do tutorial para entender o funcionamento do projeto, porém, ao invés de utilizar a ID da estação mais próxima, utilize a seguinte <span style="text-decoration:underline;">ID 966583</span>, base climática instalada na UFSC, que ainda possui dados funcionais.
* Configure o Git em sua rasp, a partir de um código de uso genérico temporário, de forma que ele esteja habilitado na rasp somente durante a duração da aula, impedindo que esta rasp possa acessar sua conta github ao final da prática (abaixo segue um tutorial de como utilizar o código).
* Lembre-se de implementar todas as etapas utilizando os comandos em git aprendidos, de forma a manter um histórico das suas versões de código, e documentadas as suas alterações. (Criar na conta GitHub um repo com código da disciplina e documentar o projeto dentro dele a partir da rasp (realizar commits e push).
* Ao final do projeto, após realizar o script conjunto, gere um arquivo README.md em seu repositório, e detalhe o funcionamento básico do seu projeto. Esta etapa será considerada como o trecho do relatório em que se explica o funcionamento do seu código, como segue pelo documento de critérios de avaliação. Portanto, deixe o link para seu repositório nesta etapa do relatório, e torne o repositório público para que possa ser avaliado. 
* Para efetuar “git push” e transferir arquivos para o repo no GitHub, gere uma **token** de acesso pessoal, n<span style="text-decoration:underline;">a conta do GitHub em _developer settings_,</span> e com essa chave, de um push no repositório remoto. Lembrando que, ao final, o repositório da dupla deve ser idêntico. 
* **<span style="text-decoration:underline;">Entrega: o script completo desenvolvido em Python “.py” e  relatório. O relatório deverá conter documentação do projeto: introdução, desenvolvimento e conclusão. Inserir no relatório as imagens salvas pela câmera, a output contendo os dados climáticos. Contudo, a parte referente a codificação, explicação da lógica, comandos e bibliotecas utilizadas deverá ser feita em um arquivo Readme a ser disponibilizado no repositório do projeto na sua conta do GitHub. Portanto, ao invés de colocar essa parte no relatório, informar o  link para o repositório que contém os dados do projeto e o arquivo “Readme”.</span>**
* Bom projeto! e em caso de dúvidas contate o professor ou monitor da disciplina.


#### Tutorial GIT



* Acesse o seu GitHub a partir do navegador, e entre em seu perfil.
* Abra suas configurações e navegue até a sessão SSH e GPG keys
* Na sessão GPG há um tutorial do próprio GitHub, a partir do [link](https://docs.github.com/pt/authentication/managing-commit-signature-verification)
* Aqui segue um passo a passo de como [configurar](https://docs.github.com/pt/authentication/managing-commit-signature-verification/adding-a-gpg-key-to-your-github-account)
* Com a chave GPG configurada, já podem ser utilizados os comandos em git vistos em aula. Atente-se ao fato que a chave GPG possui duração mínima de um dia, portanto, ao final da aula, acesse seu perfil, e remova manualmente a sua chave GPG, mantendo sua conta segura .
* Remova a chave GPG da rasp, a partir dos seguintes comandos:

    ```
gpg -list-keys
gpg -delete-secret-key NAME_OF_YOUR_KEY
gpg -delete-key NAME_OF_YOUR_KEY
gpg -list-keys
```
