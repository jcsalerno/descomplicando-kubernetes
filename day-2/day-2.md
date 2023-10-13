# Day-2: Foco em pods

## O que é um Pod?

<p> Em Kubernetes, um "pod" é a menor unidade computacional que você pode criar e implantar. Um pod pode conter um ou mais conteiners, compartilhando o mesmo espaço de rede, armazenamento e outras especificações.

## É normal e comum um pod ter vários containers? 

<p> Sim! É normal e comum ter um pod com vários containers em Kubernetes. A capacidade de ter múltiplos conteiners em um único pod é uma das características poderosas do Kubernetes e é frequentemente utilizada para atender a várias necessidades em um ambiente de implantação de aplicativos.

<p> Os conteiners dentro de um pod compartilham o mesmo namespace de rede, o que significa que eles podem se comunicar facilmente uns com os outros usando "localhost". Além disso, eles podem compartilhar volumes, que podem ser usados para compartilhar dados.


<p>Um exemplo em que você pode utilizar vários conteiners dentro de um único pod.

1. **Comunicação e Colaboração:** Você pode ter conteinerss que precisam colaborar entre si. Por exemplo, um conteiner de aplicativo principal pode estar acoplado a um conteiner de sidecar que lida com funções de registro, monitoramento ou envio de logs.

## Criação e exclusão de pods

<p> Para realizar a criação de um pod é bem simples. 
Utilize o seguinte comando:

`kubectl run NOME-DO-POD --image nginx --port 80 `

<p> Detalhe, foi utilizada a porta 80, pois é um web service. Você pode ajustar de acordo com a sua necessidade. 

![Alt text](image-3.png)

<p> Para a exclusão, utilize:

`kubectl delete pods NOME-DOS-SEU-POD`
<p> Neste caso foi:

`kubectl delete pods strigus`

![Alt text](image-2.png)

<p> Para ver o pod criado, utilize:

`kubectl get pods`

![Alt text](image-4.png)

<p> Dale! Seu pod tá vivão! 

## kubectl get pods e o kubectl describe pods

<p> São dois comandos no Kubernetes que permitem listar e obter informações detalhadas sobre os pods em um cluster.

1. Kubectl get pods:
<p> Este comando `kubectl get pods` é utilizado para listar todos os pods no namespace atual ou em um namespace específico. Ele fornece uma visão geral de todos os pods.

<p>Exemplo de uso:

`kubectl get pods`

![kubectl get pods](image.png)

2. Kubectl describe pods
<p> O comando `kubectl describe pod NOME-DO-SEU-POD` fornece informações detalhadas sobre um pod específico. 

<p> Exemplo de uso:

`kubectl describe pod strigus`

![comando](image-1.png)

<p> Os comandos: `kubectl get pods` fornece uma visão geral rápida dos pods em execução, enquanto `kubectl describe pod NOME-DO-POD` permite uma investigação mais profunda quando é necessário diagnosticar problemas ou entender melhor o estado de um pod específico.

## Criando nosso primeiro pod multicontainer utilizando um manifesto

<p> Para criar um pod multicontainer utilizando um manifesto no Kubernetes, você precisará criar um arquivo YAML que descreve a configuração do pod com múltiplos conteiners. 
<p> Abaixo, vou fornecer um exemplo simples de como criar um pod multicontainer com dois conteineres, um baseado no Nginx e outro baseado no Alpine, usando um arquivo de manifesto YAML:
Usando o kubectl para criar um arquivo de manifesto YAML que descreve um pod no Kubernetes. 
<p> Vou explicar cada parte do comando:

`kubectl run girus --image alpine --dry-run=client -o yaml`

![Alt text](image-5.png)

1. kubectl run girus --image alpine: Este comando cria um novo recurso chamado "girus" (que é o nome do pod) usando a imagem "alpine". Isso cria um pod simples com um único conteiner baseado na imagem Alpine.

2. --dry-run=client: Este sinalizador indica ao kubectl para não efetuar nenhuma alteração no cluster, mas apenas gerar o manifesto YAML que seria usado para criar o recurso. É útil quando você deseja apenas gerar o arquivo de configuração sem realmente criar o recurso no cluster.

3. -o yaml: Este sinalizador especifica o formato de saída desejado, que é YAML. Isso faz com que o kubectl gere o manifesto no formato YAML.

`kubectl run girus --image alpine --dry-run=client -o yaml > pod.yaml`

<p> Agora crie o seu pod utilizando este manifesto:

`kubectl apply -f pod.yaml`

![Alt text](image-6.png)

<p> Pod criado com sucesso!
Mas vale ressaltar que é necessário realizar a troca da imagem para nginx, pois a imagem Alpine não é um serviço.
<p> Entre no manifesto através do comando: 

`vim pod.yaml`

![vim pod.yaml](image-7.png)

<p> Caso você utilize o comando kubectl get pods, sempre você terá esse erro: 

![erro](image-8.png)

<p> Então é necessário que você utilize o comando apply.

`kubectl apply -f pod.yaml`

<p> Pronto! Configurado

![certo](image-9.png)

![ta rodando liso](image-10.png)

<p> Vamos adicionar uma nova imagem ao pod.yaml

`vim pod.yaml`

![ubuntu-image](image-11.png)

<p> Você enfretará um erro assim que utilizar o comando `kubectl apply -f pod.yaml` Pois estar sendo adicionando um container novo.

![erro](image-12.png)

<p> Para resolver este erro, utilize o seguinte comando: 

`kubectl delete -f pod.yaml`

<p>Irá deletar o pod e depois: 

`kubectl apply -f pod.yaml`

![tudo certo](image-13.png)

<p> Você vai identificar que existe um "CrashLoopBackOff". Este erro acontece pois o Ubuntu não possui um serviço, para isso, vamos trocar para o httpd, que é um apache.

`vim pod.yaml `

![Alt text](image-14.png)

`kubectl delete -f pod.yaml` 

`kubectl apply -f pod.yaml` 

![comandos acima](image-15.png)

`kubectl get pods`

![comando kubectl get pods](image-16.png)

<p> Você está vendo que o erro ainda existe. Como sabemos o motivo do erro?
<p> Simples! Utilize o comando 

`kubectl describe pods girus`

![Alt text](image-18.png)

![Alt text](image-20.png)

`kubectl logs girus`

![Alt text](image-17.png)

<p> Podemos utilizar outro comando para especificar cada vez mais.

`kubectl logs girus -c apache` 

<p> Lembrando, o nome apache foi definido no nosso pod.yaml, ou seja, é o nome do nosso container. 

![achamos o erro](image-21.png)

<p> O erro é na porta 80, pois são dois web services querendo utilizar a porta 80 ao mesmo tempo. 


### Vamos resolver esse problema! 

<p> Vamos entrar novamente no nosso arquivo pod.yaml através do comando já visto:

`vim pod.yaml`

<p> E alterar a imagem para busybox e passar alguns argumentos.

![Alt text](image-22.png)

<p> O argumento sleep é um argumento que irá esperar e depois informamos quantos segundos.

<p> Depois é o mesmo passo a passo. Delete o pod e crie novamente

`kubectl delete -f pod.yaml` 

`kubectl apply -f pod.yaml `

![funcionando](image-23.png)

<p> Com o exemplo acima, fica mais fácil o conceito de pods, que eles utilizam o mesmo namespace, como foi realizada a inserção de mais um conteiner, eles compartilharam o mesmo recurso, como foi o de redes, por isso, tivemos o erro acima. 

<p> Tá lá! Tudo funcionando certo. Sem problemas. 

## Limitando o consumo de recursos de CPU e memória.

<p> Como é ensencial que o pod uma imagem, é de suma importância que o mesmo possua limites, pois é graças a isso que você conseguirá realizar o gerenciamento correto do seu cluster. 

**Vamos começar!**

<p> Faça um cópia do arquivo .yaml com o seguinte comando. 

`cp pod.yaml pod-limitado.yaml`

<p> Entre no arquivo:

`vim pod-limitado.yaml`

![comandos acima](image-24.png)

![Alt text](image-25.png)

1. **resources:** Isso é uma seção em um manifesto do Kubernetes que permite definir os recursos (CPU e memória) alocados para um contêiner em um pod.

2. **limits:** Esta seção define os limites máximos de recursos que o conteiner pode consumir. No exemplo:
cpu: "0.5": Isso define um limite máximo de CPU de 0.5 unidades. As unidades de CPU são uma representação da capacidade de processamento disponível no cluster Kubernetes. Esse limite significa que o conteiner não pode consumir mais do que 0.5 unidades de CPU, independentemente de quanto poder de processamento esteja disponível no nó em que o pod está sendo executado.

3. **memory:** "128Mi": Isso define um limite máximo de uso de memória de 128 Mebibytes (MiB). Isso significa que o contêiner não pode usar mais de 128 MiB de memória, mesmo que mais memória esteja disponível no nó.

4. **requests:** Esta seção define as solicitações de recursos, ou seja, a quantidade mínima de recursos que o conteiner precisa para ser executado de maneira eficaz. No exemplo: cpu: "0.3": Isso significa que o contêiner requer pelo menos 0.3 unidades de CPU para funcionar corretamente. memory:"64Mi": Isso significa que o conteiner requer pelo menos 64 MiB de memória para funcionar corretamente.

<p> Para identificarmos para se deu tudo certo basta executar os seguintes comandos:

`kubectl apply -f pod-limitado.yaml`

`kubectl get pods`

![comandos](image-26.png)

<p> Depois utilize o describe:

`kubectl describe pods giropops`

![describe giropops](image-28.png)

![limitação ok](image-27.png)

## Configurando o nosso primeiro volume EmptyDir

<p> No Kubernetes, um volume EmptyDir é um tipo de volume que é montado no sistema de arquivos do contêiner e compartilhado entre os contêineres no mesmo pod. É um volume temporário que existe apenas enquanto o pod estiver em execução. Assim que o pod for excluído, o conteúdo do volume EmptyDir também é perdido.

<p> Para configurar um volume EmptyDir em um pod, você precisa definir uma seção volumes no manifesto do pod e, em seguida, montar esse volume em cada contêiner que deseja acessá-lo. Aqui está um exemplo de como fazer isso:

`cp pod-limitado.yaml pod-emptydir.yaml`

`vim pod-emptydir.yaml`

![comandos](image-29.png)

<p> Passe as seguintes especificações para seu arquivo .yaml.

![Alt text](image-31.png)

<p> Depois é só criar utilizando:

`kubectl apply -f pod-emptydir.yaml`

![Alt text](image-32.png)

![Alt text](image-33.png)

<p> Você também pode utilizar o describe para realizar a verificação do mounts.

`kubectl describe pods giropops`

![describe](image-34.png)

![Alt text](image-36.png)

<p> Para você entrar dentro do bash do pod e verificar o mount.

`kubectl exec -ti giropops -- bash`

`ls`

`mount`

<p> Pronto! Você verá a montagem dele.

![mount](image-37.png)
