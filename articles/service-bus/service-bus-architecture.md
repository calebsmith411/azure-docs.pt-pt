---
title: Arquitetura do Service Bus | Microsoft Docs
description: Descreve a arquitetura de processamento de mensagens e do reencaminhamento do Azure Service Bus.
services: service-bus
documentationcenter: na
author: sethmanheim
manager: timlt
editor: ''

ms.service: service-bus
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/11/2016
ms.author: sethm

---
# Arquitetura do Service Bus
Este artigo descreve a arquitetura de processamento de mensagens e do reencaminhamento do Azure Service Bus.

## Unidades de escala do Service Bus
O Service Bus está organizado por *unidades de escala*. Uma unidade de escala é uma unidade de implementação e contém todos os componentes necessários para executar o serviço. Cada região implementa uma ou mais unidades de escala do Service Bus.

Um espaço de nomes do Service Bus é mapeado para uma unidade de escala. A unidade de escala processa todos os tipos de entidades do Service Bus: reencaminhamentos e entidades de mensagens mediadas (filas, tópicos, subscrições). A unidade de escala do Service Bus é constituída pelos seguintes componentes:

* **Um conjunto de nós de gateway** Os nós de gateway autenticam pedidos recebidos e processam pedidos de reencaminhamento. Cada nó de gateway tem um endereço IP público.
* **Um conjunto de nós do mediador de mensagens.** Os nós do mediador de mensagens processam pedidos relativos a entidades de mensagens.
* **Um arquivo de gateway.** O arquivo de gateway contém os dados para cada entidade que está definida nessa unidade de escala. O arquivo de gateway é implementado por cima de uma SQL Database do Azure.
* **Vários arquivos de mensagens.** Os arquivos de mensagens contêm as mensagens de todas as filas, tópicos e subscrições definidas nessa unidade de escala. Além disso, contêm todos os dados de subscrição. A menos que a opção [entidades de mensagens particionadas](../service-bus-messaging/service-bus-partitioning.md) esteja ativada, uma fila ou um tópico é mapeado para um arquivo de mensagens. As subscrições são armazenadas no mesmo arquivo de mensagens do respetivo tópico principal. Exceto as [Mensagens Premium](../service-bus-messaging/service-bus-premium-messaging.md) do Service Bus, os arquivos de mensagens são implementados por cima de bases de dados SQL Azure.

## Contentores
A cada entidade de mensagens é atribuído um contentor específico. Um contentor é uma construção lógica que utiliza exatamente um arquivo de mensagens para armazenar todos os dados relevantes para esse contentor. Cada contentor é atribuído a um nó do mediador de mensagens. Normalmente, existem mais contentores do que nós do mediador de mensagens. Por conseguinte, cada nó do mediador de mensagens carrega vários contentores. A distribuição de contentores para um nó do mediador de mensagens está organizada de modo que todos os nós do mediador de mensagens sejam equitativamente carregados. Caso o padrão de carga seja alterado (por exemplo, um dos contentores fique muito ocupado) ou caso um nó do mediador de mensagens fique temporariamente indisponível, os contentores são redistribuídos entre os nós do mediador de mensagens.

## Processamento de pedidos de mensagens recebidos
Quando um cliente envia um pedido para o Service Bus, o Azure Load Balancer encaminha o mesmo para qualquer um dos nós de gateway. O nó de gateway autoriza o pedido. Caso o pedido seja relativo a uma entidade de mensagens (fila, tópico, subscrição), o nó de gateway procura a entidade no arquivo de gateway e determina em que arquivo de mensagens está localizada a entidade. Em seguida, procura o nó do mediador de mensagens que está atualmente a utilizar esse contentor e envia o pedido para esse nó do mediador de mensagens. O nó do mediador de mensagens processa o pedido e atualiza o estado da entidade no contentor. O nó do mediador de mensagens envia a resposta novamente para o nó de gateway, o qual envia uma resposta adequada novamente para o cliente que emitiu o pedido original.

![Processamento de Pedidos de Mensagens Recebidos](./media/service-bus-architecture/IC690644.png)

## Processamento de pedidos de reencaminhamento recebidos
Quando um cliente envia um pedido para o Service Bus, o Azure Load Balancer encaminha o mesmo para qualquer um dos nós de gateway. Caso o pedido seja um pedido de escuta, o nó de gateway cria um novo reencaminhamento. Se o pedido for um pedido de ligação para um reencaminhamento específico, o nó de gateway reencaminha o pedido de ligação para o nó de gateway que detém o reencaminhamento.  O nó de gateway que detém o reencaminhamento envia um pedido de encontro para o cliente de escuta, pedindo para o serviço de escuta criar um canal temporário para o nó de gateway que recebeu o pedido de ligação.

Quando a ligação de reencaminhamento é estabelecida, os clientes podem trocar mensagens através do nó de gateway utilizado para o encontro.

![Processamento de Pedidos de Reencaminhamento Recebidos](./media/service-bus-architecture/IC690645.png)

## Passos seguintes
Agora que já leu uma descrição geral da arquitetura do Service Bus, visite as seguintes ligações para começar:

* [Descrição geral de mensagens do Service Bus](../service-bus-messaging/service-bus-messaging-overview.md)
* [Noções básicas do Service Bus](service-bus-fundamentals-hybrid-solutions.md)
* [Uma solução de mensagens em fila a utilizar filas do Service Bus](../service-bus-messaging/service-bus-dotnet-multi-tier-app-using-service-bus-queues.md)

<!--HONumber=Sep16_HO4-->


