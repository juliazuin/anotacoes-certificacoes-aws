#### IAM - Basico
autorização = o que esse usuario pode fazer/acessar?

autenticação = quem é essa pessoa


Porque usar IAM?
- Utilização do MFA
- permissões granulares
- Permite acesso compartilhado à aws
- Permite acesso seguro entre aplicações dentro da aws
- Federação de identidades

Quando não pode usar o IAM:
- Chaves SSH de uma EC2
- Certificados RDP Windows


AWS contém 2 tipos de endpoint acessíveis ao usuário:
- Console - acessado com usuario e senha
- API de consulta - access_key e secret_access_key


#### AWS Security, Identity, and Compliance

A segurança é a prática de proteger sua propriedade intelectual contra acesso, uso ou modificação não autorizados. 


Tríade -> CIA – confidencialidade, integridade e disponibilidade

- confidentiality - limitação de acesso e divulgação a usuários autorizados e impedir acesso de pessoas não autorizadas
- integrity - preservação do que foi transmitido o inserido no sistema
- availability - prontidão de recursos
    

##### AWS Well-Architected Framework
1. Excelência operacional - execução e monitoramento de sistemas para entregar valor, melhoria de processos e procedimentos
2. Segurança - proteção de informações e do sistema
3. Confiabilidade - capacidade de evitar e se recuperar de falhas
4. Eficiência de performance - uso eficiente dos recursos de TI
5. Otimização de custos - evita gastos desnecessários

Serviços da AWS utilizados para Identity and access management:
- Amazon cognito
- AWS Directory Service
- IAM
- Single sign on 