---
title: Información general de los registros de Azure Firewall
description: Este artículo es una introducción a los registros de diagnóstico de Azure Firewall.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 9/24/2018
ms.author: victorh
ms.openlocfilehash: c129c394f3d694b832722287027c1f9e58028a33
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61065859"
---
# <a name="azure-firewall-logs"></a>Registros de Azure Firewall

Puede supervisar Azure Firewall mediante los registros del firewall. También puede usar los registros de actividad para auditar las operaciones de los recursos de Azure Firewall.

Se puede acceder a algunos de estos registros mediante el portal. Se pueden enviar registros a los [registros de Azure Monitor](../azure-monitor/insights/azure-networking-analytics.md), a Storage y a Event Hubs, y se pueden analizar en los registros de Azure Monitor o mediante otras herramientas como Excel y Power BI.

## <a name="diagnostic-logs"></a>Registros de diagnóstico

 Los siguientes registros de diagnóstico están disponibles para Azure Firewall:

* **Registro de regla de aplicación**

   El registro de la regla de aplicación se guarda en una cuenta de almacenamiento, transmitir a Event hubs o enviar a los registros de Azure Monitor solo si lo habilitó para cada servidor de seguridad de Azure. Cada nueva conexión que coincida con una de las reglas de la aplicación configuradas dará como resultado un registro de la conexión aceptada o rechazada. Los datos se registran en formato JSON, tal y como se muestra en el ejemplo siguiente:

   ```
   Category: application rule logs.
   Time: log timestamp.
   Properties: currently contains the full message. 
   note: this field will be parsed to specific fields in the future, while maintaining backward compatibility with the existing properties field.
   ```

   ```json
   {
    "category": "AzureFirewallApplicationRule",
    "time": "2018-04-16T23:45:04.8295030Z",
    "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/AZUREFIREWALLS/{resourceName}",
    "operationName": "AzureFirewallApplicationRuleLog",
    "properties": {
        "msg": "HTTPS request from 10.1.0.5:55640 to mydestination.com:443. Action: Allow. Rule Collection: collection1000. Rule: rule1002"
    }
   }
   ```

* **Registro de regla de red**

   El registro de la regla de red se guarda en una cuenta de almacenamiento, transmitir a Event hubs o enviar a los registros de Azure Monitor solo si lo habilitó para cada servidor de seguridad de Azure. Cada nueva conexión que coincida con una de las reglas de red configuradas dará como resultado un registro de la conexión aceptada o rechazada. Los datos se registran en formato JSON, tal y como se muestra en el ejemplo siguiente:

   ```
   Category: network rule logs.
   Time: log timestamp.
   Properties: currently contains the full message. 
   note: this field will be parsed to specific fields in the future, while maintaining backward compatibility with the existing properties field.
   ```

   ```json
  {
    "category": "AzureFirewallNetworkRule",
    "time": "2018-06-14T23:44:11.0590400Z",
    "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/AZUREFIREWALLS/{resourceName}",
    "operationName": "AzureFirewallNetworkRuleLog",
    "properties": {
        "msg": "TCP request from 111.35.136.173:12518 to 13.78.143.217:2323. Action: Deny"
    }
   }

   ```

Tiene tres opciones para almacenar los archivos de registro:

* **Cuenta de almacenamiento**: cuentas que resultan especialmente útiles para registros cuando estos se almacenan durante mucho tiempo y se revisan cuando es necesario.
* **Centros de eventos**: es una buena opción para la integración con otras herramientas de administración de eventos e información de seguridad (SIEM) para obtener alertas sobre los recursos.
* **Registros de Azure Monitor**: se usan para la supervisión general en tiempo real de la aplicación o para examinar las tendencias.

## <a name="activity-logs"></a>Registros de actividad

   Las entradas del registro de actividades se recopilan de forma predeterminada y se pueden ver en Azure Portal.

   Puede usar el [registro de actividades de Azure](../azure-resource-manager/resource-group-audit.md) (anteriormente conocido como registros operativos y registros de auditoría) para ver todas las operaciones enviadas a sus suscripciones de Azure.


## <a name="next-steps"></a>Pasos siguientes

Para obtener información sobre cómo supervisar métricas y registros de Firewall de Azure, consulte [Tutorial: Supervisión de los registros de Azure Firewall](tutorial-diagnostics.md).