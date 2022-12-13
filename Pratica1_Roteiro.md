<!-- Output copied to clipboard! -->

<!-----

Yay, no errors, warnings, or alerts!

Conversion time: 0.587 seconds.


Using this Markdown file:

1. Paste this output into your source file.
2. See the notes and action items below regarding this conversion run.
3. Check the rendered output (headings, lists, code blocks, tables) for proper
   formatting and use a linkchecker before you publish this page.

Conversion notes:

* Docs to Markdown version 1.0β34
* Tue Dec 13 2022 08:23:46 GMT-0800 (PST)
* Source doc: Roteiro_pratica-1
----->



### Prática 1: Introdução a comandos em bash e sistemas de arquivos linux

NOME:									 Nº USP:

NOME:									 Nº USP: 


#### Resumo

Introdução aos sistemas de arquivos em terminais linux (bash), criação de arquivos, criação de diretórios, navegação entre diretórios, edição de textos e códigos a partir do terminal.

**Comandos importantes:**

_cd, ls, pwd, cat, find, grep, history, echo, touch, nano, man, help._

**Comandos de manipulação de arquivos:**

_mv, rm, cp, mkdir, rmdir._


#### Objetivo

O objetivo desta prática é a familiarização com comandos utilizados em terminais linux, a fim de realizar operações básicas em sistemas de arquivos, utilizando a linguagem de programação bash, padrão em sistemas operacionais linux, o bash é um shell de comandos que serão executados pelo computador para realizar tarefas passadas a ele.


#### Aplicação

Portanto, a partir dos comandos listados acima, e os conceitos passados em aula, a prática consiste em criar um diretório, e em uma pasta interna criar um arquivo de texto simples com o nome dos autores da prática, e após isso, realizar algumas manipulações com arquivos, os passos para a execução da prática podem ser conferidos abaixo, com as sequências de atividades a serem executadas no terminal.


#### Motivação

A motivação para conhecer os conceitos básicos da linguagem bash é que terminais em sistemas operacionais embarcados são de extrema importância, visto que em muitas aplicações, não é possível utilizar interfaces gráficas para configuração do sistema, devido a limitações de hardware, ou quando são necessárias manutenções em campo, e não há a disponibilidade de conexão de novos equipamentos para o uso da interface gráfica, além disso, como visto pela teoria, o uso de recursos de uma SBC deve ser otimizado, visto que seu poder de processamento é reduzido comparado a um computador de uso geral, desta forma, reduzir o poder de processamento na renderização de uma interface gráfica libera recursos do processador para as aplicações do produto. 


#### Roteiro

Roteiro a ser seguido para execução da prática:



* Antes de iniciar a prática, escreva no terminal a partir do comando _echo_, “INÍCIO DA PRÁTICA 1”.
* Verifique em qual diretório o terminal está a partir do terminal, e sua posição relativa.
* Crie um diretório na pasta _/home/sel_ com o código da disciplina.
* Acesse o diretório criado.
* Verifique se foi acessado o diretório desejado.
* Crie uma pasta interna, com o nome pratica_1
* Acesse a pasta interna
* Volte ao diretório pai
* Liste as pastas criadas
* Acesse novamente a pasta criada
* Crie um arquivo helloworld.txt
* Escreva o nome e número usp da dupla neste arquivo utilizando o nano.
* Dentro do nano, ao terminar de editar, pressione ctrl+x para salvar e sair.
* Visualize o que foi escrito utilizando o comando cat.
* Verifique se há somente um arquivo .txt dentro do seu diretório, a partir do comando de terminal (find / -name ‘*.txt’) (uso de bash wildcards)
* Utilize o comando grep para encontrar ambos os números USP da dupla dentro do arquivo de texto.
* Copie o seu arquivo hello world para o próprio diretório com outro nome
* Mova a cópia para o diretório pai (utilizando relacionais ou o caminho do arquivo)
* Remova o arquivo original 
* Saia da pasta interna (volte ao diretório pai utilizando os relacionais)
* Remova a pasta pratica_1
* Gere um arquivo backup_historico.txt com o histórico dos seus comandos (comandos de input/output)
* Utilize o comando tail -N para determinar quantas linhas serão salvas (inclua até encontrar a linha com o comando echo do início do tutorial)
* extra: em todos os computadores do laboratório foi instalado o aplicativo neofetch, este mostra detalhes do hardware, mas também do sistema operacional e do ambiente gráfico instalado, sendo um padrão quando se pretende compartilhar com outros usuários linux um status geral do seu sistema, seja para correção de problemas, avaliando o hardware instalado, ou para passar padrões de gráficos utilizados, para acompanhar o mesmo estilo de outra pessoa, para isso, coloque o comando _neofetch_ no terminal e será impressa as informações com uma logo da distribuição que está sendo utilizada no momento
