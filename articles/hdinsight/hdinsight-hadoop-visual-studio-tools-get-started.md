---
title: Aprender a utilizar as Ferramentas do Data Lake (HDInsight) para Visual Studio | Microsoft Docs
description: Saiba como instalar e utilizar as Ferramentas do Data Lake (HDInsight) para Visual Studio para se ligar a um cluster de Hadoop e executar uma consulta do Hive.
keywords: ferramentas do hadoop,consulta do hive,visual studio
services: HDInsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: ce9c572a-1e98-46bf-9581-13a9767f1fa5
ms.service: hdinsight
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/02/2016
ms.author: jgao
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 8bd214c166190248662634d488d8e7b1427b5738


---
# <a name="get-started-using-azure-data-lake-hdinsight-tools-for-visual-studio-to-run-a-hive-query"></a>Começar a utilizar as Ferramentas do Azure Data Lake (HDInsight) para Visual Studio para executar uma consulta do Hive
Saiba como utilizar as Ferramentas do Data Lake (HDInsight) para Visual Studio para se ligar a clusters do HDInsight e submeter consultas do Hive. Para obter mais informações sobre a utilização do HDInsight, consulte [Introdução ao HDInsight][hdinsight.introduction] e [Começar a utilizar o HDInsight][hdinsight.get.started]. Para obter mais informações sobre a ligação a um cluster do Storm, consulte [Desenvolver topologias C# para o Apache Storm no HDInsight com o Visual Studio][hdinsight.storm.visual.studio.tools].

As Ferramentas do Data Lake para Visual Studio podem ser utilizadas para aceder ao Data Lake Analytics e ao HDInsight.  Para obter informações sobre as Ferramentas do Data Lake, veja o [Tutorial: desenvolver scripts U-SQL com as Ferramentas do Data Lake para Visual Studio](../data-lake-analytics/data-lake-analytics-data-lake-tools-get-started.md).

**Pré-requisitos**

Para concluir este tutorial e utilizar as Ferramentas do Data Lake para Visual Studio, precisa do seguinte:

* Um cluster do Azure HDInsight: é possível utilizar um cluster baseado em Linux ou um cluster baseado em Windows para executar os passos neste documento. Consulte um dos seguintes artigos para obter informações sobre a criação de um cluster:
  
  * [Começar a utilizar o HDInsight baseado em Linux](hdinsight-hadoop-linux-tutorial-get-started.md)
  * [Começar a utilizar o HDInsight baseado em Windows](hdinsight-hadoop-tutorial-get-started-windows.md)
* Uma estação de trabalho com o seguinte software:
  
  * Windows 8.1, Windows 8 ou Windows 7
  * Visual Studio (uma das seguintes versões):
    
    * Visual Studio 2013 Community/Professional/Premium/Ultimate com [Atualização 4](https://www.microsoft.com/download/details.aspx?id=44921)
    * Visual Studio 2015 (Community/Enterprise)
    
    > [!NOTE]
    > Atualmente, as Ferramentas do Data Lake para Visual Studio são fornecidas apenas com a versão em inglês.
    > 
    > 

## <a name="install-data-lake-tools-for-visual-studio"></a>Instalar as Ferramentas do Data Lake para Visual Studio
Pode instalar as Ferramentas do Data Lake com o [Instalador de Plataforma Web](https://www.microsoft.com/web/downloads/). Tem de escolher aquele que corresponde à sua versão do Visual Studio. Se não tiver o Visual Studio instalado, pode instalar o mais recente Visual Studio Community e o SDK do Azure com o [Instalador de Plataforma Web](https://www.microsoft.com/web/downloads/):

![Instalador de Plataforma Web das Ferramentas do Data Lake para Visual Studio.][1]

## <a name="connect-to-azure-subscriptions"></a>Ligar a subscrições do Azure
As Ferramentas do Data Lake para Visual Studio permitem-lhe ligar-se aos seus clusters do HDInsight, realizar algumas operações de gestão básicas e executar consultas do Hive.

> [!NOTE]
> Para obter informações sobre a ligação a um cluster do Hadoop genérico, consulte [Escrever e submeter consultas do Hive com o Visual Studio](http://blogs.msdn.com/b/xiaoyong/archive/2015/05/04/how-to-write-and-submit-hive-queries-using-visual-studio.aspx).
> 
> 

**Para ligar à sua subscrição do Azure**

1. Abra o Visual Studio.
2. No menu **Vista**, clique em **Explorador de Servidores** para abrir a janela do Explorador de Servidores.
3. Expanda **Azure** e, em seguida, expanda **HDInsight**.
   
   > [!NOTE]
   > A janela **Lista de Tarefas do HDInsight** deve ser apresentada. Se não a vir, clique em **Outras Janelas** no menu **Vista** e, em seguida, clique em **Janela de Lista de Tarefas do HDInsight**.  
   > 
   > 
4. Introduza as credenciais da sua subscrição do Azure e clique em **Iniciar Sessão**. Esta ação só é necessária se nunca se tiver ligado à subscrição do Azure a partir do Visual Studio nesta estação de trabalho.
5. No Explorador de Servidores, é apresentada uma lista dos clusters do HDInsight existentes. Se não tiver clusters, pode criar um através do Portal do Azure, do Azure PowerShell ou do SDK do HDInsight. Para obter mais informações, veja [Criar clusters do HDInsight][hdinsight-create-clusters].
   
   ![Lista de clusters do Explorador de Servidores das Ferramentas do Data Lake para Visual Studio][5]
6. Expanda um cluster do HDInsight. Verá **Bases de Dados do Hive**, uma conta do Storage predefinida, contas do Storage ligadas e o **Registo do serviço do Hadoop**. Pode expandir ainda mais as entidades.

Depois de se ligar à sua subscrição do Azure, poderá fazer o seguinte:

**Ligar-se ao portal do Azure a partir do Visual Studio**

* No Explorador de Servidores, expanda **Azure** > **HDInsight**, clique com o botão direito do rato num cluster do HDInsight e, em seguida, clique em **Gerir o Cluster no portal do Azure**.

**Colocar questões e enviar comentários a partir do Visual Studio**

* No menu **Ferramentas**, clique em **HDInsight** e, em seguida, clique em **Fórum MSDN** para colocar questões ou clique em **Enviar Comentários**.

## <a name="navigate-the-linked-resources"></a>Navegar nos recursos ligados
No Explorador de Servidores, pode ver a conta do Storage predefinida e quaisquer contas do Storage ligadas. Se expandir a conta do Storage predefinida, pode ver os contentores incluídos na mesma. A conta do Storage predefinida e o contentor predefinido estão marcados. Também pode clicar com o botão direito do rato em qualquer um dos contentores para ver o respetivo conteúdo.

![Lista de clusters do explorador de servidores das Ferramentas do Data Lake para Visual Studio][2]

Depois de abrir um contentor, pode utilizar os botões seguintes para carregar, eliminar e transferir blobs:

![Operações de blobs do explorador de servidores das Ferramentas do Data Lake para Visual Studio](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.blob.operations.png)

## <a name="run-a-hive-query"></a>Executar uma consulta do Hive
O [Apache Hive][apache.hive] é uma infraestrutura de armazém de dados baseada em Hadoop destinada a fornecer resumo de dados, consultas e análises. As Ferramentas do Data Lake para Visual Studio suportam a execução de consultas do Hive a partir do Visual Studio. Para obter mais informações sobre o Hive, consulte [Utilizar o Hive com o HDInsight][hdinsight.hive].

O teste de scripts Hive contra um cluster do HDInsight é um processo demorado. Pode demorar vários minutos ou mais. As Ferramentas do Data Lake para Visual Studio são capazes de validar os scripts de Hive localmente, sem estabelecer ligação a um cluster ativo.

Além disso, as Ferramentas do Data Lake para Visual Studio também permitem aos utilizadores ver o conteúdo da tarefa do Hive ao recolher e analisar os registos YARN de determinadas tarefas do Hive.

### <a name="view-the-hivesampletable"></a>Ver a tabela **hivesampletable**
Todos os clusters do HDInsight incluem uma tabela do Hive de exemplo denominada *hivesampletable*. Utilizaremos esta tabela para lhe mostrar como listar as tabelas do Hive, ver os esquemas de tabela e listar as linhas da tabela do Hive.

**Para listar as tabelas do Hive e ver o esquema da tabela do Hive**

1. No **Explorador de Servidores**, expanda **Azure** > **HDInsight** > o cluster pretendido > **Bases de Dados do Hive** > **Predefinição** > **hivesampletable** para ver o esquema da tabela.
2. Clique com o botão direito do rato em **hivesampletable** e, em seguida, clique em **Ver as Primeiras 100 Linhas** para listar as linhas. Esta ação é equivalente a executar a seguinte consulta do Hive com o controlador ODBC do Hive:
   
     SELECT * FROM hivesampletable LIMIT 100
   
   Pode personalizar a contagem de linhas.
   
   ![Ferramentas do Data Lake: consulta de esquema do Hive do HDinsight para Visual Studio][6]

### <a name="create-hive-tables"></a>Criar tabelas do Hive
Pode utilizar a GUI para criar uma tabela do Hive ou utilizar consultas do Hive. Para obter informações sobre a utilização de consultas do Hive, consulte [Executar consultas do Hive](#run.queries).

**Para criar uma tabela do Hive**

1. No **Explorador de Servidores**, expanda **Azure** > **Clusters do HDInsight** um cluster do HDInsight > **Bases de Dados do Hive**, clique com o botão direito do rato em **predefinição** e clique em **Criar Tabela**.
2. Configure a tabela.
3. Clique em **Criar Tabela** para submeter a tarefa de criação da nova tabela do Hive.
   
    ![Ferramentas do Data Lake: HDInsight Visual Studio Tools - criar uma tabela do hive][7]

### <a name="a-namerunqueriesavalidate-and-run-hive-queries"></a><a name="run.queries"></a>Validar e executar consultas do Hive
Existem duas formas de criar e executar consultas do Hive:

* Criar consultas ad-hoc
* Criar uma aplicação do Hive

**Para criar, validar e executar consultas ad-hoc**

1. No **Explorador de Servidores**, expanda **Azure** e, em seguida, expanda **Clusters do HDInsight**.
2. Clique com o botão direito do rato no cluster no qual pretende executar a consulta e, em seguida, clique em **Escrever uma consulta do Hive**.
3. Introduza as consultas do Hive. Tenha em atenção que o editor do Hive suporta IntelliSense. As Ferramentas do Data Lake para Visual Studio suportam o carregamento de metadados remotos durante a edição do script do Hive. Por exemplo, quando escreve "SELECT * FROM", o IntelliSense apresenta uma lista de todos os nomes de tabela sugeridos. Quando é especificado um nome de tabela, o IntelliSense mostra uma lista dos nomes das colunas. As ferramentas suportam quase todas as instruções DML do Hive, subconsultas e os UDFs incorporados.
   
    ![Ferramentas do Data Lake: IntelliSense do HDInsight para Visual Studio Tools][13]
   
    ![Ferramentas do Data Lake: IntelliSense do HDInsight para Visual Studio Tools][14]
   
   > [!NOTE]
   > Serão sugeridos apenas os metadados dos clusters selecionados na Barra de Ferramentas do HDInsight.
   > 
   > 
4. (Opcional): clique em **Validar Script** para verificar os erros de sintaxe do script.
   
    ![Ferramentas do Data Lake: validação local das Ferramentas do Data Lake para Visual Studio][10]
5. Clique em **Submeter** ou **Submeter (Avançado)**. Com a opção de submissão avançada, irá configurar **Nome da Tarefa**, **Argumentos**, **Configurações Adicionais** e **Diretório de Estado** para o script:
   
    ![consulta do hive do Hadoop HDInsight][9]
   
    Depois de submeter a tarefa, será apresentada uma janela **Resumo da Tarefa do Hive**.
   
    ![Resumo de uma consulta do Hive do Hadoop HDInsight][8]
6. Utilize o botão **Atualizar** para atualizar o estado da tarefa até que este mude para **Concluído**.
7. Clique nas ligações na parte inferior para ver o seguinte: **Consulta da Tarefa**, **Saída da Tarefa**, **Registo da tarefa** ou **Registo Yarn**.

**Para criar e executar uma solução do Hive**

1. No menu **FICHEIRO**, clique em **Novo** e, em seguida, clique em **Projeto**.
2. Selecione **HDInsight** no painel esquerdo, selecione **Aplicação do Hive** no painel do meio, introduza as propriedades e, em seguida, clique em **OK**.
   
    ![Ferramentas do Data Lake: novo projeto do hive no HDInsight Visual Studio Tools][11]
3. No **Explorador de Soluções**, faça duplo clique em **Script.hql** para o abrir.
4. Para validar o script do Hive, pode clicar no botão **Validar Script** ou clicar com o botão direito do rato no script no editor do Hive e, em seguida, clicar em **Validar Script** no menu de contexto.

### <a name="view-hive-jobs"></a>Ver tarefas do Hive
Pode ver as consultas da tarefa, a saída da tarefa, os registos da tarefa e os registos Yarn para as tarefas do Hive. Para obter mais informações, consulte a captura de ecrã anterior.

A versão mais recente das ferramentas permite-lhe ver o conteúdo das tarefas do Hive ao recolher e analisar registos YARN. Um registo YARN pode ajudar a investigar problemas de desempenho. Para obter mais informações sobre a forma como o HDInsight recolhe registos YARN, consulte [Aceder Programaticamente aos Registos de Aplicações do HDInsight][hdinsight.access.application.logs].

**Para ver tarefas do Hive**

1. No **Explorador de Servidores**, expanda **Azure** e, em seguida, expanda **HDInsight**.
2. Clique com o botão direito do rato num cluster do HDInsight e, em seguida, clique em **Ver Tarefas**. É apresentada uma lista das tarefas do Hive executadas no cluster.
3. Clique numa tarefa na lista de tarefas para a selecionar e, em seguida, utilize a janela **Resumo da Tarefa do Hive** para abrir **Consulta da Tarefa**, **Saída da Tarefa**, **Registo da Tarefa** ou **Registo Yarn**.
   
    ![Ferramentas do Data Lake: ver tarefas do Hive no HDInsight Visual Studio Tools][12]

### <a name="faster-path-hive-execution-via-hiveserver2"></a>Execução do Hive de caminho mais rápido através do HiveServer2
> [!NOTE]
> Esta funcionalidade funciona apenas nos clusters do HDInsight versão 3.2 e mais recente.
> 
> 

Anteriormente, as Ferramentas do Data Lake submetiam as tarefas do Hive através de [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (também conhecido como Templeton). No entanto, a devolução dos detalhes da tarefa e de informações de erro demorava muito tempo.
Para resolver este problema de desempenho, as Ferramentas do Data Lake executam as tarefas do Hive diretamente no cluster através do HiveServer2, de modo a ignorar os protocolos RDP/SSH.
Para além de usufruírem de um melhor desempenho, os utilizadores também podem ver o Hive em gráficos Tez, bem como os detalhes da tarefa.

Para os clusters do HDInsight versão 3.2 ou posterior, é apresentado um botão **Executar através de HiveServer2**:

![Data Lake visual studio Tools executar através de hiveserver2](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.execute.via.hiveserver2.png)

Além disso, se a consulta do Hive for executada no Tez, pode ver os registos transmitidos em tempo real e ver os gráficos da tarefa.

![Data Lake visual studio Tools execução do hive através do caminho rápido](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.fast.path.hive.execution.png)

**Diferença entre executar consultas através de HiveServer2 e submeter consultas através de WebHCat**

Embora a execução de consultas através de HiveServer2 tenha várias vantagens ao nível do desempenho, tem também várias limitações. Algumas das limitações não são adequadas para utilização em produção. A tabela seguinte mostra as diferenças:

|  | Execução através de HiveServer2 | Submissão através de WebHCat |
| --- | --- | --- |
| Executar consultas |Elimina o overhead no WebHCat (que inicia uma Tarefa de MapReduce com o nome “TempletonControllerJob”). |Sempre que uma consulta seja executada através de WebHCat, o WebHCat inicia uma tarefa de MapReduce que cria latência adicional. |
| Transmitir os registos |Quase em tempo real. |Os registos de execução da tarefa ficam disponíveis apenas quando a tarefa estiver concluída. |
| Ver histórico de tarefas |Se uma consulta for executada através de HiveServer2, o respetivo histórico de tarefas (registo da tarefa, resultado da tarefa) não é mantido. É possível ver a aplicação na IU do YARN com informações limitadas. |Se uma consulta for executada através de WebHCat, o respetivo histórico de tarefas (registo da tarefa, resultado da tarefa) é preservado e pode ser visualizado através do Visual Studio/SDK do HDInsight/PowerShell. |
| Fechar janela |A execução através de HiveServer2 é realizada de forma “síncrona”, pelo que é necessário manter as janelas abertas. Se as janelas forem fechadas, a execução da consulta é cancelada. |A submissão através de WebHCat é realizada de forma “assíncrona”, pelo que pode submeter a consulta através de WebHCat e fechar o Visual Studio. Pode voltar atrás e ver os resultados em qualquer altura. |

### <a name="tez-hive-job-performance-graph"></a>Gráfico de desempenho da tarefa do Hive no Tez
As ferramentas do Data Lake suportam a apresentação de gráficos de desempenho para as tarefas do Hive executadas pelo motor de execução Tez. Para obter informações sobre como ativar o Tez, consulte [Utilizar o Hive no HDInsight][hdinsight.hive]. Depois de submeter uma tarefa do Hive no Visual Studio, o Visual Studio mostra-lhe o gráfico quando a tarefa estiver concluída.  Poderá ter de clicar no botão **Atualizar** para obter o estado mais recente da tarefa.

> [!NOTE]
> Esta funcionalidade só está disponível para os clusters do HDInsight superior à versão 3.2.4.593 e funciona apenas para as tarefas concluídas (se tiver submetido a tarefa através de WebHCat; este gráfico será apresentado quando executar a consulta através de HiveServer2). Isto funciona para os clusters baseados em Windows e os clusters baseados em Linux.
> 
> 

![gráfico de desempenho do hadoop hive no tez](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.hive.tez.performance.graph.png)

Para ajudar a compreender melhor a sua consulta do Hive, as ferramentas adicionam a vista Operador do Hive nesta versão. Basta fazer duplo clique nos vértices do gráfico da tarefa para ver todos os operadores dentro do vértice. Também pode pairar o rato sobre um operador específico para ver mais detalhes do mesmo.

### <a name="task-execution-view-for-hive-on-tez-jobs"></a>Vista de execução de tarefas para tarefas do Hive no Tez
É possível utilizar a Vista de execução de tarefas para tarefas do Hive no Tez para obter informações estruturadas e visualizadas sobre tarefas do Hive, bem como para obter mais detalhes da tarefa. Caso existam problemas de desempenho, pode utilizar esta vista para obter mais detalhes. Por exemplo, pode obter dados sobre o funcionamento de cada tarefa e informações detalhadas sobre as mesmas (leitura/escrita de dados, hora de agendamento/início/fim, etc.), de modo a poder ajustar as configurações da tarefa ou a arquitetura do sistema com base nas informações visualizadas.

![vista de execução de tarefas no Data Lake Visual Studio Tools](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.task.execution.view.png)

## <a name="run-pig-scripts"></a>Executar scripts Pig
As Ferramentas do Data Lake para Visual Studio suportam a criação e submissão de scripts Pig para clusters do HDInsight. Os utilizadores podem criar um projeto do Pig a partir de um modelo e, em seguida, submeter o script para os clusters do HDInsight.

## <a name="feedbacks-known-issues"></a>Comentários e problemas conhecidos
* Atualmente, os resultados do HiveServer2 são apresentados sob a forma de texto simples, o que não é ideal. Estamos a trabalhar para resolver este problema.
* Se os resultados forem iniciados com valores NULL, os resultados não são apresentados atualmente. Resolvemos este problema. Contudo, se estiver bloqueado devido ao mesmo, não hesite em enviar-nos um e-mail ou contactar a equipa de suporte.
* O script HQL criado pelo Visual Studio é codificado consoante a definição de região local do utilizador. Se o utilizador o carregar para o cluster como binário, este script pode não ser executado corretamente.

## <a name="next-steps"></a>Passos seguintes
Neste artigo, aprendeu a ligar-se a clusters do HDInsight a partir do Visual Studio, com o pacote de Ferramentas do Data Lake, e a executar uma consulta do Hive. Para obter mais informações, consulte:

* [Utilizar o Hive do Hadoop no HDInsight][hdinsight.hive]
* [Começar a utilizar o Hadoop no HDInsight][hdinsight.get.started]
* [Submeter tarefas do Hadoop no HDInsight][hdinsight.submit.jobs]
* [Analisar dados do Twitter com o Hadoop no HDInsight][hdinsight.analyze.twitter.data]

<!--Anchors-->
[Instalação]: #installation
[Ligar à sua subscrição do Azure]: #connect-to-your-azure-subscription
[Navegar nos recursos ligados]: #navigate-the-linked-resources
[Executar consultas do Hive]: #run-hive-queries
[Passos seguintes?]: #next-steps

<!--Image references-->
[1]: ./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.wpi.png
[2]: ./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.linked.resources.png
[5]: ./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.server.explorer.png
[6]: ./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.hive.schema.png
[7]: ./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.create.hive.table.png
[8]: ./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.run.hive.job.summary.png
[9]: ./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.submit.jobs.advanced.png
[10]: ./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.validate.hive.script.png
[11]: ./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.new.hive.project.png
[12]: ./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.view.hive.jobs.png
[13]: ./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.intellisense.table.names.png
[14]: ./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.intellisense.column.names.png


<!--Link references-->
[hdinsight-create-clusters]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight.introduction]: hdinsight-hadoop-introduction.md
[hdinsight.get.started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight.hive]: hdinsight-use-hive.md
[hdinsight.submit.jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight.analyze.twitter.data]: hdinsight-analyze-twitter-data.md
[hdinsight.storm.visual.studio.tools]: hdinsight-storm-develop-csharp-visual-studio-topology.md
[hdinsight.access.application.logs]: hdinsight-hadoop-access-yarn-app-logs.md

[apache.hive]: http://hive.apache.org



<!--HONumber=Nov16_HO2-->


