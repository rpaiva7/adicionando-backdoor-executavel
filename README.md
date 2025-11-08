## Adicionando backdoor em um executável 

### Criando um backdoor embarcado em um executável e testando o ataque em uma VM (Virtual Machine) do Windows 7

![](https://www.sandsteinwandern.de/wandern/wp-content/uploads/2019/11/giphy-hack2.gif)

Mascarando um backdoor para varredura em um arquivo executável. Geralmente somos infectados nesta modalidade quando baixamos software de origem duvidosa.

Backdoor (porta dos fundos) é o uso de qualquer malware/vírus/tecnologia para obter acesso não autorizado ao aplicativo/sistema/rede, driblando a segurança implementada. É uma brecha/porta que não foi fechada. 

Spyware, Ransomware, DDoS e Cryptojacking são alguns tipos de ataques com backdoor.

Nesta prática vamos utilizar o Meterpreter, que é um payload do metasploit que funciona por injeção dll. O meterpreter reside inteiramente na memória do anfitrião e não deixa vestígios no disco rígido (o que o torna de difícil detecção nas técnicas forenses).
