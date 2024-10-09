
### Certificados
#### ACM
Suporta criação certificados SSL/TLS que serão utilizados nos recursos da AWS, também suporta a importação dos certificados. Para ambos os casos a data de expiração dos certificados são monitoradas.

:warning: para utilização de certificados **importados** no ACM com o CloudFront é necessário que a importação seja feita na região **us-east (n. virginia)**.


### Banco de dados
#### RDS - Relational database service
- Solução de bom custo benefício, que tem capacity adaptável para bancos relacionais e gerencia tarefas administratvas de BD comuns.
-- Gerencia backups
- Patch de software
- Detecção automáticas de falhas e recovery
- Backups automatizados
- Suporte para MySQL, MariaDB. PostgreSQL, Oracle, Microsoft SQL Server e amazon aurora
- Tem uma instância primária e sincroniza com uma instância secundária que serve para o `fail over` em caso de problemas, isso aumenta a resiliência

Backup:
É possível configurar backups que serão feitos numa janela estipulada automaticamente.
Um snapshot da instãncia de BD é criada num volume de storage.
O primeiro backup contém todos dados, os backups subsequêntes realizam um "diff" dos dados e incrementam os dados do anterior.

**Alta disponibilidade**
Read replicas - alta escalabilidade
Read replicas são réplicas do RDS em modo read-only que provê um alto volume de consultas na aplicação.
Os dados são replicados da instância master (primary) de banco de dados para as réplicas de forma **assíncrona**.
Possuí suporte cross-region.
casos de uso ->**
pode ser utilizado para chavear o tráfego em alguma indiponibilidade da instancia de banco,
utilização para rodar queries -> geração de reports, insumo de dados, etc.

Multi AZ-deployment - alta durabilidade
Realiza a replicação dos dados de forma **síncrona**


RPO - Recovery Point Objective
O máximo aceitável de perda de dados num evento de falha ou incidente
Medido em minutos
exemplo: um RPO de 5min significa perder logs de transação por mais de 5min é inaceitável.

RTO - Recovery Time Objective
O maximo de `downtime` que é permitido para recuperar dados do backup e continuar o processamento.
Normalmente medido em horas ou dias
exemplo: RTO de 1h significa ue o sistema tem que se recuerar em até 1h após a queda.

#### DynamoDB
Banco de dados NoSQL
Componentes:
Tables: Coleção de itens em uma tabla, número de celulas/colunas não é fixada
Items: todos elementos da tabela são itens
Attributes: dados que residem com o ítem

Primary key na verdade é uma **partition key**
E também é possível combinar essa PK com uma **sort key**

Benefícios:
- o DynamoDB escala up/down automaticamente para acomodar os dados
- built-in suporte para transações ACID
- Point in time recoveryeemplos  de dados não
- Criptografia em repouso

##### DynamoDB Streams
Feature adicional que captura eventos de modficação dos dados em tempo real na mesma ordem que eles acontecem, com isso é gerado um "Stream record" que podem ser:
- Um novo item adicionado à tabela
- Um item foi atualizado
- Um item foi deletado


##### DynamoDB Acelerator (DAX)
Cache para o dynamodb


#### Aurora - serverless V1
Escalonamento sob demanda para o Aurora
Pode ser útil em workloads que não tem previsibilidade de 

Como funciona:
Especificar mínimo e máximo de ACU (aurora capacity unit) 
ACU é umacombinação de 2gb de memória, CPU e redes correspondentes
Escala automaticamente de 10gb até 128tb
Cria automaticamente regras para escalonamento por uso de cpu, número de conexões e memoria.
Tem um pool de recursos que ficam em estado de quase pronto, para que o tempo de escalonamento seja menor.

Limitações:
- Só funciona em regiõs específicas
- Tem suporte apenas para Aurora MySQL e Aurora PostgreSQL


#### Aurora - global
Banco que está em várias regiões, garantindo baixa latência e fast recovery de qualquer interrupção que afete uma única região, é um único banco de dados que abrange múltiplas regiões.
O banco primário está em uma região e os secundários está em outra(s)

É possível mudar a configuração do banco enquanto ele está em uso, assim como também é possível promover um banco secundário para primário em menos de 1 min.

Somente a instância primária performa ação de escrita no banco.

Não é possível realizar ações de stop/start nas instâncias de Aurora Global individualmente.


#### Elasticache
Guarda dados críticos em memória, provendo uma baixa latencia de dados que não são modificados com muita frequência.
Suporta dois tipos de protocolos `ìn-memory`:
- Memchached - Memória Distribuída Simples
    1. Funciona num modelo de chave-valor
    2. Armazenamento em memoria RAM, quando reiniciado perdem-se os dados
    3.  Possúi escalonamento horizontal (aumenta a quantidade de nós para aumentar a capacidade de cache), cada nó funciona de forma independente
    4. Eficiente para workloads com alta concorrência
- Redis - Estrutura de dados em memória
    1. Tem suporte para outros modelos alem de chave-valor Redis suporta estruturas como listas, conjuntos, hashes e sorted sets.
    2. Tem maior durabilidade, pode armazenar os dados em disco
    3. Possuí replicação de dados com ativo/passivo com failover automático

    casos de uso: cargas de trabalho que não somente usem armazenamento em cache, mas necessitem de manipulação de estrutura de dados, persistência de dados  ou alta disponibilidade.

#### Neptune
Banco de dados gerenciado serverless que faz analytics com gráficos, podeser utilizado para buscar e realizar querys em bilhões de relacionamentos


##### Session Store
É possível utilizar o elasticache redis para sessões.
Aplicação que usa de sessões (aplicações web) começa uma sessão quando um usuaŕio se loga, e essa sessão fica ativa até que o usuário deslogue ou a sessão expire.
Enquanto ativa a aplicação guarda todos dados em memória ou numa "session store" que um banco de dados que não perde dados quando a aplicação cai.

Session data incluí dados do perfil do usuário, dados personaliados, recomendações, promoções direcionadas e descontos. 


### Analytics
#### Redshift
Data warehouse gerenciado desenhado para queries de alta performance e análise de grandes datasets utilizando query SQL, tem suporte para altos níveis de concorrência.

Componentes:
1. Nodes
1.1 Clusters: conjunto de 1 ou mais compute nodes
1.2 Leader node: gerencia comunicação com o cliente e coordena comunicação entre os compute nodes, delegando tarefas.
1.3 Compute node: executam tarefas e enviam resultados para agregar no resultado final, cada node tem sua própria configuração de capacity.

2. Storage
2.1 redshift managed store
2.2 bancos de dados


#### EMR
Elastic Map Resources
Permite a execução de frameworks de bigdata como apache Spark e apache hadoop, que faclita a analise de grandes conjuntos de dados. Consegue converter e transferir os dados para várias soluções de banco e storage tipo dynamoDB e s3.

Layers:
* Storage: HSFS (hadoop distributed file system), EMRFS (EMR file system) e local file system.
* Cluster resource management: responsável pelo gerenciamento de recursos no cluster e agendamento de atividades de processamento de dados.
* Data processing framework: hadoop mapreduce, apache spark
* Application e programs: ajuda no processamento e gerenciamento de conuntos de bigdata tipo algoritimos de machine learning

#### Kinesis
Serviço gerenciado para ingstão, processamento e análise de streaming em tempo real e em escala, podendo coletar, armazenar e analisar grandes quantidades de dados gerados continuamente por uma vasta variadade de serviços incluindo aplicações, websites, aparelhos IoT...

Features:
* Ingestao de dados
* Processamento de dados
* Armazenamento de dados
* Analise em tempo-real
* Resisliência e escalabilidade
* segurança e complience

Casos de uso:
1. Construção de aplicações em tempo-real, pode ser utilizada como detector de fraudes, live leaderboards e monitoramento de aplicações 
2. Evolução de batch para análise em tempo-real.
3. Análise de aparelhos de IoT
4. Construção de aplicações de análise de vídeo

#### Glue
Serviço de integração de dados **serverless** e ETL (extract, transform, load)


#### QuickSight
Provê dashboards iterativos conectando com dados em qualquer lugar incluindo serviços nativos da aws, excel, source de big data e bancos de dados terceiros sendo da cloud ou on-premisses.

Componentes:
Designer
Server
SPICE - Super fast, Parallel, in memory Calculations Engine
Analytics
SDKs


### Front-end
#### AppSync
Conecta aplicações aos dados e eventos.
Utiliza GraphQ e APIS Pub/Sub (publish/subscribe (publicação/subscrição)) linguagem de dados que habilita puxar os dados de uma forma estruturada, sendo possível realizar queries somente com os dados/campos necessários.


### File System
POSIX é um padrão definido pelo IEEE (Institute of Electrical and Electronics Engineers) que descreve uma interface de sistema operacional comum. Ele foi criado para garantir portabilidade entre sistemas operacionais, fornecendo um conjunto de APIs que garantem que programas escritos para um sistema compatível com POSIX possam ser facilmente adaptados para outros sistemas compatíveis.

É a forma que arquivos e diretórios devem ser criados, manipulados e destruídos.

Features:
sistema de arquivos
processos
sinais
threads
manipulação de dispositivos de entrada e saída (I/O)

#### FSx for Lustre
É utilizado para workloads q necessítem de velocidade como machine learning, HPC (high performance computing), processamento de vídeos e modelos financeiros.

Existem dois tipos de opções de deploy:
Scratch - Desenhadas para armazenamento temporario e processamento de dados de curta duração, nesse modo o dado não persiste e não é replicado em caso de falha do file system

Persistent - desenhado para armazenamento de longa duração, altamente disponível os dados são automaticamente replicados dentro da mesma AZ


### Storage Gateways
Utilizado para compatibilidade da cloud com sistemas ja existentes utlizando NFS, SMB, iSCSI ou iSCSI-VTL

#### s3 File Gateway
Interface que possibilita armazenar arquivos como objetos no s3 utilizando protocolos padrão NFS e SMB, sendo possível acessar os arquivos do data center ou de uma EC2, ou também pode ser acessados diretamente no s3.

Pode ser utilizado para backup de dados on-premisse (SQL server e oracle DB e logs), para workloads de cloud hibridas

#### FSx file gateuway
Acessar e armazenar dados com 100% de compatibilidade com windows 
pode ser usado para aplicações de negório baseadas em arquivos(on-premisses) e workloads tipo compartilhamento de arquivos com usuários ou grupos, web content management e media workflows.


### Backup
Serviço gerenciado que centraliza e automatiza proteção de dados entre serviços da AWS na cloud ou on-premisses, sendo possível configurar policies de backup e monitrar atividades dos recursos.
Features:
Gerenciamento de backups centralizados
backup baseado em policies
backup baseado em tags 
policies de gerenciamento de ciclo de vida (lifecyle)
backup cross-region
gerenciamento e backup cross-account 
auditoria e reports
backups incrementais
