## EC2
### Termination policy in EC2 instances:

Default – Validação quantas instâncias EC2 existem por AZ e pelo menos uma que não tenha proteção de scale-in.

AllocationStrategy – Terminate de uma instância alinhado com as demais instâncias ativas
Útil quando a preferencia de tipo de instância mudou, por exemplo, se a estratégia de spot são otimizadas para custos, é possível rebalançear a distribuição de instâncias Spot por todos os N pools de opções Spot de baixo custo.

OldestLaunchTemplate – Terminate a instância que tem o lauch **template** mais antigo, aquelas que não estao usando o atual.
Útil quando estamos atualizando um grupo e descontinuando instâncias de uma configuração antiga.

OldestLaunchConfiguration – Terminate a instância que tem o lauch **configuration** mais antigo.

ClosestToNextInstanceHour – Terminate a instância que está mais proxima do horario de cobrança. Ajuda a maximizar o uso de instâncias que tem cobrança de hora-em-hora.

NewestInstance – Terminate a instância mais recente do grupo. Útil quando uma lauch configuração está sendo testada porém não é necessário mantê-la em produção.

OldestInstance – Termina a instância mais antiga do grupo. Útil quando estamos atualizando as instâncias de um grupo para um novo tipo de EC2.2 instance type. You can gradually replace instances of the old type with instances of the new type.

### ENI
Junção de várias ENIs a uma instância é útil nos casos:
- Para criar uma rede gerenciada
- Utilização de ferramentas de rede esegurança na VPC
- Criação de instâncias com "duas casas" (duas interfaces de rede) contendo workfloads e roles em subnets distintas
- Criação de uma solução de alta disponibilidade e baixo orçamento

### Enhanced networking
Elastic Network Adapter (ENA)
Tem suporte para aumentar em ate 100gbps nas instâncias compatíveis.
Utiliza virtualização de I/O(SR-IOV) para fornecer recursos de rede de alto desempenho em instâncias compatíveis. 

SR-IOV é um métido de virtualização de aparelhos que provê baixo uso de CPU, quando comparado com virtualizadores tradicionais, e um alto desempenho para I/O.

Tipos de instância suportadas: 
H1, G3, m4.16xlarge, P2, P3, P3dn, and R4.


### Banco de dados:
Aurora é um banco de dados relacional tem total compatibilidade para MySQL e PostgreSQL.
É um banco de dados serverless então possuí um mecanismo de auto-scalling, sendo compatível com múltiplas AZs e cross region (é possível manter dados em múltiplas regiões pra disaster recovery).


### Network
AWS WAF - Ajuda a proteger aplicações WEB contra exploração web e bots mais comuns que podem afetar a disponibilidade do sistema, comprometer a segurança ou consumir recursos excessivos.
Interage com Amazon CloudFront, Application Load Balancer, Amazon API Gateway, AWS AppSync 


top 3 rate-based rules:
* A blanket rate-based rule é usada para proteger a aplicação de múltiplas requisições HTTP.
Exemplo: com um threshold de 2000 requisições/5min a regra vai bloquear todos IPs que estão realizando mais chamadas que isso dentro do período. Tipo de regra mais básico.

* Uma rate-based rule também podeser usada para proteger URIs específicas, de uma forma mais restrita que a regra de "blanket".
Exemplo: múltiplas requisições dentro de um período de 5min em uma página de login é suspeito e pode indicar uso potencial de força bruta ou preenchimento de credenciais contra à aplicação.

*Uma rate-based rule que protege a aplicação de IPs conhecidos como maliciosos.


https://aws.amazon.com/blogs/security/three-most-important-aws-waf-rate-based-rules/


### Route 53
É possível configurar health check do route 53 para verificar configurações ativo-ativo e ativo-passivo.

### Application Discovery Service
Coleta e apresenta a data para que seja possivel entender a configuração, uso e comportamento dos servidores no ambiente on-premisses. Dados obtidos são retidos no Application Disacovery Service e então pode ser taggeado e agrupado para facilitar um plano de migração para a cloud.analysis tools.

Agent-based discovery -> é utilizado para coletar dados de hosts que não sejam do tipo VMWare. Podendo ser, windows, linux, ubuntu, centos e suse OS.

Agentless discovery -> é utilizado somente para coletar dados de hosts que **sejam** do tipo VMWare


## Security
### GuardDuty
protege contra o uso de crendenciais comprometidas ou acesso não usual no s3, mas não escaneia o conteúdo do bucket s3


### Detective
analisa e visualiza informações de segurança dos serviços como o Guard Duty, Macie e Security Hub para resgatar a causa raiz de um potencial problema de segurança.


### Macie
utiliza machine learning e correspondência de padrões para descobrir, monitorar e proteger dados sensíveis nos Buckets s3
Consegue identificar buckets que têm excesso de permissões ou estão descriptografados.


### Inspector
Avalia vulnerabilidade de software ou exposições de rede nos EC2.
Teste acessibilidade de instâncias EC2 e estado de segurança de aplicações que rodam nessas instâncias;
Avalia aplicações em exposição, vulnerabilidades e desvios de boas práticas.
Faz um relatório de assessment com uma lista detalhada organizada por nível de seridade.

Também pode automatizar achados de vulnerabilidade em pipelines de ci/cd

Para uma avalição de "host"
Deve ter um agent instalado nas instâncias ec2 que monitora o comportamento dela incluindo monitorações de redes, file system e atividade de processos coletando uma grande quantidade de dados de configuração e comportamento (telemetry).

para avaliação de redes, o agent não é necessário.

### Shield
Gerencia a proteção de Distributed definal of service (DDoS), provendo detecção dinâmica e mitigações automáticas que minimizam tempo de inatividade e latência da aplicação.
-> Standard: defende as aplicações contra os ataques DDoS mais comuns na camada 3 e 4 (redes e transporte)
-> Advanced: EC2, ELB, Cloud Front, Global accelerator e Route53. Possuí integração com o AWS WAF

### WAF
servicço Web application firewall monitora requisições direcionadas ao API gateway/cloudfront/ALB, é possível proteger esses recursos baseados em condições específicas como endereço de IP que a requisição veio.

### AWS Transfer family
Oferecesuporte para transiferência de arquivos com SFTP, AS2, FTPS e FTP diretamente para o s3 ou EFS.

SFTP - Secure Shell (SSH) file transfer protocol - protocolo de rede q é usado para transferẽncia segura de dados pela internet.

AS2 - Applicabilitty Statement 2 - protocolo de redes utilizado para transferência de dados de business-to-business(B2B) num rede de internet publica com HTTP ou HTTPS (o qualquer TCP/IP protocolo)

FTP - File transfer protocol - protocolo de redes utilizado para transferência de dados, utiliza um canal separado para transferências de "control" e "data", sendo "control" aberto até terminar ou até a inatividade (timeout) e o canal "data" fica ativo durante a transferência do dados. FTP não suporta criptografia dos dados.

FTPS - File Transfer Protocol over SSL - é uma extensão do FTP, como ele utiliza um canal separado para transferências de "control" e "data", utiliza TLS (Transport layer security) para criptografia do tráfego sendo utilizada em ambos canais "control" e "data".


### Outposts
Permite estender e operar serviços nativos da AWS no ambiente on-premisses ou qualquer serviço de borda (edge).
É possível rodar alguns serviços da aws localmente e integrar com outros vários serviços disponíveis na região.

Outposts é compatível com o ECS sendo ideal para workloads que necessitem de baixa latência e que necessitem proximidade com dados e/ou aplicações do ambiente on-premisses.
ECS tipo Fargate não está disponpível com Outposts.


### Bucket s3
Para configurar um bucket com os arquivos estáticos de um website, esse bucket s3 deve ser público e deve-se criar uma policy com acessos de leitura. Se esse bucket contém objetos que não são do mesmo dono do bucket, por exemplo, uma outra conta AWS "pai" que também lê desse bucket s3, também deve-se criar uma ACL(access control list) para garantir o acesso.

>ps. Se não quiser colocar o bucket s3 como público é possível criar um cloudfront para realizar a entrega do conteúdo estático.

Presigned URL ->
Buckets s3 podem ter URLs pré assinadas, que podem ajudar a dar acesso a um objeto específico de dentro do bucket. Essas URLs devem ter um período de expiração.


### Polly:
Transforma texto em uma fala, premitindo criar aplicaçoes que falam, por exemplo, bots de telemarketing.


### Rekognition:
Utiliza machine learning para identificar objetos, pessoas, textos, cenários e atividade em imagens e vídeos, etc.
Também provê uma analise facial muito aguçada a qual é possível ser utilizada para detectar, analizar e comparar faces em vários casos de uso.


### Textract: 
Utiliza machine learning para extrair textos impressos, textos escritos à mão e outros tipos de dados de documentos escaneados.


### Comprehend: 
Entrega insights de textos, por exemplo, extrai textos, frases, tópicos e sentimentos pré configurados.

### Kinesis
É possível realizar a ingestão de dados em tempo real, como vídeo audio, logs de aplicação, cliques de websites, dados de telemetria com IoT...


### Lex
Chatbot facilitado, é uma inteligência artificial para criar uma interface de conversa numa aplicação.


### Personalize 
Integra recomendações personalizadas para cada usuário em websites, aplicações, email, sistemas de marketing, etc.


### Timestream
É um banco de dados que utiliza dados tipo `time-series`, que são bancos de dados baseados em tempo.


### Analytics
#### QuickSightanaly
Dashboard like, serviço para business analytics.
É possível conectar os seguintes serviços ao QuickSight:
RDS, aurora, redshift, athena e s3.
É possível também inputar arquivos de excel, conectar a bancos de dados on-premisses (SQL server, MySQL e PostGreSQL), importar dados de um SaaS (tipo salesforce)


#### Lake Formation
Centraliza a governança, segurança e compartilhamento global dos dados para análise e machine learning.


### Metrics
CloudWatch agent:
Coleta informações de métricas em nível de sistema nas instâncias EC2 e on-premisses entre sistemas operacionais, as métricas coletadas pelo agent são consideradas metricas customizadas e são faturadas dessa forma.


### s3 <- CloudFront
> mais recomendado OAC
Origin Access control (OAC) - suporta criptografia server side com KMS e requisições dinamicas. 
Origin Access Identity (OAI)


### ECS
Orqustração de containers 100% totalmente gerenciado.
3 camadas:
1. Capacity- a infra que roda os containers
1.1 Instâncias EC2
1.2 Serverless com Fargate
1.3 Máquinas virtuais ou servidores On-premisses  
2. Controller - deploy e gerencia as aplicações que rodam nos containers
3. Provisionamento - ferramentas que podem ser utilizadas para fazer interface com o "agendador" para gerenciar as aplicações e os containers.
3.1 Aws console
3.2 AWS CLI
3.3 SDKs
3.4 copilot

Network modes no ECS:
Host mode: a rede do container está amarrada diretamente com o host "pai" que está rodando o container.

Bridge mode: utiliza uma ponte de rede virtual que cria uma camada entre o host e a rede do container. 

AWSVPC mode: cria e gerencia uma Elastic Network Interface (ENI) para cada task e cada uma dessas tasks recebem seu próprio IP privado dentro da VPC


### Cloud Formation
IaC - infraestructure as a code
Seções de um template de stack cloudformation:
- Format version - contém a data da versão
- Description - descrição da stack em uma frase
- Metadata - contém dados adicionas sobre a stack
- Resources(*obrigatório*) - set de recursos que serão criados 
- Paramenter - set de valores customizados
- Mappings - contém pares de chave:valor 
- Conditions - define as circunstâncias que os recursos serão criados
- Transform - especifica processos macro no template
- Outputs - valores que são retornados após a execução e podem ser usados em outras stacks como input.

Helper scriptssignal
São scripts em python que podeser usados para instalar softwar e iniciar serviços numa EC2 que é criada a partir de uma stack.
- cfn-init > recupera a interpreta metadata dos recursos, instala pacotes, cria arquios e inicia serviços;
- cfn-signal > utilizado como "sinal" com uma "Creationpolicy" ou "WaitCondition" para que seja possível sincronizar outros recursos da stack quando o pré-requisito é o recurso ou a aplicação estar em status "pronto";
- cfn-get-metadata > recupera metadata de um recurso ou "path" para uma chave específica;
- cfn-hup > utilizado para verificar atualização nos metadados e executar hooks customizados quando mudanças são detectadas.


### Key Management Service - KMS
Algumas features:
Gerencia chaves criptografadas facilitando proteção de dados porchaves simétricas e assimétricas.
Pode ser integrado ao cloudtrail para auditoria e compliance.
Também possuem `key policies` que dão acesso granular determinando usuários, ações, operações e condições às chaves.
Se integra com o Cloud HSM para segurança avançada das chaves
Suporta replicação multi-região para redundancia.


Criptografia (definição): processo de converter um plaintext (dados legíveis) em ciphertext (dado codificado) utilizando um algorítimo e uma chave de criptografia.

Symmetric encryption (AKA Secret-key encryption) - utiliza a mesma chave para criptografar e descriptografar o dado. O tamanho da chave implica em maior segurança.

Asymmetric Encryption (AKA Public-key encryption) - possuí 2 chaves sendo uma pública para criptografar e uma privada para descriptografar, que garante troca segura das chaves e assinaturas digitais.

Hybrid encryption - combinação dos dois métodos anteriores, utiliza uma symmetric key para criptografar o dado e então faz a criptografia da própria chave utilizando método assímetrico utilizando a chave pública.

2 tipos de chaves suportadas pelo KMS:
AWS managed keys - chaves geradas peloas kms, podem ser utilizadas para criptografar dados de serviços como s3, EBS e RDS, eliminando a preocupação do gerenciamento de chaves enquanto ainda mantém o controle das chaves.

Customer Managed keys - essas são chaves que são controladas no KMS permitindo gerenciamento de lifecycle abrangente, key rotation, auditoria e access control. Garantindo a segurança nos serviços AWS e aplicações externas também.
    -> symmeetric cmks 
    -> asymmetric cmks
