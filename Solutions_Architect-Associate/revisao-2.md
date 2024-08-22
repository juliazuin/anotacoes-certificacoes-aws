### API Gateway
Mapping templates são trechos de código na linguagem VTL (Velocity Template Language) que podem traduzir requisições do backend para o frontendou vice e versa.
Pode ser uma solução se o payload aceitável no backend tenha mudado numa nova versão, sendo assim clientes utilizando a versão antiga não terão que atualizar às pressas e continuarão utilizar os serviços sem impacto.

### EventBridge
Aplicações orientadas a eventos

#### Pipes
Ajuda a criar integrações entre eventos `producers` e `consumers`, pode filtrar transformar e enriquecer o evento.
Casos de uso:
* Alterações no DynamoDb podem ser integradas com o eventbridge pipes para serem dissipadas em múltiplas opções de aṕlicações na AWS.
* É possível mover dados de um kafka hospedado no on-premisses para kinesis data, por exemplo.
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
#### Glacier 
Vault Lock Policies - O s3 glacier pode ter uma vault access policy baseada em recursos e uma vault *lock* policy anexada à ela.
Uma vault *lock* policy é uma política de acesso a qual é possível "trancar", utilizando essa policy é possível reforçar politicas de compliance e regulatórias.
exemplo: É necessário reter arquivos durante um ano antes que possam ser deletados, com a essa policy é possível negar a deleção desses arquivos ate que tenham existido por um ano. É possível testar a policy antes de aplicar o "lock, pois depois ela se torna imutável 


#### EBS
* dados são armazenados em volumes e blocos, onde os arquivos são dividos em blocos de tamanhos iguais, então quando são armazenados blocos de dados muito grandes os arquivos dão divididos em "pedaços" menores ou um tamanho fixado
Cada bloco tem seu próprio endereço, mas não tem nenhuma metadado.

Esse armazenamento pode ler ou escrever informação a nível de bloco, isso habilita maior performance de I/O


É possível utilizar o EBS em modo "multi-attach" que permite acoplar um único volume IOPS SSD em várias instancias ec2



#### s3
* dados são armazenados como objetos, o s3 não sabe nada sobre o dado que está armazenando, única informação relevante os os metadados, são sets de chave/valor que descrevem o objeto e um identificador.
dassa forma é possível encontrar osdados sem saber onde estão fisicamente.
* não existe hierarquia entre aquivos e objetos
*deve* ser usado quando se precisade um armazenamento de alta disponibilidade, altamente durável, exemplo: armazenamento de imagens, audio, vídeo, aquivamento de grandes arquivos, etc

*não deve* ser usado para instalação de sistemas operacionais ou ser utilizado como volume onde é necessário uma alta performance de I/O (input/outout)

Frequência de acesso determina o tipo do bucket, como os seguintes:
s3 standard - default acesso + frequente.

s3 standard infrequent Access - menor frequencia de acesso oferece menor preço comparado ao default, ideal para os tipos de dados que necessitam acesso sem uma regra de latência tão rigorosa.

s3 one zone infrequent access - guarda os dados numa unica AZ, que reduz muito as custos comparados ao standard IA (infrequent access), ideal para dados que podem ser facilmente recriados, ou *dados que não sejam criticos*

s3 intelligent tiering  - move automaticamente os arquivos de acesso frequente para o "não-frequente", ideal para dados que não tem uma frequencia de acesso definida ou que podem mudar.

Glacier - tem um custo extremamente baixo, com os tempos de recuperação do dados entre min e horas, é ideal onde o acesso aos dados não é frequênte e quando o tempo de recuperação não é crítico.
 Glacier instant retrieval - entrega o menor custo de armazenamento para dados de longo termo que são raramente acessados mas precisam de recuperação em milisegundos.

 Glacier flexible retrival - ideal para dados que são acessados 1 ou 2 vezes no ano e podem ser recuperados de forma assíncrona.

 Glacier deep archive - é o tipo de s3 de menor custo que suporta reteção de longa data e preservação digital dos dados, podem ser acessados 1 ou 2 vezespor ano, ate mais raramente.

 s3 outposts - entrega armazenamento de objetos no ambiente on-premisses, utiliza de apis do s3 para ter uma única classe de s3.

Lifecycle dos dados nos s3

Outras features dos buckets s3:
- Bucket policy - policies de recurso, utilizadas para politicas de acesso a nível de bucket
- Bucket acl (access control list) - pode ser aplicada a nível de bucket ou de objeto dentro do bucket específico

- IAM
- Criptografia - dados em transito ou em repouso
- MFA
- Versionamento
- Replicação de dados - habilidade de replicar dados entre buckets, normalmente recursos em regiões diferentes, também conhecida como "CRR - cross region replication", também é possível ter replicação de dados entre a mesma região como fonte e destino, mais conhecido como "SRR - same region replication"
Replicação acontece de forma assíncrona.


### Kinesis
Consegue processar e analisar dados de streaming (Dados de streaming são dados que são gerados continuamente por diferentes fontes. Esses dados devem ser processados ​​de forma incremental usando técnicas de processamento de fluxo sem ter acesso a todos os dados.).
Com ele é possível ingerir dados em tempo real como video, áudio, logs de aplicação, dados de 
 IoT, etc.

Data firehose: utilizado para fazer ETL de vários recursos da AWS e armazenar num s3 ou redshift.. mas não captura dados de streaming de vídeos como câmeras de sgurança.

Data analytics: utilizado para agrupar dados de streaming que vem do kinesis data streams, s3, aparelhos IoT. Analisa o dado com queries e manda o output para outros serviços da AWS ou ferramentas analiticas. Esse output que pode ser utilizado para criação de alertas ou respostas em tempo real.

Vídeo streams: criado expecificamente para casos de uso como playback de vídeo, detecção de rostos, monitorações de segurança.

Data streams: útil para capturar vários GBs de dado em tempo real de vários recursos, esse dado pode ser utilizado posteriormente pelo data analytcs, amazon EMR.

### Route53
- Alias records:
Um registro de alias só pode redirecionar queris para recursos selecionados da AWS, s3, cloudfront, outro registro na mesma `hosted zone` do route53
exemplo: é possível criar um registro tipo alias chamado "acme.example.com" que redireciona as chamadas para um bucket s3. Também é possível criar esse mesmo alias que redireciona as chamadas para "zenith.example.com" dentro da hosted zone example.com

- CNAME records: 
Pode redirecionar qualquer chamada DNS para qualquer registro de DNS.
exemplo: é possível criar um registro CNAME que redireciona chamadas de "acme.example.com" pra "zenith.example.com" ou "acme.example.org", não é necessário que o outro registro de DNS esteha dentro do route53

most common records supported in Route 53 are:
● A: hostname to IPv4
● AAAA: hostname to IPv6
● CNAME: hostname to hostname
● Alias: hostname to AWS resource


### Autenticação e permissão
IAM policies são mais comuns e abrangentes, são usadas para gerenciar acesso a maioria dos serviços da AWS, controlam acessos de usuário, grupos ou roles podem executar mvarios serviços.
São centralizadas e aplicáveis em nível de identidade.

Resource based policies, alguns recursos permitem o uso desse tipo de política, como:

S3
SQS
SNS
KMS
Lambda  
S3 Glacier
API Gateway
CloudWatch logs