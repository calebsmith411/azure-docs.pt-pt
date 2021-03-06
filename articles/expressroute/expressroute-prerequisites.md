---
title: "Pré-requisitos para adoção do ExpressRoute | Microsoft Docs"
description: "Esta página fornece uma lista de requisitos a serem satisfeitos para que possa ordenar um circuito do ExpressRoute do Azure."
documentationcenter: na
services: expressroute
author: cherylmc
manager: carmonm
editor: 
ms.assetid: f872d25e-acfd-405d-9d1b-dcb9f323a2ff
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/10/2016
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: a582941b06fa7f81d7db188f2a9feba837b9bfd8


---
# <a name="expressroute-prerequisites-checklist"></a>Pré-requisitos e lista de verificação do ExpressRoute
Para ligar a serviços em nuvem da Microsoft com o ExpressRoute, terá de verificar se foram cumpridos os seguintes requisitos listados nas secções abaixo.

[!INCLUDE [expressroute-office365-include](../../includes/expressroute-office365-include.md)]

## <a name="azure-account"></a>Conta do Azure
* Uma conta ativa do Microsoft Azure. É necessário ter uma conta para configurar o circuito do ExpressRoute. Os circuitos do ExpressRoute são recursos incluídos nas subscrições do Azure. Uma subscrição do Azure é um requisito, mesmo se a conectividade estiver limitada a serviços em nuvem não pertencentes ao Microsoft Azure, como serviços do Office 365 e o CRM online.
* Uma subscrição ativa do Office 365 (se utilizar os serviços do Office 365). Consulte a secção [Requisitos específicos do Office 365](#office-365-specific-requirements) deste artigo para obter mais informações.

## <a name="connectivity-provider"></a>Fornecedor de conectividade
* Pode trabalhar com um [Parceiro de conectividade do ExpressRoute](expressroute-locations.md#partners) para se ligar à nuvem da Microsoft. Pode configurar uma ligação entre a sua rede no local e a Microsoft de [três modos](expressroute-introduction.md#howtoconnect). 
* Se o seu fornecedor não for um parceiro de conectividade do ExpressRoute, ainda pode ligar-se à nuvem da Microsoft através de um [fornecedor do Exchange na nuvem](expressroute-locations.md#nonpartners).

## <a name="network-requirements"></a>Requisitos da rede
* **Conectividade redundante**: não é necessário haver redundância na conectividade física entre o utilizador e o fornecedor. A Microsoft exige a configuração de sessões de BGP redundantes entre routers da Microsoft e routers de peering, mesmo se tiver apenas [uma ligação física a um Exchange de nuvem](expressroute-faqs.md#onep2plink). 
* **Encaminhamento**: dependendo do modo como se liga ao Microsoft Cloud, o utilizador ou o fornecedor precisa de configurar e gerir as sessões de BGP para [domínios de encaminhamento](expressroute-circuit-peerings.md). Alguns fornecedores de conectividade Ethernet ou fornecedores do Exchange na nuvem poderão oferecer gestão de BGP como um serviço de valor acrescentado.
* **NAT**: a Microsoft só aceita endereços IP públicos através do peering da Microsoft. Se estiver a utilizar endereços IP privados na rede no local, o utilizador ou o fornecedor precisará de converter os endereços IP privados em endereços IP públicos [com o NAT](expressroute-nat.md).
* **QoS**: o Skype para Empresas tem vários serviços (por exemplo, voz, vídeo, texto), que exigem um tratamento QoS diferenciado. O utilizador e o fornecedor devem cumprir os [Requisitos de QoS](expressroute-qos.md).
* **Segurança de Rede**: deve ter em conta a [segurança de rede](../best-practices-network-security.md) quando estabelecer ligação ao Microsoft Cloud através do ExpressRoute.

## <a name="office-365"></a>Office 365
Se planear ativar o Office 365 no ExpressRoute, reveja os seguintes documentos para obter mais informações acerca dos requisitos do Office 365.

* [Descrição geral do ExpressRoute para o Office 365](https://support.office.com/en-us/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd)
* [Encaminhamento com o ExpressRoute para o Office 365](https://support.office.com/en-us/article/Routing-with-ExpressRoute-for-Office-365-e1da26c6-2d39-4379-af6f-4da213218408)
* [Intervalos de endereços IP e URLs do Office 365](https://support.office.com/en-us/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2)
* [Planeamento de rede e otimização de desempenho para o Office 365](https://support.office.com/en-us/article/Network-planning-and-performance-tuning-for-Office-365-e5f1228c-da3c-4654-bf16-d163daee8848)
* [Calculadoras e ferramentas de largura de banda de rede](https://support.office.com/en-us/article/Network-and-migration-planning-for-Office-365-f5ee6c33-bcd7-4b0b-b0f8-dc1d9fb8d132)
* [Integração do Office 365 com ambientes no local](https://support.office.com/en-us/article/Office-365-integration-with-on-premises-environments-263faf8d-aa21-428b-aed3-2021837a4b65)

## <a name="crm-online"></a>CRM Online
Se planear ativar o CRM Online no ExpressRoute, reveja os seguintes documentos para obter mais informações acerca do CRM Online.

* [URLs do CRM Online](https://support.microsoft.com/kb/2655102) e [Intervalos de endereços IP](https://support.microsoft.com/kb/2728473)

## <a name="next-steps"></a>Passos seguintes
* Para obter mais informações acerca do ExpressRoute, veja as [FAQs do ExpressRoute](expressroute-faqs.md).
* Localizar um fornecedor de conectividade do ExpressRoute. Veja [Parceiros e localizações de peering do ExpressRoute ](expressroute-locations.md).
* Veja os requisitos de [Encaminhamento](expressroute-routing.md), [NAT](expressroute-nat.md) e [QoS](expressroute-qos.md).
* Configurar a ligação do ExpressRoute.
  * [Crie um circuito do ExpressRoute](expressroute-howto-circuit-classic.md)
  * [Configure o encaminhamento](expressroute-howto-routing-classic.md)
  * [Ligar uma VNet a um circuito do ExpressRoute](expressroute-howto-linkvnet-classic.md)




<!--HONumber=Nov16_HO2-->


