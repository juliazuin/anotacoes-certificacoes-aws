
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

