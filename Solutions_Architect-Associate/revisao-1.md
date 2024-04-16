### Termination policy in EC2 instances:

Default – Validação quantas instâncias EC2 existem por AZ e pelo menos uma que não tenha proteção de scale-in.


AllocationStrategy – Terminate de uma instância alinhado com as demais instâncias ativas
Útil quando a preferencia de tipo de instância mudou, por exemplo, se a estratégia de spot são otimizadas para custos, é possível rebalançear a distribuição de instâncias Spot por todos os N pools de opções Spot de baixo custo.


OldestLaunchTemplate – Terminate a instância que tem o lauch **template** mais antigo, aquelas que não estao usando o atual.
Útil quando estamos atualizando um grupo e descontinuando instâncias de uma configuração antiga.

OldestLaunchConfiguration – Terminate a instância que tem o lauch **configuration** mais antigo.

ClosestToNextInstanceHour – Terminate a instância que está mais proxima do horario de cobrança. Ajuda a maximizar o uso de instâncias que tem cobrança de hora-em-hora.

NewestInstance – Terminate a instância mais recente do grupo. Útil quando uma lauch configuração está sendo testada porém não é necessário mantê-la em produção.

OldestInstance – Termina a instância mais antiga do grupo. Útel quando estamos atualizando as instâncias de um grupopara um novo tipo de EC2.2 instance type. You can gradually replace instances of the old type with instances of the new type.


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

### Security
* GuardDuty -  protege contra o uso de crendenciais comprometidas ou acesso não usual no s3, mas não escaneia o conteúdo do bucket s3
* Detective - analisa e visualiza informações de segurança dos serviços como o Guard Duty, Macia e Security Hub para resgatar a causa raiz de um potencial problema de segurança.
* Macie - utiliza machine learning e correspondência de padrões  para discobrir monitorar e proteger dados sensíveis nos Buckets s3 
* Inspector- Avalia vulnerabilidade de software ou exposições de rede nos EC2.

### AWS Transfer family
Oferecesuporte para transiferência de arquivos com SFTP, AS2, FTPS e FTP diretamente para o S3 ou EFS.

SFTP - Secure Shell (SSH) file transfer protocol - protocolo de rede q é usado para transferẽncia segura de dados pela internet.

AS2 - Applicabilitty Statement 2 - protocolo de redes utilizado para transferência de dados de business-to-business(B2B) num rede de internet publica com HTTP ou HTTPS (o qualquer TCP/IP protocolo)

FTP - File transfer protocol - protocolo de redes utilizado para transferência de dados, utiliza um canal separado para transferências de "control" e "data", sendo "control" aberto até terminar ou até a inatividade (timeout) e o canal "data" fica ativo durante a transferência do dados. FTP não suporta criptografia dos dados.

FTPS - File Transfer Protocol over SSL - é uma extensão do FTP, como ele utiliza um canal separado para transferências de "control" e "data", utiliza TLS (Transport layer security) para criptografia do tráfego sendo utilizada em ambos canais "control" e "data".

### Outposts
Permite estender e operar serviços nativos da AWS no ambiente on-premisses.
É possível rodar alguns serviços da aws localmente e integrar com outros vários serviços disponíveis na região.

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
Entrega insights de textos, por exemplo, extrai textos, frases, tópicos e sentimentos pré ocnfigurados.

### Kinesis
É possível realizar a ingestão de dados em tempo real, como vídeo audio, logs de aplicação, cliques de websites, dados de telemetria com IoT...

### Lex
Chatbot facilitado, é uma inteligência artificial para criar uma interface de conversa numa aplicação.

### Personalize 
Integra recomendações personalizadas para cada usuário em websites, aplicações, email, sistemas de marketing, etc.


### Timestream
É um banco de dados que utiliza dados tipo `time-series`, que são bancos de dados baseados em tempo.

### QuickSight
Dashboard like, serviço para business analytics.
É possível conectar os seguintes serviços ao QuickSight:
RDS, aurora, redshoft, athena e s3.
É possível também inputar arquivos de excel, conectar a bancos de dados on-premisses (SQL server, MySQL e PostGreSQL), importar dados de um SaaS (tipo salesforce)
