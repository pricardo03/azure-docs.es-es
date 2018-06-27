---
title: Uso de Azure Portal para crear alertas clásicas de los servicios de Azure | Microsoft Docs
description: Desencadene correos electrónicos o notificaciones o llamadas a direcciones URL de sitios web (webhooks) o a la automatización cuando se cumplen las condiciones especificadas.
author: rboucher
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/23/2016
ms.author: robb
ms.component: alerts
ms.openlocfilehash: d0e5512eb3f963898ded6d7155f8b75cfb6ef911
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36287436"
---
# <a name="use-the-azure-portal-to-create-classic-metric-alerts-in-azure-monitor-for-azure-services"></a>Uso de Azure Portal para crear alertas de métricas clásicas en Azure Monitor para servicios de Azure 

> [!div class="op_single_selector"]
> * [Portal](insights-alerts-portal.md)
> * [PowerShell](insights-alerts-powershell.md)
> * [CLI](insights-alerts-command-line-interface.md)
>

> [!NOTE]
> En este artículo se describe cómo crear alertas de métrica clásicas más antiguas. Azure Monitor admite ahora [nuevas alertas de métricas más](monitoring-near-real-time-metric-alerts.md). 


En este artículo se muestra cómo configurar alertas de métricas clásicas de Azure con Azure Portal. 

Puede recibir una alerta basada en métricas de los servicios de Azure, o puede recibir alertas para eventos que se producen en Azure.

* **Valores de métrica**: la alerta se desencadena cuando el valor de una métrica específica cruza un umbral asignado en cualquier dirección. Es decir, se desencadena tanto la primera vez que se cumple la condición como después, cuando esa condición ya deja de cumplirse.    

* **Eventos de registro de actividades**: una alerta puede desencadenarse con *cada* evento o cuando se producen ciertos eventos concretos. Más información sobre las [alertas del registro de actividad](monitoring-activity-log-alerts.md).

Puede configurar una alerta de métrica clásica para hacer lo siguiente cuando se desencadena:

* Enviar notificaciones de correo electrónico al administrador y los coadministradores del servicio.
* Enviar un correo electrónico a direcciones de correo electrónico adicionales que especifique.
* Llamar a un webhook.
* Iniciar la ejecución de un runbook de Azure (solo desde Azure Portal).

Puede obtener información sobre las reglas de alerta de métricas clásicas y configurarlas en las siguientes ubicaciones: 

* [Azure Portal](insights-alerts-portal.md)
* [PowerShell](insights-alerts-powershell.md)
* [CLI de Azure](insights-alerts-command-line-interface.md)
* [API de REST de Azure Monitor](https://msdn.microsoft.com/library/azure/dn931945.aspx)

## <a name="create-an-alert-rule-on-a-metric-with-the-azure-portal"></a>Creación de una regla de alerta de una métrica con Azure Portal
1. En el [portal](https://portal.azure.com/), busque el recurso que desea supervisar y, a continuación, selecciónelo.

2. En la sección **SUPERVISIÓN**, seleccione **Alertas (clásico)**. El texto y el icono pueden variar ligeramente en los distintos recursos. Si no encuentra **Alertas (clásico)** aquí, puede que lo encuentre en **Alertas** o en **Reglas de alertas**.

    ![Supervisión](./media/insights-alerts-portal/AlertRulesButton.png)

3. Seleccione el comando **Agregar una alerta de métrica (clásica)** y rellene los campos.

    ![Agregar alerta](./media/insights-alerts-portal/AddAlertOnlyParamsPage.png)

4. Asigne un **nombre** a la regla de alerta. Después, elija una **descripción**, que también aparecerá en los correos electrónicos de notificación.

5. Seleccione la **métrica** que desea supervisar. Posteriormente, elija una **condición** y el valor de **umbral** de la métrica. También debe elegir el **período** de la regla de métrica que se debe cumplir antes de que se desencadene la alerta. Por ejemplo, si usa el período "En los últimos 5 minutos" y la alerta busca una CPU por encima del 80 %, la alerta se desencadena cuando la CPU ha estado por encima del 80 % durante 5 minutos de manera uniforme. Una vez que se desencadena por primera vez, se vuelve a desencadenar cuando la CPU se mantiene por debajo del 80 % durante 5 minutos. La medición de la métrica de CPU se produce cada minuto.

6. Seleccione **Enviar correo electrónico a propietarios...** si desea que se envíe una notificación por correo electrónico a los administradores y coadministradores cuando se active la alerta.

7. Si quiere enviar notificaciones a otras direcciones de correo electrónico cuando se active la alerta, agréguelas en el campo **Correos electrónicos adicionales del administrador**. Separe las direcciones de correo electrónico con punto y coma, en el siguiente formato: *email@contoso.com; email2@contoso.com*

8. Escriba un identificador URI válido en el campo **Webhook** si desea llamarlo cuando se active la alerta.

9. Si usa Azure Automation, puede seleccionar un runbook para que se ejecute cuando se active la alerta.

10. Seleccione **Aceptar** para crear la alerta.   

En cuestión de minutos, se activa la alerta y se desencadena tal como se describió anteriormente.

## <a name="manage-your-alerts"></a>Administración de alertas
Una vez que haya creado una alerta, puede seleccionarla y realizar alguna de las acciones siguientes:

* Ver un gráfico que muestre el umbral de las métricas y los valores reales del día anterior.
* Editar la alerta o eliminarla.
* **Deshabilitar** la alerta, si desea dejar de recibir notificaciones de esa alerta de manera temporal, o **habilitarla** si desea reanudar sus notificaciones.

## <a name="next-steps"></a>Pasos siguientes
* [Obtenga información general sobre la supervisión de Azure](monitoring-overview.md), incluidos los tipos de información que puede recopilar y supervisar.
* Obtenga más información sobre las [alertas de métricas más recientes](monitoring-near-real-time-metric-alerts.md).
* Obtenga más información sobre cómo [configurar webhooks en las alertas](insights-webhooks-alerts.md).
* Obtenga más información sobre la [configuración de alertas sobre los eventos de registro de actividad](monitoring-activity-log-alerts.md).
* Obtenga más información sobre los [runbooks de Azure Automation](../automation/automation-starting-a-runbook.md).
* Obtenga [información general sobre los registros de diagnóstico](monitoring-overview-of-diagnostic-logs.md) para recopilar métricas detalladas de alta frecuencia sobre el servicio.
* Obtenga [información general sobre la colección de métricas](insights-how-to-customize-monitoring.md) para garantizar que el servicio está disponible y que responde adecuadamente.
