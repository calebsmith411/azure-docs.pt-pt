---
title: "Criar um Contrato de Parceiro Comercial no Serviço de Aplicações do Azure | Microsoft Docs"
description: Criar Contratos de Parceiros Comerciais
services: logic-apps
documentationcenter: .net,nodejs,java
author: rajram
manager: erikre
editor: 
ms.assetid: 319e46fa-fd81-4730-a742-768bf1676972
ms.service: logic-apps
ms.devlang: multiple
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 08/23/2016
ms.author: rajram
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: e41ac0e91bd66fbc7df08b4397e78377021fcbca


---
# <a name="creating-a-trading-partner-agreement"></a>Criar um Contrato de Parceiro Comercial
[!INCLUDE [app-service-logic-version-message](../../includes/app-service-logic-version-message.md)]

Os parceiros comerciais são as entidades envolvidas nas comunicações B2B (empresa-empresa). Quando dois parceiros estabelecem uma relação, esta é referida como um *Contrato*. O contrato definido baseia-se na comunicação que os dois parceiros pretendem alcançar e é especifico do protocolo ou do transporte. Os vários protocolos e transportes B2B suportados pelo App Service do Azure incluem:

* AS2 (Declaração de Aplicabilidade 2)
* EDIFACT (Nações Unidas/Transferência Eletrónica de Dados para a Administração, o Comércio e os Transportes [UN/EDIFACT])
* X12 (ASC X12)

### <a name="biztalk-api-apps-that-support-b2b-scenarios"></a>API Apps BizTalk que suportam cenários B2B
As seguintes API Apps ativam estas capacidades utilizando uma experiência avançada e intuitiva no Portal do Azure:

## <a name="biztalk-trading-partner-management-tpm"></a>Gestão de Parceiros Comerciais (TPM) BizTalk
* Criação e gestão de Parceiros, Perfis e Identidades
* Armazenamento e gestão de Esquemas EDI
* Armazenamento e gestão de certificados (utilizados no protocolo AS2)
* Criação e gestão de Contratos AS2
* Criação e gestão de Contratos EDIFACT (inclui a criação de batches no lado de envio)
* Criação e gestão de Contratos X12 (inclui a criação de batches no lado de envio)

![][1]

## <a name="as2-connector"></a>Conector AS2
* Executa Contratos AS2, conforme definido na instância da Aplicação API da TPM relacionada
* Analisa informações de processamento/controlo AS2 para a resolução de problemas

## <a name="biztalk-edifact"></a>EDIFACT de BizTalk
* Executa Contratos EDIFACT, conforme definido na instância da Aplicação API da TPM relacionada
* Analisa informações de processamento/controlo EDIFACT para a resolução de problemas
* Fornece a gestão do estado dos batches (início e fim), conforme definido no(s) Contrato(s) EDIFACT na instância da Aplicação API da TPM relacionada

## <a name="biztalk-x12"></a>X12 de BizTalk
* Executa Contratos X12, conforme definido na instância da Aplicação API da TPM relacionada 
* Analisa informações de processamento/controlo X12 para a resolução de problemas
* Fornece a gestão do estado dos batches (início e fim), conforme definido no(s) Contrato(s) X12 na instância da Aplicação API da TPM relacionada

Conforme indicado anteriormente, as API Apps AS2, X12 e EDIFACT precisam que a Aplicação API da TPM funcione conforme esperado.

## <a name="getting-started"></a>Introdução
Para criar contratos de parceiros comerciais:

1. Crie uma instância do conector **Gestão do Parceiro Comercial BizTalk**. É necessária uma SQL Database em branco. Antes de iniciar, assegure-se de que tem disponível uma base de dados em branco e pronta a utilizar.
2. Carregue esquemas e certificados, conforme exigidos pelos contratos. Para tal, procure a instância da TPM criada e avance para a parte “Esquemas” e/ou “Certificados”
3. Procure a instância da TPM criada e avance para a parte **Parceiros**
4. Crie parceiros, conforme pretender. Edite também os perfis, conforme adequado, e adicione as identidades necessárias
5. Agora, utilize a parte **Contratos** para criar Contratos. Quando cria um Contrato, deve selecionar o protocolo que será utilizado. As restantes opções de configuração baseiam-se no protocolo que selecionou.

![][2]

![][3]

<!--Image references-->
[1]: ./media/app-service-logic-create-a-trading-partner-agreement/TPMResourceView.png
[2]: ./media/app-service-logic-create-a-trading-partner-agreement/ProtocolSelection.png
[3]: ./media/app-service-logic-create-a-trading-partner-agreement/X12AgreementCreation.png




<!--HONumber=Nov16_HO2-->


