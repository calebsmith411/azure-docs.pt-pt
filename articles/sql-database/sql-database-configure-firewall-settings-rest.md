---
title: "Azure SQL Database server-level firewall rules using the REST API (Configurar regras de firewall ao nível do servidor da Base de Dados SQL do Azure com a API REST) | Microsoft Docs"
description: "Saiba como configurar a firewall para endereços IP que acedem a bases de dados SQL do Azure."
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: fc874f3c-c623-4924-8cb7-32a8c31e666c
ms.service: sql-database
ms.custom: authentication and authorization
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: get-started-article
ms.date: 08/09/2016
ms.author: sstein
translationtype: Human Translation
ms.sourcegitcommit: a9b48f149427e5ceb69bcaa97b1bf08519499b6f
ms.openlocfilehash: cc0faa49daaafe19c71d2c765b8e865be04f81e2


---
# <a name="configure-azure-sql-database-server-level-firewall-rules-using-the-rest-api"></a>Configure Azure SQL Database server-level firewall rules using the REST API (Configurar regras de firewall ao nível do servidor da Base de Dados SQL do Azure com a API REST)
> [!div class="op_single_selector"]
> * [Descrição geral](sql-database-firewall-configure.md)
> * [Portal do Azure](sql-database-configure-firewall-settings.md)
> * [TSQL](sql-database-configure-firewall-settings-tsql.md)
> * [PowerShell](sql-database-configure-firewall-settings-powershell.md)
> * [API REST](sql-database-configure-firewall-settings-rest.md)
> 
> 

A Base de dados SQL do Microsoft Azure utiliza regras de firewall para permitir ligações para os servidores e bases de dados. Pode alterar as definições da firewall ao nível do servidor e ao nível da base de dados para a base de dados mestra ou do utilizador no seu servidor de Base de Dados SQL do Azure para seletivamente permitir o acesso à base de dados.

> [!IMPORTANT]
> Para permitir que as aplicações do Azure se liguem ao seu servidor de base de dados, as ligações do Azure têm de estar ativadas. Para obter mais informações sobre regras de firewall e como ativar ligações a partir do Azure, consulte [Azure SQL Database Firewall (Firewall de Base de Dados SQL do Azure)](sql-database-firewall-configure.md). Se estiver a efetuar ligações dentro do limite de cloud do Azure, poderá ter de abrir algumas das portas TCP adicionais. Para obter mais informações, consulte a secção **V12 of SQL Database: Outside vs inside (V12 da Base de Dados SQL: exterior vs interior)** de [Ports beyond 1433 for ADO.NET 4.5 and SQL Database V12 (Portas além de 1433 para ADO.NET 4.5 e Base de Dados SQL V12)](sql-database-develop-direct-route-ports-adonet-v12.md)
> 
> 

## <a name="manage-server-level-firewall-rules-through-rest-api"></a>Gerir regras de firewall ao nível do servidor através da API REST
1. A gestão das regras de firewall através da API REST tem de ser autenticada. Para obter informações, consulte [Guia para programadores para autorização com a API do Azure Resource Manager](../azure-resource-manager/resource-manager-api-authentication.md).
2. As regras ao nível do servidor podem ser criadas, atualizadas ou eliminadas com a API REST
   
    Para criar ou atualizar uma regra de firewall ao nível do servidor, execute o método PUT da seguinte forma:
   
        https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/firewallRules/{rule-name}?api-version={api-version}
   
    Corpo do Pedido
   
        {
         "properties": { 
            "startIpAddress": "{start-ip-address}", 
            "endIpAddress": "{end-ip-address}
            }
        } 

    Para remover uma regra de firewall ao nível do servidor existente, execute o método DELETE da seguinte forma:

        https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/firewallRules/{rule-name}?api-version={api-version}


## <a name="manage-firewall-rules-using-the-rest-api"></a>Gerir regras de firewall com a API REST
* [Criar ou Atualizar Regra de Firewall](https://msdn.microsoft.com/library/azure/mt445501.aspx)
* [Eliminar Regra de Firewall](https://msdn.microsoft.com/library/azure/mt445502.aspx)
* [Obter Regra de Firewall](https://msdn.microsoft.com/library/azure/mt445503.aspx)
* [Listar Todas as Regras da Firewall](https://msdn.microsoft.com/library/azure/mt604478.aspx)

## <a name="next-steps"></a>Passos seguintes
Para ler um artigo explicativo sobre como utilizar o Transact-SQL para criar regras de firewall ao nível do servidor e ao nível da base de dados, consulte [Configure Azure SQL Database server-level and database-level firewall rules using T-SQL (Configurar regras de firewall ao nível do servidor e ao nível da base de dados da Base de Dados SQL do Azure com o T-SQL)](sql-database-configure-firewall-settings-tsql.md). 

Para ler artigos explicativos sobre como criar regras de firewall ao nível do servidor com outros métodos, consulte: 

* [Configure Azure SQL Database server-level firewall rules using the Azure portal (Configurar regras de firewall ao nível do servidor da Base de Dados SQL do Azure com o Portal do Azure)](sql-database-configure-firewall-settings.md)
* [Configurar regras de firewall ao nível do servidor da Base de Dados SQL do Azure com o PowerShell](sql-database-configure-firewall-settings-powershell.md)

Para ver um tutorial sobre como criar uma base de dados, consulte [Create a SQL database in minutes using the Azure portal (Criar uma Base de Dados SQL em poucos minutos com o Portal do Azure)](sql-database-get-started.md).
Para obter ajuda para ligar a uma base de dados SQL do Azure a partir de aplicações de código aberto ou de terceiros, veja [Client quick-start code samples to SQL Database (Exemplos de código de início rápido de cliente para a Base de Dados SQL)](https://msdn.microsoft.com/library/azure/ee336282.aspx).
Para compreender como navegar para as bases de dados, veja [Manage database access and login security (Gerir acesso da base de dados e segurança do início de sessão)](https://msdn.microsoft.com/library/azure/ee336235.aspx).

## <a name="additional-resources"></a>Recursos adicionais
* [Proteger a sua base de dados](sql-database-security-overview.md)
* [Centro de Segurança do Motor da Base de Dados do SQL Server e da Base de Dados SQL do Azure](https://msdn.microsoft.com/library/bb510589)

<!--Image references-->
[1]: ./media/sql-database-configure-firewall-settings/AzurePortalBrowseForFirewall.png
[2]: ./media/sql-database-configure-firewall-settings/AzurePortalFirewallSettings.png
<!--anchors-->





<!--HONumber=Jan17_HO1-->


