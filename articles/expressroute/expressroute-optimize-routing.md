---
title: Otimizar o encaminhamento do ExpressRoute | Microsoft Docs
description: "Esta página fornece detalhes sobre como otimizar o encaminhamento quando um cliente tiver mais do que um circuito do ExpressRoute a estabelecer ligação entre a Microsoft e a rede empresarial do cliente."
documentationcenter: na
services: expressroute
author: charwen
manager: carmonm
editor: 
ms.assetid: fca53249-d9c3-4cff-8916-f8749386a4dd
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/10/2016
ms.author: charwen
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 26f0992e734f0aae96ac6e8b7040d661d5fb063c


---
# <a name="optimize-expressroute-routing"></a>Otimizar o Encaminhamento do ExpressRoute
Se tem vários circuitos do ExpressRoute, significa que tem mais do que um caminho para se ligar à Microsoft. Sendo assim, o encaminhamento poderá não ser o ideal, ou seja, o tráfego poderá optar por um caminho mais longo para chegar à Microsoft e da Microsoft à sua rede. Quanto mais longo for o caminho de rede, maior será a latência. A latência tem um impacto direto no desempenho das aplicações e na experiência do utilizador. Este artigo ilustra este problema e explica como otimizar o encaminhamento com as tecnologias de encaminhamento padrão.

## <a name="suboptimal-routing-case-1"></a>Encaminhamento inferior ao ideal – Caso 1
Vamos analisar o problema de encaminhamento com um exemplo. Imagine que tem dois escritórios nos EUA, um em Los Angeles e outro em Nova Iorque. Os escritórios estão ligados através de uma Rede Alargada (WAN), que pode ser a sua própria rede principal ou a VPN de IP do seu fornecedor de serviços. Tem dois circuitos do ExpressRoute, um nos EUA Oeste e o outro nos EUA Leste, que também estão ligados através da WAN. Tem, obviamente, dois caminhos para se ligar à rede da Microsoft. Imagine agora que tem a implementação do Azure (por exemplo, Serviço de Aplicações do Azure) tanto nos EUA Oeste como nos EUA Leste. A sua intenção é ligar os utilizadores em Los Angeles ao Azure dos EUA Oeste e os utilizadores em Nova Iorque ao Azure dos EUA Leste, porque o administrador de serviços anuncia que os utilizadores em cada escritório acederão aos serviços mais próximos do Azure, para uma experiência ideal. Infelizmente, o plano funciona bem para os utilizadores da costa leste mas não para os utilizadores da costa oeste. A causa do problema é a seguinte. Em cada circuito do ExpressRoute, anunciamos o prefixo no Azure dos EUA Leste (23.100.0.0/16) e o prefixo no Azure dos EUA Oeste (13.100.0.0/16). Se não souber que prefixo corresponde a que região, não conseguirá tratá-los de forma diferente. A rede WAN poderá achar que ambos os prefixos estão mais próximos dos EUA Leste do que dos EUA Oeste e, por conseguinte, encaminhar ambos os utilizadores do escritório para o circuito do ExpressRoute nos EUA Leste. No final, terá muitos utilizadores insatisfeitos no escritório de Los Angeles.

![](./media/expressroute-optimize-routing/expressroute-case1-problem.png)

### <a name="solution-use-bgp-communities"></a>Solução: utilizar Comunidades do BGP
Para otimizar o encaminhamento para os utilizadores dos dois escritório, é necessário saber qual é o prefixo do Azure dos EUA Oeste e qual é o do Azure dos EUA Leste. Codificamos estas informações com os [Valores das Comunidades do BGP](expressroute-routing.md). Atribuímos um valor exclusivo de Comunidades do BGP a cada região do Azure, por exemplo, “12076:51004” para EUA Leste e “12076:51006” para EUA Oeste. Agora que sabe que prefixo corresponde a que região do Azure, já pode configurar que circuito do ExpressRoute deve ser o preferencial. Uma vez que utilizamos o BGP para trocar informações de encaminhamento, pode utilizar a Preferência Local do BGP para influenciar o encaminhamento. No nosso exemplo, pode atribuir um valor de preferência local mais alto a 13.100.0.0/16 nos EUA Oeste do que nos EUA Leste, e, da mesma forma, um valor de preferência local mais alto a 23.100.0.0/16 nos EUA Leste do que nos EUA Oeste. Esta configuração permitirá garantir que, quando estiverem disponíveis os dois caminhos para Microsoft, os utilizadores em Los Angeles optam pelo circuito do ExpressRoute nos EUA Oeste para se ligarem ao Azure dos EUA Oeste, ao passo que os utilizadores em Nova Iorque optarão pelo ExpressRoute nos EUA Leste para o Azure dos EUA Leste. O encaminhamento fica otimizado em ambos os lados. 

![](./media/expressroute-optimize-routing/expressroute-case1-solution.png)

## <a name="suboptimal-routing-case-2"></a>Encaminhamento inferior ao ideal – Caso 2
Veja outro exemplo em que as ligações da Microsoft optam por um caminho mais longo para chegarem à sua rede. Neste caso, está a utilizar servidores do Exchange no local e o Exchange Online num [ambiente híbrido](https://technet.microsoft.com/library/jj200581%28v=exchg.150%29.aspx). Os seus escritórios estão ligados a uma WAN. Anuncia os prefixos dos servidores no local em ambos os escritórios para a Microsoft através de dois circuitos do ExpressRoute. O Exchange Online irá iniciar ligações aos servidores no local em casos como a migração da caixa de correio. Infelizmente, a ligação ao seu escritório em Los Angeles é encaminhada para o circuito do ExpressRoute nos EUA Leste, atravessando todo o continente antes de voltar à costa oeste. A causa do problema é semelhante à anterior. Sem sugestões, a rede da Microsoft não sabe qual dos prefixos do cliente está próximo dos EUA Leste e qual deles está próximo dos EUA Oeste. E escolhe o caminho errado para o seu escritório em Los Angeles.

![](./media/expressroute-optimize-routing/expressroute-case2-problem.png)

### <a name="solution-use-as-path-prepending"></a>Solução: utilizar prefixação COMO CAMINHO
Existem duas soluções para o problema. A primeira é simplesmente anunciar o prefixo no local para o escritório de Los Angeles, 177.2.0.0/31, no circuito do ExpressRoute dos EUA Oeste e o prefixo no local para o escritório de Nova Iorque, 177.2.0.2/31, no circuito do ExpressRoute nos EUA Leste. Sendo assim, existe apenas um caminho para a Microsoft estabelecer ligação a cada um dos seus escritórios. Não há ambiguidade e o encaminhamento é otimizado. Com esta estrutura, tem de pensar sobre a sua estratégia de ativação pós-falha. No caso do caminho para a Microsoft via ExpressRoute ser interrompido, tem de certificar-se de que o Exchange Online ainda consegue ligar-se aos seus servidores no local. 

A segunda solução é continuar a anunciar ambos os prefixos em ambos os circuitos do ExpressRoute e, além disso, dar-nos uma sugestão de qual é o prefixo que está próximo de qual dos seus escritórios. Uma vez que suportamos a prefixação COMO Caminho do BGP, pode configurá-la para o seu prefixo de modo a influenciar o encaminhamento. Neste exemplo, pode aumentar a prefixação COMO Caminho para 172.2.0.0/31 nos EUA Leste de modo a preferirmos o circuito do ExpressRoute nos EUA Oeste para o tráfego destinado a este prefixo (uma vez que a nossa rede irá considerar o caminho para este prefixo mais curto no Oeste). Pode, do mesmo modo, aumentar a prefixação COMO Caminho para 172.2.0.2/31 nos EUA Oeste, de modo a preferirmos o circuito do ExpressRoute nos EUA Leste. O encaminhamento fica otimizado para ambos os escritórios. Com esta estrutura, se um circuito do ExpressRoute for interrompido, o Exchange Online ainda consegue contactá-lo através de outro circuito do ExpressRoute e da sua WAN. 

> [!IMPORTANT]
> Removemos os números privados COMO no COMO Caminho para os prefixos recebidos no Peering da Microsoft. Temos de acrescentar números públicos COMO no COMO Caminho de modo a influenciar o encaminhamento para o Peering da Microsoft.
> 
> 

![](./media/expressroute-optimize-routing/expressroute-case2-solution.png)

> [!IMPORTANT]
> Apesar de os exemplos apresentados aqui se referirem à Microsoft e aos Peerings públicos, também suportamos as mesmas funções no Peering privado. Além disso, a prefixação COMO Caminho funciona dentro de um circuito único do ExpressRoute para influenciar a seleção dos caminhos primários e secundários.
> 
> 




<!--HONumber=Nov16_HO2-->


