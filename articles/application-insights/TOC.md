# Descrição geral
## [O que é o Application Insights?](app-insights-overview.md)
## [Monitorização de desempenho num ciclo de devOps](app-insights-detect-triage-diagnose.md)

# Introdução
## Monitorizar o Azure
### [Aplicações Web do Azure](app-insights-azure-web-apps.md)
### [Azure Cloud Services](app-insights-cloudservices.md)

## Monitorizar aplicações ASP.NET
### [Aplicações Web](app-insights-asp-net.md)
### [Aplicações Web já em direto](app-insights-monitor-performance-live-website-now.md)
### [Serviços Windows](app-insights-windows-services.md)
### [Ambiente de trabalho do Windows](app-insights-windows-desktop.md)

## Monitorizar aplicações Java
### [Aplicações Web](app-insights-java-get-started.md)
### [Aplicações Web – runtime](app-insights-java-live.md)
### [Aplicações Docker](app-insights-docker.md)

## Monitorizar páginas Web
### [JavaScript](app-insights-javascript.md)

## Monitorizar outras plataformas
### [Aplicações Node.js](app-insights-nodejs.md)
### [Sites do SharePoint](app-insights-sharepoint.md)
### [Mais plataformas](app-insights-platforms.md)

## [FAQ do ASP.NET](app-insights-troubleshoot-faq.md)

# Procedimento
## Planear e conceber
### [Diagnósticos avançados de aplicações Web e serviços](app-insights-devops.md)
### [Análise de Programador com o Application Insights e o HockeyApp](app-insights-developer-analytics.md)
### [Monitorizar o desempenho nas aplicações Web](app-insights-web-monitor-performance.md)
### [Análise de utilização com o Application Insights](app-insights-overview-usage.md)
### [Separar recursos do Application Insights](app-insights-separate-resources.md)
### [Como... no Application Insights?](app-insights-how-do-i.md)
## Migrar
### [Migração da Monitorização do Ponto Final do Azure para os Testes de disponibilidade](app-insights-migrate-azure-endpoint-tests.md)

## Configurar
### [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md)
### Azure
#### [Diagnóstico](app-insights-azure-diagnostics.md)
## [Endereços IP](app-insights-ip-addresses.md)

### ASP.NET
#### [Recolher mais telemetria](app-insights-asp-net-more.md)
#### [Exceções](app-insights-asp-net-exceptions.md)
#### [Rastreios de registos](app-insights-asp-net-trace-logs.md)
#### [Contadores de desempenho](app-insights-performance-counters.md)
#### [Dependências](app-insights-asp-net-dependencies.md)
#### [Anotações da versão](app-insights-annotations.md)


### J2EE
#### [Rastreios de registos](app-insights-java-trace-logs.md)
#### [Métricas do Unix](app-insights-java-collectd.md)
#### [Dependências](app-insights-java-agent.md)

### Alertas

#### [Disponibilidade](app-insights-monitor-web-app-availability.md)
#### [Alertas de Métricas](app-insights-alerts.md)

### [Deteção Inteligente](app-insights-proactive-diagnostics.md)
#### [Anomalias de falha](app-insights-proactive-failure-diagnostics.md)
#### [Anomalias de desempenho](app-insights-proactive-performance-diagnostics.md)

## Analisar

### Portal do Application Insights

#### [Dashboards](app-insights-dashboards.md)
#### [Pesquisa](app-insights-diagnostic-search.md)
#### [Métricas](app-insights-metrics-explorer.md)
#### Análise

##### [Análise](app-insights-analytics.md)
##### [Uma visita guiada da Análise](app-insights-analytics-tour.md)
##### [Utilizar a Análise](app-insights-analytics-using.md)

#### [Mapeamento de Aplicações](app-insights-app-map.md)
#### [Dados do HockeyApp](app-insights-hockeyapp-bridge-app.md)
#### [Criar um grupo de recursos](app-insights-create-new-resource.md)

### Visual Studio

#### [Informações do F5](app-insights-visual-studio.md)
#### [Tendências](app-insights-visual-studio-trends.md)
#### [CodeLens](app-insights-visual-studio-codelens.md)

## Automatizar

### [Configuração do PowerShell](app-insights-powershell.md)
### [Criar recursos](app-insights-powershell-script-create-resource.md)
### [Definir alertas](app-insights-powershell-alerts.md)
### [Obter diagnóstico do Azure](app-insights-powershell-azure-diagnostics.md)


## Integrar

### [Exportação contínua](app-insights-export-telemetry.md)
### [Exportar para o Power BI](app-insights-export-power-bi.md)

## Programar

### [API de métricas e eventos personalizados](app-insights-api-custom-events-metrics.md)
### [Filtragem e processamento prévio de telemetria](app-insights-api-filtering-sampling.md)
### [Núcleo do ASP.NET](app-insights-asp-net-core.md)


## Gerir
### [Gerir preços e quotas](app-insights-pricing.md)
### [Monitorização do Desempenho de Aplicações com o Application Insights para SCOM](app-insights-scom.md)

##Exportar
## [Exportar modelo de dados](app-insights-export-data-model.md)

## Proteger
### [Recolha, retenção e armazenamento de dados](app-insights-data-retention-privacy.md)
### [Recursos, funções e controlo de acesso](app-insights-resources-roles-access-control.md)
## Resolução de problemas
### [Não existem dados para o .NET](app-insights-asp-net-troubleshoot-no-data.md)
### [Análise](app-insights-analytics-troubleshooting.md)
### [Java](app-insights-java-troubleshoot.md)

# Referência
## [.NET](https://docs.microsoft.com/dotnet/api/microsoft.applicationinsights)
## [Java](http://dl.windowsazure.com/applicationinsights/javadoc/)
## [REST](https://dev.applicationinsights.io/)

# Recursos
## [Referência de análise](app-insights-analytics-reference.md)
## [JavaScript](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md)
## [Análise do programador: idiomas, plataformas e integrações](app-insights-platforms.md)
### [Amostragem](app-insights-sampling.md)
### [Exemplos e instruções](app-insights-code-samples.md)
#### [Instruções: Ativar a Telemetria para o Microsoft Dynamics CRM Online](app-insights-sample-mscrm.md)
#### [Instruções: Exportar para o SQL com o Stream Analytics](app-insights-code-sample-export-sql-stream-analytics.md)
#### [Exemplo de código: analisar dados exportados](app-insights-code-sample-export-telemetry-sql-database.md)
## [Notas de Versão do SDK do Application Insights para Windows Phone e Loja](app-insights-release-notes-windows.md)
## [Notas de Versão das Ferramentas de Análise de Programador](app-insights-release-notes-vsix.md)
## [Notas de Versão do SDK do Application Insights](app-insights-release-notes.md)
## [Preços](https://azure.microsoft.com/pricing/details/application-insights/)  
## [Fórum do MSDN](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=ApplicationInsights)  
## [Stack Overflow](http://stackoverflow.com/questions/tagged/az-application-insights)
## [Vídeos](https://azure.microsoft.com/documentation/videos/index/?services=application-insights) 
## [Atualizações de serviço](https://azure.microsoft.com/en-us/updates/?product=application-insights) 
## [Suporte](app-insights-get-dev-support.md)




<!--HONumber=Dec16_HO1-->


