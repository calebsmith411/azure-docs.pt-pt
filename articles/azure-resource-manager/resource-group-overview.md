---
title: "Descrição geral do Azure Resource Manager | Microsoft Docs"
description: "Descreve como utilizar o Azure Resource Manager para a implementação, a gestão e o controlo de acesso de recursos no Azure."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 76df7de1-1d3b-436e-9b44-e1b3766b3961
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/12/2016
ms.author: tomfitz
translationtype: Human Translation
ms.sourcegitcommit: dabe7d9796ab24a257ea904bc5d978cb71d7e149
ms.openlocfilehash: 1733edf961c2ce1297fc148d3a844ce141f5d7c2


---
# <a name="azure-resource-manager-overview"></a>Descrição geral do Azure Resource Manager
 Normalmente, a infraestrutura da sua aplicação é composta por vários componentes, como uma máquina virtual, uma conta do Storage e uma rede virtual, ou uma aplicação Web, uma base de dados, um servidor de base de dados e serviços de terceiros. Não vê estes componentes como entidades separadas. Em vez disso, vê-os como partes relacionadas e interdependentes de uma única entidade. Deve implementá-los, geri-los e monitorizá-los como um grupo. O Azure Resource Manager permite trabalhar com os recursos na sua solução como um grupo. Pode implementar, atualizar ou eliminar todos os recursos da sua solução numa operação única e coordenada. Utiliza um modelo para a implementação e esse modelo pode funcionar para ambientes diferentes, como de teste e produção. O Resource Manager fornece funcionalidades de segurança, auditoria e etiquetagem para o ajudar a gerir os recursos após a implementação. 

## <a name="terminology"></a>Terminologia
Se é a primeira vez que utiliza o Azure Resource Manager, existem alguns termos com os quais poderá não estar familiarizado.

* **recurso** -um item gerível que está disponível através do Azure. Alguns recursos comuns são uma máquina virtual, conta de armazenamento, aplicação web, base de dados e rede virtual, mas existem muitos mais.
* **grupo de recursos** - um contentor que retém recursos relacionados para uma solução do Azure. O grupo de recursos pode incluir todos os recursos para a solução ou apenas os recursos que pretende gerir como um grupo. Decida como pretende atribuir recursos a grupos de recursos com base no que é mais adequado para a sua organização. Veja [Grupos de recursos](#resource-groups).
* **fornecedor de recursos** - Um serviço que fornece os recursos que pode implementar e gerir através do Resource Manager. Cada fornecedor de recursos oferece operações para trabalhar com os recursos implementados. Alguns fornecedores de recursos comuns são Microsoft.Compute, que fornece o recurso de máquina virtual, o Microsoft.Storage, que fornece o recurso de conta de armazenamento, e o Microsoft.Web que fornece recursos relacionados com aplicações Web. Veja [Fornecedores de recursos](#resource-providers).
* **Modelo do Resource Manager** - Ficheiro de um JavaScript Object Notation (JSON) que define um ou mais recursos para implementar num grupo de recursos. Também define dependências entre os recursos implementados. O modelo pode ser utilizado para implementar os recursos de forma consistente e repetida. Veja [Implementação de modelos](#template-deployment).
* **sintaxe declarativa** - sintaxe que permite afirmar "Aqui está o que pretendo criar" sem ter de escrever a sequência de comandos de programação para criá-la. O modelo do Resource Manager é um exemplo de sintaxe declarativa. No ficheiro, é possível definir as propriedades da infraestrutura de implementação para o Azure. 

## <a name="the-benefits-of-using-resource-manager"></a>Vantagens da utilização do Resource Manager
O Resource Manager oferece várias vantagens:

* Pode implementar, gerir e monitorizar todos os recursos da sua solução como um grupo, em vez de os processar individualmente.
* Pode implementar repetidamente a solução durante todo o ciclo de vida de desenvolvimento e ter a confiança de que os recursos são implementados num estado consistente.
* Pode gerir a sua infraestrutura através de modelos declarativos em vez de scripts.
* Pode definir as dependências entre os recursos, de modo a que sejam implementados na ordem correta.
* Pode aplicar o controlo de acesso a todos os serviços no seu grupo de recursos porque o Controlo de Acesso Baseado em Funções (RBAC) está integrado de forma nativa na plataforma de gestão.
* Pode aplicar etiquetas a recursos para organizar logicamente todos os recursos na sua subscrição.
* Pode clarificar a faturação da sua organização visualizando os custos de um grupo de recursos partilhando a mesma etiqueta.  

O Resource Manager proporciona uma nova forma de implementar e gerir as suas soluções. Se utilizou o modelo de implementação anterior e pretende obter informações sobre as alterações, consulte [Compreender a implementação do Resource Manager e a implementação clássica](resource-manager-deployment-model.md).

## <a name="consistent-management-layer"></a>Camada de gestão consistente
O Resource Manager fornece uma camada de gestão consistente para as tarefas efetuadas através do Azure PowerShell, CLI do Azure, portal do Azure, API REST e ferramentas de desenvolvimento. Todas as ferramentas utilizam um conjunto comum de operações. Pode utilizar as ferramentas mais adequadas para si e alternadamente sem confusão. 

A imagem seguinte mostra como todas as ferramentas interagem com a mesma API do Azure Resource Manager. A API transmite pedidos ao serviço Resource Manager, o qual autentica e autoriza os pedidos. em seguida, o Resource Manager encaminha os pedidos para os fornecedores de recursos adequados.

![Modelo de pedido do Resource Manager](./media/resource-group-overview/consistent-management-layer.png)

## <a name="guidance"></a>Orientação
As seguintes sugestões ajudam a tirar o máximo partido do Resource Manager ao trabalhar com as suas soluções.

1. Defina e implemente a infraestrutura através da sintaxe declarativa nos modelos do Resource Manager, em vez de utilizar comandos imperativos.
2. Defina todos os passos de implementação e configuração no modelo. A configuração da sua solução não deverá ter quaisquer passos manuais.
3. Execute comandos imperativo para gerir os recursos, tal como para iniciar ou parar uma aplicação ou uma máquina.
4. Disponha os recursos com o mesmo ciclo de vida num grupo de recursos. Utilize etiquetas para todas as outras organizações de recursos.

Para obter recomendações sobre modelos, veja o artigo [Melhores práticas para criar modelos do Azure Resource Manager](resource-manager-template-best-practices.md).

Para obter documentação de orientação sobre como as empresas podem utilizar o Resource Manager para gerir subscrições de forma eficaz, consulte [Azure enterprise scaffold - prescriptive subscription governance (Andaime empresarial do Azure - governação de subscrições prescritivas)](resource-manager-subscription-governance.md).

## <a name="resource-groups"></a>Grupos de recursos
Existem alguns fatores importantes a considerar ao definir o grupo de recursos:

1. Todos os recursos do grupo devem partilhar o mesmo ciclo de vida. Implemente-os, atualize-os e elimine-os em conjunto. Se um recurso, tal como um servidor de base de dados, tiver de existir num ciclo de implementação diferente, deve ser colocado noutro grupo de recursos.
2. Cada recurso só pode existir num grupo de recursos.
3. Pode adicionar ou remover um recurso de um grupo de recursos em qualquer altura.
4. Pode mover um recurso de um grupo de recursos para outro grupo. Para obter mais informações, consulte [Mover recursos para um novo grupo de recursos ou subscrição](resource-group-move-resources.md).
5. Um grupo de recursos pode conter recursos que residem em regiões diferentes.
6. Um grupo de recursos pode ser utilizado para definir o âmbito do controlo de acesso para ações administrativas.
7. Um recurso pode interagir com recursos de outros grupos de recursos. Esta interação é comum quando os dois recursos estão relacionados mas não partilham o mesmo ciclo de vida (por exemplo, aplicações Web a ligar a uma base de dados).

Ao criar um grupo de recursos, deve fornecer uma localização para esse grupo de recursos. Pode perguntar-se, "Porque é que um grupo de recursos necessita de uma localização? E, se os recursos podem ter diferentes localizações em relação ao grupo de recursos, por que motivo é que a localização do grupo de recursos é sequer relevante?" O grupo de recursos armazena metadados sobre os recursos. Por conseguinte, quando especifica uma localização para o grupo de recursos, está a especificar onde esses metadados estão armazenados. Por motivos de conformidade, poderá ter de certificar que os dados estão armazenados numa determinada região.

## <a name="resource-providers"></a>Fornecedores de recursos
Cada fornecedor de recursos oferece um conjunto de recursos e operações para trabalhar com a um serviço do Azure. Por exemplo, se pretende armazenar chaves e segredos, trabalha com o fornecedor de recursos **Microsoft.KeyVault**. Este fornecedor de recursos oferece um tipo de recurso denominado **vaults** para criar o cofre de chaves e um tipo de recurso denominado **vaults/secrets** para criar um segredo no cofre de chaves. 

Antes de começar com a implementação de recursos, deve tomar conhecimento dos fornecedores de recursos disponíveis. Saber os nomes dos recursos e os fornecedores de recursos ajuda-o a definir os recursos que pretende implementar no Azure.

Pode ver todos os fornecedores de recursos através do portal. No painel da sua subscrição, selecione **Fornecedores de recursos**:

![veja fornecedores de recursos](./media/resource-group-overview/view-resource-providers.png)

Pode obter todos os fornecedores de recursos com o seguinte cmdlet do PowerShell:

    Get-AzureRmResourceProvider -ListAvailable

Ou, com CLI do Azure, pode obter todos os fornecedores de recursos com o seguinte comando:

    azure provider list

Pode consultar a lista devolvida de fornecedores de recursos que tem de utilizar.

Para obter detalhes sobre um fornecedor de recursos, adicione o espaço de nomes do fornecedor para o comando. O comando devolve os tipos de recurso suportados para o fornecedor de recursos e as localizações suportadas e versões de API para cada tipo de recurso. O seguinte cmdlet do PowerShell obtém os detalhes sobre Microsoft.Compute:

    (Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Compute).ResourceTypes

Ou, com o CLI do Azure, obtenha os tipos de recurso suportados, as localizações e as versões de API para Microsoft.Compute, com o seguinte comando:

    azure provider show Microsoft.Compute --json > c:\Azure\compute.json

Para obter mais informações, consulte [Resource Manager providers, regions, API versions, and schemas (Fornecedores do Resource Manager, regiões, versões de API e esquemas)](resource-manager-supported-services.md).

## <a name="template-deployment"></a>Implementação de modelos
Com o Resource Manager, pode criar um modelo (no formato JSON) que define a infraestrutura e configuração da sua solução do Azure. Ao utilizar um modelo, pode implementar repetidamente a solução durante o ciclo de vida da mesma e ter a confiança de que os recursos são implementados num estado consistente. Quando cria uma solução a partir do portal, esta inclui automaticamente um modelo de implementação. Não é necessário criar o modelo a partir do zero porque pode começar com o modelo para a sua solução e personalizá-lo de modo a satisfazer as suas necessidades específicas. Pode obter um modelo para um grupo de recursos existente ao exportar o estado atual do grupo de recursos ou ao visualizar o modelo utilizado para uma implementação específica. Visualizar o [modelo exportado](resource-manager-export-template.md) é uma forma útil de saber mais sobre a sintaxe do modelo.

Para saber mais sobre o formato do modelo e como construí-lo, consulte [Authoring Azure Resource Manager Templates (Criação de Modelos do Azure Resource Manager)](resource-group-authoring-templates.md) e [Resource Manager Template Walkthrough (Instruções do Modelo do Resource Manager)](resource-manager-template-walkthrough.md).

O Resource Manager processa o modelo como qualquer outro pedido (veja a imagem da [Camada de gestão consistente](#consistent-management-layer)). Este analisa o modelo e converte a respetiva sintaxe em operações de API REST para os fornecedores de recursos adequados. Por exemplo, quando o Resource Manager recebe um modelo com a seguinte definição de recursos:

    "resources": [
      {
        "apiVersion": "2016-01-01",
        "type": "Microsoft.Storage/storageAccounts",
        "name": "mystorageaccount",
        "location": "westus",
        "sku": {
          "name": "Standard_LRS"
        },
        "kind": "Storage",
        "properties": {
        }
      }
      ]

Converte a definição para a operação de API REST seguinte, a qual é enviada para o fornecedor de recursos Microsoft.Storage:

    PUT
    https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/mystorageaccount?api-version=2016-01-01
    REQUEST BODY
    {
      "location": "westus",
      "properties": {
      }
      "sku": {
        "name": "Standard_LRS"
      },   
      "kind": "Storage"
    }

A forma como define modelos e grupos de recursos é inteiramente da sua responsabilidade, bem como pretende gerir a sua solução. Por exemplo, pode implementar a sua aplicação de três camadas através de um único modelo para um grupo de recursos.

![modelo de três camadas](./media/resource-group-overview/3-tier-template.png)

No entanto, não é necessário definir toda a infraestrutura num único modelo. Muitas vezes, faz sentido dividir os requisitos de implementação num conjunto de modelos direcionados e com uma finalidade específica. Pode reutilizar facilmente estes modelos para soluções diferentes. Para implementar uma solução específica, cria um modelo global que liga todos os modelos necessários. A imagem seguinte mostra como implementar uma solução de três camadas através de um modelo principal que inclui três modelos aninhados.

![modelo de camadas aninhado](./media/resource-group-overview/nested-tiers-template.png)

Se imaginar as suas camadas com ciclos de vida separados, pode implementar as três camadas em grupos de recursos separados. Repare que os recursos ainda podem ser ligados a recursos noutros grupos de recursos.

![modelo de camadas](./media/resource-group-overview/tier-templates.png)

Para obter mais sugestões sobre a criação dos seus modelos, veja [Padrões para criar modelos do Azure Resource Manager](best-practices-resource-manager-design-templates.md). Para obter mais informações sobre modelos aninhados, veja [Utilizar modelos ligados com o Azure Resource Manager](resource-group-linked-templates.md).

Para uma série de quatro partes sobre automatizar a implementação, consulte [Automating application deployments to Azure Virtual Machines (Automatizar implementações de aplicações para Máquinas Virtuais do Azure)](../virtual-machines/virtual-machines-windows-dotnet-core-1-landing.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Esta série abrange arquitetura de aplicação, acesso e segurança, disponibilidade e dimensionamento, e implementação de aplicação.

O Azure Resource Manager analisa as dependências para garantir que os recursos são criados pela ordem correta. Se um recurso depende de um valor de outro recurso (como uma máquina virtual necessita de uma conta de armazenamento para discos), pode definir uma dependência. Para obter mais informações, consulte [Definir dependências nos modelos do Azure Resource Manager](resource-group-define-dependencies.md).

Também pode utilizar o modelo para atualizar a infraestrutura. Por exemplo, pode adicionar um recurso à solução e adicionar regras de configuração para os recursos que já estão implementados. Se o modelo especificar a criação de um novo mas esse recurso já existir, o Azure Resource Manager efetua uma atualização em vez de criar um novo recurso. O Azure Resource Manager atualiza o recurso existente para o mesmo estado que teria como novo.  

O Resource Manager fornece extensões para cenários em que necessita de operações adicionais como a instalação de software específico que não está incluído na configuração. Se já estiver a utilizar um serviço de gestão de configuração, como o DSC, Chef ou Puppet, pode continuar a trabalhar com esse serviço ao utilizar extensões. Para obter informações sobre as extensões de máquina virtual, consulte [About virtual machine extensions and features (Sobre extensões e funcionalidades da máquina virtual)](../virtual-machines/virtual-machines-windows-extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

Por último, o modelo torna-se parte do código fonte da sua aplicação. Pode verificá-lo no repositório de código fonte e atualizá-lo à medida que a aplicação evolui. Pode editar o modelo através do Visual Studio.

Após definir o seu modelo, está pronto para implementar os recursos para o Azure. Para os comandos implementarem os recursos, consulte:

* [Implementar recursos com modelos do Resource Manager e o Azure PowerShell](resource-group-template-deploy.md)
* [Implementar recursos com modelos do Resource Manager e a CLI do Azure](resource-group-template-deploy-cli.md)
* [Implementar recursos com modelos do Resource Manager e o Portal do Azure](resource-group-template-deploy-portal.md)
* [Implementar recursos com modelos do Resource Manager e a API REST do Resource Manager](resource-group-template-deploy-rest.md)

## <a name="tags"></a>Etiquetas
O Resource Manager fornece uma funcionalidade de etiquetagem que permite categorizar os recursos de acordo com os seus requisitos de gestão ou faturação. Utilize etiquetas quando tem uma coleção complexa de grupos de recursos e recursos e precisa de visualizar esses elementos da forma mais adequada para si. Por exemplo, pode etiquetar recursos que desempenham uma função semelhante na sua organização ou pertencem ao mesmo departamento. Sem etiquetas, os utilizadores da organização podem criar vários recursos que poderá ser difícil identificar e gerir mais tarde. Por exemplo, poderá pretender eliminar todos os recursos de um projeto específico. Se esses recursos não estão marcados para o projeto, terá de encontrá-los manualmente. A etiquetagem pode ser um meio importante para reduzir os custos desnecessários associados à sua subscrição. 

Os recursos não precisam de residir no mesmo grupo de recursos para partilhar uma etiqueta. Pode criar a sua própria taxonomia de etiquetas para se certificar de que todos os utilizadores da sua organização utilizam etiquetas comuns em vez de aplicarem inadvertidamente etiquetas ligeiramente diferentes (por exemplo, “depart” em vez de “departamento”).

O exemplo seguinte mostra uma etiqueta aplicada a uma máquina virtual.

    "resources": [    
      {
        "type": "Microsoft.Compute/virtualMachines",
        "apiVersion": "2015-06-15",
        "name": "SimpleWindowsVM",
        "location": "[resourceGroup().location]",
        "tags": {
            "costCenter": "Finance"
        },
        ...
      }
    ]

Para obter todos os recursos com um valor de etiqueta, utilize o seguinte cmdlet do PowerShell:

    Find-AzureRmResource -TagName costCenter -TagValue Finance

Ou, o seguinte comando CLI do Azure:

    azure resource list -t costCenter=Finance --json

Também pode ver recursos com etiquetas através do Portal do Azure.

O [relatório de utilização](../billing/billing-understand-your-bill.md) para a sua subscrição inclui nomes de etiquetas e valores, o que lhe permite dividir os custos por etiquetas. Para obter mais informações sobre etiquetas, consulte [Utilizar etiquetas para organizar os recursos do Azure](resource-group-using-tags.md).

## <a name="access-control"></a>Controlo de acesso
O Resource Manager permite controlar quem tem acesso a ações específicas para a sua organização. Integra de forma nativa o controlo de acesso baseado em funções (RBAC) na plataforma de gestão e aplica esse controlo de acesso a todos os serviços no seu grupo de recursos. 

Existem dois conceitos principais que convém compreender quando utilizar o controlo de acesso baseado em funções:

* Definições de funções – descrevem um conjunto de permissões e podem ser utilizadas em inúmeras atribuições.
* Atribuições de funções – associam uma definição a uma identidade (utilizador ou grupo) para um determinado âmbito (subscrição, grupo de recursos ou recurso). A atribuição é herdada dos âmbitos inferiores.

Pode adicionar utilizadores à plataforma predefinida e funções de recursos específicos. Por exemplo, pode tirar partido da função predefinida denominada Leitor que permite aos utilizadores verem recursos, mas não alterá-los. Adiciona os utilizadores da sua organização que necessitam deste tipo de acesso à função do Leitor e aplica a função à subscrição, ao grupo de recursos ou ao recurso.

O Azure oferece as seguintes quatro funções de plataforma:

1. Proprietário - pode gerir tudo, incluindo o acesso
2. Contribuinte - pode gerir tudo, exceto o acesso
3. Leitor - pode ver tudo, mas não pode efetuar alterações
4. Administrador de Acesso do Utilizador - pode gerir o acesso dos utilizadores aos recursos do Azure

O Azure também fornece várias funções de recursos específicos. Algumas mais comuns são:

1. Contribuinte da Máquina Virtual - pode gerir máquinas virtuais, mas não concede acesso às mesmas e não pode gerir a conta de armazenamento ou a rede virtual ao qual está ligado
2. Contribuinte de Rede - pode gerir todos os recursos de rede, mas não pode conceder acesso aos mesmos
3. Contribuinte de Conta de Armazenamento - pode gerir contas de armazenamento, mas não pode conceder acesso às mesmas
4. Contribuinte do SQL Server - pode gerir servidores e bases de dados SQL, mas não pode gerir as respetivas políticas relacionadas com a segurança
5. Contribuinte de Web site - pode gerir Web sites, mas não pode gerir os planos Web ao qual estão ligados

Para obter uma lista completa de funções e ações permitidas, consulte [RBAC: Built in Roles (RBAC: Funções Incorporadas)](../active-directory/role-based-access-built-in-roles.md). Para obter mais informações sobre o controlo de acesso baseado em funções, consulte [Controlo de Acesso Baseado em Funções do Azure](../active-directory/role-based-access-control-configure.md). 

Em alguns casos, deve executar o código ou script que acede aos recursos, mas não deve executá-lo com as credenciais de um utilizador. Em vez disso, deve criar uma identidade designada por um serviço principal para a aplicação e atribuir a função adequada para o serviço principal. O Resource Manager permite-lhe criar credenciais para a aplicação e autenticar a aplicação com programação. Para saber mais sobre a criação de principais de serviço, consulte um dos seguintes tópicos:

* [Utilizar o Azure PowerShell para criar um principal de serviço para aceder aos recursos](resource-group-authenticate-service-principal.md)
* [Utilizar a CLI do Azure para criar um principal de serviço para aceder aos recursos](resource-group-authenticate-service-principal-cli.md)
* [Utilizar o portal para criar um principal de serviço e aplicações do Active Directory que possam aceder aos recursos](resource-group-create-service-principal-portal.md)

Pode também bloquear explicitamente recursos críticos para impedir que os utilizadores os eliminem ou modifiquem. Para obter mais informações, consulte [Bloquear recursos com o Azure Resource Manager](resource-group-lock-resources.md).

## <a name="activity-logs"></a>Registos de atividade
O Resource Manager regista todas as operações que criam, modificam ou eliminam um recurso. Pode utilizar os registos de atividade para encontrar um erro ao resolver um problema ou para monitorizar a forma como um utilizador na sua organização alterou um recurso. Para ver os registos, selecione **Registos de atividade** no painel **Definições** para um grupo de recursos. Pode filtrar os registos com muitos valores diferentes, incluindo o utilizador que iniciou a operação. Para obter informações sobre como trabalhar com os registos de atividade, consulte [Auditar operações com o Resource Manager](resource-group-audit.md).

## <a name="customized-policies"></a>Políticas personalizadas
O Resource Manager permite criar políticas personalizadas para gerir os seus recursos. Os tipos de políticas que cria podem incluir diversos cenários. Pode impor uma convenção de nomenclatura nos recursos, limitar que tipos e que instâncias de recursos pode ser implementados ou limitar as regiões que podem alojar um tipo de recurso. É possível requerer um valor de etiqueta para os recursos de modo a organizar a faturação por departamentos. Cria políticas para ajudar a reduzir os custos e manter a consistência na sua subscrição. 

O utilizador define políticas com JSON e, em seguida, aplica essas políticas em toda a sua subscrição ou dentro de um grupo de recursos. As políticas são diferentes do controlo de acesso baseado em funções, uma vez que estas são aplicadas a tipos de recursos.

O exemplo seguinte mostra uma política que garante a consistência da etiqueta, especificando que todos os recursos incluem uma etiqueta de costCenter.

    {
      "if": {
        "not" : {
          "field" : "tags",
          "containsKey" : "costCenter"
        }
      },
      "then" : {
        "effect" : "deny"
      }
    }

Existem muitos mais tipos de políticas que pode criar. Para obter mais informações, consulte [Utilizar a Política para gerir recursos e controlar o acesso](resource-manager-policy.md).

## <a name="sdks"></a>SDKs
Os SDKs do Azure estão disponíveis para vários idiomas e plataformas.
Cada uma destas implementações de idiomas está disponível através do respetivo gestor de pacotes ecossistema de pacotes e do GitHub.

O código em cada um destes SDKs é gerado a partir de especificações de API RESTful do Azure.
Estes especificações são de fonte aberta e baseadas na especificação do Swagger 2.0.
O código SDK é gerado através de um projeto de fonte aberta denominado AutoRest.
O AutoRest transforma estas especificações de API RESTful em bibliotecas de cliente em vários idiomas.
Se pretender melhorar os aspetos do código gerado nos SDKs, o conjunto completo de ferramentas para criar os SDKs são abertos, livremente disponíveis e com base num formato de especificação de API amplamente adotado.

Aqui estão os nossos repositórios de SDKs Open Source. Agradecemos o envio de comentários, problemas e pedidos pull.

[.NET](https://github.com/Azure/azure-sdk-for-net) | [Java](https://github.com/Azure/azure-sdk-for-java) | [Node.js](https://github.com/Azure/azure-sdk-for-node) | [PHP](https://github.com/Azure/azure-sdk-for-php) | [Python](https://github.com/Azure/azure-sdk-for-python) | [Ruby](https://github.com/Azure/azure-sdk-ruby)

> [!NOTE]
> Se o SDK não fornecer a funcionalidade necessária, também pode ligar para a [API de REST do Azure](https://docs.microsoft.com/rest/api/resources/) diretamente.
> 
> 

## <a name="samples"></a>Amostras
### <a name="net"></a>.NET
* [Gerir recursos do Azure e grupos de recursos](https://azure.microsoft.com/documentation/samples/resource-manager-dotnet-resources-and-groups/)
* [Implementar uma VM com SSH através de um modelo](https://azure.microsoft.com/documentation/samples/resource-manager-dotnet-template-deployment/)

### <a name="java"></a>Java
* [Gerir recursos do Azure](https://azure.microsoft.com/documentation/samples/resources-java-manage-resource/)
* [Gerir grupos de recursos do Azure](https://azure.microsoft.com/documentation/samples/resources-java-manage-resource-group/)
* [Implementar uma VM com SSH através de um modelo](https://azure.microsoft.com/documentation/samples/resources-java-deploy-using-arm-template/)

### <a name="nodejs"></a>Node.js
* [Gerir recursos do Azure e grupos de recursos](https://azure.microsoft.com/documentation/samples/resource-manager-node-resources-and-groups/)
* [Implementar uma VM com SSH através de um modelo](https://azure.microsoft.com/documentation/samples/resource-manager-node-template-deployment/)

### <a name="python"></a>Python
* [Gerir recursos do Azure e grupos de recursos](https://azure.microsoft.com/documentation/samples/resource-manager-python-resources-and-groups/)
* [Implementar uma VM com SSH através de um modelo](https://azure.microsoft.com/documentation/samples/resource-manager-python-template-deployment/)

### <a name="ruby"></a>Ruby
* [Gerir recursos do Azure e grupos de recursos](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-resources-and-groups/)
* [Implementar uma VM com SSH através de um modelo](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-template-deployment/)

Para além estes exemplos, pode procurar nos exemplos da galeria.

[.NET](https://azure.microsoft.com/documentation/samples/?service=azure-resource-manager&platform=dotnet) | [Java](https://azure.microsoft.com/documentation/samples/?service=azure-resource-manager&platform=java) | [Node.js](https://azure.microsoft.com/documentation/samples/?service=azure-resource-manager&platform=nodejs) | [Python](https://azure.microsoft.com/documentation/samples/?service=azure-resource-manager&platform=python) | [Ruby](https://azure.microsoft.com/documentation/samples/?service=azure-resource-manager&platform=ruby)

## <a name="next-steps"></a>Passos seguintes
* Para obter uma introdução simples sobre como trabalhar com modelos, consulte [Exportar um modelo do Azure Resource Manager a partir de recursos existentes](resource-manager-export-template.md).
* Para obter instruções mais detalhadas sobre como criar um modelo, consulte [Instruções do Modelo do Resource Manager](resource-manager-template-walkthrough.md).
* Para compreender as funções que pode utilizar num modelo, consulte [Funções de modelo](resource-group-template-functions.md)
* Para obter informações sobre como utilizar o Visual Studio com o Resource Manager, consulte [Criar e implementar grupos de recursos do Azure através do Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).
* Para obter informações sobre como utilizar o VS Code com o Resource Manager, veja o artigo [Trabalhar com modelos do Azure Resource Manager no Visual Studio Code](resource-manager-vs-code.md).

Eis uma demonstração em vídeo desta descrição geral:

>[!VIDEO https://channel9.msdn.com/Blogs/Azure-Documentation-Shorts/Azure-Resource-Manager-Overview/player]


[powershellref]: https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.2.0/azurerm.resources



<!--HONumber=Dec16_HO2-->


