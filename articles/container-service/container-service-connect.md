---
title: Ligar a um cluster do Azure Container Service | Microsoft Docs
description: "Ligar a um cluster do Serviço de Contentor Azure utilizando um túnel SSH."
services: container-service
documentationcenter: 
author: rgardler
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Docker, Contentores, Microserviços, DC/OS, Azure"
ms.assetid: ff8d9e32-20d2-4658-829f-590dec89603d
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/13/2016
ms.author: rogardle
translationtype: Human Translation
ms.sourcegitcommit: a4882b6fcd75ecaa826cdda3e25ee690b85a0670
ms.openlocfilehash: 34450e25941e0be97b72c1ba30ee348d73f4bc67


---
# <a name="connect-to-an-azure-container-service-cluster"></a>Ligar a um cluster do Serviço de Contentor Azure
Os clusters DC/OS, Kubernetes e do Docker Swarm que são implementados pelo Azure Container Service expõem todos pontos finais REST.  Para os Kubernetes, este ponto final é exposto de forma segura na Internet e pode aceder-lhe diretamente a partir de qualquer computador ligado à Internet. Para DC/SO e Docker Swarm é necessário criar um túnel SSH para ligar de forma segura ao ponto final do REST. Cada uma destas ligações é descrita abaixo.

## <a name="connecting-to-a-kubernetes-cluster"></a>Ligar a um cluster do Kubernetes.
Para ligar a um cluster do Kubernetes, deve ter a linha de comandos `kubectl` instalada.  A forma mais fácil para instalar esta ferramenta é utilizar ferramenta de linha de comandos `az` do Azure 2.0 .

```console
az acs kubernetes install cli [--install-location=/some/directory]
```

Em alternativa, pode transferir o cliente diretamente a partir da [página de lançamentos](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG.md#downloads-for-v146)

Assim que tiver o `kubectl` instalado, deve copiar as credenciais de cluster para a máquina.  A forma mais fácil de fazê-lo é utilizar a ferramenta de linha de comandos `az`:

```console
az acs kubernetes get-credentials --dns-prefix=<some-prefix> --location=<some-location>
```

Esta ação irá transferir as credenciais de cluster para `$HOME/.kube/config`, onde `kubectl` espera ser localizado.

Em alternativa, pode utilizar `scp` para copiar de forma segura o ficheiro a partir de `$HOME/.kube/config` na VM principal da máquina local.

```console
mkdir $HOME/.kube/config
scp azureuser@<master-dns-name>:.kube/config $HOME/.kube/config
```

Se estiver no Windows, terá de utilizar o Bash no Ubuntu no Windows ou a ferramenta "pscp" Putty .

Assim que tiver o `kubectl` configurado, pode testá-lo com:

```console
kubectl get nodes
```

que deve mostrar-lhe os nós do cluster.

Para mais instruções pode ver o [Guia de Introdução do Kubernetes](http://kubernetes.io/docs/user-guide/quick-start/)

## <a name="connecting-to-a-dcos-or-swarm-cluster"></a>Ligar a um cluster de DC/SO ou Swarm

Os clusters DC/OS e do Docker Swarm que são implementados pelo Serviço de Contentor do Azure expõem pontos finais REST. No entanto, estes pontos finais não estão abertos ao mundo externo. Para gerir estes pontos finais, tem de criar um túnel Secure Shell (SSH). Depois de estabelecer um túnel SSH, pode executar comandos nos pontos finais de cluster e ver a IU do cluster através de um browser no seu próprio sistema. Este documento explica-lhe como criar um túnel SSH no Linux, OS X e Windows.

> [!NOTE]
> Pode criar uma sessão SSH com um sistema de gestão de clusters. No entanto, não é recomendável. Trabalhar diretamente num sistema de gestão expõe o risco de alterações de configuração inadvertidas.   
> 
> 

## <a name="create-an-ssh-tunnel-on-linux-or-os-x"></a>Criar um túnel SSH no Linux ou OS X
A primeira coisa que faz quando cria um túnel SSH no Linux ou OS X é localizar o nome DNS público dos modelos de estrutura mestres com balanceamento de carga. Para fazer isto, expanda o grupo de recursos, de modo a que cada recurso seja apresentado. Localize e selecione o endereço IP público do modelo de estrutura mestre. Esta ação abre um painel que contém informações sobre o endereço IP público, que inclui o nome DNS. Guarde este nome para utilização posterior. <br />

![Nome DNS público](media/pubdns.png)

Agora, abra uma shell e execute o seguinte comando em que:

**PORT** é a porta do ponto final que pretende expor. Para o Swarm, é 2375. Para o DC/OS, utilize a porta 80.  
**USERNAME** é o nome de utilizador que foi fornecido quando implementou o cluster.  
**DNSPREFIX** é o prefixo DNS que forneceu quando implementou o cluster.  
**REGION** é a região no qual o grupo de recursos está localizado.  
**PATH_TO_PRIVATE_KEY** [OPCIONAL] é o caminho para a chave privada que corresponde à chave pública que forneceu ao criar o cluster do Serviço de Contentor. Utilize esta opção com o sinalizador -i.

```bash
ssh -L PORT:localhost:PORT -f -N [USERNAME]@[DNSPREFIX]mgmt.[REGION].cloudapp.azure.com -p 2200
```
> A porta de ligação SSH é 2200 – não a porta padrão 22.
> 
> 

## <a name="dcos-tunnel"></a>Túnel do DC/OS
Para abrir um túnel para os pontos finais relacionados ao DC/OS, execute um comando semelhante ao seguinte:

```bash
sudo ssh -L 80:localhost:80 -f -N azureuser@acsexamplemgmt.japaneast.cloudapp.azure.com -p 2200
```

Agora, pode aceder aos pontos finais relacionados ao DC/OS em:

* DC/OS: `http://localhost/`
* Marathon: `http://localhost/marathon`
* Mesos: `http://localhost/mesos`

Da mesma forma, pode aceder ao resto das APIs de cada aplicação através deste túnel.

## <a name="swarm-tunnel"></a>Túnel do Swarm
Para abrir um túnel para o ponto final Swarm, execute um comando semelhante ao seguinte:

```bash
ssh -L 2375:localhost:2375 -f -N azureuser@acsexamplemgmt.japaneast.cloudapp.azure.com -p 2200
```

Agora pode definir a variável de ambiente de DOCKER_HOST da seguinte forma. Pode continuar a utilizar a interface de linha de comandos (CLI) Docker normalmente.

```bash
export DOCKER_HOST=:2375
```

## <a name="create-an-ssh-tunnel-on-windows"></a>Criar um túnel SSH no Windows
Existem várias opções para criar túneis SSH no Windows. Este documento descreve como utilizar o PuTTY para efetuar este procedimento.

Transfira o PuTTY para o sistema Windows e execute a aplicação.

Introduza um nome de anfitrião composto pelo nome de utilizador administrador do cluster e o nome DNS público do primeiro modelo de estrutura mestre no cluster. O **Nome de Anfitrião** terá este aspeto: `adminuser@PublicDNS`. Introduza 2200 para a **Porta**.

![Configuração do puTTY 1](media/putty1.png)

Selecione **SSH** e **Autenticação**. Adicione o ficheiro de chave privada para autenticação.

![Configuração do puTTY 2](media/putty2.png)

Selecione **Tunnels** e configure as seguintes portas reencaminhadas:

* **Porta de Origem:** a sua preferência – utilize a 80 para DC/OS ou 2375 para Swarm.
* **Destino:** utilize localhost:80 para DC/OS ou localhost:2375 para Swarm.

O exemplo seguinte foi configurado para o DC/OS, mas será semelhante para o Docker Swarm.

> [!NOTE]
> A porta 80 não deve ser utilizada quando cria este túnel.
> 
> 

![Configuração do puTTY 3](media/putty3.png)

Quando tiver terminado, guarde a configuração da ligação e ligue a sessão PuTTY. Quando se liga, pode ver a configuração da porta no registo de eventos do PuTTY.

![Registo de eventos do puTTY](media/putty4.png)

Quando tiver configurado o túnel para DC/OS, pode aceder ao ponto final relacionado em:

* DC/OS: `http://localhost/`
* Marathon: `http://localhost/marathon`
* Mesos: `http://localhost/mesos`

Quando tiver configurado o túnel para Docker Swarm, pode aceder ao cluster Swarm através da CLI do Docker. Primeiro terá de configurar uma variável de ambiente do Windows com o nome `DOCKER_HOST` com um valor de ` :2375`.

## <a name="next-steps"></a>Passos seguintes
Implementar e gerir contentores com DC/OS ou Swarm:

* [Trabalhar com o Azure Container Service e o DC/OS](container-service-mesos-marathon-rest.md)
* [Trabalhar com o Azure Container Service e o Docker Swarm](container-service-docker-swarm.md)




<!--HONumber=Nov16_HO4-->


