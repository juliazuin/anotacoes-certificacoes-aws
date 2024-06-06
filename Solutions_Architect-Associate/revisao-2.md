### API Gateway
Mapping templates são trechos de código na linguagem VTL (Velocity Template Language) que podem traduzir requisições do backend para o frontendou vice e versa.
Pode ser uma solução se o payload aceitável no backend tenha mudado numa nova versão, sendo assim clientes utilizando a versão antiga não terão que atualizar às pressas e continuarão utilizar os serviços sem impacto.

### EventBridge
Aplicações orientadas a eventos

#### Pipes
Ajuda a criar integrações entre eventos `producers` e `consumers`, pode filtrar transformar e enriquecer o evento.
Casos de uso:
* Alterações no DynamoDb podem ser integradas com o eventbridge pipes para serem dissipadas em múltiplas opções de aṕlicações na AWS.
* É possível mover dados de um kafka hospedado no on-premisses para kineses data, por exemplo.
* É possível complementar os dados enviados para um sofware realizando chamadas de API no "meio do caminho" para enriquecer os dados.

#### Scheduler
É possível agendar eventos, configurar padrões, definir politicas de re-tentativa

### Resouce Access Manager (RAM) 
Pode ser usado para compartilhar recursos entre contas AWS


### EFS - DataSync
Transfere dados online entre serviços, simplifica, automatiza e acelera movimentação e replicação dos dados entre sistemas de armazenamento on-premisses e também entre serviços de armazenamento da AWS
Pode copar dados entre Network File Systems (NFS), Server Message Block (SMB) file servers, self-managed object storage, AWS Snowcone, Amazon S3 buckets, Amazon EFS file systems, and FSx for Windows File Server file systems.
Também pode ser utilizado para transferência de dados entre EFS de regiões diferentes e EFS de contas diferentes.

### Cloud Formation
Custom Resources ->
Permitem escrever uma lógicad e provisionament customizada nos templates do cloud formation. São utilizadas para realizar chamada a outros serviços, como lambda, para executar uma função da stack.

Pode ser utilizado com o SNS no caso onde é necessário inclusão de novos recursos em uma stack existente ou injetar recursos dinâmicos na stack


### EC2
*Hibernate* ->
Pode ser útil quando é neessário reduzir o tempo de que uma instância nova é criada, por exemplo, devido a um problema de estouro de memóriau uma instância EC2 para de receber requisições, para subir uma instância em tempo hábil pode-se colocar uma instância backup em estado do hibernação, assim quando há impacto é possivel iniciá-la e o tempo é reduzido.
Instâncias em hibernação tem seu estado salvo num volume EBS que fica em stand-by enquanto isso, ao solicitar que de inicie novamente o volume se anexa à instância novamente.
Importante: é necessário habilitar o modo de hibernação no ato de criação das instâncias

### Network
Transit Gateway - conecta VPCs (virtual private clouds) e redes on-premisses por um hub central.


*VPN*
Roteamento ECMP -  estratégia utilizada em redes para distribuir tráfego entre vários caminhos de mesmo custo (têm a mesma métrica ou distância de roteamento) simultâneamente. Benefícios: melhora performanceda rede, aumenta a largura de banda (bandwidth) e aumenta a tolerância a falhas.
Na AWS pode ser utilizado para criação de multiplas conexões VPN site-to-site,aumentando assm a larguda de banda da conexão entre o ambiente on-premissese a AWS.


*IPs* 
IPs não roteaveis são IPs deinifidos na RFC1918 que são determinados para conexões privadas, são eles:

* 10.0.0.0 – 10.255.255.255 (10.0.0.0/8)
* 172.16.0.0 – 172.31.255.255 (172.16.0.0/12)
* 192.168.0.0 – 192.168.255.255 (192.168.0.0/16)


### NAT Gateway
Publica - pode ser utilizada para prover conectividade entre recursos AWS numa subnet privada e a internet ou redes externas. Precisam necessáriamente ser posicionadas numa subnet pública e um elastic IP precisa ser associado à ela.

Privada - Pode ser usada para prover conectividade com recursos on-premisses ou outras VPCs (com subnets privadas). o Elastic IP não precisa ser associado a um NAT gateway privado


### Storage
Glacier - Vault Lock Policies - O s3 glacier pode ter uma vault access policy baseada em recursos e uma vault *lock* policy anexada à ela.
Uma vault *lock* policy é uma política de acesso a qual é possível "trancar", utilizando essa policy é possível reforçar politicas de compliance e regulatórias.
exemplo: É necessário reter arquivos durante um ano antes que possam ser deletados, com a essa policy é possível negar a deleção desses arquivos ate que tenham existido por um ano. É possível testar a policy antes de aplicar o "lock, pois depois ela se torna imutável 




