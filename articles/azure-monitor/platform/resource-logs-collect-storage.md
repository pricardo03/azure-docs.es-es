---
title: Archivado de registros de recurso de Azure en la cuenta de almacenamiento | Microsoft Docs
description: Aprenda a archivar los registros de recurso de Azure para su retención a largo plazo en una cuenta de almacenamiento.
author: bwren
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/20/2019
ms.author: bwren
ms.subservice: logs
ms.openlocfilehash: 8a1802f0f24ba5ccad3ec1c45f3baa29dfe6909f
ms.sourcegitcommit: 55f7fc8fe5f6d874d5e886cb014e2070f49f3b94
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2019
ms.locfileid: "71263039"
---
# <a name="archive-azure-resource-logs-to-storage-account"></a>Archivado de registros de recurso de Azure en la cuenta de almacenamiento
Los [registros de recurso](resource-logs-overview.md) en Azure proporcionan datos exhaustivos y frecuentes acerca del funcionamiento interno de un recurso de Azure. En este artículo se describe la recopilación de registros de recurso en una cuenta de almacenamiento de Azure para conservar los datos para su archivado.

## <a name="prerequisites"></a>Requisitos previos
Si no la tiene, puede crear una [cuenta de almacenamiento de Azure](../../storage/common/storage-quickstart-create-account.md). La cuenta de almacenamiento no tiene que estar en la misma suscripción que la del recurso que envía los registros, siempre que el usuario que realiza la configuración tenga el acceso RBAC adecuado a ambas suscripciones.

No debe utilizar una cuenta de almacenamiento existente que tenga otros datos sin supervisión almacenados en ella, para que pueda controlar mejor el acceso a los datos de supervisión. Sin embargo, si también va a archivar el [registro de actividad](activity-logs-overview.md) en una cuenta de almacenamiento, puede que tenga sentido utilizar esa misma cuenta de almacenamiento para mantener todos los datos de supervisión en una ubicación central.

## <a name="create-a-diagnostic-setting"></a>Creación de una configuración de diagnóstico
Los registros de recurso no se recopilan de forma predeterminada. Recopílelos en una cuenta de almacenamiento de Azure y otros destinos mediante la creación de configuración de diagnóstico para un recurso de Azure. Consulte [Creación de una configuración de diagnóstico para recopilar registros y métricas en Azure](diagnostic-settings.md) para más información.


## <a name="data-retention"></a>Retención de datos
La directiva de retención define el número de días que deben conservarse los datos de cada categoría de registro y los de las métricas almacenados en una cuenta de almacenamiento. Una directiva de retención puede durar de 0 a 365 días. Si se establece en cero, los eventos para de categoría de registro se almacenan indefinidamente.

Las directivas de retención se aplican a diario, por lo que al final de un día (UTC) se eliminan los registros del día que quede fuera de la directiva de retención. Por ejemplo, si tuviera una directiva de retención de un día, se eliminarían los registros de anteayer al principio del día de hoy. El proceso de eliminación empieza a medianoche (UTC), pero tenga en cuenta que eliminar los registros de la cuenta de almacenamiento puede tardar hasta 24 horas. 


## <a name="schema-of-resource-logs-in-storage-account"></a>Esquema de los registros de recurso en una cuenta de almacenamiento

Una vez creada la configuración de diagnóstico, en cuanto se produzca un evento en una de las categorías de registro que haya habilitado se creará un contenedor de almacenamiento en la cuenta de almacenamiento. Los blobs del contenedor usan la siguiente convención de nomenclatura:

```
insights-logs-{log category name}/resourceId=/SUBSCRIPTIONS/{subscription ID}/RESOURCEGROUPS/{resource group name}/PROVIDERS/{resource provider name}/{resource type}/{resource name}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json
```

Por ejemplo, el blob de un grupo de seguridad de red puede tener un nombre similar al siguiente:

```
insights-logs-networksecuritygrouprulecounter/resourceId=/SUBSCRIPTIONS/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUP/TESTNSG/y=2016/m=08/d=22/h=18/m=00/PT1H.json
```

Cada blob PT1H.json contiene un blob JSON de eventos que se produjeron en la hora especificada en la dirección URL del blob (por ejemplo h=12). Durante la hora en cuestión, los eventos se anexan al archivo PT1H.json a medida que se producen. El valor en minutos (m = 00) siempre es 00, ya que los eventos del registro de recurso se dividen en blobs individuales por hora.

En el archivo PT1H.json, los eventos se almacenan con este formato; con un esquema general común, pero único para cada uno de los servicios de Azure, tal y como se describe en [Esquema de los registros de recurso](resource-logs-overview.md#resource-logs-schema).

``` JSON
{"time": "2016-07-01T00:00:37.2040000Z","systemId": "46cdbb41-cb9c-4f3d-a5b4-1d458d827ff1","category": "NetworkSecurityGroupRuleCounter","resourceId": "/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/TESTNSG","operationName": "NetworkSecurityGroupCounters","properties": {"vnetResourceGuid": "{12345678-9012-3456-7890-123456789012}","subnetPrefix": "10.3.0.0/24","macAddress": "000123456789","ruleName": "/subscriptions/ s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg/securityRules/default-allow-rdp","direction": "In","type": "allow","matchedConnections": 1988}}
```

> [!NOTE]
> Los registros de plataforma se escriben en el almacenamiento de blobs en formato [JSON Lines](http://jsonlines.org/), donde cada evento es una línea y el carácter de nueva línea indica un evento nuevo. Este formato se implementó en noviembre de 2018. Antes de esta fecha, los registros se escribían en el almacenamiento de blobs como matriz JSON de registros, tal como se describe en [Preparación para el cambio de formato a los registros de plataforma de Azure Monitor archivados en una cuenta de almacenamiento](resource-logs-blob-format.md).

## <a name="next-steps"></a>Pasos siguientes

* [Descargar blobs para el análisis](../../storage/blobs/storage-quickstart-blobs-dotnet.md).
* [Archivar registros de Azure Active Directory con Azure Monitor](../../active-directory/reports-monitoring/quickstart-azure-monitor-route-logs-to-storage-account.md).
* [Más información sobre los registros de recurso](../../azure-monitor/platform/resource-logs-overview.md).

