1. Inicie sessão no [Portal do Azure][Portal do Azure].
2. No painel de navegação à esquerda do portal, clique em **Novo** e, em seguida, clique em **Integração do Enterprise** e em **Service Bus**.
3. Na caixa de diálogo **Criar espaço de nomes**, introduza um nome de espaço de nomes. O sistema verifica imediatamente a disponibilidade do nome.
4. Após se certificar de que o espaço de nomes está disponível, selecione o escalão de preço (Básico, Standard ou Premium).
5. No campo **Subscrição**, selecione a subscrição do Azure em que pretende criar o espaço de nomes.
6. No campo **Grupo de recursos**, selecione um grupo de recursos existente em que o espaço de nomes será colocado ou crie um novo.      
7. Em **Localização**, selecione o país ou a região em que o espaço de nomes deverá ser alojado.
   
    ![Create namespace][create-namespace]
8. Clique em **Criar**. O sistema cria o espaço de nomes e ativa-o. Poderá ter de aguardar alguns minutos enquanto o sistema aprovisiona recursos para a sua conta.

### <a name="obtain-the-management-credentials"></a>Obter as credenciais de gestão
1. Na lista de espaços de nomes, clique no nome do espaço de nomes criado recentemente.
2. No painel de espaço de nomes clique em **Políticas de Acesso Partilhado**.
3. No painel **Políticas de acesso partilhado**, clique em **RootManageSharedAccessKey**.
   
    ![connection-info][connection-info]
4. No painel **Política: RootManageSharedAccessKey**, clique no botão de cópia junto a **Cadeia de ligação – chave primária** para copiar a cadeia de ligação para a sua área de transferência e utilizá-la mais tarde. Cole este valor no Bloco de Notas ou noutra localização temporária.
   
    ![connection-string][connection-string]

<!--Image references-->

[create-namespace]: ./media/service-bus-create-namespace-portal/create-namespace.png
[connection-info]: ./media/service-bus-create-namespace-portal/connection-info.png
[connection-string]: ./media/service-bus-create-namespace-portal/connection-string.png

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[Portal do Azure]: https://portal.azure.com

<!--HONumber=Nov16_HO2-->


