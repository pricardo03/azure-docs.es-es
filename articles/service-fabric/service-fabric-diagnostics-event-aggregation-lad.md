---
title: Agregación de eventos de Azure Service Fabric con Linux Azure Diagnostics | Microsoft Docs
description: Obtenga información sobre la agregación y la recopilación de eventos con LAD para la supervisión y el diagnóstico de clústeres de Azure Service Fabric.
services: service-fabric
documentationcenter: .net
author: srrengar
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 2/25/2019
ms.author: srrengar
ms.openlocfilehash: ca6be2b08c9a38813b2c98eef7e5696bd61fd074
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "57893040"
---
# <a name="event-aggregation-and-collection-using-linux-azure-diagnostics"></a>Recopilación y agregación de eventos con Linux Azure Diagnostics
> [!div class="op_single_selector"]
> * [Windows](service-fabric-diagnostics-event-aggregation-wad.md)
> * [Linux](service-fabric-diagnostics-event-aggregation-lad.md)
>
>

Cuando se ejecuta un clúster de Azure Service Fabric, es conveniente recopilar los registros de todos los nodos en una ubicación central. La presencia de los registros en una ubicación central facilita el análisis y la solución de los problemas del clúster o de las aplicaciones y los servicios que se ejecutan en ese clúster.

Uno de los métodos para cargar y recopilar registros es usar la extensión Linux Azure Diagnostics (LAD), que carga registros en Azure Storage, y también tiene la opción de enviar registros a Azure Application Insights o Event Hubs. También puede usar un proceso externo para leer los eventos de almacenamiento y colocarlos en un producto de plataforma de análisis, como [registros de Azure Monitor](../log-analytics/log-analytics-service-fabric.md) u otra solución de análisis de registros.

## <a name="log-and-event-sources"></a>Orígenes de eventos y registros

### <a name="service-fabric-platform-events"></a>Eventos de plataforma de Service Fabric
Service Fabric genera una serie de registros de serie a través de [LTTng](https://lttng.org), incluidos eventos de runtime o eventos operativos. Estos registros se almacenan en la ubicación que la plantilla de Resource Manager del clúster especifica. Para obtener o establecer los detalles de la cuenta de almacenamiento, busque la etiqueta **AzureTableWinFabETWQueryable** y busque **StoreConnectionString**.

### <a name="application-events"></a>Eventos de aplicación
 Eventos emitidos desde el código de los servicios y las aplicaciones especificados por el usuario al instrumentar el software. Puede utilizar cualquier solución de registro que escriba archivos de registro basados en texto, por ejemplo, LTTng. Para obtener más información, consulte la documentación de LTTng sobre el seguimiento de la aplicación.

[Supervisión y diagnóstico de servicios en una configuración de desarrollo de máquina local](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md)

## <a name="deploy-the-diagnostics-extension"></a>Implementación de la extensión de Diagnósticos
El primer paso para recopilar registros será implementar la extensión WAD en cada una de las máquinas virtuales del clúster de Service Fabric. La extensión de Diagnósticos recopila registros en cada máquina virtual y los carga a la cuenta de almacenamiento que especifique. 

Para implementar la extensión de Diagnósticos en las máquinas virtuales del clúster como parte de la creación de dicho clúster, establezca **Diagnósticos** en **Activado**. Después de crear el clúster, no se puede cambiar esta configuración mediante el portal, por lo que tendrá que realizar los cambios apropiados en la plantilla de Resource Manager.

Esto configura el agente LAD para supervisar los archivos de registro especificados. Siempre que se anexa una nueva línea al archivo, se crea una entrada syslog que se envía al almacenamiento (tabla) especificado.


## <a name="next-steps"></a>Pasos siguientes

1. Compruebe la [documentación de LTTng](https://lttng.org/docs) y el [uso de LAD](https://docs.microsoft.com/azure/virtual-machines/extensions/diagnostics-linux) para obtener información más detallada sobre qué eventos debe examinar durante la solución de problemas.
2. [Configuración del agente de Log Analytics](service-fabric-diagnostics-event-analysis-oms.md) con el fin de recopilar métricas, supervisar contenedores implementados en el clúster y visualizar los registros 
