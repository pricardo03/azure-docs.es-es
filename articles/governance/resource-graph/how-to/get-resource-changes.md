---
title: Obtención de los cambios del recurso
description: Comprenda cómo saber cuándo un recurso se cambió y obtener una lista de las propiedades que cambiaron.
services: resource-graph
author: DCtheGeek
ms.author: dacoulte
ms.date: 05/10/2019
ms.topic: conceptual
ms.service: resource-graph
manager: carmonm
ms.openlocfilehash: b6ef57a3f39c82be30d92aef72c1bbe03b653768
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66236505"
---
# <a name="get-resource-changes"></a>Obtención de los cambios del recurso

Los recursos se modifican a través del uso diario, la reconfiguración e incluso la reimplementación.
Los cambios pueden proceder de un usuario individual o un proceso automatizado. La mayoría de los cambios son así por diseño, pero a veces no lo son. Con los últimos 14 días de historial de cambios,Azure Resource Graph le permite hacer lo siguiente:

- Buscar cuándo se detectaron cambios en una propiedad de Azure Resource Manager.
- Ver qué propiedades cambiaron como parte de ese evento de cambio.

La detección y los detalles de cambios son de utilidad para los siguientes escenarios de ejemplo:

- Durante la administración de incidentes para comprender los cambios _potencialmente_ relacionados. Consultar los eventos de cambio durante un período de tiempo específico y evaluar los detalles del cambio.
- Mantener actualizada una base de datos de administración de configuración, conocida como una CMDB. En lugar de actualizar todos los recursos y sus conjuntos de propiedad completos según una frecuencia programada, obtener solo lo que cambió.
- Comprender qué otras propiedades podrían haber cambiado cuando un recurso puede cambió de estado de cumplimiento. La evaluación de estas propiedades adicionales puede proporcionar información sobre otras propiedades que quizás tengan que administrarse a través de una definición de Azure Policy.

En este artículo se muestra cómo recopilar esta información a través del SDK del Resource Graph. Para ver esta información en Azure Portal, consulte Azure Policy [Historial de cambios](../../policy/how-to/determine-non-compliance.md#change-history-preview) o [Historial de cambios](../../../azure-monitor/platform/activity-log-view.md#azure-portal) del Registro de actividad de Azure.

> [!NOTE]
> Los detalles de cambio en Resource Graph son para las propiedades de Resource Manager. Para realizar un seguimiento de los cambios dentro de una máquina virtual, consulte las opciones [Seguimiento de cambios](../../../automation/automation-change-tracking.md) o Azure Policy [Configuración de invitado para VM](../../policy/concepts/guest-configuration.md) de Azure Automation.

> [!IMPORTANT]
> El historial de cambios en Azure Resource Graph está disponible como versión preliminar pública.

## <a name="find-when-changes-were-detected"></a>Búsqueda de la detección de cambios

El primer paso para ver qué cambió en un recurso es buscar los eventos de cambio relacionados con ese recurso dentro de una ventana de tiempo. Este paso se realiza a través del punto de conexión REST **resourceChanges**.

El punto de conexión **resourceChanges** requiere dos parámetros en el cuerpo de solicitud:

- **resourceId**: el recurso de Azure para el que se buscan cambios.
- **interval**: una propiedad con fecha de _inicio_ y _finalización_ durante las que se buscará evento de cambio mediante la **zona horaria Zulu (Z)** .

Ejemplo del cuerpo de solicitud:

```json
{
    "resourceId": "/subscriptions/{subscriptionId}/resourceGroups/MyResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
    "interval": {
        "start": "2019-03-28T00:00:00.000Z",
        "end": "2019-03-31T00:00:00.000Z"
    }
}
```

Con el cuerpo de solicitud anterior, el URI de la API de REST para **resourceChanges** es:

```http
POST https://management.azure.com/providers/Microsoft.ResourceGraph/resourceChanges?api-version=2018-09-01-preview
```

La respuesta será similar a la siguiente:

```json
{
    "changes": [{
            "changeId": "{\"beforeId\":\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\",\"beforeTime\":'2019-05-09T00:00:00.000Z\",\"afterId\":\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\",\"beforeTime\":'2019-05-10T00:00:00.000Z\"}",
            "beforeSnapshot": {
                "timestamp": "2019-03-29T01:32:05.993Z"
            },
            "afterSnapshot": {
                "timestamp": "2019-03-29T01:54:24.42Z"
            }
        },
        {
            "changeId": "9dc352cb-b7c1-4198-9eda-e5e3ed66aec8",
            "beforeSnapshot": {
                "timestamp": "2019-03-28T10:30:19.68Z"
            },
            "afterSnapshot": {
                "timestamp": "2019-03-28T21:12:31.337Z"
            }
        }
    ]
}
```

Cada evento de cambio detectado para **resourceId** tiene un elemento **changeId** que es único para ese recurso. Si bien la cadena **changeId** a veces puede contener otras propiedades, solo se garantiza que sea única. El registro de cambios incluye las horas en las que se capturaron las instantáneas antes y después.
El evento de cambio se produjo en algún momento en esta ventana de tiempo.

## <a name="see-what-properties-changed"></a>Visualización de las propiedades que cambiaron

Con el objeto **changeId** del punto de conexión **resourceChanges**, el punto de conexión REST **resourceChangeDetails** luego se usar para obtener los detalles del evento de cambio.

El punto de conexión **resourceChangeDetails** requiere dos parámetros en el cuerpo de solicitud:

- **resourceId**: el recurso de Azure para el que se buscan cambios.
- **changeId**: el evento de cambio único para el objeto **resourceId** recopilado desde **resourceChanges**.

Ejemplo del cuerpo de solicitud:

```json
{
    "resourceId": "/subscriptions/{subscriptionId}/resourceGroups/MyResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
    "changeId": "{\"beforeId\":\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\",\"beforeTime\":'2019-05-09T00:00:00.000Z\",\"afterId\":\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\",\"beforeTime\":'2019-05-10T00:00:00.000Z\"}"
}
```

Con el cuerpo de solicitud anterior, el URI de la API de REST para **resourceChangeDetails** es:

```http
POST https://management.azure.com/providers/Microsoft.ResourceGraph/resourceChangeDetails?api-version=2018-09-01-preview
```

La respuesta será similar a la siguiente:

```json
{
    "changeId": "{\"beforeId\":\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\",\"beforeTime\":'2019-05-09T00:00:00.000Z\",\"afterId\":\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\",\"beforeTime\":'2019-05-10T00:00:00.000Z\"}",
    "beforeSnapshot": {
        "timestamp": "2019-03-29T01:32:05.993Z",
        "content": {
            "sku": {
                "name": "Standard_LRS",
                "tier": "Standard"
            },
            "kind": "Storage",
            "id": "/subscriptions/{subscriptionId}/resourceGroups/MyResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
            "name": "mystorageaccount",
            "type": "Microsoft.Storage/storageAccounts",
            "location": "westus",
            "tags": {},
            "properties": {
                "networkAcls": {
                    "bypass": "AzureServices",
                    "virtualNetworkRules": [],
                    "ipRules": [],
                    "defaultAction": "Allow"
                },
                "supportsHttpsTrafficOnly": false,
                "encryption": {
                    "services": {
                        "file": {
                            "enabled": true,
                            "lastEnabledTime": "2018-07-27T18:37:21.8333895Z"
                        },
                        "blob": {
                            "enabled": true,
                            "lastEnabledTime": "2018-07-27T18:37:21.8333895Z"
                        }
                    },
                    "keySource": "Microsoft.Storage"
                },
                "provisioningState": "Succeeded",
                "creationTime": "2018-07-27T18:37:21.7708872Z",
                "primaryEndpoints": {
                    "blob": "https://mystorageaccount.blob.core.windows.net/",
                    "queue": "https://mystorageaccount.queue.core.windows.net/",
                    "table": "https://mystorageaccount.table.core.windows.net/",
                    "file": "https://mystorageaccount.file.core.windows.net/"
                },
                "primaryLocation": "westus",
                "statusOfPrimary": "available"
            }
        }
    },
    "afterSnapshot": {
        "timestamp": "2019-03-29T01:54:24.42Z",
        "content": {
            "sku": {
                "name": "Standard_LRS",
                "tier": "Standard"
            },
            "kind": "Storage",
            "id": "/subscriptions/{subscriptionId}/resourceGroups/MyResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
            "name": "mystorageaccount",
            "type": "Microsoft.Storage/storageAccounts",
            "location": "westus",
            "tags": {},
            "properties": {
                "networkAcls": {
                    "bypass": "AzureServices",
                    "virtualNetworkRules": [],
                    "ipRules": [],
                    "defaultAction": "Allow"
                },
                "supportsHttpsTrafficOnly": true,
                "encryption": {
                    "services": {
                        "file": {
                            "enabled": true,
                            "lastEnabledTime": "2018-07-27T18:37:21.8333895Z"
                        },
                        "blob": {
                            "enabled": true,
                            "lastEnabledTime": "2018-07-27T18:37:21.8333895Z"
                        }
                    },
                    "keySource": "Microsoft.Storage"
                },
                "provisioningState": "Succeeded",
                "creationTime": "2018-07-27T18:37:21.7708872Z",
                "primaryEndpoints": {
                    "blob": "https://mystorageaccount.blob.core.windows.net/",
                    "queue": "https://mystorageaccount.queue.core.windows.net/",
                    "table": "https://mystorageaccount.table.core.windows.net/",
                    "file": "https://mystorageaccount.file.core.windows.net/"
                },
                "primaryLocation": "westus",
                "statusOfPrimary": "available"
            }
        }
    }
}
```

Los objetos **beforeSnapshot** y **afterSnapshot** proporcionan cada uno la hora en que se capturó la instantánea y las propiedades en ese momento. El cambio se produjo en algún momento entre estas instantáneas. Al examinar el ejemplo anterior, podemos ver que la propiedad que cambió fue **supportsHttpsTrafficOnly**.

Para comparar los resultados mediante programación, compare la parte **content** de cada instantánea para determinar la diferencia. Si compara toda la instantánea, el objeto **timestamp** siempre se muestra como una diferencia, a pesar de que sea esperado.

## <a name="next-steps"></a>Pasos siguientes

- Consulte el lenguaje en uso en[Consultas básicas](../samples/starter.md).
- Consulte los usos avanzados en [Consultas avanzadas](../samples/advanced.md).
- Más información sobre la [exploración de recursos](../concepts/explore-resources.md).