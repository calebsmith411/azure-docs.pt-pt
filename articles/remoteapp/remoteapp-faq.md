---
title: Perguntas mais frequentes (FAQ) do Azure RemoteApp | Microsoft Docs
description: "Conheças as respostas às perguntas mais frequentes sobre o Azure RemoteApp."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: swadhwa
editor: 
ms.assetid: bad66603-91f9-437f-8a70-236405d2a27f
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/23/2016
ms.author: mbaldwin
translationtype: Human Translation
ms.sourcegitcommit: fcc53e4073a6957fae263cfb5d38023ed9710d21
ms.openlocfilehash: 31df73e3fc4142ba8c3634ac9d2b6fea4cc1b2d9


---
# <a name="azure-remoteapp-faq"></a>Perguntas mais frequentes (FAQ) do Azure RemoteApp
> [!IMPORTANT]
> O Azure RemoteApp está a ser descontinuado. Leia o [anúncio](https://go.microsoft.com/fwlink/?linkid=821148) para obter detalhes.
> 
> 

A seguir são apresentadas as perguntas relativas ao Azure RemoteApp. Tem outras perguntas? Visite os [fóruns do RemoteApp](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureRemoteApp) e indique o que precisa de saber ou deixe um comentário a seguir.

## <a name="cant-find-what-youre-looking-for-have-a-question-we-didnt-answer"></a>Não consegue encontrar o que procura? Tem alguma pergunta que não tenhamos respondido?
Se não consegue encontrar as informações que procura ou tem uma pergunta adicional que não é abordada aqui, visite o [Fórum do Azure RemoteApp](http://aka.ms/araforum) e coloque lá a sua pergunta. Podemos sempre adicionar mais respostas aqui.

## <a name="what-is-azure-remoteapp"></a>O que é o Azure RemoteApp?
* **O que é o Azure RemoteApp?** O RemoteApp é um serviço do Azure que ajuda a fornecer um acesso remoto seguro às aplicações a partir de vários dispositivos de utilizador distintos. Saiba mais sobre o [Azure RemoteApp](remoteapp-whatis.md).
* **Quais são as opções de implementação?** Existem dois tipos de coleções do RemoteApp: a coleção na nuvem e a coleção híbrida. A coleção de que necessita depende de vários fatores como, por exemplo, se é necessária a associação de um domínio. Todas essas decisões são abordadas [aqui](remoteapp-collections.md).

## <a name="quick-tips-on-using-azure-remoteapp"></a>Sugestões rápidas sobre a utilização do Azure RemoteApp
* **Quanto tempo tenho até ser desligado? Durante quanto tempo posso ficar inativo antes de ser realizado um reinício?** 4 horas. Caso o utilizador ou um dos respetivos utilizadores fique inativo durante quatro horas, a sessão no Azure RemoteApp será automaticamente terminada. Verifique as outras predefinições em [Subscrição e Limites de Serviço, Quotas e Restrições do Azure](../azure-subscription-service-limits.md).
* **Posso experimentar este serviço gratuitamente?** Sim. Existe uma versão de avaliação gratuita disponível durante 30 dias. Após terminar a avaliação, pode efetuar a transição para uma conta paga (que pode utilizar em produção) ou deixar de utilizar o serviço. Inicie a avaliação gratuita acedendo a [portal.azure.com](http://portal.azure.com) – criar uma nova instância do RemoteApp. Juntamente com a versão de avaliação gratuita, pode criar 2 instâncias do RemoteApp com 10 utilizadores por instância. Lembre-se de que esta versão de avaliação tem apenas uma duração de 30 dias.
  
  ## <a name="azure-remoteapp-subscription-details"></a>Detalhes da subscrição do Azure RemoteApp
* **Quais são os limites de serviço?** Pode saber mais sobre as predefinições e os limites de serviço do Azure RemoteApp em [Subscrição e Limites de Serviço, Quotas e Restrições do Azure](../azure-subscription-service-limits.md). Informe-nos caso tenha mais perguntas.
* **Quantos utilizadores devo ter?** Existe um mínimo de 20 utilizadores. Permita-me repetir para ser extremamente claro – 20 no MÍNIMO. Será faturado por 20. 
* **Qual é o custo do RemoteApp?** Consulte os [Detalhes dos Preços do Azure RemoteApp ](https://azure.microsoft.com/pricing/details/remoteapp/).
* **O custo de um tipo de coleção é superior ao outro?** Sim, poderá ser superior, dependendo dos requisitos da coleção. Uma coleção híbrida requer uma ligação do Azure RemoteApp à sua rede no local. Caso utilize uma VNET/Express Route, não existe qualquer custo adicional. No entanto, se utilizar uma nova VNET do Azure e um gateway ou ExpressRoute, ser-lh-á cobrado um montante pelo [Gateway de VPN](https://azure.microsoft.com/pricing/details/vpn-gateway) ou o [ExpressRoute](https://azure.microsoft.com/pricing/details/expressroute/). Esse custo (detalhado nas ligações) reflete-se no custo mensal do Azure RemoteApp.

## <a name="collections---whats-supported-which-should-you-use-and-others"></a>Coleções – o que é suportado e qual deverá utilizar, entre outros
* **São suportadas aplicações de linha de negócio (LOB) personalizadas?** Sim. Para utilizar uma aplicação personalizada no Azure RemoteApp, crie uma [imagem de modelo personalizada](remoteapp-create-custom-image.md) e, em seguida, carregue a mesma para a coleção do RemoteApp.
* **A minha aplicação LOB personalizada funcionará no Azure RemoteApp?** A melhor forma de descobrir é efetuando um teste. Consulte o [Centro de Compatibilidade de RD](http://www.rdcompatibility.com/compatibility/default.aspx).
* **Qual é o método de implementação (na nuvem ou híbrida) mais adequado para a minha empresa?** As coleções híbridas proporcionam a experiência mais completa caso pretenda uma integração completa com o início de sessão único (SSO) e uma conectividade de rede no local segura. As coleções na nuvem proporcionam uma forma célere e simples de isolar a implementação através de múltiplos métodos de autenticação. Saiba mais acerca das [opções de implementação](remoteapp-whatis.md).
* **Temos uma base de dados SQL ou de outra natureza no local ou no Azure. Que tipo de implementação devemos utilizar?** Depende do local onde se encontra a SQL Database ou back-end. Se a base de dados estiver numa rede privada, utilize a coleção híbrida. Caso a base de dados esteja exposta à Internet e permita a respetiva ligação através de ligações de cliente, pode utilizar a coleção na nuvem.
* **E o mapeamento de unidades, a porta USB e série, a partilha da área de transferência e o redirecionamento de impressoras?** Todas estas funcionalidades são suportadas no Azure RemoteApp. A partilha da área de transferência e o redirecionamento de impressoras estão ativados por predefinição. Pode saber mais sobre o redirecionamento [aqui](remoteapp-redirection.md). 

## <a name="template-images"></a>Imagens de modelo
* **Posso utilizar uma máquina virtual existente na nuvem como modelo para a coleção do RemoteApp?** Sim! Pode criar uma imagem com base numa VM do Azure, utilizar uma das imagens incluídas na sua subscrição ou criar uma imagem personalizada. Consulte as [Opções de imagem do RemoteApp](remoteapp-imageoptions.md).

## <a name="network-options"></a>Opções de rede
* **A coleção híbrida requer uma VNET. Podemos utilizar a VNET existente?** Pode caso a VNET existente seja uma Azure VNET. Consulte o "Passo 1: configurar a rede virtual" das [instruções da coleção híbrida](remoteapp-create-hybrid-deployment.md) para obter mais informações.
* **Posso utilizar uma VNET com uma coleção na nuvem?** Na verdade, pode. Consulte [Criar uma coleção na nuvem](remoteapp-create-cloud-deployment.md), em particular o Passo 1, para obter mais informações.

## <a name="authentication-options"></a>Opções de autenticação
* **E a autenticação? Quais são os métodos suportados?** A coleção na nuvem suporta contas Microsoft e contas do Azure Active Directory, as quais são também contas do Office 365. A coleção híbrida suporta apenas contas do Azure Active Directory que foram sincronizadas (utilizando uma ferramenta como a [Sincronização do Azure Active Directory](http://blogs.technet.com/b/ad/archive/2014/09/16/azure-active-directory-sync-is-now-ga.aspx)) a partir de uma implementação do Windows Server Active Directory; de modo específico, sincronizadas com a opção Sincronização de Palavra-Passe ou sincronizadas com a federação configurada dos Serviços de Federação do Active Directory (AD FS). Também pode configurar uma [Multi-Factor Authentication (MFA)](https://azure.microsoft.com/services/multi-factor-authentication/).

> [!NOTE]
> Os utilizadores do Azure Active Directory devem pertencer ao inquilino associado à sua subscrição. (Pode ver e modificar a sua subscrição no separador **Definições** no portal. Consulte [Alterar o inquilino do Azure Active Directory utilizado pelo RemoteApp](remoteapp-changetenant.md) para obter mais informações.)
> 
> 

* **Por que motivo não posso conceder acesso à minha conta do Azure Active Directory?** Os utilizadores do Azure Active Directory devem pertencer ao diretório associado à sua subscrição. Pode ver ou modificar o diretório no separador Definições no portal. Consulte [Alterar o inquilino do Azure Active Directory utilizado pelo RemoteApp](remoteapp-changetenant.md) para obter mais informações.

## <a name="clients---what-device-can-i-use-to-access-azure-remoteapp"></a>Clientes – que dispositivo posso utilizar para aceder ao Azure RemoteApp?
Pode encontrar boas informações de cliente, incluindo passos para a instalação de diferentes clientes, em [Aceder às aplicações no Azure RemoteApp](remoteapp-clients.md).

* **Quais são os dispositivos e os sistemas operativos suportados pelas aplicações cliente?**
  Em primeiro lugar, computadores e tablets: 
  
  * Windows 10 (pré-visualização do cliente)
  * Windows 8.1 e Windows 8
  * Windows 7 Service Pack 1
  * Mac OS X
  * Windows RT
  * Tablets Android
  * iPads e telemóveis:
  * iPhone
  * Telemóvel Android
  * Windows Phone
    
    [Transferir](https://www.remoteapp.windowsazure.com/ClientDownload/AllClients.aspx) agora um cliente do RemoteApp.
* **O Azure RemoteApp suporta Clientes Magros?** Sim, os seguintes clientes magros do Windows Embedded são suportados:
  
  * Windows Embedded Standard 7
  * Windows Embedded Standard 8
  * Windows Embedded 8.1 Industry Pro
  * Windows 10 IoT Enterprise
* **Qual é a versão do Windows Server suportada para o Anfitrião de Sessões de Ambiente de Trabalho Remoto (RDSH)?** Windows Server 2012 R2.

## <a name="support-and-feedback"></a>Suporte e comentários
* **Qual é o plano de suporte do RemoteApp?** O suporte à faturação e a gestão de subscrições são fornecidos gratuitamente. O suporte técnico está disponível através dos [planos de serviço do Azure](https://azure.microsoft.com/support/plans/). Também pode obter suporte da comunidade gratuito através do [fórum de discussão do Azure](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=AzureRemoteApp). 
* **Como posso enviar comentários?** Aceda ao [fórum de comentários](https://feedback.azure.com/forums/247748-azure-remoteapp/).
* **Com quem posso falar para saber mais sobre o Azure RemoteApp?** Além do [fórum de discussão](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=AzureRemoteApp), um excelente local para colocar perguntas, pode participar no [seminário virtual de Perguntas para Especialistas](https://azureinfo.microsoft.com/US-Azure-WBNR-FY15-11Nov-AzureRemoteAppAskTheExperts-Registration-Page.html) semanal, no qual são abordados todos os aspetos do RemoteApp.
* **E a documentação do RemoteApp?** Ainda bem que perguntou. Além do conteúdo de ajuda na gaveta de ajuda do portal (basta clicar em **?** em qualquer página do portal), estão disponíveis os seguintes artigos para lhe ensinar tudo sobre o RemoteApp:
  
  * **Introdução:**
    * [O que é o RemoteApp?](remoteapp-whatis.md)
    * [O que existe nas imagens de modelo do RemoteApp?](remoteapp-images.md)
    * [Como funciona o licenciamento?](remoteapp-licensing.md)
    * [Como o RemoteApp e o Office funcionam em conjunto?](remoteapp-o365.md)
    * [Como funciona o redirecionamento no RemoteApp](remoteapp-redirection.md)?
  * **Implementação:**
    * [Criar uma imagem de modelo personalizada](remoteapp-create-custom-image.md)
    * [Criar uma coleção híbrida](remoteapp-create-hybrid-deployment.md)
    * [Criar uma coleção na nuvem](remoteapp-create-cloud-deployment.md)
    * [Configurar o Azure Active Directory para o RemoteApp](remoteapp-ad.md)
    * [Publicar uma aplicação no RemoteApp](remoteapp-publish.md)
  * **Gerir:**
    
    * [Adicionar utilizadores](remoteapp-user.md)
    * [Melhores práticas para configuração e utilização do RemoteApp](remoteapp-bestpractices.md)    
    
    Vídeos! Além disso, temos vários vídeos sobre o RemoteApp. Alguns fornecem uma introdução ([Introdução ao Azure RemoteApp](https://azure.microsoft.com/documentation/videos/cloud-cover-ep-150-azure-remote-app-with-thomas-willingham-and-nihar-namjoshi/)) enquanto outros apresentam a implementação ([Implementação na nuvem](https://www.youtube.com/watch?v=3NAv2iwZtGc&feature=youtu.be) e [Implementação híbrida](https://www.youtube.com/watch?v=GCIMxPUvg0c&feature=youtu.be)). Assista aos vídeos!

### <a name="help-us-help-you"></a>Ajude-nos para o podermos ajudar
Sabia que além de classificar este artigo e deixar um comentário a seguir, pode alterar o próprio artigo? Falta algo? Algo errado? Escrevi algo que é simplesmente confuso? Desloque-se para cima e clique em **Editar no GitHub** para efetuar alterações – estas serão enviadas para nós para revisão e, em seguida, após serem aceites, as suas alterações e melhorias serão apresentadas aqui.




<!--HONumber=Dec16_HO1-->


