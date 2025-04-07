7.1.1 Autenticação sem AAA
Os hackers de rede podem potencialmente obter acesso a equipamentos e serviços de rede confidenciais. O controle de acesso limita quem ou o quê pode usar recursos específicos. Também limita os serviços ou as opções disponíveis depois que o acesso é concedido. Muitos tipos de autenticação podem ser executados em um dispositivo Cisco e cada método oferece vários níveis de segurança.

O método mais simples de autenticação de acesso remoto é configurar uma combinação de login e senha no console, linhas vty e portas auxiliares, conforme mostrado na figura.
![image](https://github.com/user-attachments/assets/1c3ef723-7581-48c4-a6eb-73c32cf1a51f)

Este método é o mais fácil de implementar, mas também é o mais fraco e menos seguro. Este método não fornece responsabilização. Qualquer pessoa com a senha pode acessar o dispositivo e alterar a configuração.

SSH é uma forma mais segura de acesso remoto. Requer um nome de usuário e uma senha, ambos criptografados durante as transmissões. O método de banco de dados local fornece segurança adicional porque um invasor precisa saber um nome de usuário e uma senha. Ele também fornece mais responsabilidade porque o nome de usuário é gravado quando um usuário faz login. Embora o Telnet possa ser configurado usando um nome de usuário e uma senha, ambos são enviados em texto simples, o que o torna vulnerável a ser capturado e explorado. O método de banco de dados local tem algumas limitações. As contas de usuário devem ser configuradas localmente em cada dispositivo, segundo as indicações para a configuração do SSH na figura.
![image](https://github.com/user-attachments/assets/32ab0316-513f-4aa1-bac2-6d7131bf3ab5)
![image](https://github.com/user-attachments/assets/3e2bc24c-eecc-40c6-9fc2-b5d73afb6938)

Em um ambiente de grande empresa com vários roteadores e switches para gerenciar, pode levar algum tempo para implementar e alterar os bancos de dados locais em cada dispositivo. Além disso, a configuração do banco de dados local não oferece nenhum método de autenticação de fallback. Por exemplo, e se o administrador esquecer o nome de usuário e a senha para este dispositivo? Sem o método alternativo disponível para autenticação, a recuperação de senha é a única opção.

Uma solução melhor é fazer com que todos os dispositivos se refiram ao mesmo banco de dados de nomes de usuário e senhas de um servidor central. Este módulo explora os vários métodos de fixação do acesso à rede usando o AAA para proteger o Roteadores Cisco.


7.1.2 Componentes AAA
Os serviços de segurança de rede AAA fornecem a estrutura principal para configurar o controle de acesso em um dispositivo de rede. AAA é uma forma de controlar quem tem permissão para acessar uma rede (autenticação) e o que eles podem fazer enquanto estão lá (autorizar). O AAA também permite a auditoria das ações que os usuários executam ao acessar a rede (contabilidade).

A segurança AAA de rede e administrativa no ambiente Cisco tem três componentes funcionais:

Autenticação - Os usuários e administradores devem provar sua identidade antes de acessar os recursos da rede e da rede. A autenticação pode ser estabelecida usando combinações de nome de usuário e senha, perguntas e respostas de desafio, tokens e outros métodos. Por exemplo: “Eu sou usuário 'estudante' e eu sei a senha para provar isso.”
Autorização - Depois que o usuário é autenticado, os serviços de autorização determinam quais recursos o usuário pode acessar e quais operações o usuário tem permissão para realizar. Um exemplo é “O usuário‘ aluno ’pode acessar o host serverXYZ usando apenas SSH.”
Contabilidade e auditoria - A contabilidade registra o que o usuário faz, incluindo o que é acessado, a quantidade de tempo que o recurso é acessado e todas as alterações feitas. A contabilidade controla como os recursos da rede são usados. Um exemplo é o usuário aluno acessado host serverXYZ usando SSH por 15 minutos.



7.1.3 Modos de Autenticação
A autenticação AAA pode ser usada para autenticar usuários para o acesso administrativo ou pode ser usada para autenticar usuários para o acesso à rede remota. A Cisco oferece dois métodos comuns para implementar os serviços AAA:

Autenticação AAA local - O AAA local usa um banco de dados local para autenticação. Às vezes, esse método é conhecido como autenticação independente. Neste curso, será referido como autenticação AAA local. Este método armazena nomes de usuário e senhas localmente no roteador Cisco e os usuários são autenticados no banco de dados local, conforme mostrado na figura. Este base de dados é o mesmo que é exigido para estabelecer o CLI baseado em função. O AAA local é ideal para redes pequenas.
![image](https://github.com/user-attachments/assets/93d5d7d6-aae5-4f0a-8cb1-e68831f689d2)


Autenticação AAA baseada em servidor - Com o método baseado em servidor, o roteador acessa um servidor AAA central, tal como o Cisco Secure Access Control System (ACS) para Windows, que é mostrado na figura. O servidor AAA central contém os nomes de usuário e a senha de todos os usuários. O roteador usa os protocolos RADIUS (Remote Authentication Dial-In User Service) ou TACACS+ (Terminal Access Controller Access Control System) para se comunicar com o servidor AAA. Quando há roteadores e switches múltiplos, o AAA baseado no servidor é mais apropriado porque as contas podem ser administradas de um lugar central um pouco do que em dispositivos individuais.
Nota: Neste curso, o foco está na implementação de segurança de rede com IPv4 em roteadores Cisco, switches e dispositivos de segurança adaptáveis. Ocasionalmente, são feitas referências a tecnologias e protocolos específicos do IPv6.

Autenticação de AAA com base em servidor
![image](https://github.com/user-attachments/assets/791915c9-5a10-493e-9c2f-a109d1877b6f)

7.1.4 Autorização
Depois que os usuários são autenticados com sucesso contra a fonte de dados AAA selecionada, local ou com base no servidor, eles são autorizados então para recursos de rede específicos, segundo as indicações da figura.

Autorização AAA

![image](https://github.com/user-attachments/assets/59e22b09-cb01-4765-91d5-5fd10012b0f0)

A autorização controla o que os usuários podem e não podem fazer na rede depois de serem autenticados. Isto é similar a como os níveis de privilégio e o CLI baseado em função dão aos usuários direitos e privilégios específicos a determinados comandos no roteador.

A autorização é executada tipicamente usando um servidor AAA. A autorização usa um conjunto de atributos que descrevem o acesso do usuário à rede. Esses atributos são comparados às informações contidas no banco de dados AAA e uma determinação das restrições para esse usuário é feita e entregue ao roteador local onde o usuário está conectado.

A autorização é automática e não exige que os usuários executem etapas adicionais após a autenticação. A autorização é implementada imediatamente após a autenticação do usuário.


7.1.5 Contabilidade
A contabilidade AAA coleta e relata dados de uso. Esses dados podem ser usados para propósitos como auditoria ou faturamento. Os dados coletados podem incluir os horários de início e término da conexão, os comandos executados, o número de pacotes e o número de bytes.

A contabilidade é executada usando um servidor AAA. Este serviço relata estatísticas de uso de volta ao servidor ACS. Estas estatísticas podem ser extraídas para criar relatórios detalhados sobre a configuração da rede.

Um uso amplamente difundido da contabilidade é combiná-lo com a autenticação AAA. Isso ajuda a gerenciar o acesso a dispositivos de interrede pela equipe administrativa da rede. Contabilidade fornece mais segurança do que apenas autenticação. Os servidores AAA mantêm um registro detalhado de exatamente o que o usuário autenticado faz no dispositivo, conforme mostrado na figura. Isso inclui todos os comandos EXEC e de configuração emitidos pelo usuário. O log contém vários campos de dados, incluindo o nome de usuário, a data e a hora, e o comando real que foi inserido pelo usuário. Esta informação é útil na solução de problemas de dispositivos. Ele também fornece vantagem contra indivíduos que realizam ações maliciosas.

Contabilidade AAA
![image](https://github.com/user-attachments/assets/65145d44-77b9-4174-be74-7249284d9d02)



7.3.4 Autenticação TACACS+
O TACACS+ é um aprimoramento da Cisco ao protocolo TACACS original. Apesar de seu nome, TACACS+ é um protocolo inteiramente novo que seja incompatível com toda a versão anterior do TACACS. O TACACS+ é apoiado pela família Cisco de roteadores e servidores de acesso.

O TACACS+ fornece serviços separados do AAA. Separar os serviços AAA fornece a flexibilidade na implementação porque é possível usar o TACACS+ para a autorização e a contabilidade ao usar um outro método de autenticação.

As extensões ao protocolo TACACS+ fornecem mais tipos de pedidos de autenticação e de códigos de resposta do que estavam na especificação original TACACS. O TACACS+ oferece suporte a vários protocolos, como o IP e o AppleTalk legado. A operação normal do TACACS+ criptografa todo o corpo do pacote para comunicações mais seguras e utiliza a porta TCP 49.

Clique em Reproduzir na figura para visualizar o processo de autenticação TACACS+.

Processo para a autenticação TACACS+
![image](https://github.com/user-attachments/assets/8b7dfb73-2dcb-4e21-98db-9ef73c50be2b)



7.3.5 Autenticação RADIUS
RADIUS, que foi desenvolvido pela Livingston Enterprises, é um protocolo AAA padrão IETF aberto para aplicações como acesso à rede ou mobilidade IP. O RADIUS funciona em situações locais e em roaming e é comumente usado para fins contábeis. RADIUS é definido atualmente pelas RFCs 2865, 2866, 2867, 2868, 3162 e 6911.

O protocolo RADIUS oculta senhas durante a transmissão, mesmo com o Protocolo de Autenticação de Senha (PAP), usando uma operação bastante complexa que envolva o hashing do Message Digest 5 (MD5) e um segredo compartilhado. Contudo, o resto do pacote é enviado em texto simples.

O RADIUS combina autenticação e autorização como um processo. Quando um usuário é autenticado, esse usuário também é autorizado. O RADIUS usa a porta UDP 1645 ou 1812 para autenticação e a porta UDP 1646 ou 1813 para contabilidade.

RADIUS é amplamente utilizado por provedores de serviços VoIP. Ele passa as credenciais de login de um terminal SIP, como um telefone de banda larga, para um registrador SIP usando autenticação digest e, em seguida, para um servidor RADIUS usando RADIUS. O RADIUS também é um protocolo de autenticação comum que seja utilizado pelo padrão de segurança 802.1X.

Clique em Reproduzir na figura para visualizar um processo de autenticação RADIUS.

Processo para autenticação RADIUS


![image](https://github.com/user-attachments/assets/9299fd55-9e77-401f-b28a-8a584eeb920f)


7.4.2 Configurar servidores TACACS+
Os protocolos TACACS e RADIUS são usados para a comunicação entre os clientes e os servidores de segurança AAA. A figura exibe a topologia de referência AAA para este tópico.

Topologia de referência AAA baseada em servidor

![image](https://github.com/user-attachments/assets/4553e4dc-3517-4c89-ac81-29401a985f8c)


Para configurar um server TACACS+, permita globalmente o AAA usando comando aaa new-model. Em seguida, use o comando tacacs server name. No modo de configuração do servidor TACACS+, configurar o endereço IPv4 do servidor TACACS+ usando comando address ipv4. O comando address ipv4 permite a opção modificar a porta de autenticação e a porta de contabilidade.x Você também pode especificar um endereço IPv6 com o comando address ipv6 ipv6-address.

Em seguida, use o comando single-connection para melhorar o desempenho do TCP mantendo uma única conexão TCP durante a vida útil da sessão. Caso contrário, por padrão, uma conexão TCP é aberta e fechada para cada sessão. Se necessário, os servidores TACACS+ múltiplos podem ser identificados incorporando seus endereços IPv4 respectivos usando comando tacacs server name .

O comando key key é usado para configurar a chave secreta compartilhada para cifrar a transferência de dados entre o servidor TACACS+ e o roteador habilitado para AAA. Esta chave deve ser configurada exatamente da mesma maneira no roteador e no server TACACS+.

O exemplo indica uma configuração do servidor TACACS+ da amostra.

R1(config)# aaa new-model
R1(config)#
R1(config)# tacacs server Server-T
R1(config-server-tacacs)# address ipv4 192.168.1.101
R1(config-server-tacacs)# single-connection
R1(config-server-tacacs)# key TACACS-Pa55w0rd
R1(config-server-tacacs)# exit
R1(config)#
7.4.3 Configurar servidores RADIUS
Para configurar um servidor RADIUS, use o comando radius server name. Isso coloca você no modo de configuração do servidor Radius.

Como o RADIUS usa UDP, não há equivalente a palavra-chave single-connection. Se necessário, vários servidores RADIUS podem ser identificados inserindo um comando radius server name para cada servidor.

No modo de configuração do servidor Radius, configurar o endereço IPv4 do servidor Radius usando comando address ipv4 ipv4-address . Você também pode especificar um endereço IPv6 com o comando address ipv6 ipv6-address.

Por padrão, os roteadores Cisco usam a porta 1645 para autenticação e a porta 1646 para a contabilidade. Contudo, a IANA reservou as portas 1812 para a porta de autenticação RADIUS e 1813 para a porta de contabilidade RADIUS. É importante certificar-se de que estas portas combinam entre o roteador Cisco e o servidor Radius.

Para configurar a chave secreta compartilhada para criptografar a senha, use o comando key. Esta chave deve ser configurada exatamente da mesma maneira no roteador e no servidor Radius.

O exemplo indica uma configuração do servidor RADIUS de exemplo.

R1(config)# aaa new-model
R1(config)#
R1(config)# radius server SERVER-R
R1(config-radius-server)# address ipv4 192.168.1.100 auth-port 1812 acct-port 1813
R1(config-radius-server)# key RADIUS-Pa55w0rd
R1(config-radius-server)# exit
R1(config)#




7.4.4 Autenticar para os comandos de configuração do servidor AAA
Quando os servidores de segurança AAA tiverem sido identificados, os server devem ser incluídos na lista do método do comando aaa authentication login. Os servidores AAA são identificados usando as palavras-chave group tacacs+ ou group radius. Consulte o exemplo para ver as opções de sintaxe de comando disponíveis com o comando aaa authentication login.

R1(config)# aaa authentication login default ?
  cache          Use Cached-group
  enable         Use enable password for authentication.
  group          Use Server-group
  krb5           Use Kerberos 5 authentication.
  krb5-telnet    Allow logins only if already authenticated via Kerberos V
                 Telnet.
  line           Use line password for authentication.
  local          Use local username authentication.
  local-case     Use case-sensitive local username authentication.
  none           NO authentication.
  passwd-expiry  enable the login list to provide password aging support
 
R1(config)# aaa authentication login default group ?
  WORD     Server-group name
  ldap     Use list of all LDAP hosts.
  radius   Use list of all Radius hosts.
  tacacs+  Use list of all Tacacs+ hosts.
Para configurar uma lista de métodos para que o início de uma sessão padrão autentique primeiramente usando um servidor TACACS+, segundo com um servidor RADIUS, e finalmente com um base de dados de nome de usuário local, especifique a ordem com comando aaa authentication login default, conforme destacado no exemplo. É importante perceber que R1 só tentará autenticar usando RADIUS se o servidor TACACS+ não estiver acessível. Da mesma forma, o R1 tentaria somente autenticar usando o base de dados local se os servidores TACACS+ e RADIUS estão indisponíveis

R1(config)# aaa new-model
R1(config)#
R1(config)# tacacs server Server-T
R1(config-server-tacacs)# address ipv4 192.168.1.100
R1(config-server-tacacs)# single-connection
R1(config-server-tacacs)# key TACACS-Pa55w0rd
R1(config-server-tacacs)# exit
R1(config)#
R1(config)# radius server SERVER-R
R1(config-radius-server)# address ipv4 192.168.1.101 auth-port 1812 acct-port 1813
R1(config-radius-server)# key RADIUS-Pa55w0rd
R1(config-radius-server)# exit
R1(config)#
R1(config)# aaa authentication login default group tacacs+ group radius local-case














