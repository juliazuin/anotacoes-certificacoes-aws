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
