---
title: "Descrição geral do encaminhamento de conteúdo baseado em URL | Microsoft Docs"
description: "Esta página fornece uma descrição geral do encaminhamento de conteúdo baseado em URL do Gateway de Aplicação, da configuração UrlPathMap e da regra PathBasedRouting."
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: 4409159b-e22d-4c9a-a103-f5d32465d163
ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/16/2016
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: ee8cfffdbf054b4251ed269745f6b9ee5a5e6c64
ms.openlocfilehash: 1f273f3b55d719e37b9cdb6cefda30c3566e7226


---
# <a name="url-path-based-routing-overview"></a>Descrição geral do Encaminhamento Baseado no Caminho do URL

O Encaminhamento Baseado no Caminho do URL permite-lhe encaminhar o tráfego para agrupamentos de servidores de back-end com base nos Caminhos de URL. Um dos cenários consiste em encaminhar pedidos de diferentes tipos de conteúdo para diversos agrupamentos de servidores de back-end.
No exemplo seguinte, o Gateway de Aplicação está a enviar tráfego para contoso.com a partir de três agrupamentos de servidores de back-end, como, por exemplo: VideoServerPool, ImageServerPool e DefaultServerPool.

![imageURLroute](./media/application-gateway-url-route-overview/figure1.png)

Os pedidos enviados para http://contoso.com/video* são encaminhados para VideoServerPool, enquanto os pedidos para http://contoso.com/images* são encaminhados para ImageServerPool. É selecionado o DefaultServerPool se nenhum dos padrões de caminho corresponder.

## <a name="urlpathmap-configuration-element"></a>Elemento de configuração UrlPathMap

O elemento UrlPathMap é utilizado para especificar padrões de Caminho para mapeamentos de agrupamentos de servidores de back-end. O seguinte exemplo de código é o fragmento do elemento UrlPathMap do ficheiro de modelo.

```json
"urlPathMaps": [
{
"name": "<urlPathMapName>",
"id": "/subscriptions/<subscriptionId>/../microsoft.network/applicationGateways/<gatewayName>/ urlPathMaps/<urlPathMapName>",
"properties": {
    "defaultBackendAddressPool": {
        "id": "/subscriptions/<subscriptionId>/../microsoft.network/applicationGateways/<gatewayName>/backendAddressPools/<poolName>"
    },
    "defaultBackendHttpSettings": {
        "id": "/subscriptions/<subscriptionId>/../microsoft.network/applicationGateways/<gatewayName>/backendHttpSettingsList/<settingsName>"
    },
    "pathRules": [
        {
            "paths": [
                <pathPattern>
            ],
            "backendAddressPool": {
                "id": "/subscriptions/<subscriptionId>/../microsoft.network/applicationGateways/<gatewayName>/backendAddressPools/<poolName2>"
            },
            "backendHttpsettings": {
                "id": "/subscriptions/<subscriptionId>/../microsoft.network/applicationGateways/<gatewayName>/backendHttpsettingsList/<settingsName2>"
            },

        },

    ],

}
}
]
```

> [!NOTE]
> PathPattern: esta definição é uma lista de padrões de caminho para fazer correspondências. Cada um deles tem de começar com / e o único local onde "*" é permitido é no fim depois de "/". A cadeia introduzida na ferramenta de correspondência de caminhos não inclui texto depois do primeiro ? ou #, sendo que esses carateres não são aqui permitidos.

Pode dar saída a um [modelo do Resource Manager através do encaminhamento baseado em URL](https://azure.microsoft.com/documentation/templates/201-application-gateway-url-path-based-routing) para obter mais informações.

## <a name="pathbasedrouting-rule"></a>Regra PathBasedRouting

A RequestRoutingRule do tipo PathBasedRouting é utilizada para vincular um serviço de escuta a um UrlPathMap. Todos os pedidos que são recebidos para este serviço de escuta são encaminhados com base na política especificada no urlPathMap.
Fragmento da regra PathBasedRouting:

```json
"requestRoutingRules": [
    {

"name": "<ruleName>",
"id": "/subscriptions/<subscriptionId>/../microsoft.network/applicationGateways/<gatewayName>/requestRoutingRules/<ruleName>",
"properties": {
    "ruleType": "PathBasedRouting",
    "httpListener": {
        "id": "/subscriptions/<subscriptionId>/../microsoft.network/applicationGateways/<gatewayName>/httpListeners/<listenerName>"
    },
    "urlPathMap": {
        "id": "/subscriptions/<subscriptionId>/../microsoft.network/applicationGateways/<gatewayName>/ urlPathMaps/<urlPathMapName>"
    },

}
    }
]
```

## <a name="next-steps"></a>Passos seguintes

Depois de saber mais sobre o encaminhamento de conteúdo baseado em URL, aceda a [Criar um gateway de aplicação com encaminhamento baseado em URL](application-gateway-create-url-route-portal.md) para criar um gateway de aplicação com regras de encaminhamento do URL.




<!--HONumber=Nov16_HO3-->


