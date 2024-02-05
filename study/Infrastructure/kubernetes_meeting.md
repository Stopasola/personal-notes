### Kubernetes (K8s)

Shellscript escrito em golang que gerencia iptables


A grande divisão que podemos fazer são entre os Nodes 

 * Master node: Não executa processos, apenas gerencia os outros workers nodes. Geralmente possúi apenas 3 nodes (sempre ímpar).

    - **kubeapi server**: api que interage com o mundo  externo. Ex: kubectl
  	- **etcd**: banco distribuido que armazena no formato chave-valor.
  	- **kube scheduler**: Agendamento da execução e apontar a maquina que o agendamento será executado.
 * Worker Node: 
   - kubelet: gerencia tudo que roda no worker node
   - kubeproxy: gerencia tráfego entre os nós
   - cni: Interface de rede
   - container runtime: 


### Aula Hands On

- POD: é a menor porção do kubernets
- Pode ter um ou mais container dentro do pod

## Recursos básicos

Workloads: 
Configs: 
Volumes:
Services:
Ingress: 
Namespaces: 
