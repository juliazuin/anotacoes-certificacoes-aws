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


### EFS -DataSync
Transfere dados online entre serviços, simplifica, automatiza e acelera movimentação e replicação dos dados entre sistemas de armazenamento on-premisses e também entre serviços de armazenamento da AWS
Pode copar dados entre Network File Systems (NFS), Server Message Block (SMB) file servers, self-managed object storage, AWS Snowcone, Amazon S3 buckets, Amazon EFS file systems, and FSx for Windows File Server file systems.
Também pode ser utilizado para transferência de dados entre EFS de regiões diferentes e EFS de contas diferentes.







