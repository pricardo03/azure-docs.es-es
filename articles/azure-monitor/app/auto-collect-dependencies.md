---
title: 'Azure Application Insights: recopilación automática de dependencias | Microsoft Docs'
description: Recopilación y visualización automática de dependencias de Application Insights
services: application-insights
documentationcenter: .net
author: nikmd23
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.topic: reference
ms.date: 08/13/2018
ms.reviewer: mbullwin
ms.author: nimolnar
ms.openlocfilehash: 68a504f43463a25a0aba8d7d72881cddbcfba816
ms.sourcegitcommit: da69285e86d23c471838b5242d4bdca512e73853
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/03/2019
ms.locfileid: "53999346"
---
# <a name="dependency-auto-collection"></a>Recopilación automática de dependencias

A continuación encontrará la lista de las llamadas de dependencia admitida actualmente que se detectan automáticamente como dependencias sin requerir ninguna modificación adicional en el código de la aplicación. Consta de las llamadas salientes a bibliotecas de comunicaciones, clientes de almacenamiento, bibliotecas de registro y métricas, así como las llamadas entrantes a marcos de trabajo de la aplicación y servidores. Estas dependencias se visualizan en el [Mapa de aplicación](https://docs.microsoft.com/azure/application-insights/app-insights-app-map) de Application Insights y las vistas de [Diagnóstico de transacciones](https://docs.microsoft.com/azure/application-insights/app-insights-transaction-diagnostics). Si la dependencia no se encuentra en la siguiente lista, todavía puede realizar un seguimiento manual con una [llamada a TrackDependency ](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#trackdependency).

## <a name="net"></a>.NET

| Marcos de trabajo de aplicaciones| Versiones |
| ------------------------|----------|
| WebForms de ASP.NET | 4.5+ |
| ASP.NET MVC | 4+ |
| WebAPI de ASP.NET | 4.5+ |
| ASP.NET Core | 1.1+ |
| <b>Bibliotecas de comunicaciones</b> |
| [HttpClient](https://www.microsoft.com/net/) | 4.5+, .NET Core 1.1+ |
| [SqlClient](https://www.nuget.org/packages/System.Data.SqlClient) | .NET Core 1.0+, NuGet 4.3.0 |
| [SDK de cliente de Event Hubs](https://www.nuget.org/packages/Microsoft.Azure.EventHubs) | 1.1.0 |
| [SDK de cliente de Service Bus](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus) | 3.0.0 |
| <b>Clientes de Storage</b>|  |
| ADO.NET | 4.5+ |
| <b>Bibliotecas de registro</b> |  |
| ILogger | 1.1+ |
| System.Diagnostics.Trace | 4.5+ |
| [nLog](https://www.nuget.org/packages/NLog/) | 4.4.12+ |
| [log4net](https://www.nuget.org/packages/log4net/) | 2.0.8+ en NetStandard 1.3, 2.0.6+ en .NET 4.5+ |

## <a name="java"></a>Java
| Servidores de aplicaciones | Versiones |
|-------------|----------|
| [Tomcat](https://tomcat.apache.org/) | 7, 8 | 
| [JBoss EAP](https://developers.redhat.com/products/eap/download/) | 6, 7 |
| [Jetty](https://www.eclipse.org/jetty/) | 9 |
| <b>Marcos de trabajo de aplicaciones</b> |  |
| [Spring](https://spring.io/) | 3.0 |
| [Spring Boot](https://spring.io/projects/spring-boot) | 1.5.9+<sup>*</sup> |
| Servlet de Java | 3.1+ |
| <b>Bibliotecas de comunicaciones</b> |  |
| [Cliente HTTP de Apache](https://mvnrepository.com/artifact/org.apache.httpcomponents/httpclient) | 4.3+<sup>†</sup> |
| <b>Clientes de Storage</b> | |
| [SQL Server]( https://mvnrepository.com/artifact/com.microsoft.sqlserver/mssql-jdbc) | 1+<sup>†</sup> |
| [Oracle]( https://www.oracle.com/technetwork/database/application-development/jdbc/downloads/index.html) | 1+<sup>†</sup> |
| [MySQL]( https://mvnrepository.com/artifact/mysql/mysql-connector-java) | 1+<sup>†</sup> |
| <b>Bibliotecas de registro</b> | |
| [Logback](https://logback.qos.ch/) | 1+ |
| [Log4j](https://logging.apache.org/log4j/) | 1.2+ |
| <b>Bibliotecas de métricas</b> |  |
| JMX | 1.0+ |

> [!NOTE]
> * Excepto el soporte técnico de programación reactivo.
> <br>† Requiere la instalación del [agente de JVM](https://docs.microsoft.com/azure/application-insights/app-insights-java-agent#install-the-application-insights-agent-for-java).

## <a name="nodejs"></a>Node.js

| Bibliotecas de comunicaciones | Versiones |
| ------------------------|----------|
| [HTTP](https://nodejs.org/api/http.html), [HTTPS](https://nodejs.org/api/https.html) | 0.10+ |
| <b>Clientes de Storage</b> | |
| [Redis](https://www.npmjs.com/package/redis) | 2.x |
| [MongoDb](https://www.npmjs.com/package/mongodb); [MongoDb Core](https://www.npmjs.com/package/mongodb-core) | 2.0.0 - 2.3.0 |
| [MySQL](https://www.npmjs.com/package/mysql) | 2.0.0 - 2.14.x |
| [PostgreSQL](https://www.npmjs.com/package/pg); | 6.x |
| [pg-pool](https://www.npmjs.com/package/pg-pool) | 1.x |
| <b>Bibliotecas de registro</b> | |
| [Consola](https://nodejs.org/api/console.html) | 0.10+ |
| [Bunyan](https://www.npmjs.com/package/bunyan) | 1.x |
| [Winston](https://www.npmjs.com/package/winston) | 2.x |

## <a name="javascript"></a>JavaScript

| Bibliotecas de comunicaciones | Versiones |
| ------------------------|----------|
| [XMLHttpRequest](https://developer.mozilla.org/docs/Web/API/XMLHttpRequest) | Todo |

## <a name="next-steps"></a>Pasos siguientes

- Configuración del seguimiento de dependencias personalizadas para [.NET](../../azure-monitor/app/asp-net-dependencies.md).
- Configuración del seguimiento de dependencias personalizadas para [Java](../../azure-monitor/app/java-agent.md).
- [Escritura de una telemetría de dependencia personalizada](../../azure-monitor/app/api-custom-events-metrics.md#trackdependency)
- Consulte [modelo de datos](../../azure-monitor/app/data-model.md) para los tipos y el modelo de datos de Application Insights.
- Consulte las [plataformas](../../azure-monitor/app/platforms.md) compatibles con Application Insights.
