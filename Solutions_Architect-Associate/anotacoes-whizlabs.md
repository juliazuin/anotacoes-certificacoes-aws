## EC2
### ENI (Elastic Network Interface)
O ENI é o tipo mais básico de capabilitty de rede disoponível na EC2 dentro de uma VPC.
É um componente lógico de rede que representa uma `virtual network`, ela pode ter:
* Um endereço primário privado de IPv4 dos endereços IPv4 alocados na VPC;
* Um ou mais endereços secundários privados de IPv4 dos endereços IPv4 alocados na VPC;
* Um endereço elastic IP (IPv4) por endereço privado de IPv4
* Um IPv4 público;
* Um ou mais endereços de IPv6;
* Um ou mais `security groups`;
* Um endereço MAC  

### ENA (Elsatic Network Adapter)
Interface de rede customizada que entega alta taxa de transferência de pacotes (PPS-packet per second) e baixa latência.
Utiliza do método de `single root i/o virtualization` (SR-IOV), que promove alta performance de I/O e pouca utilização de CPU, quando comparada com outras formas de virtualização.
2 implementações:
1. Elastic network adapter que suporta uma velocidade de rede ate 100gbps
2. Virtual function interface que suporte velocidade de rede de até 10gbps

### EFA (Elastic Fabric Adapter)
Utilizado para aceleração de computação de alta perfomance (HPC - high performance computing) e aplicações de machine learning, com ele é possível ter uma performance de um cluster HPC on-premisses porém com a flexibilidade e elasticidade da cloud.
EFA é basicamente uma ENA tunada, o principal componente que as diferenciam é OS-bypass (operating system-bypass).
OS-bypass permite que o HPC se comunique diretamente com o NIC(network interface card), provendo baixa latência
