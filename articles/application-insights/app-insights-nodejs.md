---
title: "Adicionar o Application Insights SDK para monitorizar a aplicação Node.js | Microsoft Docs"
description: "Analise a utilização, a disponibilidade e o desempenho da aplicação Web no local ou do Microsoft Azure com o Application Insights."
services: application-insights
documentationcenter: 
author: alancameronwills
manager: douge
ms.assetid: 2ec7f809-5e1a-41cf-9fcd-d0ed4bebd08c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/30/2016
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: fb80168b38be88ab18952569e6b6f9bcb53d473a


---
# <a name="add-application-insights-sdk-to-monitor-your-nodejs-app"></a>Adicionar o Application Insights SDK para monitorizar a aplicação Node.js
*O Application Insights está em pré-visualização.*

O [Visual Studio Application Insights](app-insights-overview.md) monitoriza a aplicação em direto para o ajudar a [detetar e diagnosticar problemas de desempenho e exceções](app-insights-detect-triage-diagnose.md), bem como a [detetar como a aplicação é utilizada](app-insights-overview-usage.md). Funciona para as aplicações alojadas nos seus próprios servidores IIS no local ou em VMs do Azure, bem como em aplicações Web do Azure.

O SDK fornece uma coleção automática de taxas de pedidos de HTTP recebidos e respostas, contadores de desempenho (CPU, memória, RPS) e exceções não processadas. Além disso, pode adicionar chamadas personalizadas para controlar as dependências, as métricas ou outros eventos.

![Gráficos de exemplo da monitorização do desempenho](./media/app-insights-nodejs/10-perf.png)

#### <a name="before-you-start"></a>Antes de começar
É necessário:

* Visual Studio 2013 ou posterior. É preferível a versão posterior.
* Uma subscrição do [Microsoft Azure](http://azure.com). Se a sua equipa ou organização tiver uma subscrição do Azure, o proprietário pode adicioná-lo utilizando a [Conta Microsoft](http://live.com).

## <a name="a-nameaddacreate-an-application-insights-resource"></a><a name="add"></a>Criar um recurso do Application Insights
Inicie sessão no [Portal do Azure][portal] e crie um novo recurso do Application Insights. Um [recurso][roles] no Azure é uma instância de um serviço. Este recurso é onde a telemetria da aplicação será analisada e apresentada.

![Clicar em Novo, Application Insights](./media/app-insights-nodejs/01-new-asp.png)

Escolha Outro como o tipo de aplicação. A escolha do tipo de aplicação define o conteúdo predefinido dos painéis de recursos e as propriedades visíveis no [Explorador de Métricas][metrics].

#### <a name="copy-the-instrumentation-key"></a>Copiar a Chave de Instrumentação
A chave identifica o recurso. Deverá instalá-lo logo no SDK para direcionar os dados para o recurso.

![Clicar em Propriedades, selecionar a chave e premir Ctrl+C](./media/app-insights-nodejs/02-props-asp.png)

## <a name="a-namesdka-install-the-sdk-in-your-application"></a><a name="sdk"></a> Instalar o SDK na aplicação
```
npm install applicationinsights --save
```

## <a name="usage"></a>Utilização
Esta operação permitirá monitorizar os pedidos, controlar as exceções não processadas e monitorizar o desempenho do sistema (CPU, memória/RPS).

```javascript

var appInsights = require("applicationinsights");
appInsights.setup("<instrumentation_key>").start();
```

Também é pode definir a chave de instrumentação na variável de ambiente APPINSIGHTS_INSTRUMENTATIONKEY Se proceder deste forma, nenhum argumento será solicitado aquando da chamada `appInsights.setup()` ou `appInsights.getClient()`.

Pode experimentar o SDK sem enviar telemetria: defina a chave de instrumentação para uma cadeia não vazia.

## <a name="a-nameruna-run-your-project"></a><a name="run"></a> Executar o projeto
Execute e experimente a aplicação: abra páginas diferentes para gerar alguma telemetria.

## <a name="a-namemonitora-view-your-telemetry"></a><a name="monitor"></a> Ver a telemetria
Volte ao [Portal do Azure](https://portal.azure.com) e navegue para o recurso do Application Insights.

Procure dados na página Descrição Geral. Inicialmente, verá apenas um ou dois pontos. Por exemplo:

![Clicar para mais dados](./media/app-insights-nodejs/12-first-perf.png)

Clique em qualquer gráfico para ver métricas mais detalhadas. [Saiba mais sobre as métricas.][perf]

#### <a name="no-data"></a>Não existem dados?
* Utilize a aplicação, abrindo páginas diferentes, de modo a gerar alguma telemetria.
* Abra o mosaico [Pesquisa](app-insights-diagnostic-search.md) para ver eventos individuais. Por vezes, os eventos demoram um pouco mais de tempo a chegar ao pipeline de métricas.
* Aguarde alguns segundos e clique em **Atualizar**. Os gráficos atualizam-se periodicamente, mas pode atualizá-los manualmente se estiver à espera que apareçam alguns dados.
* Veja [Resolução de Problemas][qna].

## <a name="publish-your-app"></a>Publicar a aplicação
Agora, implemente a aplicação no IIS ou no Azure e veja os dados a acumularem.

#### <a name="no-data-after-you-publish-to-your-server"></a>Não existem dados depois de publicar no servidor?
Abra estas portas para o tráfego de saída na firewall do servidor:

* `dc.services.visualstudio.com:443`
* `f5.services.visualstudio.com:443`

#### <a name="trouble-on-your-build-server"></a>Problemas no servidor de compilação?
Veja [este item de Resolução de Problemas](app-insights-asp-net-troubleshoot-no-data.md#NuGetBuild).

## <a name="customized-usage"></a>Utilização Personalizada
### <a name="disabling-autocollection"></a>Desativar a coleção automática
```javascript
import appInsights = require("applicationinsights");
appInsights.setup("<instrumentation_key>")
    .setAutoCollectRequests(false)
    .setAutoCollectPerformance(false)
    .setAutoCollectExceptions(false)
    // no telemetry will be sent until .start() is called
    .start();
```

### <a name="custom-monitoring"></a>Monitorização personalizada
```javascript
import appInsights = require("applicationinsights");
var client = appInsights.getClient();

client.trackEvent("custom event", {customProperty: "custom property value"});
client.trackException(new Error("handled exceptions can be logged with this method"));
client.trackMetric("custom metric", 3);
client.trackTrace("trace message");
```

[Saiba mais sobre a API de telemetria](app-insights-api-custom-events-metrics.md).

### <a name="using-multiple-instrumentation-keys"></a>Utilizar várias chaves de instrumentação
```javascript
import appInsights = require("applicationinsights");

// configure auto-collection with one instrumentation key
appInsights.setup("<instrumentation_key>").start();

// get a client for another instrumentation key
var otherClient = appInsights.getClient("<other_instrumentation_key>");
otherClient.trackEvent("custom event");
```

## <a name="examples"></a>Exemplos
### <a name="tracking-dependency"></a>Controlar a dependência
```javascript
import appInsights = require("applicationinsights");
var client = appInsights.getClient();

var startTime = Date.now();
// execute dependency call
var endTime = Date.now();

var elapsedTime = endTime - startTime;
var success = true;
client.trackDependency("dependency name", "command name", elapsedTime, success);
```



### <a name="manual-request-tracking-of-all-get-requests"></a>Controlo manual de todos os pedidos “GET”
```javascript
var http = require("http");
var appInsights = require("applicationinsights");
appInsights.setup("<instrumentation_key>")
    .setAutoCollectRequests(false) // disable auto-collection of requests for this example
    .start();

// assign common properties to all telemetry sent from the default client
appInsights.client.commonProperties = {
    environment: process.env.SOME_ENV_VARIABLE
};

// track a system startup event
appInsights.client.trackEvent("server start");

// create server
var port = process.env.port || 1337
var server = http.createServer(function (req, res) {
    // track all "GET" requests
    if(req.method === "GET") {
        appInsights.client.trackRequest(req, res);
    }

    res.writeHead(200, { "Content-Type": "text/plain" });
    res.end("Hello World\n");
}).listen(port);

// track startup time of the server as a custom metric
var start = +new Date;
server.on("listening", () => {
    var end = +new Date;
    var duration = end - start;
    appInsights.client.trackMetric("StartupTime", duration);
});
```

## <a name="next-steps"></a>Passos seguintes
* [Monitorizar a telemetria no portal](app-insights-dashboards.md)
* [Escrever consultas de análise sobre a telemetria](app-insights-analytics-tour.md)

<!--Link references-->

[knowUsers]: app-insights-overview-usage.md
[metrics]: app-insights-metrics-explorer.md
[perf]: app-insights-web-monitor-performance.md
[portal]: http://portal.azure.com/
[qna]: app-insights-troubleshoot-faq.md
[roles]: app-insights-resources-roles-access-control.md



<!--HONumber=Nov16_HO2-->


