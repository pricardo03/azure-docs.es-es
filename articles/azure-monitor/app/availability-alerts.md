---
title: Configuración de alertas de disponibilidad con Azure Application Insights | Microsoft Docs
description: Configure pruebas web en Application Insights. Obtenga alertas si un sitio web deja de estar disponible o responde con lentitud.
services: application-insights
documentationcenter: ''
author: lgayhardt
manager: carmonm
ms.assetid: 46dc13b4-eb2e-4142-a21c-94a156f760ee
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 06/19/2019
ms.reviewer: sdash
ms.author: lagayhar
ms.openlocfilehash: 1d7527d6f52235c6b95ad2e336ea9f9ba85d6344
ms.sourcegitcommit: 8e1fb03a9c3ad0fc3fd4d6c111598aa74e0b9bd4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2019
ms.locfileid: "70114394"
---
# <a name="availability-alerts"></a>Alertas de disponibilidad

[Azure Application Insights](../../azure-monitor/app/app-insights-overview.md) envía solicitudes web a su aplicación a intervalos regulares desde puntos de todo el mundo. Puede enviar una alerta si la aplicación no responde o si responde de manera demasiada lenta.

## <a name="enable-alerts"></a>Habilitación de alertas

Las alertas ahora se habilitan automáticamente de forma predeterminada, pero para configurar por completo la alerta, primero debe crear inicialmente la prueba de disponibilidad.

![Experiencia de creación](./media/availability-alerts/create-test.png)

> [!NOTE]
>  Con las [nuevas alertas unificadas](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-unified-alerts), la gravedad de la regla de alertas y las preferencias de notificación con [grupos de acciones](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-action-groups) **se tienen que** configurar en la experiencia de alertas. Sin los pasos siguientes, solo recibirá las notificaciones del portal.

1. Después de guardar la prueba de disponibilidad, en la pestaña de detalles, haga clic en el signo de puntos suspensivos junto a la prueba que acaba de realizar. Haga clic en “Editar alerta”.

   ![Editar después de guardar](./media/availability-alerts/edit-alert.png)

2. Establezca el nivel de gravedad deseado, la descripción de la regla y lo más importante: el grupo de acciones con las preferencias de notificación que le gustaría usar para esta regla de alertas.

   ![Editar después de guardar](./media/availability-alerts/set-action-group.png)

### <a name="alert-on-x-out-of-y-locations-reporting-failures"></a>Alertas para X de las Y ubicaciones que notifican errores

La regla de alertas X de las Y ubicaciones se habilita de forma predeterminada en la [nueva experiencia de alertas unificadas](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-unified-alerts) cuando se crea una prueba de disponibilidad. Puede cancelar esta acción si selecciona la opción "clásica" o si deshabilita la regla de alertas.

> [!NOTE]
> Siga los pasos anteriores para configurar los grupos de acción para recibir notificaciones cuando se desencadene la alerta. Sin este paso, solo recibirá las notificaciones en el portal cuando se desencadene la regla.
>

### <a name="alert-on-availability-metrics"></a>Alertas sobre las métricas de disponibilidad

Mediante las [nuevas alertas unificadas](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-unified-alerts), también puede generar alertas sobre la disponibilidad total segmentada y las métricas de duración de la prueba:

1. Seleccione un recurso de Application Insights en la experiencia de métricas y seleccione una métrica de disponibilidad:

    ![Selección de métricas de disponibilidad](./media/availability-alerts/select-metric.png)

2. Al configurar la opción de alertas desde el menú se abrirá la nueva experiencia, donde podrá seleccionar pruebas o ubicaciones específicas para establecer la regla de alertas. También puede configurar los grupos de acciones para esta regla de alertas.

### <a name="alert-on-custom-analytics-queries"></a>Alertas sobre las consultas de análisis personalizadas

Mediante las [nuevas alertas unificadas](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-unified-alerts), puede generar alertas sobre las [consultas de registro personalizadas](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-log). Con las consultas personalizadas, puede enviar alertas sobre cualquier condición arbitraria que le ayude a obtener los indicadores de problemas de disponibilidad más fiables. Esta opción también la puede usar si envía resultados personalizados de disponibilidad mediante el SDK de TrackAvailability.

> [!Tip]
> Las métricas sobre datos de disponibilidad incluyen los resultados personalizados de disponibilidad que puede enviar mediante una llamada a nuestro SDK de TrackAvailability. Puede usar la compatibilidad con las alertas sobre métricas para generar alertas sobre resultados personalizados de disponibilidad.
>

## <a name="automate-alerts"></a>Automatización de alertas

Para automatizar este proceso con plantillas de Azure Resource Manager, consulte la documentación relativa a la [creación de una alerta de métrica con una plantilla de Resource Manager](../../azure-monitor/platform/alerts-metric-create-templates.md#template-for-a-availability-test-along-with-availability-test-alert).

## <a name="troubleshooting"></a>solución de problemas

[Artículo de solución de problemas](troubleshoot-availability.md) dedicado.

## <a name="next-steps"></a>Pasos siguientes

* [Pruebas web de varios pasos](availability-multistep.md)
* [Pruebas web de ping de URL](monitor-web-app-availability.md)
