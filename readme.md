# Descomplicando o Kubernetes - Guia de Introdução

## Introdução

Bem-vindo ao guia "Descomplicando o Kubernetes"! Este guia tem como objetivo fornecer uma introdução ao Kubernetes, containerização e demonstrar como criar, gerenciar pods e configurar recursos no Kubernetes. Este guia é dividido em dois dias, cada um cobrindo tópicos específicos.

## Dia 1: Entendendo o que são Containers e o Kubernetes

- **O que é um Container?**
  - Explica o conceito de containers e como eles funcionam, destacando a independência e isolamento de recursos.

- **O que é Kubernetes?**
  - Introduz o Kubernetes como um orquestrador de containers e destaca sua importância na implantação de aplicativos escaláveis.

- **Arquitetura do Kubernetes: Control Plane e Workers**
  - Descreve a arquitetura do Kubernetes, destacando o Control Plane e os Workers.

- **Introdução a Pods, ReplicaSets, Deployments e Services**
  - Explora os principais recursos do Kubernetes, como Pods, ReplicaSets, Deployments e Services.

- **Entendendo e instalando o kubectl para Linux**
  - Fornece instruções sobre como instalar a ferramenta kubectl no Linux para interagir com um cluster Kubernetes.

- **Criando o nosso primeiro cluster com kind**
  - Demonstra como criar um cluster local do Kubernetes usando o Kind (Kubernetes in Docker).

- **Simular um Kubernetes multi nodes**
  - Orienta sobre a criação de um cluster de Kubernetes com vários nós usando o Kind.

## Dia 2: Foco em Pods

- **O que é um Pod?**
  - Define o conceito de pods no Kubernetes, explicando como eles são a unidade mais simples no ecossistema e podem conter múltiplos containers.

- **Criação e Exclusão de Pods**
  - Mostra como criar e excluir pods no Kubernetes.

- **kubectl get pods e kubectl describe pods**
  - Explica os comandos kubectl para listar e obter informações detalhadas sobre os pods no cluster.

- **Criando nosso primeiro pod multicontainer utilizando um manifesto**
  - Orienta sobre a criação de um pod com múltiplos containers usando um arquivo de manifesto YAML.

- **Limitando o consumo de recursos de CPU e memória**
  - Demonstração de como definir limites de recursos (CPU e memória) para os containers em um pod.

- **Configurando o nosso primeiro volume EmptyDir**
  - Mostra como configurar um volume EmptyDir compartilhado entre os containers em um pod.

**Observação:** Cada seção fornece instruções detalhadas e exemplos para facilitar a compreensão e aplicação dos conceitos.

## Conclusão

Esperamos que este guia tenha ajudado você a entender os fundamentos do Kubernetes, containers e a criar e gerenciar pods em um cluster Kubernetes. Este é um guia inicial e há muito mais a explorar no mundo do Kubernetes. 
Aproveite sua jornada no Kubernetes!
