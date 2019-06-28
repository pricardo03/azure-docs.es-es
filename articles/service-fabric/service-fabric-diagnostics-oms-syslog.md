---
title: Supervisión de eventos de clúster Linux en Azure Service Fabric | Microsoft Docs
description: Obtenga información sobre cómo supervisar los eventos de clúster Linux desde Syslog.
services: service-fabric
documentationcenter: .net
author: srrengar
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/23/2018
ms.author: srrengar
ms.openlocfilehash: 402e3dfe018c94ef068caf918b38aaad00064a49
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "62118383"
---
# <a name="service-fabric-linux-cluster-events-in-syslog"></a>Eventos de clúster Linux de Service Fabric en Syslog

Service Fabric expone un conjunto de eventos de la plataforma para informarle de actividad importante en el clúster. La lista completa de eventos expuestos está disponible [aquí](service-fabric-diagnostics-event-generation-operational.md). Hay varias maneras de consumir estos eventos. En este artículo, vamos a explicar cómo configurar Service Fabric para escribir estos eventos en Syslog.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="introduction"></a>Introducción

En la versión 6.4, se ha introducido SyslogConsumer para enviar los eventos de la plataforma de Service Fabric a Syslog para clústeres Linux. Una vez activados, los eventos fluirán automáticamente a Syslog, donde los recopilará y enviará el agente de Log Analytics.

Cada evento Syslog tiene 4 componentes:
* Facility
* Identidad
* Message
* Gravedad

SyslogConsumer escribe todos los eventos de la plataforma mediante el recurso `Local0`. Puede actualizar a cualquier recurso válido cambiando la configuración. La identidad utilizada es `ServiceFabric`. El campo de mensaje contiene el evento completo serializado en JSON, por lo que pueden consultarlo y utilizarlo distintas herramientas. 

## <a name="enable-syslogconsumer"></a>Habilitar SyslogConsumer

Para habilitar SyslogConsumer, deberá realizar una actualización del clúster. La sección `fabricSettings` debe actualizarse con el siguiente código. Tenga en cuenta que este código solo incluye las secciones relacionadas con SyslogConsumer.

```json
    "fabricSettings": [
        {
            "name": "Diagnostics",
            "parameters": [
            {
                "name": "ConsumerInstances",
                "value": "AzureWinFabCsv, AzureWinFabCrashDump, AzureTableWinFabEtwQueryable, SyslogConsumer"
            }
            ]
        },
        {
            "name": "SyslogConsumer",
            "parameters": [
            {
                "name": "ProducerInstance",
                "value": "WinFabLttProducer"
            },
            {
            "name": "ConsumerType",
            "value": "SyslogConsumer"
            },
            {
                "name": "IsEnabled",
                "value": "true"
            }
            ]
        },
        {
            "name": "Common",
            "parameters": [
            {
                "name": "LinuxStructuredTracesEnabled",
                "value": "true"
            }
            ]
        }
    ],
```

Estos son los cambios que se deben realizar:
1. En la sección Común, hay un nuevo parámetro llamado `LinuxStructuredTracesEnabled`. **Es necesario que los eventos de Linux estén estructurados y serializados cuando se envían a Syslog**.
2. En la sección Diagnóstico, se a agregado un nuevo elemento ConsumerInstance: SyslogConsumer. Esto indica a la plataforma que hay otro consumidor de los eventos. 
3. La nueva sección SyslogConsumer debe tener `IsEnabled` como `true`. Está configurada para utilizar el recurso Local0 automáticamente. Puede invalidar esto agregando otro parámetro.

```json
    {
        "name": "New LogFacility",
        "value": "<Valid Syslog Facility>"
    }
```

## <a name="azure-monitor-logs-integration"></a>Integración de los registros de Azure Monitor
Puede leer estos eventos de Syslog en una herramienta de supervisión como los registros de Azure Monitor. Puede crear un área de trabajo de Log Analytics con Azure Marketplace mediante estas [instrucciones].(../azure-monitor/learn/quick-create-workspace.md) También debe agregar el agente de Log Analytics al clúster para recopilar y enviar estos datos al área de trabajo. Este es el mismo agente que se usa para recopilar los contadores de rendimiento. 

1. Navegue hasta la hoja `Advanced Settings`.

    ![Configuración de área de trabajo](media/service-fabric-diagnostics-oms-syslog/workspace-settings.png)

2. Haga clic en `Data`
3. Haga clic en `Syslog`
4. Configure Local0 como el recurso del que realizar el seguimiento. Puede agregar otro recurso si lo cambia en fabricSettings.

    ![Configuración de Syslog](media/service-fabric-diagnostics-oms-syslog/syslog-configure.png)
5. Acceda al explorador de consultas; para ello, haga clic en `Logs` en el menú del recurso de área de trabajo para iniciar la consulta.

    ![Registros del área de trabajo](media/service-fabric-diagnostics-oms-syslog/workspace-logs.png)
6. Puede consultar la tabla `Syslog` si busca `ServiceFabric` como ProcessName. La consulta siguiente es un ejemplo de cómo analizar el esquema JSON en el evento y mostrar su contenido.

```kusto
    Syslog | where ProcessName == "ServiceFabric" | extend $payload = parse_json(SyslogMessage) | project $payload
```

![Consulta de Syslog](media/service-fabric-diagnostics-oms-syslog/syslog-query.png)

El ejemplo anterior es de un evento NodeDown. Puede ver la lista completa de eventos [aquí](service-fabric-diagnostics-event-generation-operational.md).

## <a name="next-steps"></a>Pasos siguientes
* [Implemente el agente de Log Analytics](service-fabric-diagnostics-oms-agent.md) en los nodos para recopilar contadores de rendimiento, así como estadísticas y registros de Docker de sus contenedores.
* Familiarícese con las característica de [búsqueda de registros y consulta](../log-analytics/log-analytics-log-searches.md) que se ofrecen como parte de los registros de Azure Monitor.
* [Uso del Diseñador de vistas para crear vistas personalizadas en los registros de Azure Monitor](../log-analytics/log-analytics-view-designer.md)
* Consulte esta referencia para obtener información sobre la [integración de los registros de Azure Monitor con Syslog](../log-analytics/log-analytics-data-sources-syslog.md).
