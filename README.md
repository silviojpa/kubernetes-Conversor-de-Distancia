# Passo a Passo das Atividades em Kubernetes (Minikube)
# O ciclo de trabalho focou em praticar o gerenciamento do ciclo de vida da aplicação (silvio69luiz/conversao-distancia) no Kubernetes, utilizando os principais componentes de Deployment, Scaling, Service, Rolling Update, Rollback e Ingress.

Fase	Atividade Realizada	Objetivo de Aprendizado
0. Deploy Inicial	Criação inicial do Deployment com 3 réplicas e o Service (inicialmente NodePort).	Expor a aplicação (que roda na porta 5000) e garantir o funcionamento básico.
1. Escalabilidade (Scaling)	Prática de alterar o número de réplicas de 3 para 5 e depois para 1, usando o kubectl scale e editando o deployment.yaml.	Dominar a mudança do número de instâncias da aplicação sem downtime.
2. Atualização (Rolling Update)	Simulação de atualização da imagem de v1 para v2 (criada localmente no ambiente Docker do Minikube).	Praticar como o Kubernetes gerencia a troca de versões de Pods gradualmente (Rolling Update).
3. Reversão (Rollback)	Execução do comando kubectl rollout undo para reverter a aplicação da versão v2 para a versão estável anterior (v1).	Dominar a recuperação rápida de um Deployment em caso de falha na nova versão.
4. Roteamento (Ingress)	Implementação do Ingress para acessar a aplicação por um nome de domínio (conversor.local) em vez de IP:Porta.	Simular um ambiente de produção e centralizar o roteamento de tráfego.
5. Configuração WSL/Hosts	Mapeamento do domínio conversor.local para o IP do Minikube (192.168.49.2) no arquivo hosts do Windows.	Solução do desafio de rede específico do ambiente WSL2/Minikube.
6. Estabilização do Ingress	Diagnóstico e resolução do problema de conexão (TIME_OUT) causado pela falta do Ingress Controller.	Aprender a identificar a dependência do Ingress Controller e resolver problemas de roteamento.

# Tipos de Services (Serviços) Utilizados
No decorrer do treinamento, exploramos e corrigimos três tipos diferentes de Services, fundamentais para expor a aplicação:

Tipo de Service	Finalidade	Como Foi Usado	Status Final
NodePort	Expõe o Service em uma porta estática (NodePort) em cada nó do cluster. É o mais comum para Minikube.	Foi o tipo de Service inicial. Acesso via http://<minikube-ip>:30007.	Funcional, mas substituído para testar LoadBalancer.
LoadBalancer	Expõe o Service externamente usando um Load Balancer da nuvem (ou um simulador, como o minikube tunnel).	Foi usado em uma fase de teste. No Minikube, necessita do comando minikube tunnel para funcionar (sem ele, fica em Pending).	Gerou problemas de Pending/Time Out e foi substituído.
ClusterIP	Expõe o Service apenas internamente no cluster, usando um IP virtual. É o tipo ideal para ser usado como backend de um Ingress.	Foi a solução final e correta para a arquitetura com Ingress. O Ingress Controller usa o IP interno deste Service para rotear o tráfego externo.	Finalizado. A arquitetura final Ingress usou este tipo.

