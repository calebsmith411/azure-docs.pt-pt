---
title: "Introdução: Gestão de Palavras-passe do Azure AD | Microsoft Docs"
description: "Permita que os utilizadores reponham as suas próprias palavras-passe, conheçam os pré-requisitos para a reposição de palavra-passe e ativem a repetição de escrita de palavras-passe para gerir no local as palavras-passe no Active Directory."
services: active-directory
keywords: "Gestão de palavra-passe do Active Directory, gestão de palavra-passe, repor a palavra-passe do Azure AD"
documentationcenter: 
author: asteen
manager: femila
editor: curtand
ms.assetid: bde8799f-0b42-446a-ad95-7ebb374c3bec
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/05/2016
ms.author: asteen
translationtype: Human Translation
ms.sourcegitcommit: e2e5c302d04a41386bfc98dd4e3f8546265dd9f3
ms.openlocfilehash: e686952a7363e4758f8a3532b54cf5e7f05ce865


---
# <a name="getting-started-with-password-management"></a>Introdução à Gestão de Palavras-passe
> [!IMPORTANT]
> **Está aqui porque está a ter problemas em iniciar sessão?** Se assim for, [Eis como pode alterar e repor a sua própria palavra-passe](active-directory-passwords-update-your-own-password.md).
> 
> 

Para permitir que os utilizadores façam a gestão das suas próprias palavras-passe do Active Directory no local ou do Azure Active Directory na nuvem precisa apenas de alguns passos simples. Depois de se certificar que cumpriu alguns pré-requisitos simples, terá a opção de alterar e repor a palavra-passe para toda a organização em menos de nada. Este artigo descreve os seguintes conceitos:

* [**Como permitir que os utilizadores reponham as respetivas palavras-passe do Azure Active Directory na nuvem**](#enable-users-to-reset-their-azure-ad-passwords)
  * [Pré-requisitos da reposição personalizada de palavra-passe](#prerequisites)
  * [Passo 1: Configurar a política de reposição de palavra-passe](#step-1-configure-password-reset-policy)
  * [Passo 2: Adicionar dados de contacto do utilizador de teste](#step-2-add-contact-data-for-your-test-user)
  * [Passo 3: Repor a palavra-passe como utilizador](#step-3-reset-your-azure-ad-password-as-a-user)
* [**Como permitir que os utilizadores reponham ou alterem as respetivas palavras-passe do Active Directory no local**](#enable-users-to-reset-or-change-their-ad-passwords)
  * [Pré-requisitos da Repetição de Escrita de Palavras-passe](#writeback-prerequisites)
  * [Passo 1: Transferir a versão mais recente do Azure AD Connect](#step-1-download-the-latest-version-of-azure-ad-connect)
  * [Passo 2: Ativar a Repetição de Escrita de Palavras-passe no Azure AD Connect através da IU ou do PowerShell e verificar](#step-2-enable-password-writeback-in-azure-ad-connect)
  * [Passo 3: Configurar a firewall](#step-3-configure-your-firewall)
  * [Passo 4: Configurar as permissões adequadas](#step-4-set-up-the-appropriate-active-directory-permissions)
  * [Passo 5: Repor a palavra-passe do AD como utilizador e verificar](#step-5-reset-your-ad-password-as-a-user)

## <a name="enable-users-to-reset-their-azure-ad-passwords"></a>Permitir que os utilizadores reponham as respetivas palavras-passe do Azure AD
Esta secção explica como ativar a reposição personalizada de palavra-passe para o diretório na nuvem do AAD, como registar os utilizadores para a reposição personalizada de palavra-passe e como executar uma reposição personalizada de palavra-passe de teste como utilizador.

* [Pré-requisitos da reposição personalizada de palavra-passe](#prerequisites)
* [Passo 1: Configurar a política de reposição de palavra-passe](#step-1-configure-password-reset-policy)
* [Passo 2: Adicionar dados de contacto do utilizador de teste](#step-2-add-contact-data-for-your-test-user)
* [Passo 3: Repor a palavra-passe como utilizador](#step-3-reset-your-azure-ad-password-as-a-user)

### <a name="prerequisites"></a>Pré-requisitos
Para ativar e utilizar a reposição personalizada de palavra-passe, terá de cumprir os seguintes pré-requisitos:

* Criar um inquilino do AAD. Para obter mais informações, consulte [ Introdução ao Azure AD](https://azure.microsoft.com/trial/get-started-active-directory/)
* Obter uma subscrição do Azure. Para obter mais informações, consulte [O que é um inquilino do Azure AD?](active-directory-administer.md#what-is-an-azure-ad-tenant)
* Associar o inquilino do AAD à subscrição do Azure. Para obter mais informações, consulte [Como associar as subscrições do Azure ao Azure AD](https://msdn.microsoft.com/library/azure/dn629581.aspx).
* Atualizar para o Azure AD Premium, Basic ou utilizar uma licença do Office 365 paga. Para obter mais informações, consulte [Edições do Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/).
  
  > [!NOTE]
  > Para ativar a reposição personalizada de palavra-passe para os utilizadores da nuvem, tem de atualizar a versão para o Azure AD Premium, Azure AD Basic ou uma licença paga do Office 365.  Para ativar a reposição personalizada de palavra-passe para os utilizadores no local, tem de atualizar a versão para o Azure AD Premium. Para obter mais informações, consulte [Edições do Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/). Estas informações incluem instruções detalhadas sobre como inscrever-se no Azure AD Premium ou Basic, como ativar o plano de licenciamento e o acesso do Azure AD e como atribuir acesso a contas de administrador e de utilizador.
  > 
  > 
* Crie, pelo menos, uma conta de administrador e uma conta de utilizador no diretório do AAD.
* Atribua uma licença do AAD Premium ou Basic ou uma licença paga do Office 365 à conta de administrador e de utilizador que criou.

### <a name="step-1-configure-password-reset-policy"></a>Passo 1: Configurar a política de reposição de palavra-passe
Para configurar a política de reposição de palavra-passe do utilizador, conclua os passos seguintes:

1. Abra um browser da sua preferência e aceda ao [Portal clássico do Azure](https://manage.windowsazure.com).
2. No [Portal clássico do Azure](https://manage.windowsazure.com), localize a **extensão do Active Directory** na barra de navegação no lado esquerdo.
   
   ![Gestão de Palavras-passe no Azure AD][001]
3. No separador **Diretório**, clique no diretório onde pretende configurar a política de reposição de palavra-passe do utilizador, por exemplo, no diretório Wingtip Toys.
   
    ![][002]
4. Clique no separador **Configurar**.
   
   ![][003]
5. No separador **Configurar**, desloque o ecrã para baixo até à secção da **política de reposição de palavra-passe do utilizador**.  É neste secção onde configura cada aspeto da política de reposição de palavra-passe do utilizador de um determinado diretório.  
   
   > [!NOTE]
   > Esta **política aplica-se apenas aos utilizadores finais da organização e não aos administradores**. Por motivos de segurança, a Microsoft controla a política de reposição de palavra-passe dos administradores. Se não vir esta secção, certifique-se de que se inscreveu no Azure Active Directory Premium ou Basic e de que **atribuiu uma licença** à conta de administrador que está a configurar esta funcionalidade.
   > 
   > 
   
   ![][004]
6. Para configurar a política de reposição de palavra-passe do utilizador, deslize o botão de alternância **utilizadores ativados para a reposição de palavra-passe** para **sim**.  Com isto são apresentados mais controlos que lhe permitem configurar o funcionamento desta funcionalidade no diretório.  Fique à vontade para personalizar como achar melhor a reposição de palavra-passe.  Se desejar saber mais sobre o que faz cada controlo da política de reposição de palavra-passe, consulte [Personalizar: Gestão de Palavras-passe do Azure AD](active-directory-passwords-customize.md).
   
   ![][005]
7. Depois de configurar a política de reposição de palavra-passe para o utilizador do inquilino, clique em **Guardar** na parte inferior do ecrã.
   
   > [!NOTE]
   > Recomenda-se uma política de reposição de palavra-passe do utilizador de dois desafios para que possa ver como a funcionalidade resulta num caso mais complexo.
   > 
   > 
   
   ![][006]

### <a name="step-2-add-contact-data-for-your-test-user"></a>Passo 2: Adicionar os dados de contacto do utilizador de teste
Tem várias opções sobre como especificar os dados dos utilizadores na organização que serão utilizados para a reposição de palavra-passe.

* Editar utilizadores no [Portal clássico do Azure](https://manage.windowsazure.com) ou [Portal de Administração do Office 365](https://portal.microsoftonline.com)
* Utilizar o AAD Connect para sincronizar as propriedades do utilizador com o Azure AD a partir de um domínio do Active Directory no local
* Utilizar o Windows PowerShell para editar as propriedades do utilizador
* Permitir que os utilizadores registem os seus próprios dados, indicando-lhes o portal de registo em [http://aka.ms/ssprsetup](http://aka.ms/ssprsetup)
* Exigir que os utilizadores se registem para a reposição de palavra-passe quando iniciam sessão no Painel de Acesso em [http://myapps.microsoft.com](http://myapps.microsoft.com) definindo a opção de configuração de SSPR **Exigir que os utilizadores se registem quando iniciam sessão no painel de acesso** para **Sim**.

Se pretender saber mais sobre os dados que são utilizados pela reposição de palavra-passe, bem como quaisquer requisitos de formatação destes dados, consulte [Quais os dados utilizados pela reposição de palavra-passe?](active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset)

#### <a name="to-add-user-contact-data-via-the-user-registration-portal"></a>Para adicionar dados de contacto do utilizador através do Portal de Registo do Utilizador
1. Para utilizar o portal de registo de reposição de palavra-passe, tem de fornecer aos utilizadores da organização uma ligação para esta página ([http://aka.ms/ssprsetup](http://aka.ms/ssprsetup)) ou ativar a opção que exige que os utilizadores se registem automaticamente.  Depois de clicarem nesta ligação, é-lhes pedido para iniciar sessão com a respetiva conta profissional.  Depois de fazê-lo, verão a página seguinte:
   
   ![][007]
2. Aqui, os utilizadores podem fornecer e verificar o número de telemóvel, o endereço de e-mail alternativo ou as perguntas de segurança.  Este é um exemplo de verificação de telemóvel.
   
   ![][008]
3. Quando o utilizador especificar estas informações, a página será atualizada para indicar que as informações são válidas (ocultas abaixo).  Ao clicar nos botões **concluir** ou **cancelar**, o utilizador é encaminhado para o Painel de Acesso.
   
   ![][009]
4. Quando o utilizador verifica estas duas informações, o respetivo perfil será atualizado com os dados fornecidos.  Neste exemplo, o número de **Telefone do Escritório** foi especificado manualmente. Assim, o utilizador também pode utilizá-lo como forma de contacto para repor a palavra-passe.
   
   ![][010]

### <a name="step-3-reset-your-azure-ad-password-as-a-user"></a>Passo 3: Repor a palavra-passe do Azure AD como utilizador
Agora que configurou a política de reposição do utilizador e especificou os respetivos detalhes de contacto, o utilizador pode efetuar uma reposição personalizada de palavra-passe.

#### <a name="to-perform-a-self-service-password-reset"></a>Para efetuar uma reposição personalizada de palavra-passe
1. Se aceder a um site como [**portal.microsoftonline.com**](http://portal.microsoftonline.com), verá um ecrã de início de sessão como o seguinte.  Clique na ligação **Não consegue aceder à sua conta?** para testar a interface de utilizador da reposição de palavra-passe.
   
   ![][011]
2. Ao clicar em **Não consegue aceder à sua conta?**, é encaminhado para uma nova página que irá pedir-lhe o **ID de utilizador** para a qual pretende repor a palavra-passe.  Introduza o **ID de utilizador** de teste, passe o captcha e clique em **seguinte**.
   
   ![][012]
3. Como o utilizador especificou um **telefone do escritório**, **telemóvel** e **e-mail alternativo** neste caso, verá que lhe são apresentadas todas essas opções para passar o primeiro desafio.
   
   ![][013]
4. Neste caso, opte por **ligar** primeiro para o **telefone do escritório**.  Tenha em atenção que, quando selecionar um método baseado em telefone, os utilizadores serão solicitados a **verificar o respetivo número de telefone** para poderem repor as palavras-passe.  Isto serve para impedir que pessoas mal-intencionadas enviem spam para os números de telefone dos utilizadores da organização.
   
   ![][014]
5. Quando o utilizador confirmar o número de telefone, ao clicar em ligar será apresentado um controlo giratório e o telefone tocará.  É reproduzida uma mensagem quando o utilizador atende o telefone indicando que o **utilizador deve premir #** para verificar a conta.  Premir esta tecla verificará automaticamente se o utilizador passa no primeiro desafio e a interface de utilizador avança para o segundo passo.
   
   ![][015]
6. Após passar no primeiro desafio, a interface de utilizador é atualizada automaticamente para removê-lo da lista de escolhas do utilizador.  Neste caso, como utilizou o **Telefone do Escritório**primeiro, apenas **Telemóvel** e **E-mail Alternativo** permanecem como opções válidas para serem utilizadas como o desafio do segundo passo de verificação.  Clique na opção de e-mail **Enviar correio eletrónico para o meu endereço alternativo**.  Depois de o ter feito, ao premir em email será enviada uma mensagem para o email alternativo registado.
   
   ![][016]
7. Apresentados a seguir um exemplo de e-mail que os utilizadores verão – repare na imagem corporativa do inquilino:
   
   ![][017]
8. Quando o email chegar, a página será atualizada e poderá introduzir a verificação existente no e-mail na caixa de entrada mostrada abaixo.  Depois de introduzir um código adequado, o botão seguinte acende-se e conseguirá passar no segundo passo de verificação.
   
   ![][018]
9. Após cumprir os requisitos da política organizacional, pode escolher uma nova palavra-passe.  A palavra-passe é validada se cumprir os requisitos de palavra-passe “segura” do AAD (consulte [Política de palavra-passe no Azure AD](https://msdn.microsoft.com/library/azure/jj943764.aspx)). Em seguida, é apresentado um controlo de validação de segurança para indicar ao utilizador se a palavra-passe introduzida cumpre essa política.
   
   ![][019]
10. Depois de fornecer palavras-passe correspondentes que cumprem a política organizacional, a palavra-passe é reposta e pode de imediato iniciar sessão com a nova palavra-passe.
    
    ![][020]

## <a name="enable-users-to-reset-or-change-their-ad-passwords"></a>Permitir que os utilizadores reponham ou alterem as respetivas Palavras-passe do AD
Esta secção explica como configurar a reposição da palavra-passe para repetir a escrita de palavras-passe no Active Directory no local.

* [Pré-requisitos da Repetição de Escrita de Palavras-passe](#writeback-prerequisites)
* [Passo 1: Transferir a versão mais recente do Azure AD Connect](#step-1-download-the-latest-version-of-azure-ad-connect)
* [Passo 2: Ativar a Repetição de Escrita de Palavras-passe no Azure AD Connect através da IU ou do PowerShell e verificar](#step-2-enable-password-writeback-in-azure-ad-connect)
* [Passo 3: Configurar a firewall](#step-3-configure-your-firewall)
* [Passo 4: Configurar as permissões adequadas](#step-4-set-up-the-appropriate-active-directory-permissions)
* [Passo 5: Repor a palavra-passe do AD como utilizador e verificar](#step-5-reset-your-ad-password-as-a-user)

### <a name="writeback-prerequisites"></a>Pré-requisitos da Repetição de Escrita
Para poder ativar e utilizar a Repetição de Escrita de Palavras-passe, tem de concluir os seguintes pré-requisitos:

* Ter um inquilino do Azure AD com o Azure AD Premium ativado.  Para obter mais informações, consulte [Edições do Azure Active Directory](active-directory-editions.md).
* A reposição de palavra-passe foi configurada e ativada no inquilino.  Para obter mais informações, consulte [Permitir que os utilizadores reponham as respetivas palavras-passe do Azure AD](#enable-users-to-reset-their-azure-ad-passwords)
* Ter, pelo menos, uma conta de administrador e uma conta de utilizador de teste com uma licença do Azure AD Premium que pode utilizar para testar esta funcionalidade.  Para obter mais informações, consulte [Edições do Azure Active Directory](active-directory-editions.md).
  
  > [!NOTE]
  > Certifique-se de que a conta de administrador que utilizou para ativar a Repetição de Escrita de Palavras-passe é uma conta de administrador de nuvem (criada no Azure AD), não uma conta federada (criada no AD no local e sincronizada com o Azure AD).
  > 
  > 
* Ter uma implementação no local do AD de floresta única ou de várias florestas com o Windows Server 2008, Windows Server 2008 R2, Windows Server 2012 ou Windows Server 2012 R2 com os mais recentes service packs instalados.
  
  > [!NOTE]
  > Se estiver a executar uma versão anterior do Windows Server 2008 ou 2008 R2, pode continuar a utilizar esta funcionalidade, mas será necessário [transferir e instalar o KB 2386717](https://support.microsoft.com/kb/2386717) para poder aplicar a política de palavra-passe local do AD na nuvem.
  > 
  > 
* Ter instalada a ferramenta do Azure AD Connect e preparado o ambiente do AD para a sincronização com a nuvem.  Para obter mais informações, consulte [Utilizar a infraestrutura de identidade no local na nuvem](connect/active-directory-aadconnect.md).
  
  > [!NOTE]
  > Antes de testar a repetição de escrita de palavras-passe, certifique-se de que efetua primeiro uma importação completa e uma sincronização completa do AD e do Azure AD no Azure AD Connect.
  > 
  > 
* Se estiver a utilizar o Azure AD Sync ou o Azure AD Connect, o **TCP 443** de saída (e, em alguns casos, o **TCP 9350-9354**) tem de estar aberto.  Consulte [Passo 3: Configurar a firewall](#step-3-configure-your-firewall) para obter mais informações. A utilização do DirSync para este cenário já não é suportada.  Se ainda estiver a utilizar o DirSync, atualize para a versão mais recente do Azure AD Connect antes de implementar a repetição de escrita de palavras-passe.
  
  > [!NOTE]
  > Recomendamos vivamente que todos os utilizadores quem estiverem a utilizar as ferramentas Azure AD Sync ou DirSync atualizem para a versão mais recente do Azure AD Connect para garantir a melhor experiência possível e obter novas funcionalidades à medida que são lançadas.
  > 
  > 

### <a name="step-1-download-the-latest-version-of-azure-ad-connect"></a>Passo 1: Transferir a versão mais recente do Azure AD Connect
A Repetição de Escrita de Palavras-passe está disponível nas versões do Azure AD Connect ou na ferramenta Azure AD Sync com o número de versão **1.0.0419.0911** ou superior.  A Repetição de Escrita de Palavras-passe com desbloqueio automático da conta está disponível nas versões do Azure AD Connect ou na ferramenta Azure AD Sync com o número de versão **1.0.0485.0222** ou superior. Se estiver a executar uma versão anterior, atualize para, pelo menos, esta versão antes de continuar. [Clique aqui para transferir a versão mais recente do Azure AD Connect](connect/active-directory-aadconnect.md#install-azure-ad-connect).

#### <a name="to-check-the-version-of-azure-ad-sync"></a>Para verificar a versão do Azure AD Sync
1. Navegue para **%ProgramFiles%\Azure Active Directory Sync\**.
2. Localize o executável **ConfigWizard.exe**.
3. Clique com o botão direito do rato no executável e selecione a opção **Propriedades** no menu de contexto.
4. Clique no separador **Detalhes**.
5. Localize o campo **Versão do ficheiro**.
   
   ![][021]

Se este número for maior ou igual a **1.0.0419.0911** ou se estiver a instalar o Azure AD Connect, poderá avançar para o [Passo 2: Ativar a Repetição de Escrita de Palavras-passe no Azure AD Connect através da IU ou do PowerShell e verificar](#step-2-enable-password-writeback-in-azure-ad-connect).

> [!NOTE]
> Se esta for a primeira vez que instala a ferramenta do Azure AD Connect, recomenda-se que siga alguns procedimentos para preparar o ambiente para sincronização de diretórios.  Antes de instalar a ferramenta do Azure AD Connect, tem de ativar a sincronização de diretórios no [Portal de Administração do Office 365](https://portal.microsoftonline.com) ou no [Portal clássico do Azure](https://manage.windowsazure.com).  Para obter mais informações, consulte [Gerir o Azure AD Connect](active-directory-aadconnect-whats-next.md).
> 
> 

### <a name="step-2-enable-password-writeback-in-azure-ad-connect"></a>Passo 2: Ativar a repetição de escrita de palavras-passe no Azure AD Connect
Agora que já transferiu a ferramenta do Azure AD Connect, está pronto para ativar a Repetição de Escrita de Palavras-passe.  Pode fazê-lo de duas maneiras.  Pode ativar a Repetição de Escrita de Palavras-passe no ecrã de funcionalidades opcionais do assistente de configuração do Azure AD Connect ou pode ativá-la através do Windows PowerShell.

#### <a name="to-enable-password-writeback-in-the-configuration-wizard"></a>Para ativar a Repetição de Escrita de Palavras-passe no assistente de configuração
1. No **computador do Directory Sync**, abra o assistente de configuração do **Azure AD Connect**.
2. Clique nos diferentes passos até chegar ao ecrã de configuração das **funcionalidades opcionais** .
3. Marque a opção **Repetição de escrita de palavras-passe**.
   
   ![][022]
4. Conclua o assistente, a página final resumirá as alterações e incluirá a alteração da configuração da Repetição de Escrita de Palavras-passe.

> [!NOTE]
> Pode desativar a Repetição de Escrita de Palavras-passe em qualquer altura executando novamente este assistente e desmarcando a funcionalidade ou definindo **Repetir Escrita de Palavras-passe no Diretório no Local** para **Não** na secção **Política de Reposição de Palavra-passe do Utilizador** do separador **Configurar** do diretório no [Portal clássico do Azure](https://manage.windowsazure.com).  Para obter mais informações sobre como personalizar a palavra-passe de reposição de teste, consulte [Personalizar: Gestão de Palavra-passe do Azure AD](active-directory-passwords-customize.md).
> 
> 

#### <a name="to-enable-password-writeback-using-windows-powershell"></a>Para ativar a Repetição de Escrita de Palavras-passe com o Windows PowerShell
1. No **computador de Sincronização de Diretórios**, abra uma nova **janela elevada do Windows PowerShell**.
2. Se o módulo ainda não estiver carregado, escreva o comando `import-module ADSync` para carregar os cmdlets do Azure AD Connect para a sessão atual.
3. Obtenha a lista dos Conectores do Azure AD no sistema ao executar o cmdlet `Get-ADSyncConnector` e ao armazenar os resultados em `$aadConnectorName`, como `$connectors = Get-ADSyncConnector|where-object {$\_.name -like "\*AAD"}`
4. Para obter o estado atual da repetição de escrita do conector atual, execute o seguinte cmdlet: `Get-ADSyncAADPasswordResetConfiguration –Connector $aadConnectorName.name`
5. Ative a Repetição de Escrita de Palavras-passe ao executar o cmdlet: `Set-ADSyncAADPasswordResetConfiguration –Connector $aadConnectorName.name –Enable $true`

> [!NOTE]
> Se lhe for pedida uma credencial, certifique-se de que a conta de administrador que especificou para AzureADCredential é uma **conta de administrador na nuvem (criada no Azure AD)** e não uma conta federada (criada no AD no local e sincronizada com o Azure AD).
> 
> [!NOTE]
> Pode desativar a Repetição de Escrita de Palavras-passe através do PowerShell repetindo as mesmas instruções acima mas indicando `$false` no passo ou definindo a **Repetição de Escrita de Palavras-passe no Diretório no Local** para **Não** na secção **Política de Reposição de Palavra-passe do Utilizador** do separador **Configurar** do diretório no [Portal clássico do Azure](https://manage.windowsazure.com).
> 
> 

#### <a name="verify-that-the-configuration-was-successful"></a>Verificar se a configuração foi concluída com êxito
Se a configuração tiver sido bem sucedida, verá a mensagem “O write-back de reposição de palavras-passe está ativado” na janela do Windows PowerShell ou uma mensagem de êxito na interface de utilizador de configuração.

Também poderá verificar se o serviço foi instalado corretamente se abrir o Visualizador de Eventos, navegar para o registo de eventos da aplicação e procurar o evento **31005 – OnboardingEventSuccess** no **PasswordResetService** de origem.

  ![][023]

### <a name="step-3-configure-your-firewall"></a>Passo 3: Configurar a firewall
Depois de ter ativado a Repetição de Escrita de Palavras-passe, tem de certificar-se de que a máquina com o Azure AD Connect consegue aceder aos serviços cloud da Microsoft, para receber pedidos de repetição de escrita da palavra-passe. Este passo envolve atualizar as regras de ligação nos seus dispositivos de rede (servidores proxy, firewalls, etc.) para permitir ligações de saída para determinados URLs pertencentes à Microsoft e endereços IP nas portas de rede específicas. Estas alterações podem variar consoante a versão da ferramenta do Azure AD Connect. Para obter mais contexto, pode ler mais sobre [como funciona a repetição de escrita de palavras-passe](active-directory-passwords-learn-more.md#how-password-writeback-works) e [o modelo de segurança de repetição de escrita de palavras-passe](active-directory-passwords-learn-more.md#password-writeback-security-model).

#### <a name="why-do-i-need-to-do-this"></a>Por que motivo é necessário fazê-lo?

Para a Repetição de Escrita de Palavras-passe funcionar corretamente, a máquina com o Azure AD Connect tem de conseguir estabelecer ligações HTTPS de saída para **.servicebus.windows.net* e endereço IP específico utilizado pelo Azure, tal como definido na [lista de Intervalos IP do Datacenter do Microsoft Azure](https://www.microsoft.com/download/details.aspx?id=41653).

Para versões da ferramenta do Azure AD Connect 1.0.8667.0 e versões posteriores:

- **Opção 1:** permitir todas as ligações HTTPS de saída através da porta 443 (com o URL ou endereço IP).
    - Quando utilizá-los:
        - Utilize esta opção se pretender que a configuração mais simples que não precisa de ser atualizada como alteração de intervalos IP do Datacenter do Azure seja alterada ao longo do tempo.
    - Passos necessários:
        - Permitir todas as ligações HTTPS de saída através da porta 443 com o URL ou endereço IP.
<br><br>
- **Opção 2:** permitir que as ligações HTTPS de saída especifiquem intervalos IP e URLs
    - Quando utilizá-los:
        - Utilize esta opção se estiver num ambiente de rede limitado ou, caso contrário, se não se sentir confortável em permitir ligações de saída.
        - Nesta configuração, para a repetição de escrita de palavras-passe continuar a trabalhar, terá de se certificar que os dispositivos de rede se mantêm atualizados semanalmente com os IPs mais recentes a partir da lista de Intervalos de IP do Datacenter do Microsoft Azure. Estes intervalos IP estão disponíveis como um ficheiro XML que é atualizado a cada quarta-feira (Hora do Pacífico) e entra em vigor na seguinte segunda-feira (Hora do Pacífico).
    - Passos necessários:
        - Permitir todas as ligações HTTPS de saída para o *.servicebus.windows.net
        - Permitir todas as ligações HTTPS de saída para todos os IPs na lista de Intervalos IP do Datacenter do Microsoft Azure e manter esta configuração atualizada semanalmente.

> [!NOTE]
> Se tiver configurado a Repetição de Escrita de Palavras-passe seguindo as instruções descritas acima e não vir erros no registo de eventos do Azure AD Connect, mas está a receber erros de conetividade ao testar, então poderá dever-se a um dispositivo de rede no seu ambiente estar a bloquear ligações HTTPS para endereços IP. Por exemplo, apesar de uma ligação para *https://*.servicebus.windows.net* ser permitida, pode estar bloqueada uma ligação para um endereço IP específico dentro do intervalo. Para resolver este problema, deve configurar o ambiente de rede para permitir todas as ligações HTTPS de saída através da porta 443 para qualquer URL ou endereço IP (Opção 1 acima) ou trabalhar com a sua equipa de rede para permitir explicitamente ligações HTTPS para endereços IP específicos (Opção 2 acima).

**Para versões mais antigas:**

- Permita ligações de saída TCP através da porta 443, 9350-9354 e da porta 5671 
- Permita ligações de saída para *https://ssprsbprodncu-sb.accesscontrol.windows.net/*

> [!NOTE]
> Se tiver uma versão do Azure AD Connect anterior à 1.0.8667.0, a Microsoft recomenda vivamente que atualize para a [versão mais recente do Azure AD Connect](https://www.microsoft.com/download/details.aspx?id=47594), que inclui uma série de melhorias de rede de repetição de escrita, para facilitar a configuração de rede.

Assim que os dispositivos de rede estiverem configurados, reinicie a máquina que executa a ferramenta do Azure AD Connect.

### <a name="step-4-set-up-the-appropriate-active-directory-permissions"></a>Passo 4: Configurar as permissões adequadas do Active Directory
Para cada floresta que contenha utilizadores cujas palavras-passe serão repostas, se X for a conta especificada para essa floresta no assistente de configuração (durante a configuração inicial), devem ser concedidos a X os direitos expandidos **Repor Palavra-passe**, **Alterar Palavra-passe**, **Permissões de Escrita** em `lockoutTime` e **Permissões de Escrita** em `pwdLastSet` no objeto raiz de cada domínio dessa floresta. O direito deve ser marcado como herdado por todos os objetos de utilizador.  

Se não tiver a certeza sobre a conta a que nos referimos acima, abra a IU de configuração do Azure Active Directory Connect e clique na opção **Rever a sua Solução**.  A conta para a qual tem de adicionar permissão está sublinhada a vermelho na captura de ecrã abaixo.

**<font color="red">Certifique-se de que define esta permissão para cada domínio em cada floresta do sistema. Caso contrário, a repetição de escrita de palavras-passe não funcionará corretamente.</font>**

  ![][032]

  Definir estas permissões permite que a conta de serviço MA de cada floresta faça a gestão de palavras-passe em nome de contas de utilizador nessa floresta. Se não atribuir estas permissões, apesar de a repetição de escrita parecer estar configurada corretamente, os utilizadores receberão erros quando tentarem gerir as respetivas palavras-passe no local a partir da nuvem. Apresentamos a seguir os passos detalhados para o procedimento utilizando o snap-in de gestão **Utilizadores e Computadores do Active Directory**:

> [!NOTE]
> Pode demorar até uma hora para que estas permissões sejam replicadas em todos os objetos do diretório.
> 
> 

#### <a name="to-set-up-the-right-permissions-for-writeback-to-occur"></a>Para configurar as permissões corretas para a repetição de escrita
1. Abra **Utilizadores e Computadores do Active Directory** com uma conta que possua as permissões de administração de domínio apropriadas.
2. Na opção **Menu Ver**, verifique se a opção **Funcionalidades Avançadas** está ativada.
3. No painel esquerdo, clique com o botão direito do rato no objeto que representa a raiz do domínio.
4. Clique no separador **Segurança**.
5. Em seguida, clique em **Avançado**.
   
   ![][024]
6. No separador **Permissões**, clique em **Adicionar**.
   
   ![][025]
7. Selecione a conta para a qual pretende atribuir permissões (esta é a mesma conta que foi especificada ao configurar a sincronização para essa floresta).
8. Na lista pendente na parte superior, selecione **Objetos de Utilizadores Descendentes**.
9. Na caixa de diálogo **Introdução de Permissão** apresentada, marque as caixas **Repor Palavra-passe**, **Alterar Palavra-passe**, **Permissões de Escrita** em `lockoutTime` e **Permissões de Escrita** em `pwdLastSet`.
   
   ![][026]
   ![][027]
   ![][028]
10. Em seguida, clique em **Aplicar/OK** em todas as caixas de diálogo abertas.

### <a name="step-5-reset-your-ad-password-as-a-user"></a>Passo 5: Repor a palavra-passe do AD como utilizador
Agora que a Repetição de Escrita de Palavras-passe foi ativada, pode testar se funciona ao repor a palavra-passe de um utilizador cuja conta tenha sido sincronizada com o inquilino de nuvem.

#### <a name="to-verify-password-writeback-is-working-properly"></a>Para verificar se a Repetição de Escrita de Palavras-passe está a funcionar corretamente
1. Navegue até [https://passwordreset.microsoftonline.com](https://passwordreset.microsoftonline.com) ou aceda a qualquer ecrã de início de sessão de ID organizacional e clique na ligação **Não consegue aceder à sua conta?**
   
   ![][029]
2. Deverá ver uma nova página que lhe pede o ID de utilizador para o qual pretende repor a palavra-passe. Introduza o ID de utilizador de teste e avance no fluxo de reposição de palavra-passe.
3. Depois de repor a palavra-passe, será apresentado um ecrã semelhante a este. Isto significa que repôs com êxito a palavra-passe no diretórios no local e/ou na nuvem.
   
   ![][030]
4. Para verificar se a operação foi concluída com êxito ou diagnosticar eventuais erros, aceda ao **computador de Directory Sync**, abra o **Visualizador de Eventos**, navegue para o **registo de eventos da aplicação** e procure o evento **31002 – PasswordResetSuccess** do **PasswordResetService** de origem do utilizador de teste.
   
   ![][031]

<br/>
<br/>
<br/>

## <a name="links-to-password-reset-documentation"></a>Ligações para a documentação de reposição de palavra-passe
Veja-se abaixo as ligações para todas as páginas da documentação de reposição de palavra-passe do Azure AD:

* **Está aqui porque está a ter problemas em iniciar sessão?** Se assim for, [Eis como pode alterar e repor a sua própria palavra-passe](active-directory-passwords-update-your-own-password.md).
* [**Como funciona**](active-directory-passwords-how-it-works.md) – saiba mais acerca dos seis componentes diferentes do serviço e o que cada um faz
* [**Personalizar**](active-directory-passwords-customize.md) – saiba como personalizar o aspeto e o comportamento do serviço de acordo com as necessidades da sua organização
* [**Práticas recomendadas**](active-directory-passwords-best-practices.md) – saiba como implementar rapidamente e gerir de forma eficaz as palavras-passe da organização
* [**Obter conhecimentos aprofundados**](active-directory-passwords-get-insights.md) – saiba mais acerca das nossas capacidades de relatório integradas
* [**FAQ**](active-directory-passwords-faq.md) – obtenha respostas às perguntas mais frequentes
* [**Resolução de problemas**](active-directory-passwords-troubleshoot.md) – aprenda rapidamente como resolver problemas relacionados com o serviço
* [**Saiba mais**](active-directory-passwords-learn-more.md) – aprofunde os detalhes técnicos acerca do funcionamento do serviço

[001]: ./media/active-directory-passwords-getting-started/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-getting-started/002.jpg "Image_002.jpg"
[003]: ./media/active-directory-passwords-getting-started/003.jpg "Image_003.jpg"
[004]: ./media/active-directory-passwords-getting-started/004.jpg "Image_004.jpg"
[005]: ./media/active-directory-passwords-getting-started/005.jpg "Image_005.jpg"
[006]: ./media/active-directory-passwords-getting-started/006.jpg "Image_006.jpg"
[007]: ./media/active-directory-passwords-getting-started/007.jpg "Image_007.jpg"
[008]: ./media/active-directory-passwords-getting-started/008.jpg "Image_008.jpg"
[009]: ./media/active-directory-passwords-getting-started/009.jpg "Image_009.jpg"
[010]: ./media/active-directory-passwords-getting-started/010.jpg "Image_010.jpg"
[011]: ./media/active-directory-passwords-getting-started/011.jpg "Image_011.jpg"
[012]: ./media/active-directory-passwords-getting-started/012.jpg "Image_012.jpg"
[013]: ./media/active-directory-passwords-getting-started/013.jpg "Image_013.jpg"
[014]: ./media/active-directory-passwords-getting-started/014.jpg "Image_014.jpg"
[015]: ./media/active-directory-passwords-getting-started/015.jpg "Image_015.jpg"
[016]: ./media/active-directory-passwords-getting-started/016.jpg "Image_016.jpg"
[017]: ./media/active-directory-passwords-getting-started/017.jpg "Image_017.jpg"
[018]: ./media/active-directory-passwords-getting-started/018.jpg "Image_018.jpg"
[019]: ./media/active-directory-passwords-getting-started/019.jpg "Image_019.jpg"
[020]: ./media/active-directory-passwords-getting-started/020.jpg "Image_020.jpg"
[021]: ./media/active-directory-passwords-getting-started/021.jpg "Image_021.jpg"
[022]: ./media/active-directory-passwords-getting-started/022.jpg "Image_022.jpg"
[023]: ./media/active-directory-passwords-getting-started/023.jpg "Image_023.jpg"
[024]: ./media/active-directory-passwords-getting-started/024.jpg "Image_024.jpg"
[025]: ./media/active-directory-passwords-getting-started/025.jpg "Image_025.jpg"
[026]: ./media/active-directory-passwords-getting-started/026.jpg "Image_026.jpg"
[027]: ./media/active-directory-passwords-getting-started/027.jpg "Image_027.jpg"
[028]: ./media/active-directory-passwords-getting-started/028.jpg "Image_028.jpg"
[029]: ./media/active-directory-passwords-getting-started/029.jpg "Image_029.jpg"
[030]: ./media/active-directory-passwords-getting-started/030.jpg "Image_030.jpg"
[031]: ./media/active-directory-passwords-getting-started/031.jpg "Image_031.jpg"
[032]: ./media/active-directory-passwords-getting-started/032.jpg "Image_032.jpg"



<!--HONumber=Jan17_HO1-->


