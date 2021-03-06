---
title: Como criar um pedido de suporte para o SQL Data Warehouse | Microsoft Docs
description: Como criar um pedido de suporte no Azure SQL Data Warehouse.
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jhubbard
editor: 
ms.assetid: a91d1f53-03cb-464b-9d5b-4a9c1a194ed3
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 10/31/2016
ms.author: barbkess
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: ede330870f451bd1e0ed6805fa937e74438fd644


---
# <a name="how-to-create-a-support-ticket-for-sql-data-warehouse"></a>Como criar um pedido de suporte para o SQL Data Warehouse
Se tiver problemas com o SQL Data Warehouse, crie um pedido de suporte para que a nossa equipa de engenharia possa ajudá-lo.

## <a name="create-a-support-ticket"></a>Criar um pedido de suporte
1. Abra o [Portal do Azure][Portal do Azure].
2. No ecrã principal, clique no mosaico **Ajuda + suporte**.
   
    ![Ajuda + suporte](./media/sql-data-warehouse-get-started-create-support-ticket/help-support.png)
3. No painel Ajuda + Suporte, clique em **Criar pedido de suporte**.
   
    ![Novo pedido de suporte](./media/sql-data-warehouse-get-started-create-support-ticket/create-support-request.png)
   
    <a name="request-quota-change"></a> 
4. Selecione o **Tipo de Pedido**.
   
    ![Tipo de pedido](./media/sql-data-warehouse-get-started-create-support-ticket/request-type.png)
   
   > [!NOTE]
   > Por predefinição, cada SQL Server (por exemplo, myserver.database.windows.net) tem uma **Quota de DTU** de 45.000. Esta quota é apenas um limite de segurança. Pode aumentar a quota ao criar um pedido de suporte e selecionar *Quota* como o tipo de pedido. Para calcular as necessidades de DTU, multiplique 7,5 pelo total de [DWU][DWU] necessários. Por exemplo, se pretende alojar dois DW6000s num SQL Server, então deve pedir uma quota de DTU de 90.000.  Pode ver o consumo de DTU atual a partir do painel SQL Server no portal. Tanto as bases de dados em pausa como as que não estão em pausa contam para a quota de DTU. 
   > 
   > 
5. Selecione a **Subscrição** que aloja a base de dados com o problema que está a denunciar.
   
    ![Subscrição](./media/sql-data-warehouse-get-started-create-support-ticket/subscription.png)
6. Selecione **SQL Data Warehouse** como o Recurso.
   
    ![Recurso](./media/sql-data-warehouse-get-started-create-support-ticket/resource.png)
7. Selecione o seu [Plano de suporte do Azure][Plano de suporte do Azure].
   
   * O suporte relativo à **faturação, quota e gestão de subscrições** está disponível em todos os níveis de suporte.
   * O suporte de **Break-fix** é fornecido através de suporte para [Programador][Programador], [Standard][Standard], [Suporte Direto Profissional][Suporte Direto Profissional] ou [Premier][Premier]. Os problemas de Break-fix são problemas encontrados pelos clientes ao utilizarem o Azure, onde existe uma expetativa razoável de que a Microsoft causou o problema.
   * A **orientação para programadores** e os **serviços de aconselhamento** estão disponíveis nos níveis de [Suporte Direto Profissional][Suporte Direto Profissional] e [Premier][Premier]. 
     
     Se tiver um plano de suporte Premier, também pode comunicar problemas relacionados com o SQL Data Warehouse no [Portal online do Microsoft Premier][Portal online do Microsoft Premier].  Consulte os [Planos de suporte do Azure][Plano de suporte do Azure] para saber mais sobre os vários planos de suporte, incluindo o âmbito, os tempos de resposta, os preços, etc.  Para perguntas mais frequentes sobre o suporte do Azure, consulte [Azure support FAQs (Perguntas frequentes do suporte do Azure)][Azure support FAQs (Perguntas frequentes do suporte do Azure)].  
     
     ![Plano de suporte](./media/sql-data-warehouse-get-started-create-support-ticket/support-plan.png)
8. Selecione o **Tipo de Problema** e a **Categoria**.
   
    ![Categoria do tipo de problema](./media/sql-data-warehouse-get-started-create-support-ticket/problem-type-category.png)
9. Descreva o problema e escolha o nível de impacto comercial.
   
    ![Descrição do problema](./media/sql-data-warehouse-get-started-create-support-ticket/problem-description.png)
10. As **informações de contacto** para este pedido de suporte serão preenchidas previamente. Atualize-as, se for necessário.
    
    ![Informações de contacto](./media/sql-data-warehouse-get-started-create-support-ticket/contact-info.png)
11. Clique em **Criar** para submeter o pedido de suporte.

## <a name="monitor-a-support-ticket"></a>Monitorizar um pedido de suporte
Depois de ter submetido o pedido de suporte, a equipa de suporte do Azure irá contactá-lo. Para verificar o estado e os detalhes do pedido, clique em **Gerir pedidos de suporte** no dashboard.

![Verificar o estado](./media/sql-data-warehouse-get-started-create-support-ticket/check-status.png)

## <a name="other-resources"></a>Outros Recursos
Além disso, pode ligar-se à comunidade do SQL Data Warehouse no [Stack Overflow][Stack Overflow] ou no [Fórum MSDN do Azure SQL Data Warehouse][Fórum MSDN do Azure SQL Data Warehouse].

<!--Image references--> 

<!--Article references--> 
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units

<!--MSDN references--> 

<!--Other web references--> 
[Portal do Azure]: https://portal.azure.com/
[Plano de suporte do Azure]: https://azure.microsoft.com/support/plans/?WT.mc_id=Support_Plan_510979/  
[Programador]: https://azure.microsoft.com/support/plans/developer/  
[Standard]: https://azure.microsoft.com/support/plans/standard/  
[Suporte Direto Profissional]: https://azure.microsoft.com/support/plans/prodirect/  
[Premier]: https://azure.microsoft.com/support/plans/premier/  
[Azure support FAQs (Perguntas frequentes do suporte do Azure)]: https://azure.microsoft.com/support/faq/
[Portal online do Microsoft Premier]: https://premier.microsoft.com/
[Stack Overflow]: https://stackoverflow.com/questions/tagged/azure-sqldw/
[Fórum MSDN do Azure SQL Data Warehouse]: https://social.msdn.microsoft.com/Forums/home?forum=AzureSQLDataWarehouse/




<!--HONumber=Nov16_HO2-->


