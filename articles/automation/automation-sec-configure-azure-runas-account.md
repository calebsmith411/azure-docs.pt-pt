---
title: Configurar Conta Run As do Azure | Microsoft Docs
description: "Tutorial que o orienta através da criação, teste e exemplo de utilização da autenticação principal de segurança na Automatização do Azure."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: 
keywords: "nome principal do serviço, setspn, autenticação do azure"
ms.assetid: 2f783441-15c7-4ea0-ba27-d7daa39b1dd3
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/17/2016
ms.author: magoedte
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 1e7529de2968b2745b42001cc16b54d70b2a5b86


---
# <a name="authenticate-runbooks-with-azure-run-as-account"></a>Autenticar Runbooks com a conta Run As do Azure
Este tópico mostra como configurar uma conta de Automatização do portal do Azure, utilizando a funcionalidade da conta Run as para autenticar runbooks que gerem recursos no Azure Resource Manager ou na Gestão de Serviço do Azure.

Quando cria uma nova conta de Automatização no portal do Azure, este cria automaticamente:

* Uma conta Run As, que cria um novo principal de serviço no Azure Active Directory, um certificado e atribui o controlo de acesso baseado em funções (RBAC) de Contribuidor, que será utilizado para gerir recursos do Gestor de Recursos através de runbooks.   
* A conta Run As clássica através do carregamento de um certificado de gestão, que será utilizada para gerir a Gestão de Serviço do Azure ou recursos clássicos através de runbooks.  

Isto simplifica o processo para si e ajuda-o a começar rapidamente a criar e implementar runbooks para suportar as suas necessidades de automatização.      

Ao utilizar uma conta Run As e Run As Clássica, pode:

* Fornecer uma forma normalizada de autenticar com o Azure ao gerir os recursos do Azure Resource Manager ou do Gestão de Serviço do Azure através de runbooks no portal do Azure.  
* Automatizar a utilização de runbooks globais configurados nos Alertas do Azure.

> [!NOTE]
> A [Funcionalidade de integração de alertas](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) do Azure com Runbooks Globais de Automatização requer uma conta de Automatização que esteja configurada com uma conta Run As e Run As clássica. Pode selecionar uma conta de Automatização que já tenha uma conta Run As e Run As clássica definida ou opte por criar um nova.
> 
> 

Iremos mostrar-lhe como criar a conta de Automatização a partir do portal do Azure, atualizar uma conta de Automatização com o PowerShell e demonstrar como autenticar nos runbooks.

Antes de o fazermos, há alguns aspetos que deve compreender e considerar antes de continuar.

1. Isto não afeta as contas de Automatização existentes já criadas no modelo de implementação clássico ou do Gestor de Recursos.  
2. Isto funciona apenas para contas de Automatização criadas através do portal do Azure.  Tentar criar uma conta a partir do portal clássico não irá replicar a configuração de conta Run As.
3. Se tiver atualmente runbooks e recursos (ou seja, agendas, variáveis, etc.) que criou anteriormente para gerir recursos clássicos e pretende que esses runbooks efetuem a autenticação com a nova conta Run As clássica, terá de migrá-los para a nova conta de automatização ou atualizar a sua conta existente, utilizando o script do PowerShell abaixo.  
4. Para efetuar a autenticação com a nova conta de Automatização Run As e a conta Run As clássica, terá de modificar os runbooks existentes com o código de exemplo abaixo.  **Tenha em atenção** que a conta Run As serve de autenticação relativamente aos recursos do Gestor de Recursos utilizando o principal de serviço baseado em certificados e a conta Run As clássica serve de autenticação relativamente aos recursos de Gestão do Serviço com o certificado de gestão.     

## <a name="create-a-new-automation-account-from-the-azure-portal"></a>Criar uma nova Conta de Automatização a partir do Portal do Azure
Nesta secção, é necessário executar os seguintes passos para criar uma nova conta de Automatização do Azure a partir do portal do Azure.  Esta ação cria a conta Run As e conta Run As clássica.  

> [!NOTE]
> O utilizador que efetua estes passos *tem* de ser membro da função de Administradores da Subscrição e o coadministrador da subscrição que está a conceder acesso à subscrição para o utilizador.  O utilizador tem também de ser adicionado como Utilizador para esse Active Directory predefinido das subscrições; a conta não necessita de ser atribuídos a uma função com privilégios.
> 
> 

1. Inicie sessão no portal do Azure com uma conta que seja membro da função de Administradores da Subscrição e o coadministrador da subscrição.
2. Selecione **Contas de Automatização**.
3. No painel Contas de Automatização, clique em **Adicionar**.<br>![Adicionar Conta de Automatização](media/automation-sec-configure-azure-runas-account/create-automation-account-properties-b.png)
   
   > [!NOTE]
   > Se vir o seguinte aviso no painel **Adicionar Conta de Automatização**, isto acontece porque a conta não é membro da função de Administradores da Subscrição nem coadministradora da subscrição.<br>![Adicionar Aviso de Conta de Automatização](media/automation-sec-configure-azure-runas-account/create-account-without-perms.png)
   > 
   > 
4. No painel **Adicionar Conta de Automatização**, no tipo de caixa **Nome**, escreva um nome para a sua nova conta de Automatização.
5. Se tiver mais do que uma subscrição, especifique uma para a nova conta, bem como um **Grupo de recursos** novo ou existente e uma **Localização** do datacenter do Azure.
6. Verifique se valor **Sim** está selecionado para a opção **Criar conta Run As do Azure** e clique no botão **Criar**.  
   
   > [!NOTE]
   > Se optar por não criar a conta Run As, selecionando a opção **Não**, será apresentada uma mensagem de aviso no painel **Adicionar Conta de Automatização**.  Enquanto a conta é criada no portal do Azure, não terá uma identidade de autenticação correspondente no seu serviço de diretório de subscrições clássico ou do Gestor de Recursos e, por isso, não terá acesso a recursos na sua subscrição.  Isto irá impedir que todos os runbooks relacionados com esta conta tenham capacidade para autenticar e executar tarefas relativamente no que respeita a recursos nesses modelos de implementação.
   > 
   > ![Adicionar Aviso de Conta de Automatização](media/automation-sec-configure-azure-runas-account/create-account-decline-create-runas-msg.png)<br>
   > Se o principal de serviço não for criado, a função Contribuidor não será atribuída.
   > 
   > 
7. Enquanto o Azure cria a conta de automatização, pode acompanhar o progresso em **Notificações** a partir do menu.

### <a name="resources-included"></a>Recursos incluídos
Quando a conta de Automatização é criada com sucesso, são criados vários recursos automaticamente para si.  A tabela seguinte resume os recursos para a conta Run As.<br>

| Recurso | Descrição |
| --- | --- |
| Runbook AzureAutomationTutorial |Um runbook do PowerShell de exemplo que demonstra como efetuar a autenticação com a conta Run As e obtém todos os recursos do Gestor de Recursos. |
| AzureAutomationTutorialScript Runbook |Um runbook do PowerShell de exemplo que demonstra como efetuar a autenticação com a conta Run As e obtém todos os recursos do Gestor de Recursos. |
| AzureRunAsCertificate |Recurso de certificado criado automaticamente durante a criação da conta de Automatização ou utilizando o script do PowerShell abaixo para uma conta existente.  Permite-lhe autenticar com o Azure, para que possa gerir recursos do Azure Resource Manager a partir dos runbooks.  Este certificado tem um tempo de vida de um ano. |
| AzureRunAsConnection |Recurso de ligação criado automaticamente durante a criação da conta de Automatização ou utilizando o script do PowerShell abaixo para uma conta existente. |

A tabela seguinte resume os recursos da conta Run As clássica.<br>

| Recurso | Descrição |
| --- | --- |
| AzureClassicAutomationTutorial Runbook |Um runbook de exemplo que obtém todas as VMs clássicas numa subscrição utilizando a conta Run As clássica (certificado) e, em seguida, devolve o nome e o estado da VM. |
| AzureClassicAutomationTutorial Script Runbook |Um runbook de exemplo que obtém todas as VMs clássicas numa subscrição utilizando a conta Run As clássica (certificado) e, em seguida, devolve o nome e o estado da VM. |
| AzureClassicRunAsCertificate |Recurso de certificado criado automaticamente utilizado para autenticar com o Azure, para que possa gerir os recursos clássicos do Azure a partir de runbooks.  Este certificado tem um tempo de vida de um ano. |
| AzureClassicRunAsConnection |Recurso de ligação criado automaticamente, que é utilizado para autenticar com o Azure, para que possa gerir os recursos clássicos do Azure a partir de runbooks. |

## <a name="verify-run-as-authentication"></a>Verificar a autenticação Run As
Em seguida, iremos efetuar um pequeno teste para confirmar se é possível efetuar a autenticação com êxito utilizando a nova conta Run As.     

1. No Portal do Azure, abra a conta de Automatização criada anteriormente.  
2. Clique no mosaico **Runbooks** para abrir a lista de runbooks.
3. Selecione o runbook **AzureAutomationTutorialScript** e, em seguida, clique em **Iniciar** para iniciar o runbook.  Irá receber um aviso a confirmar que pretende iniciar o runbook.
4. Uma [tarefa do runbook](automation-runbook-execution.md) é criada, o painel Tarefa é apresentado e o estado da tarefa é apresentado no mosaico **Resumo da Tarefa**.  
5. O estado da tarefa começará como *Em fila* com a indicação de que está à espera que uma função de trabalho de runbook na nuvem fique disponível. Em seguida, irá mudar para *A iniciar* quando uma função de trabalho reivindicar a tarefa e, em seguida, *A executar* quando o runbook começar a ser executado.  
6. Quando tiver concluído a tarefa de runbook, vemos um estado de **Concluído**.<br> ![Teste do Runbook de Principal de Segurança](media/automation-sec-configure-azure-runas-account/job-summary-automationtutorialscript.png)<br>
7. Para ver os resultados detalhados do runbook, clique no mosaico **Saída**.
8. No painel **Saída**, deve verificar que foi autenticado com sucesso e devolveu uma lista de todos os recursos disponíveis no grupo de recursos.
9. Feche o painel **Saída** para voltar para o painel **Resumo da Tarefa**.
10. Feche o **Resumo da tarefa** e o painel do runbook **AzureAutomationTutorialScript** correspondente.

## <a name="verify-classic-run-as-authentication"></a>Certifique-se de autenticação Run As clássica
Em seguida, iremos efetuar um pequeno teste para confirmar que é possível autenticar com êxito utilizando a nova conta Run As clássica.     

1. No Portal do Azure, abra a conta de Automatização criada anteriormente.  
2. Clique no mosaico **Runbooks** para abrir a lista de runbooks.
3. Selecione o runbook **AzureClassicAutomationTutorialScript** e, em seguida, clique em **Iniciar** para iniciar o runbook.  Irá receber um aviso a confirmar que pretende iniciar o runbook.
4. Uma [tarefa do runbook](automation-runbook-execution.md) é criada, o painel Tarefa é apresentado e o estado da tarefa é apresentado no mosaico **Resumo da Tarefa**.  
5. O estado da tarefa começará como *Em fila* com a indicação de que está à espera que uma função de trabalho de runbook na nuvem fique disponível. Em seguida, irá mudar para *A iniciar* quando uma função de trabalho reivindicar a tarefa e, em seguida, *A executar* quando o runbook começar a ser executado.  
6. Quando tiver concluído a tarefa de runbook, vemos um estado de **Concluído**.<br> ![Teste do Runbook de Principal de Segurança](media/automation-sec-configure-azure-runas-account/job-summary-automationclassictutorialscript.png)<br>
7. Para ver os resultados detalhados do runbook, clique no mosaico **Saída**.
8. No painel **Saída**, deve verificar que foi autenticado com sucesso e devolveu uma lista de todas as VMs clássicas na subscrição.
9. Feche o painel **Saída** para voltar para o painel **Resumo da Tarefa**.
10. Feche o **Resumo da Tarefa** e painel do runbook **AzureClassicAutomationTutorialScript** correspondente.

## <a name="update-an-automation-account-using-powershell"></a>Atualizar uma Conta de Automatização com o PowerShell
Aqui fornecemos-lhe a opção para utilizar o PowerShell para atualizar a sua conta de automatização existente se:

1. É criada uma conta de Automatização, mas recusou-se a criar a conta Run As
2. Já tem uma conta de Automatização para gerir os recursos do Gestor de Recursos e pretende atualizá-la para incluir a conta Run As para autenticação de runbook
3. Já tem uma conta de utomatização para gerir os recursos clássicos e pretende atualizá-lo para utilizar a Run As clássica em vez de criar uma nova conta e migrar os runbooks e recursos para o mesmo   

Antes de continuar, verifique o seguinte:

1. Transferiu e instalou [Windows Management Framework (WMF) 4.0](https://www.microsoft.com/download/details.aspx?id=40855) se estiver a executar o Windows 7.   
    Se estiver a executar o Windows Server 2012 R2, Windows Server 2012, Windows 2008 R2, Windows 8.1 e Windows 7 SP1, o [Windows Management Framework 5.0](https://www.microsoft.com/download/details.aspx?id=50395) está disponível para instalação.
2. Azure PowerShell 1.0. Para obter informações sobre esta versão e como a instalar, consulte o artigo [Como instalar e configurar o Azure PowerShell](../powershell-install-configure.md).
3. Criou uma conta de automatização.  Esta conta vai ser referenciada como valor para os parâmetros - AutomationAccountName e ApplicationDisplayName - em ambos os scripts abaixo.

Para obter os valores de *SubscriptionID*, *ResourceGroup*, e *AutomationAccountName*, que são os parâmetros necessários para os scripts, no portal do Azure selecione a sua conta de automatização do painel **Conta de automatização** e selecione **Todas as definições**.  A partir do painel **Todas as definições**, em **Definições da Conta** selecione **Propriedades**.  No painel **Propriedades**, pode ter em atenção estes valores.<br> ![Propriedades da Conta de Automatização](media/automation-sec-configure-azure-runas-account/automation-account-properties.png)  

### <a name="create-run-as-account-powershell-script"></a>Criar o script do PowerShell da Conta Run As
O script do PowerShell abaixo irá configurar o seguinte:

* Uma aplicação do Azure AD que será autenticada com o certificado autoassinado, crie uma conta do principal de serviço para esta aplicação no Azure AD e atribuída a função Contribuidor (pode alterar isto para Proprietário ou função) para esta conta na sua subscrição atual.  Para obter mais informações, consulte o [Controlo de acesso baseado em funções na Automatização do Azure](automation-role-based-access-control.md).
* Um recurso de certificado de Automatização na conta de automatização especificada com o nome **AzureRunAsCertificate**, que contém o certificado utilizado no principal de serviço.
* Um recurso de ligação de Automatização na conta de automatização especificada com o nome **AzureRunAsConnection**, que contém applicationId, tenantId, subscriptionId e o thumbprint do certificado.    

Os passos abaixo irão guiá-lo através do processo de execução do script.

1. Guarde o seguinte script no seu computador.  Neste exemplo, guarde-o com o nome de ficheiro **New-AzureServicePrincipal.ps1**.  
   
        #Requires -RunAsAdministrator
        Param (
        [Parameter(Mandatory=$true)]
        [String] $ResourceGroup,
   
        [Parameter(Mandatory=$true)]
        [String] $AutomationAccountName,
   
        [Parameter(Mandatory=$true)]
        [String] $ApplicationDisplayName,
   
        [Parameter(Mandatory=$true)]
        [String] $SubscriptionId,
   
        [Parameter(Mandatory=$true)]
        [String] $CertPlainPassword,
   
        [Parameter(Mandatory=$false)]
        [int] $NoOfMonthsUntilExpired = 12
        )
   
        Login-AzureRmAccount
        Import-Module AzureRM.Resources
        Select-AzureRmSubscription -SubscriptionId $SubscriptionId
   
        $CurrentDate = Get-Date
        $EndDate = $CurrentDate.AddMonths($NoOfMonthsUntilExpired)
        $KeyId = (New-Guid).Guid
        $CertPath = Join-Path $env:TEMP ($ApplicationDisplayName + ".pfx")
   
        $Cert = New-SelfSignedCertificate -DnsName $ApplicationDisplayName -CertStoreLocation cert:\LocalMachine\My -KeyExportPolicy Exportable -Provider "Microsoft Enhanced RSA and AES Cryptographic Provider"
   
        $CertPassword = ConvertTo-SecureString $CertPlainPassword -AsPlainText -Force
        Export-PfxCertificate -Cert ("Cert:\localmachine\my\" + $Cert.Thumbprint) -FilePath $CertPath -Password $CertPassword -Force | Write-Verbose
   
        $PFXCert = New-Object -TypeName System.Security.Cryptography.X509Certificates.X509Certificate -ArgumentList @($CertPath, $CertPlainPassword)
        $KeyValue = [System.Convert]::ToBase64String($PFXCert.GetRawCertData())
   
        $KeyCredential = New-Object  Microsoft.Azure.Commands.Resources.Models.ActiveDirectory.PSADKeyCredential
        $KeyCredential.StartDate = $CurrentDate
        $KeyCredential.EndDate= $EndDate
        $KeyCredential.KeyId = $KeyId
        $KeyCredential.Type = "AsymmetricX509Cert"
        $KeyCredential.Usage = "Verify"
        $KeyCredential.Value = $KeyValue
   
        # Use Key credentials
        $Application = New-AzureRmADApplication -DisplayName $ApplicationDisplayName -HomePage ("http://" + $ApplicationDisplayName) -IdentifierUris ("http://" + $KeyId) -KeyCredentials $keyCredential
   
        New-AzureRMADServicePrincipal -ApplicationId $Application.ApplicationId | Write-Verbose
        Get-AzureRmADServicePrincipal | Where {$_.ApplicationId -eq $Application.ApplicationId} | Write-Verbose
   
        $NewRole = $null
        $Retries = 0;
        While ($NewRole -eq $null -and $Retries -le 6)
        {
           # Sleep here for a few seconds to allow the service principal application to become active (should only take a couple of seconds normally)
           Sleep 5
           New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $Application.ApplicationId | Write-Verbose -ErrorAction SilentlyContinue
           Sleep 10
           $NewRole = Get-AzureRMRoleAssignment -ServicePrincipalName $Application.ApplicationId -ErrorAction SilentlyContinue
           $Retries++;
        }
   
        # Get the tenant id for this subscription
        $SubscriptionInfo = Get-AzureRmSubscription -SubscriptionId $SubscriptionId
        $TenantID = $SubscriptionInfo | Select TenantId -First 1
   
        # Create the automation resources
        New-AzureRmAutomationCertificate -ResourceGroupName $ResourceGroup -AutomationAccountName $AutomationAccountName -Path $CertPath -Name AzureRunAsCertificate -Password $CertPassword -Exportable | write-verbose
   
        # Create a Automation connection asset named AzureRunAsConnection in the Automation account. This connection uses the service principal.
        $ConnectionAssetName = "AzureRunAsConnection"
        Remove-AzureRmAutomationConnection -ResourceGroupName $ResourceGroup -AutomationAccountName $AutomationAccountName -Name $ConnectionAssetName -Force -ErrorAction SilentlyContinue
        $ConnectionFieldValues = @{"ApplicationId" = $Application.ApplicationId; "TenantId" = $TenantID.TenantId; "CertificateThumbprint" = $Cert.Thumbprint; "SubscriptionId" = $SubscriptionId}
        New-AzureRmAutomationConnection -ResourceGroupName $ResourceGroup -AutomationAccountName $AutomationAccountName -Name $ConnectionAssetName -ConnectionTypeName AzureServicePrincipal -ConnectionFieldValues $ConnectionFieldValues
2. No seu computador, inicie o **Windows PowerShell** a partir do ecrã **Iniciar** ecrã com direitos de utilizador elevados.
3. A partir da shell de linha de comandos do PowerShell elevada, navegue para a pasta que contém o script criado no Passo 1 e execute o script alterando os valores para os parâmetros *– ResourceGroup*, *- AutomationAccountName*, *- ApplicationDisplayName*, *- SubscriptionId* e *- CertPlainPassword*.<br>
   
   > [!NOTE]
   > Será solicitado para autenticar com o Azure, depois de executar o script. Tem de iniciar sessão com uma conta que seja membro da função de Administradores da Subscrição e o coadministrador da subscrição.
   > 
   > 
   
        .\New-AzureServicePrincipal.ps1 -ResourceGroup <ResourceGroupName>
        -AutomationAccountName <NameofAutomationAccount> `
        -ApplicationDisplayName <DisplayNameofAutomationAccount> `
        -SubscriptionId <SubscriptionId> `
        -CertPlainPassword "<StrongPassword>"  
   <br>

Após a conclusão do script com êxito, consulte o [código de exemplo](#sample-code-to-authenticate-with-resource-manager-resources) abaixo para autenticar com recursos do Gestor de Recursos e validar a configuração da credencial.

### <a name="create-classic-run-as-account-powershell-script"></a>Criar o script do PowerShell da conta Run As Clássica
O script do PowerShell abaixo irá configurar o seguinte:

* Um recurso de certificado de Automatização na conta de automatização especificada com o nome **AzureClassicRunAsCertificate**, que contém o certificado utilizado para autenticar os seus runbooks.
* Um recurso de ligação de Automatização na conta de automatização especificada com o nome **AzureRunAsConnection**, que contém o nome da subscrição, o subscriptionId e o nome do recurso do certificado.

O script irá criar um certificado de gestão autoassinado e guardá-lo na pasta de ficheiros temporários no seu computador com o perfil de utilizador utilizado para executar a sessão do PowerShell - *%USERPROFILE%\AppData\Local\Temp*.  Após a execução do script, terá de carregar o certificado de gestão do Azure no arquivo de gestão para que seja criada a subscrição da conta de Automatização.  Os passos abaixo irão guiá-lo através do processo de execução do script e carregar o certificado.  

1. Guarde o seguinte script no seu computador.  Neste exemplo, guarde-o com o nome de ficheiro **New-AzureClassicRunAsAccount.ps1**.
   
        #Requires -RunAsAdministrator
        Param (
        [Parameter(Mandatory=$true)]
        [String] $ResourceGroup,
   
        [Parameter(Mandatory=$true)]
        [String] $AutomationAccountName,
   
        [Parameter(Mandatory=$true)]
        [String] $ApplicationDisplayName,
   
        [Parameter(Mandatory=$true)]
        [String] $SubscriptionId,
   
        [Parameter(Mandatory=$true)]
        [String] $CertPlainPassword,
   
        [Parameter(Mandatory=$false)]
        [int] $NoOfMonthsUntilExpired = 12
        )
   
        Login-AzureRmAccount
        Import-Module AzureRM.Resources
        $Subscription = Select-AzureRmSubscription -SubscriptionId $SubscriptionId
        $SubscriptionName = $subscription.Subscription.SubscriptionName
   
        $CurrentDate = Get-Date
        $EndDate = $CurrentDate.AddMonths($NoOfMonthsUntilExpired)
        $KeyId = (New-Guid).Guid
        $CertPath = Join-Path $env:TEMP ($ApplicationDisplayName + ".pfx")
        $CertPathCer = Join-Path $env:TEMP ($ApplicationDisplayName + ".cer")
   
        $Cert = New-SelfSignedCertificate -DnsName $ApplicationDisplayName -CertStoreLocation cert:\LocalMachine\My -KeyExportPolicy Exportable -Provider "Microsoft Enhanced RSA and AES Cryptographic Provider"
   
        $CertPassword = ConvertTo-SecureString $CertPlainPassword -AsPlainText -Force
        Export-PfxCertificate -Cert ("Cert:\localmachine\my\" + $Cert.Thumbprint) -FilePath $CertPath -Password $CertPassword -Force | Write-Verbose
        Export-Certificate -Cert ("Cert:\localmachine\my\" + $Cert.Thumbprint) -FilePath $CertPathCer -Type CERT | Write-Verbose
   
        # Create the automation resources
        $ClassicCertificateAssetName = "AzureClassicRunAsCertificate"
        New-AzureRmAutomationCertificate -ResourceGroupName $ResourceGroup -AutomationAccountName $AutomationAccountName -Path $CertPath -Name $ClassicCertificateAssetName  -Password $CertPassword -Exportable | write-verbose
   
        # Create a Automation connection asset named AzureClassicRunAsConnection in the Automation account. This connection uses the ClassicCertificateAssetName.
        $ConnectionAssetName = "AzureClassicRunAsConnection"
        Remove-AzureRmAutomationConnection -ResourceGroupName $ResourceGroup -AutomationAccountName $AutomationAccountName -Name $ConnectionAssetName -Force -ErrorAction SilentlyContinue
        $ConnectionFieldValues = @{"SubscriptionName" = $SubscriptionName; "SubscriptionId" = $SubscriptionId; "CertificateAssetName" = $ClassicCertificateAssetName}
        New-AzureRmAutomationConnection -ResourceGroupName $ResourceGroup -AutomationAccountName $AutomationAccountName -Name $ConnectionAssetName -ConnectionTypeName AzureClassicCertificate -ConnectionFieldValues $ConnectionFieldValues
   
        Write-Host -ForegroundColor red "Please upload the cert $CertPathCer to the Management store by following the steps below."
        Write-Host -ForegroundColor red "Log in to the Microsoft Azure Management portal (https://manage.windowsazure.com) and select Settings -> Management Certificates."
        Write-Host -ForegroundColor red "Then click Upload and upload the certificate $CertPathCer"
2. No seu computador, inicie o **Windows PowerShell** a partir do ecrã **Iniciar** ecrã com direitos de utilizador elevados.  
3. A partir da shell de linha de comandos do PowerShell elevada, navegue para a pasta que contém o script criado no Passo 1 e execute o script alterando os valores para os parâmetros *– ResourceGroup*, *- AutomationAccountName*, *- ApplicationDisplayName*, *- SubscriptionId* e *- CertPlainPassword*.<br>
   
   > [!NOTE]
   > Será solicitado para autenticar com o Azure, depois de executar o script. Tem de iniciar sessão com uma conta que seja membro da função de Administradores da Subscrição e o coadministrador da subscrição.
   > 
   > 
   
        .\New-AzureClassicRunAsAccount.ps1 -ResourceGroup <ResourceGroupName>
        -AutomationAccountName <NameofAutomationAccount> `
        -ApplicationDisplayName <DisplayNameofAutomationAccount> `
        -SubscriptionId <SubscriptionId> `
        -CertPlainPassword "<StrongPassword>"

Após a conclusão do script com êxito, terá de copiar o certificado que criou na sua pasta do perfil de utilizador **Temp**.  Siga os passos para [carregar um certificado da API de gestão](../azure-api-management-certs.md) no portal clássico do Azure e, em seguida, consulte o [código de exemplo](#sample-code-to-authenticate-with-service-management-resources) para validar a configuração de credenciais com recursos de Gestão do Serviço.

## <a name="sample-code-to-authenticate-with-resource-manager-resources"></a>Código de exemplo para autenticar com recursos do Resource Manager
Pode utilizar o código de exemplo atualizado abaixo, retirado do runbook de exemplo **AzureAutomationTutorialScript**, para efetuar a autenticação com a conta Run As para gerir os recursos do Gestor de Recursos com os runbooks.   

    $connectionName = "AzureRunAsConnection"
    $SubId = Get-AutomationVariable -Name 'SubscriptionId'
    try
    {
       # Get the connection "AzureRunAsConnection "
       $servicePrincipalConnection=Get-AutomationConnection -Name $connectionName         

       "Logging in to Azure..."
       Add-AzureRmAccount `
         -ServicePrincipal `
         -TenantId $servicePrincipalConnection.TenantId `
         -ApplicationId $servicePrincipalConnection.ApplicationId `
         -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint
       "Setting context to a specific subscription"     
       Set-AzureRmContext -SubscriptionId $SubId              
    }
    catch {
        if (!$servicePrincipalConnection)
        {
           $ErrorMessage = "Connection $connectionName not found."
           throw $ErrorMessage
         } else{
            Write-Error -Message $_.Exception
            throw $_.Exception
         }
    }


O script inclui duas linhas adicionais de código para suportar a referência a um contexto de subscrição para que possa trabalhar facilmente entre várias subscrições. Um recurso variável com o nome SubscriptionId contém o ID da subscrição e, após a declaração de cmdlet Add-AzureRmAccount, o [cmdlet Set-AzureRmContext](https://msdn.microsoft.com/library/mt619263.aspx) é declarado com o conjunto de parâmetros *-SubscriptionId*. Se o nome da variável for demasiado genérico, pode rever o nome da variável para incluir um prefixo ou outra convenção de nomenclatura para ser mais fácil de identificar para os fins pretendidos. Em alternativa, pode utilizar o parâmetro definido -SubscriptionName em vez de -SubscriptionId com um recurso de variável correspondente.  

Repare que o cmdlet utilizado para autenticar no runbook - **Add-AzureRmAccount**, utiliza o conjunto de parâmetros *ServicePrincipalCertificate*.  Autentica utilizando o certificado de serviço principal, não as credenciais.  

## <a name="sample-code-to-authenticate-with-service-management-resources"></a>Código de exemplo para autenticar com recursos do Service Management
Pode utilizar o código de exemplo atualizado abaixo, retirado do runbook de exemplo **AzureClassicAutomationTutorialScript** para efetuar a autenticação com a conta Run As para gerir os recursos clássicos dos runbooks.

    $ConnectionAssetName = "AzureClassicRunAsConnection"
    # Get the connection
    $connection = Get-AutomationConnection -Name $connectionAssetName        

    # Authenticate to Azure with certificate
    Write-Verbose "Get connection asset: $ConnectionAssetName" -Verbose
    $Conn = Get-AutomationConnection -Name $ConnectionAssetName
    if ($Conn -eq $null)
    {
       throw "Could not retrieve connection asset: $ConnectionAssetName. Assure that this asset exists in the Automation account."
    }

    $CertificateAssetName = $Conn.CertificateAssetName
    Write-Verbose "Getting the certificate: $CertificateAssetName" -Verbose
    $AzureCert = Get-AutomationCertificate -Name $CertificateAssetName
    if ($AzureCert -eq $null)
    {
       throw "Could not retrieve certificate asset: $CertificateAssetName. Assure that this asset exists in the Automation account."
    }

    Write-Verbose "Authenticating to Azure with certificate." -Verbose
    Set-AzureSubscription -SubscriptionName $Conn.SubscriptionName -SubscriptionId $Conn.SubscriptionID -Certificate $AzureCert
    Select-AzureSubscription -SubscriptionId $Conn.SubscriptionID


## <a name="next-steps"></a>Passos seguintes
* Para mais informações sobre Principais de Serviço, consulte a [Objetos da Aplicação e Objetos de Principais de Serviço](../active-directory/active-directory-application-objects.md).
* Para obter mais informações sobre o Controlo de Acesso baseado em Funções na Automatização do Azure, consulte o [Controlo de acesso baseado em funções da Automatização do Azure](automation-role-based-access-control.md).
* Para obter mais informações sobre certificados e serviços do Azure, veja [Descrição geral de certificados para serviços de nuvem do Azure](../cloud-services/cloud-services-certs-create.md)




<!--HONumber=Nov16_HO2-->


