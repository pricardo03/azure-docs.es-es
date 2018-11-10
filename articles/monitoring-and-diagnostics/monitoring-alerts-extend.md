---
title: 'Extensión (copia) de alertas de Log Analytics en Alertas de Azure: información general'
description: Información general sobre el proceso de copia de alertas desde Log Analytics en el portal de OMS a Alertas de Azure, con detalles que abordan las inquietudes más frecuentes de los usuarios.
author: msvijayn
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 10/01/2018
ms.author: vinagara
ms.component: alerts
ms.openlocfilehash: 60ecc2bc1d700c223d4a52b05e3b9362241f3f82
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/31/2018
ms.locfileid: "50414625"
---
# <a name="extend-log-analytics-alerts-to-azure-alerts"></a>Extensión de alertas de Log Analytics en Alertas de Azure
Hasta hace poco, Azure Log Analytics incluía su propia funcionalidad de alerta, que podía informarle proactivamente de las condiciones en función de datos de Log Analytics. Las reglas de alertas se administraban en el portal de Microsoft Operations Management Suite. La nueva experiencia de alertas integra ahora las alertas en distintos servicios de Microsoft Azure. Está disponible como **Alertas** en Azure Monitor en Azure Portal e incluye las alertas de registros de actividad, las métricas y los registros tanto de Log Analytics como de Azure Application Insights. 

## <a name="benefits-of-extending-your-alerts"></a>Ventajas de extender las alertas
Hay varias ventajas de crear y administrar alertas en Azure Portal, tales como:

- A diferencia del portal de Operations Management Suite portal, donde solo se podían crear y visualizar 250 alertas, en Alertas de Azure no existe esta limitación.
- En Alertas de Azure, se pueden administrar, mostrar y visualizar todos los tipos de alertas. Anteriormente, esto solo se podía hacer con las alertas de Log Analytics.
- Puede limitar el acceso de los usuarios solo a supervisión y alertas, mediante el [rol de Azure Monitor](monitoring-roles-permissions-security.md).
- En Alertas de Azure, puede usar [grupos de acciones](monitoring-action-groups.md). Esto le permite tener más de una acción por alerta. Puede enviar un SMS, enviar una llamada de voz, invocar un runbook de Azure Automation, invocar un webhook y configurar un conector de administración de servicios de TI (ITSM). 

## <a name="process-of-extending-your-alerts"></a>Proceso de extensión de las alertas
El proceso de trasladar las alertas de Log Analytics a Alertas de Azure no implica cambiar de ningún modo la configuración, la consulta ni la definición de las alertas. El único cambio necesario es que, en Azure, todas las acciones se realizan mediante un grupo de acciones. Si los grupos de acciones ya están asociados a la alerta, se incluyen al ampliar a Azure.

> [!NOTE]
> A partir del 14 de mayo de 2018, Microsoft extenderá automáticamente las alertas creadas en las instancias de la nube pública de Log Analytics a Alertas de Azure en una serie periódica hasta que se completen. Si tiene problemas para crear [ grupos de acciones](monitoring-action-groups.md), siga [estos pasos de corrección](monitoring-alerts-extend-tool.md#troubleshooting) para crear grupos de acciones automáticamente. Puede seguir estos pasos hasta el 5 de julio de 2018. *No es aplicable para usuarios de Azure Government y la nube soberana de Log Analytics*. 
> 

Cuando programa alertas en un área de trabajo de Log Analytics para que se extiendan a Azure, éstas continúan funcionando y no comprometen en modo alguno la configuración. Es posible que, una vez programadas, las alertas no estén disponibles para ser modificadas temporalmente, pero se pueden seguir creando nuevas Alertas de Azure durante este tiempo. Si intenta editar o crear alertas desde el portal de Operations Management Suite, tiene la posibilidad de seguir creándolas desde el área de trabajo de Log Analytics. También puede crearlas desde Alertas de Azure en Azure Portal.

 ![Captura de pantalla de la opción para crear alertas en Log Analytics o en Alertas de Azure](media/monitoring-alerts-extend/ScheduledDirection.png)

> [!NOTE]
> La extensión de alertas de Log Analytics a Azure no ocasionará cargos en su cuenta. El uso de Alertas de Azure con alertas de Log Analytics basadas en consultas no se facturará, siempre que el uso cumpla con los límites y las condiciones que se estipulan en la [directiva de precios de Azure Monitor](https://azure.microsoft.com/pricing/details/monitor/).  


### <a name="how-to-extend-your-alerts-voluntarily"></a>Cómo extender las alertas voluntariamente
Para extender las alertas a Alertas de Azure, puede usar a un asistente disponible en el portal de Operations Management Suite o puede hacerlo mediante programación a través de una API. Para más información, vea el artículo sobre [cómo extender las alertas a Azure mediante la API y el portal de Operations Management Suite](monitoring-alerts-extend-tool.md).

## <a name="experience-after-extending-your-alerts"></a>Experiencia después de extender las alertas
Cuando las alertas se extienden a Alertas de Azure, siguen estando disponibles en el portal de Operations Management Suite para su administración de la misma manera que antes.

![Captura de pantalla del portal de Operations Management Suite, con presentación de las alertas](media/monitoring-alerts-extend/PostExtendList.png)

Cuando intente modificar una alerta existente o crear una alerta en el portal de Operations Management Suite, se le redirigirá automáticamente a Alertas de Azure.  

> [!NOTE]
> Asegúrese de que los permisos que asigne a las personas que han de agregar o editar alertas se hayan asignado correctamente en Azure. Para entender qué permisos tiene que conceder, vea [permisos de uso de Azure Monitor y Alertas de Azure](monitoring-roles-permissions-security.md).  
> 

Aún puede crear alertas de la [API de Log Analytics](../log-analytics/log-analytics-api-alerts.md) y [plantilla de recursos de Log Analytics](../monitoring/monitoring-solutions-resources-searches-alerts.md). Debe incluir grupos de acciones cuando lo haga.

## <a name="next-steps"></a>Pasos siguientes

* Conozca las herramientas para [iniciar la extensión de alertas de Log Analytics a Azure](monitoring-alerts-extend-tool.md).
* Obtenga más información sobre la experiencia de [Alertas de Azure](monitoring-overview-unified-alerts.md).
* Aprenda a crear [alertas de registro en Alertas de Azure](monitor-alerts-unified-log.md).
