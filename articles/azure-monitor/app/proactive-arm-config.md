---
title: Configuración de los valores de reglas de detección inteligente de Azure Application Insights con plantillas de Azure Resource Manager | Microsoft Docs
description: Automatización de la administración y configuración de reglas de detección inteligente de Azure Application Insights con plantillas de Azure Resource Manager
services: application-insights
documentationcenter: ''
author: harelbr
manager: carmonm
ms.assetid: ea2a28ed-4cd9-4006-bd5a-d4c76f4ec20b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 02/07/2019
ms.reviewer: mbullwin
ms.author: harelbr
ms.openlocfilehash: 3ab50c92543615488d9ced599df433bf7e1e4061
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "61461568"
---
# <a name="manage-application-insights-smart-detection-rules-using-azure-resource-manager-templates"></a>Administración de reglas de detección inteligente de Application Insights con plantillas de Azure Resource Manager

Las reglas de detección inteligente de Application Insights se pueden administrar y configurar con [plantillas de Azure Resource Manager](../../azure-resource-manager/resource-group-authoring-templates.md).
Este método puede usarse al implementar nuevos recursos de Application Insights con la automatización de Azure Resource Manager o para modificar la configuración de los recursos existentes.

## <a name="smart-detection-rule-configuration"></a>Configuración de una regla de detección inteligente

Puede configurar los siguientes valores en una regla de detección inteligente:
- Si la regla se habilita (el valor predeterminado es **true**).
- Si deben enviarse correos electrónicos a los lectores, colaboradores y propietarios de la suscripción cuando se encuentra una detección (el valor predeterminado es **true**).
- Los destinatarios de correo electrónico adicionales que deben recibir una notificación cuando se encuentra una detección.
- * La configuración de correo electrónico no está disponible para las reglas de detección inteligente marcadas como _Versión preliminar_.

Para permitir la configuración de los valores de regla a través de Azure Resource Manager, la configuración de reglas de detección inteligente ahora está disponible como recurso interno en el recurso de Application Insights denominado **ProactiveDetectionConfigs**.
Para obtener la máxima flexibilidad, cada regla de detección inteligente puede configurarse con valores de notificación únicos.

## <a name="examples"></a>Ejemplos

A continuación se muestran algunos ejemplos que muestran cómo configurar los valores de reglas de detección inteligente mediante plantillas de Azure Resource Manager.
Todos los ejemplos hacen referencia a un recurso de Application Insights denominado _"myApplication"_ y a la "regla de detección inteligente de duración de la dependencia prolongada" que internamente se denomina _"longdependencyduration"_ .
Asegúrese de reemplazar el nombre de recurso de Application Insights y de especificar el nombre interno de la regla de detección inteligente pertinente. En la tabla siguiente, busque una lista de los nombres internos de Azure Resource Manager correspondientes a cada regla de detección inteligente.

### <a name="disable-a-smart-detection-rule"></a>Deshabilitación de una regla de detección inteligente

```json
{
      "apiVersion": "2018-05-01-preview",
      "name": "myApplication",
      "type": "Microsoft.Insights/components",
      "location": "[resourceGroup().location]",
      "properties": {
        "ApplicationId": "myApplication"
      },
      "resources": [
        {
          "apiVersion": "2018-05-01-preview",
          "name": "longdependencyduration",
          "type": "ProactiveDetectionConfigs",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[resourceId('Microsoft.Insights/components', 'myApplication')]"
          ],
          "properties": {
            "name": "longdependencyduration",
            "sendEmailsToSubscriptionOwners": true,
            "customEmails": [],
            "enabled": false
          }
        }
      ]
    }
```

### <a name="disable-sending-email-notifications-for-a-smart-detection-rule"></a>Deshabilitación del envío de notificaciones por correo electrónico para una regla de detección inteligente

```json
{
      "apiVersion": "2018-05-01-preview",
      "name": "myApplication",
      "type": "Microsoft.Insights/components",
      "location": "[resourceGroup().location]",
      "properties": {
        "ApplicationId": "myApplication"
      },
      "resources": [
        {
          "apiVersion": "2018-05-01-preview",
          "name": "longdependencyduration",
          "type": "ProactiveDetectionConfigs",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[resourceId('Microsoft.Insights/components', 'myApplication')]"
          ],
          "properties": {
            "name": "longdependencyduration",
            "sendEmailsToSubscriptionOwners": false,
            "customEmails": [],
            "enabled": true
          }
        }
      ]
    }
```

### <a name="add-additional-email-recipients-for-a-smart-detection-rule"></a>Adición de otros destinatarios de correo electrónico para una regla de detección inteligente

```json
{
      "apiVersion": "2018-05-01-preview",
      "name": "myApplication",
      "type": "Microsoft.Insights/components",
      "location": "[resourceGroup().location]",
      "properties": {
        "ApplicationId": "myApplication"
      },
      "resources": [
        {
          "apiVersion": "2018-05-01-preview",
          "name": "longdependencyduration",
          "type": "ProactiveDetectionConfigs",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[resourceId('Microsoft.Insights/components', 'myApplication')]"
          ],
          "properties": {
            "name": "longdependencyduration",
            "sendEmailsToSubscriptionOwners": true,
            "customEmails": ['alice@contoso.com', 'bob@contoso.com'],
            "enabled": true
          }
        }
      ]
    }

```

## <a name="smart-detection-rule-names"></a>Nombres de regla de detección inteligente

Seguidamente se muestra una tabla de nombres de reglas de detección inteligente tal y como aparecen en el portal, así como sus nombres internos, que deben utilizarse en la plantilla de Azure Resource Manager.

> [!NOTE]
> Las reglas de detección inteligente marcadas como de versión preliminar no admiten notificaciones por correo electrónico. Por lo tanto, solo se puede establecer la propiedad habilitada para estas reglas. 

| Nombre de regla de Azure Portal | Nombre interno
|:---|:---|
| Carga lenta de página | slowpageloadtime |
| Lentitud en el tiempo de respuesta del servidor | slowserverresponsetime |
| Duración de la dependencia prolongada | longdependencyduration |
| Degradación del tiempo de respuesta del servidor | degradationinserverresponsetime |
| Reducción de la duración de la dependencia | degradationindependencyduration |
| Degradación en la relación de gravedad de seguimiento (versión preliminar) | extension_traceseveritydetector |
| Aumento anormal del volumen de excepciones (versión preliminar) | extension_exceptionchangeextension |
| Detección de una posible fuga de memoria (versión preliminar) | extension_memoryleakextension |
| Detección de un posible problema de seguridad (versión preliminar) | extension_securityextensionspackage |
| Detección de un posible problema de uso de recursos (versión preliminar) | extension_resourceutilizationextensionspackage |

## <a name="who-receives-the-classic-alert-notifications"></a>¿Quién recibe las notificaciones de alerta (clásicas)?

Esta sección solo se aplica a las alertas clásicas de detección inteligente y le ayudará a optimizar las notificaciones de alerta para asegurarse de que solo reciban las notificaciones los destinatarios que elija. Para más información sobre la diferencia entre [alertas clásicas](../platform/alerts-classic.overview.md) y la nueva experiencia de alertas, consulte el [artículo de información general de alertas](../platform/alerts-overview.md). En este momento, las alertas de detección inteligente admiten únicamente la experiencia de alertas clásicas. La única excepción son [las alertas de detección inteligente en Azure Cloud Services](./proactive-cloud-services.md). Use los [grupos de acciones](../platform/action-groups.md) para controlar las notificaciones para alertas de detección inteligente en Azure Cloud Services.

* Se recomienda el uso de destinatarios específicos para las notificaciones de alerta clásicas o de detección inteligente.

* Si está habilitada la opción de casilla de verificación **masiva o grupo**, las alertas de detección inteligente se envían a los usuarios con roles de propietario, colaborador o lector de la suscripción. De hecho, _todos_ los usuarios con acceso a la suscripción del recurso de Application Insights están dentro del ámbito y recibirán notificaciones. 

> [!NOTE]
> Si actualmente usa la opción de casilla de verificación **masiva o grupo** y la deshabilita, no podrá revertir el cambio.

## <a name="next-steps"></a>Pasos siguientes

Más información sobre la detección automática:

- [Anomalías de error](../../azure-monitor/app/proactive-failure-diagnostics.md)
- [Fugas de memoria](../../azure-monitor/app/proactive-potential-memory-leak.md)
- [Anomalías de rendimiento](../../azure-monitor/app/proactive-performance-diagnostics.md)