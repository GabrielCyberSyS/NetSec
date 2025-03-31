6.2.3 AutoSecure Cisco

Lançado na versão 12.3 do IOS, o Cisco AutoSecure é uma característica que seja iniciada do CLI e execute um script. AutoSecure primeiro faz recomendações para corrigir vulnerabilidades de segurança e, em seguida, modifica a configuração de segurança do roteador, como mostrado na figura.

O AutoSecure pode bloquear as funções do plano de gerenciamento e os serviços e funções do plano de encaminhamento de um roteador. Existem vários serviços e funções de plano de gestão:

Bootp seguro, CDP, FTP, TFTP, PAD, UDP e TCP pequenos servidores, MOP, ICMP (redirecionamentos, mask-replies), roteamento de fonte IP, Finger, criptografia de senha, keepalives TCP, ARP gratuito, proxy ARP e transmissão direcionada
Notificação legal usando um banner
Senha segura e funções de login
NTP seguro
Acesso seguro ao SSH
Serviços de interceptação TCP
Há três serviços e funções de plano de encaminhamento que o AutoSecure permite:

Cisco Express Forwarding (CEF)
Filtragem de tráfego com ACLs
Inspeção de firewall do Cisco IOS para protocolos comuns
AutoSecure é usado frequentemente no campo para fornecer uma política de segurança da linha de base em um roteador novo. Os recursos podem então ser alterados para dar suporte à política de segurança da organização.
~~~~
R1# auto secure  
  --- AutoSecure Configuration ---
 
*** AutoSecure configuration enhances the security
of the router but it will not make router
absolutely secure from all security attacks ***
 
All the configuration done as part of AutoSecure
will be shown here. For more details of why and
how this configuration is useful, and any possible
side effects, please refer to Cisco documentation of
AutoSecure.
 
At any prompt you may enter '?' for help.
Use ctrl-c to abort this session at any prompt.
 
Gathering information about the router for
AutoSecure
 
Is this router connected to internet? [no]:yes
~~~~
# 6.2.4 Usando o recurso Cisco AutoSecure
Use o comando auto secure permitir a instalação da característica AutoSecure Cisco. Esta configuração pode ser interativa ou não interativa. A figura mostra a sintaxe do comando para o comando auto secure.


    Router# auto secure {no-interact | full} [forwarding | management] [ntp | login | ssh | firewall | top-intercept]
Aqui estão os parâmetros de comando.

Nota: As opções podem variar de acordo com a plataforma.
~~~~
R1# auto secure ?
  forwarding   Secure Forwarding Plane
  management   Secure Management Plane
  no-interact  Non-interactive session of AutoSecure
  <cr>
 
R1#
~~~~
No modo interativo, o roteador solicita opções para habilitar e desativar serviços e outros recursos de segurança. Este é o modo padrão, mas também pode ser configurado usando o comando auto secure full.

O modo não interativo é configurado com o comando auto secure no-interact. Isto executará automaticamente a característica AutoSecure de Cisco com as configurações padrão recomendadas da Cisco. O comando auto secure também pode ser inserido com palavras-chave para configurar componentes específicos, como o plano de gerenciamento (management palavra-chave) e o plano de encaminhamento (forwarding palavra-chave).



![image](https://github.com/user-attachments/assets/b1e03663-079c-4f32-b9a1-fe3a455983a4)


# 6.3.1 Dynamic Routing Protocols
Protocolos de roteamento dinâmico são usados pelos roteadores para compartilhar automaticamente informações sobre a acessibilidade e o status das redes remotas. Os protocolos de roteamento dinâmico executam várias atividades, incluindo descoberta de rede e manutenção de tabelas de roteamento.

As vantagens importantes dos protocolos de roteamento dinâmico são a capacidade de selecionar um melhor caminho e a capacidade de descobrir automaticamente um novo caminho melhor quando houver uma alteração na topologia.

A avaliação da rede é a capacidade de um protocolo de roteamento de compartilhar informações sobre as redes que ele conhece com outros roteadores que também estão usando o mesmo protocolo de roteamento. Em vez de depender de rotas estáticas configuradas manualmente para redes remotas em cada roteador, um protocolo de roteamento dinâmico permite que os roteadores aprendam automaticamente sobre essas redes de outros roteadores. Essas redes, e o melhor caminho para cada uma, são adicionados à tabela de roteamento do roteador e identificadas como uma rede aprendida por um protocolo de roteamento dinâmico específico.

A figura mostra os roteadores R1 e R2 usando um protocolo de roteamento comum para compartilhar informações de rede.
![image](https://github.com/user-attachments/assets/5e7f31d0-8d46-43fb-a7aa-88ed15347ec3)


# 6.3.2 Spoofing de Protocolo de Roteamento
Os sistemas de roteamento podem ser atacados interrompendo roteadores de rede de pares ou falsificando ou falsificando as informações transportadas dentro dos protocolos de roteamento. O spoofing de informações de roteamento geralmente pode ser usado para fazer com que os sistemas se informem mal (mentem) uns para os outros, causem um ataque DoS ou façam com que o tráfego siga um caminho que normalmente não seguiria. Há várias consequências da informação de roteamento que está sendo falsificado:

Redirecionando o tráfego para criar loops de roteamento
Redirecionando o tráfego para que possa ser monitorado em um link inseguro
Redirecionando o tráfego para descartá-lo
Clique no botão Reproduzir na animação para ver um exemplo de um ataque que cria um loop de roteamento.

Suponha que um invasor tenha sido capaz de se conectar diretamente ao link entre R1e R2. O invasor envia informações de roteamento falso R1 indicando que R2 é o destino preferido para a rede 192.168.10.0/24. Embora o R1 já tenha uma entrada de tabela de roteamento à rede 192.168.10.0/24, a nova rota tem uma métrica mais baixa e, portanto, é a entrada preferida na tabela de roteamento.

Consequentemente, quando o PC3 envia um pacote ao PC1 (192.168.10.10/24), o R3 envia o pacote ao R2 que por sua vez o encaminha ao R1. O R1 não envia o pacote ao host PC1. Em vez disso, roteia o pacote para R2 porque o melhor caminho aparente para 192.168.10.0 /24 é através do R2. Quando o R2 obtém o pacote, olha em sua tabela de roteamento e encontra uma rota legítima à rede 192.168.10.0/24 através do R1 e para a frente o pacote de volta ao R1, criando o loop. O loop foi causado pela desinformação injetada no R1.

Para obter mais informações sobre ameaças genéricas aos protocolos de roteamento, clique aqui e consulte o RFC 4593. Mitigue contra ataques de protocolo de roteamento configurando a autenticação OSPF.

Os invasores podem manipular atualizações de roteamento não autenticado
A figura é uma animação que demonstra como um atacante pode criar um loop de roteamento.

![image](https://github.com/user-attachments/assets/23855359-ee5a-4e90-9537-7d07623e4afc)

# 6.3.3 Autenticação de Protocolo de Roteamento OSPF MD5
O OSPF apoia a autenticação do protocolo de roteamento usando o MD5. A autenticação MD5 pode ser permitida globalmente para todas as interfaces ou em uma base por interface.

Habilite a autenticação OSPF MD5 globalmente:

Comando de configuração de interface ip ospf message-digest-key key md5 password.
Comando de configuração de roteamento area area-id authentication message-digest .
Este método força a autenticação em todas as interfaces habilitadas para OSPF. Se uma interface não estiver configurada com o ip ospf message-digest-key , ele não poderá formar adjacências com outros vizinhos OSPF.
Habilite a autenticação MD5 em uma base por interface:

Comando de configuração de interface ip ospf message-digest-key key md5 password.
Comando de configuração de interface ip ospf authentication message-digest .
A configuração da interface substitui a configuração global. As senhas de autenticação MD5 não precisam ser as mesmas em uma área. No entanto, eles precisam ser os mesmos entre os vizinhos.

Nesta figura, o R1 e o R2 são configurados com OSPF e o roteamento está funcionando corretamente. Contudo, as mensagens OSPF não são autenticadas nem cifradas.

OSPF configurado sem autenticação
![image](https://github.com/user-attachments/assets/6853f925-bcc1-47cf-8a1f-7710deac4655)
Na figura abaixo, o R1 e o R2 são configurados com autenticação OSPF MD5. A autenticação é configurada por base de interface porque ambos os roteadores estão usando somente uma interface para formar adjacências OSPF. Observe que quando o R1 é configurado, a adjacência OSPF está perdida com R2 até que o R2 esteja configurado com a autenticação MD5 correspondente.

OSPF configurado com autenticação MD5

![image](https://github.com/user-attachments/assets/43707a10-90d8-4661-8284-55915093736b)

# 6.3.4 Autenticação do protocolo de roteamento OSPF SHA
MD5 agora é considerado vulnerável a ataques e só deve ser usado quando uma autenticação mais forte não está disponível. O Cisco IOS release 15.4 (1) T adicionou suporte para autenticação OSPF SHA, conforme detalhado no RFC 5709. Consequentemente, o administrador deve usar a autenticação SHA contanto que todos os sistemas operacionais do roteador apoiam a autenticação OSPF SHA.

A autenticação OSPF SHA inclui duas etapas principais. A sintaxe dos comandos é mostrada na figura:

Etapa 1. Especifique um chaveiro de autenticação no modo de configuração global:

Configure um nome de chaveiro com o comando key chain.
Atribua ao chaveiro um número e uma senha com os comandos key e key-string.
Especifique a autenticação SHA com o comando cryptographic-algorithm.
Especifique quando esta chave irá expirar com o comando send-lifetime (Opcional).
~~~~
Router(config)# key chain name

Router(config-keychain)# key key-id

Router(config-keychain-key)# key-string string

Router(config-keychain-key)# cryptographic-algorithm {hmac-sha-1 | hmac-sha-256 | hmac-sha-384 | hmac-sha-512 | md5} 

Router(config-keychain-key)# send-lifetime start-time {infinite | end-time | duration seconds}
Etapa 2. Atribua a chave de autenticação às relações desejadas com o comando ip ospf authentication key-chain.


Router(config)# interface type number 

Router(config-if)# ip ospf authentication key-chain name
~~~~
Na figura, o R1 e o R2 são configurados com autenticação OSPF SHA usando uma chave nomeada SHA256 e a cadeia de caracteres de chave OSPFSHA256. Observe que quando o R1 é configurado, a adjacência OSPF está perdida com R2 até que o R2 esteja configurado com a autenticação de harmonização SHA.

OSPF configurado com autenticação SHA
![image](https://github.com/user-attachments/assets/3bd8cb4c-798b-4de0-9a85-61c427c1d7d5)

To configure OSPF with SHA authentication, you must first configure a key chain:

Issue the key chain command to create a key chain named SHA256.
Assign the key chain number 1
Assign the key-string name of ospfSHA256.
Assign hmac-sha-256 as the cryptographic-algorithm.
Enter exit twice to exit key chain configuration.
R1(config)#key chain SHA256
R1(config-keychain)#key 1
R1(config-keychain-key)#key-string ospfSHA256
R1(config-keychain-key)#cryptographic-algorithm hmac-sha-256
R1(config-keychain-key)#exit
R1(config-keychain)#exit
Entre no modo de configuração da interface e atribua key-chain SHA256 para autenticação OSPF em S0/0/0.

R1(config)#interface S0/0/0
R1(config-if)#ip ospf authentication key-chain SHA256
R1(config-if)#  
*Mar 1 16:52:26.615: %OSPF-5-ADJCHG: Process 1, Nbr 10.2.2.2 on Serial0/0/0 from LOADING to FULL, Loading Done
Emita o comando end para sair do modo de configuração.

R1(config-if)#end
R1#
Você configurou com êxito a autenticação NTP em R1.

-------------------------------------------------------

# 6.4.1 Determinando o tipo de acesso de gerenciamento
Em uma rede pequena, gerenciar e monitorar um pequeno número de dispositivos de rede é uma operação direta. No entanto, em uma grande empresa com centenas de dispositivos, o monitoramento, o gerenciamento e o processamento de mensagens de log pode ser um desafio. Do ponto de vista do relatório, a maioria dos dispositivos de rede pode enviar dados de log que podem ser inestimáveis ao solucionar problemas de rede ou ameaças à segurança. Esses dados podem ser visualizados em tempo real, sob demanda e em relatórios agendados.

Ao registrar e gerenciar informações, o fluxo de informações entre hosts de gerenciamento e os dispositivos gerenciados pode seguir dois caminhos:

Em banda (In-band) - A informação flui através de uma rede de produção empresarial, Internet ou ambas, usando canais de dados regulares.
Fora de banda (Out-of-band OOB) - A informação flui em uma rede de gerenciamento dedicada na qual nenhum tráfego de produção reside.
Por exemplo, a rede na figura tem dois segmentos de rede separados por um roteador Cisco IOS que esteja fornecendo serviços de firewall para proteger a rede de gerenciamento. A conexão com a rede de produção permite que os anfitriões de gerenciamento acessem a Internet e forneçam tráfego limitado de gerenciamento em banda. O gerenciamento da Em-faixa ocorre somente quando o gerenciamento OOB não é possível ou disponível. Se o gerenciamento da em-faixa é exigido, então esse tráfego deve ser enviado com segurança usando um túnel cifrado privado ou um túnel VPN.

Gerenciamento In-Band
![image](https://github.com/user-attachments/assets/73bad4de-8ab8-4386-9fb0-31c7b630d0b0)


A figura abaixo mostra mais detalhes para a rede de gerenciamento protegida. É aqui que residem os hosts de gerenciamento e servidores de terminal. Quando colocados dentro da rede de gerenciamento, os servidores de terminal oferecem conexões de console direto OOB através da rede de gerenciamento para qualquer dispositivo de rede que exija gerenciamento na rede de produção. A maioria dos dispositivos deve ser conectada a este segmento de gerenciamento e ser configurada usando o gerenciamento OOB.

Como a rede de gerenciamento tem acesso administrativo a quase todas as áreas da rede, ela pode ser um alvo muito atraente para hackers. O módulo de gerenciamento no firewall incorpora várias tecnologias projetadas para mitigar tais riscos. A principal ameaça é um hacker tentando obter acesso à rede de gerenciamento. Isso pode ser realizado por meio de um host gerenciado comprometido que um dispositivo de gerenciamento deve acessar. Para mitigar a ameaça de um dispositivo comprometido, um forte controle de acesso deve ser implementado no firewall e em todos os outros dispositivos. Os dispositivos de gerenciamento devem ser configurados de forma que impeça a comunicação direta com outros hosts na mesma sub-rede de gerenciamento usando segmentos de LAN separados ou VLAN.

Gerenciamento out-of-band
![image](https://github.com/user-attachments/assets/c11e9d2b-87c0-4042-86c4-e968bd8e224f)


6.4.2 Acessos Out-of-Band e In-Band
Como regra geral, para fins de segurança, o gerenciamento OOB é apropriado para grandes redes empresariais. No entanto, nem sempre é desejável. A decisão de usar o gerenciamento OOB depende do tipo de aplicativos de gerenciamento em execução e dos protocolos que estão sendo monitorados. Por exemplo, considere uma situação em que dois switches principais são gerenciados e monitorados usando uma rede OOB. Se um link crítico entre esses dois switches principais falhar na rede de produção, o aplicativo que monitora esses dispositivos pode nunca determinar que o link falhou e nunca alertar o administrador. Isto é porque a rede OOB faz todos os dispositivos parecem ser unidos a uma única rede de gerenciamento OOB. A rede de gerenciamento OOB permanece não afetada pelo link abatido. Com aplicativos de gerenciamento como esses, é preferível executar o aplicativo de gerenciamento em banda de forma segura.

As diretrizes de gerenciamento OOB são:

Fornecer o mais alto nível de segurança.
Reduza o risco de passar protocolos de gerenciamento inseguros pela rede de produção.
O gerenciamento em banda é recomendado em redes menores como meio de alcançar uma implantação de segurança mais econômica. Em tais arquiteturas, o tráfego de gerenciamento flui em banda em todos os casos. Ele é feito o mais seguro possível usando protocolos de gerenciamento seguros, por exemplo usando SSH em vez de Telnet. Outra opção é criar túneis seguros, usando protocolos como IPsec, para o tráfego de gerenciamento. Se o acesso de gerenciamento não for necessário em todos os momentos, buracos temporários podem ser colocados em um firewall enquanto as funções de gerenciamento são executadas. Esta técnica deve ser usada com cautela, e todos os furos devem ser fechados imediatamente quando as funções de gerenciamento forem concluídas.

As diretrizes de gerenciamento em In-band são:

Aplicar apenas a dispositivos que precisam ser gerenciados ou monitorados.
Use IPsec, SSH ou SSL quando possível.
Decida se o canal de gerenciamento precisa estar aberto o tempo todo.
Finalmente, se estiver usando ferramentas de gerenciamento remoto com gerenciamento em banda, tenha cuidado com as vulnerabilidades de segurança subjacentes da própria ferramenta de gerenciamento. Por exemplo, os gerentes SNMP são usados frequentemente para facilitar o Troubleshooting e as tarefas de configuração em uma rede. Contudo, o SNMP deve ser tratado com o máximo cuidado porque o protocolo subjacente tem seu próprio conjunto de vulnerabilidades de segurança.
