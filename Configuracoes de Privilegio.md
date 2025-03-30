# 5.1.1 Limitando a disponibilidade do comando
Grandes organizações têm muitas funções de trabalho variadas dentro de um departamento de TI. Nem todas as funções de trabalho devem ter o mesmo nível de acesso aos dispositivos de infra-estrutura. O Cisco IOS Software tem dois métodos de fornecer o acesso à infraestrutura: nível de privilégio e CLI baseado em função. Ambos os métodos ajudam a determinar quem deve ter permissão para se conectar ao dispositivo e o que essa pessoa deve ser capaz de fazer com ele. O acesso à CLI baseado em função fornece mais granularidade e controle.

À revelia, o Cisco IOS Software CLI tem dois níveis de acesso aos comandos:

Modo EXEC do usuário (nível de privilégio 1) - Isso fornece os mais baixos privilégios de usuário do modo EXEC e permite somente comandos de nível de usuário disponíveis na alerta Router>.
Modo EXEC privilegiado (nível de privilégio 15) - Inclui todos os comandos de nível de ativação no prompt Router#.
Existem 16 níveis de privilégios no total, conforme listado abaixo. Quanto maior o nível de privilégio, mais acesso de roteador um usuário tem. Os comandos que estão disponíveis em níveis de privilégios mais baixos também são executáveis em níveis mais altos.

Nível 0: predefinido para privilégios de acesso no nível do usuário. Raramente usado, mas inclui cinco comandos: disable, enable, exit, help,e, logout.
Nível 1: O nível padrão para o início de uma sessão com o roteador alerta do roteador >. Um usuário não pode fazer alterações ou visualizar o arquivo de configuração em execução.
Níveis 2 -14: pode ser personalizado para privilégios de nível de usuário. Comandos de níveis mais baixos podem ser movidos para outro nível mais alto, ou comandos de níveis mais altos podem ser movidos para baixo para um nível mais baixo.
Nível 15: Reservado para os privilégios do modo habilitado (comando enable). Os usuários podem alterar configurações e visualizar arquivos de configuração.
Para atribuir comandos a um nível de privilégio personalizado, use o comando do modo de configuração global privilege mostrado abaixo.

Router(config)# privilege mode {level level|reset} command
![image](https://github.com/user-attachments/assets/6110e4f2-3c99-4f7f-a87e-6786fbd53f43)


5.1.2 Configurando e Atribuindo Níveis de Privilégio
Para configurar um nível de privilégio com comandos específicos, use o privilege exec level level [command]. O exemplo mostra exemplos para três níveis de privilégios diferentes.

O nível de privilégio 5 possui acesso a todos os comandos disponíveis para o nível 1 e ao comando ping.
O nível de privilégio 10 possui acesso a todos os comandos disponíveis para o nível 5 e ao comando reload.
O nível de privilégio 15 é predefinido e não precisa de ser configurado explicitamente. Este nível de privilégio tem acesso a todos os comandos, incluindo visualizar e alterar a configuração.
~~~~
R1# conf t

R1(config)# !Level 5 and SUPPORT user configuration

R1(config)# privilege exec level 5 ping

R1(config)# enable algorithm-type scrypt secret level 5 cisco5

R1(config)# username SUPPORT privilege 5 algorithm-type scrypt secret cisco5

R1(config)# !Level 10 and JR-ADMIN user configuration

R1(config)# privilege exec level 10 reload  

R1(config)# enable algorithm-type scrypt secret level 10 cisco10  

R1(config)# username JR-ADMIN privilege 10 algorithm-type scrypt secret cisco10  

R1(config)# !Level 15 and ADMIN user configuration

R1(config)# enable algorithm-type scrypt secret level 15 cisco123

R1(config)# username ADMIN privilege 15 algorithm-type scrypt secret cisco123
~~~~
Existem dois métodos para atribuir senhas aos diferentes níveis de privilégio:

comando do modo de configuração global * To a user that is granted a specific privilege level, use the username name privilege level secret password
comando do modo de configuração global * To the privilege level, use the enable secret level level password

Nota: Ambos os comandos username secret e o enable secret são configurados para a criptografia do tipo 9.

Use o username para atribuir um nível de privilégio a um usuário específico. Use o comando enable secret para atribuir um nível de privilégio a uma senha específica do modo EXEC. Por exemplo, o usuário SUPPORT recebe o nível de privilégio 5 com a senha cisco5. Contudo, segundo as indicações do exemplo abaixo, todo o usuário pode alcançar o nível de privilégio 5 se esse usuário sabe que a senha secreta da possibilidade é cisco5. O exemplo também demonstra que o nível de privilégio 5 não pode recarregar o roteador.
~~~~
R1> enable 5
Password: <cisco5>
R1# show privilege  Current privilege level is 5
R1# ping 10.1.1.1
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.1.1.1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/2/4 ms
R1# reload
Translating "reload"
 
% Bad IP address or host name
Translating "reload"
 
% Unknown command or computer name, or unable to find computer address
R1#
~~~~
No exemplo abaixo, o usuário permite o nível de privilégio 10 que tem acesso ao comando reload. No entanto, os usuários no nível de privilégio 10 não podem visualizar a configuração em execução.
~~~~
R1# enable 10
Password: <cisco10>
R1# show privilege
Current privilege level is 10
R1# ping 10.1.1.1
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.1.1.1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/2/4 ms
R1# reload
 
System configuration has been modified. Save? [yes/no]: ^C
 
R1# show running-config
^
% Invalid input detected at '^' marker.
R1#
~~~~
No próximo exemplo, o usuário habilita o nível de privilégio 15, que tem acesso total para visualizar e alterar a configuração, incluindo a visualização da configuração em execução.
~~~~
R1# enable 15
Password:  
R1# show privilege
Current privilege level is 15
R1# show running-config Building configuration...
 
Current configuration : 1979 bytes
!
! Last configuration change at 15:30:07 UTC Tue Feb 17 2015
!
version 15.4
 
R1#
~~~~
# 5.1.3 Limitações dos Níveis de Privilégio
O uso de níveis de privilégio tem suas limitações:

Não há controle de acesso a interfaces, portas, interfaces lógicas e slots específicos em um roteador.
Os comandos disponíveis em níveis de privilégios mais baixos são sempre executáveis em níveis mais altos.
Comandos especificamente definidos em um nível de privilégio mais alto não estão disponíveis para usuários com privilégios mais baixos comandos.
Atribuir um comando com várias palavras-chave permite o acesso a todos os comandos que usam essas palavras-chave. Por exemplo, permitir o acesso à show ip route permite ao usuário acessar todos os show e show ip.
Nota: Se um administrador precisar criar uma conta de usuário que tenha acesso à maioria, mas não a todos os comandos, as instruções privilege exec precisam ser configuradas para cada comando que deve ser executado em um nível de privilégio inferior a 15.



# 5.2.2 Exibições baseadas em função
A CLI baseada em função fornece três tipos de pontos de vista que ditam quais comandos estão disponíveis:

Exibição Raiz

Para configurar qualquer exibição para o sistema, o administrador deve estar na exibição raiz. A exibição raiz tem os mesmos privilégios de acesso que um usuário que tem privilégios de nível 15. No entanto, uma visualização raiz não é a mesma que um usuário de nível 15. Somente um usuário de exibição raiz pode configurar uma nova exibição e adicionar ou remover comandos das exibições existentes.

Visualização CLI

Um conjunto específico de comandos pode ser empacotado em uma opinião CLI. Ao contrário dos níveis de privilégio, uma visualização CLI não tem hierarquia de comando e nenhuma visão superior ou inferior. Cada exibição deve receber todos os comandos associados a essa exibição. Uma exibição não herda comandos de nenhuma outra exibição. Além disso, os mesmos comandos podem ser usados em várias exibições.

Superview

Uma superview consiste em uma ou mais visualizações CLI. Os administradores podem definir quais comandos são aceitos e quais informações de configuração são visíveis. Superviews permitem que um administrador de rede atribua usuários e grupos de visualizações múltiplas CLI dos usuários ao mesmo tempo, em vez de ter que atribuir uma única visão CLI pelo usuário com todos os comandos associados a essa uma visão CLI.

Superviews têm várias características específicas:

Uma única visualização CLI pode ser compartilhada em várias superviews.
Os comandos não podem ser configurados para uma superview. Um administrador deve adicionar comandos à opinião CLI e adicionar essa opinião CLI ao superview.
Os usuários que são registrados em um superview podem alcançar todos os comandos que são configurados para alguns dos pontos de vista CLI que são parte do superview.
Cada superview tem uma senha usada para alternar entre superviews ou de uma visualização CLI a um superview.
A exclusão de um superview não suprime das visualizações CLI associadas. As visualizações CLI permanecem disponíveis para serem atribuídas a outra superview.
Clique em Reproduzir na animação para obter uma explicação das visualizações.

![image](https://github.com/user-attachments/assets/376631af-14d0-4728-9fe6-b27f23176731)


5.2.3 Configurar exibições baseadas em função
Antes que um administrador possa criar uma vista, o AAA deve ser permitido usando o comando aaa new-model. Para configurar e editar visualizações, um administrador deve efetuar login como visualização root usando o comando EXEC privilegiado enable view. O comando enable view root também pode ser usado. Quando solicitado, digite enable secret senha.

Há cinco etapas para criar e gerenciar uma exibição específica.

Etapa 1. Habilite AAA com o comando do modo de configuração global aaa new-model. Sair e insira a exibição raiz com o comando enable view.

![image](https://github.com/user-attachments/assets/3e0722dc-7fbd-477c-9908-2ff77533fce5)



``Router# enable [view [view-name]]``

Etapa 2. Crie uma visualização usando o comando do modo de configuração global parser view view-name. Isso habilita o modo de configuração de exibição. Excluindo a visualização raiz, há um limite máximo de 15 visualizações no total.


``Router(config)# parser view view-name``

Etapa 3. Atribua uma senha secreta à visualização usando o comando do modo de configuração secret password view.

Isso define uma senha para proteger o acesso à superview. A senha deve ser criada imediatamente após a criação de uma exibição, caso contrário, uma mensagem de erro aparecerá.

``Router(config-view)# secret password``

Etapa 4. Atribua comandos à visualização selecionada usando o comando commands parser-mode no modo de configuração da visualização.


``Router(config-view)# commands parser-mode {include | include-exclusive | exclude} [all] [interface interface-name | command]
``

![image](https://github.com/user-attachments/assets/62d2741d-2abf-4223-98ec-910a88473df4)


Etapa 5. Sair do modo de configuração da vista digitando o comando exit.

O exemplo abaixo mostra a configuração de três visualizações. Observe no exemplo, que o comando secret apoia somente a criptografia MD5 (tipo 5). Além disso, observe que quando um comando foi adicionado a uma exibição antes que a senha fosse atribuída, ocorreu um erro.
~~~~
R1(config)# aaa new-model
R1(config)# parser view SHOWVIEW
R1(config-view)# secret ?
0     Specifies an UNENCRYPTED password will follow
5     Specifies an ENCRYPTED secret will follow
LINE  The UNENCRYPTED (cleartext) view secret string
R1(config-view)# secret cisco
R1(config-view)# commands exec include show
R1(config-view)# exit
R1(config)# parser view VERIFYVIEW
R1(config-view)# commands exec include ping
% Password not set for the view VERIFYVIEW  
R1(config-view)# secret cisco5
R1(config-view)# commands exec include ping
R1(config-view)# exit
R1(config)# parser view REBOOTVIEW
R1(config-view)# secret cisco10
R1(config-view)# commands exec include reload
R1(config-view)# exit
R1(config)#
~~~~
Verifique a configuração da interface usando o comando show running-config, conforme mostrado.
~~~~
R1# show running-config
<output omitted>
parser view SHOWVIEW
  secret 5 $1$GL2J$8njLecwTaLAc0UuWo1/Fv0
  commands exec include show
!
parser view VERIFYVIEW
  secret 5 $1$d08J$1zOYSI4WainGxkn0Hu7lP1
  commands exec include ping
!
parser view REBOOTVIEW
  secret 5 $1$L7lZ$1Jtn5IhP43fVE7SVoF1pt.
  commands exec include reload
~~~~

5.2.6 Configurar superviews CLI com base em função
As etapas para configurar uma superview são essencialmente as mesmas que configurar uma visualização CLI, exceto que o comando view view-name é usado para atribuir comandos ao superview. O administrador deve estar na exibição raiz para configurar uma superview. Para confirmar que a exibição raiz está sendo usada, use o comando enable view ou enable view root. Quando solicitado, digite a secret senha.

Há quatro etapas para criar e gerenciar uma superview.

Mais de uma exibição podem ser atribuídas a uma superview e as exibições podem ser compartilhadas entre superviews. O exemplo mostra a configuração de três superviews: USER, SUPPORT e JR-ADMIN.

R1(config)# parser view USER superview
R1(config-view)# secret cisco
R1(config-view)# view SHOWVIEW
R1(config-view)# exit
R1(config)#    
R1(config)# parser view SUPPORT superview
R1(config-view)# secret cisco1
R1(config-view)# view SHOWVIE
% Invalid view name SHOWVIE
 
R1(config-view)# view SHOWVIEW
R1(config-view)# view VERIFYVIEW
R1(config-view)# exit
R1(config)#    
R1(config)# parser view JR-ADMIN superview
R1(config-view)# secret cisco2
R1(config-view)# view SHOWVIEW
R1(config-view)# view VERIFYVIEW
R1(config-view)# view REBOOTVIEW
R1(config-view)# exit
R1(config)#
O exemplo abaixo exibe as superviews configuradas na configuração running.

Para acessar exibições existentes, insira o comando enable view view-name no modo de usuário e insira a senha que foi atribuída à exibição personalizada. Use o mesmo comando para alternar de uma vista para outra.

R1# show running-config
<output omitted>
!
parser view SUPPORT superview
  secret 5 $1$Vp1O$BBB1N68Z2ekr/aLHledts.
  view SHOWVIEW
  view VERIFYVIEW
!
parser view USER superview
  secret 5 $1$E4k5$ukHyfYP7dHOC48N8pxm4s/
  view SHOWVIEW
!
parser view JR-ADMIN superview
  secret 5 $1$8kx2$rbAe/ji220OmQ1yw.568g0
  view SHOWVIEW
  view VERIFYVIEW
  view REBOOTVIEW
!


# 5.2.8 Verificar exibições de CLI baseadas em função
Para verificar uma exibição, use o comando enable view. Digite o nome da exibição a ser verificada e forneça a senha para fazer login na exibição. Use o comando ponto de interrogação (?) para verificar se os comandos disponíveis na exibição estão corretos.

O exemplo ativa a superview USER e lista os comandos disponíveis na exibição.

R1# enable view USER
Password: <cisco1>
 
R1# ?
Exec commands:
  <0-0>/<0-4>  Enter card slot/sublot number
  do-exec      Mode-independent "do-exec" prefix support
  enable       Turn on privileged commands
  exit         Exit from the EXEC
  show         Show running system information
 
R1# show ?    banner      Display banner information
  flash0:     display information about flash0: file system
  flash1:     display information about flash1: file system
  flash:      display information about flash: file system
  parser      Display parser information
  usbflash0:  display information about usbflash0: file system
O exemplo abaixo ativa a superview SUPPORT e lista os comandos disponíveis na exibição.

R1# enable view SUPPORT
Password: <cisco1>
 
R1# ?
Exec commands:
  <0-0>/<0-4>  Enter card slot/sublot number
  do-exec      Mode-independent "do-exec" prefix support
  enable       Turn on privileged commands
  exit         Exit from the EXEC
  ping         Send echo messages
  show         Show running system information
 
R1#
Este exemplo habilita a visualização JR-ADMIN e lista os comandos disponíveis na exibição.

R1# enable view JR-ADMIN
Password:  
 
R1# ?
Exec commands:
  <0-0>/<0-4>  Enter card slot/sublot number
  do-exec      Mode-independent "do-exec" prefix support
  enable       Turn on privileged commands
  exit         Exit from the EXEC
  ping         Send echo messages
  reload       Halt and perform a cold restart
  show         Show running system information
 
R1#
Ao não especificar uma exibição para o comando enable view, como mostrado aqui, você pode fazer login como root. A partir da visualização raiz, use o comando show parser view all para ver um resumo de todas as exibições. Observe como o asterisco identifica superviews.

R1# show parser view
Current view is 'JR-ADMIN'
 
R1# enable view
Password:  
 
R1# show parser view
Current view is 'root'
 
R1# show parser view all
Views/SuperViews Present in System:
  SHOWVIEW
  VERIFYVIEW
  REBOOTVIEW
  USER *  
 
  SUPPORT *
 
  JR-ADMIN *
 
-------(*) represent superview-------
R1#


