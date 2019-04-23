---
title: Supervisión del uso y costos estimados en Azure Monitor
description: Introducción al proceso de uso de la página Uso y costos estimados de Azure Monitor
author: dalekoetke
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 04/18/2019
ms.author: mbullwin
ms.reviewer: Dale.Koetke
ms.subservice: ''
ms.openlocfilehash: 7117e7287f601b306893cb02dc5d7599d7c6224d
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/22/2019
ms.locfileid: "60007702"
---
# <a name="monitoring-usage-and-estimated-costs-in-azure-monitor"></a>Supervisión del uso y costos estimados en Azure Monitor

> [!NOTE]
> En este artículo se describe cómo ver el uso y los costos estimados a través de varias características de supervisión de Azure para los distintos modelos de precios.  Consulte los artículos siguientes para obtener información relacionada.
> - En [Administración de los costos mediante el control del volumen de datos y la retención en Log Analytics](manage-cost-storage.md) se describe cómo controlar los costos al cambiar el período de retención de datos.
> - En [Análisis del uso de datos en Log Analytics](../../azure-monitor/platform/data-usage.md) se describe cómo analizar y alertar sobre el uso de datos.
> - En [Administración de precios y volúmenes de datos de Application Insights](../../azure-monitor/app/pricing.md) se describe cómo analizar el uso de datos en Application Insights.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

En el centro Monitor de Azure Portal, la página **Uso y costos estimados** explica el uso de características de supervisión principales como [alertas, métricas, notificaciones](https://azure.microsoft.com/pricing/details/monitor/), [Azure Log Analytics](https://azure.microsoft.com/pricing/details/log-analytics/) y [Azure Application Insights](https://azure.microsoft.com/pricing/details/application-insights/). Para los clientes en los planes de precios disponibles antes de abril de 2018, esto también incluye el uso de Log Analytics adquirido a través de la oferta Insights y Analytics.

En esta página, los usuarios pueden ver el uso de sus recursos durante los últimos 31 días, agregados por suscripción. La obtención de detalles muestra las tendencias de uso durante el período de 31 días. Es necesario que una gran cantidad de datos se reúna para este cálculo, por lo que agradecemos su paciencia mientras se carga la página.

En este ejemplo se muestra la supervisión del uso y una estimación de los costos resultantes:

![Captura de pantalla de la página Uso y costos estimados del portal](./media/usage-estimated-costs/001.png)

Seleccione el vínculo de la columna de uso mensual para abrir un gráfico que muestra las tendencias de uso en el último período de 31 días:

![Captura de la pantalla del gráfico de barras Incluido por nodo](./media/usage-estimated-costs/002.png)

Este es otro resumen similar del uso y los costos. En este ejemplo se muestra una suscripción en el nuevo modelo de precios basado en el consumo de abril de 2018. Tenga en cuenta la falta de facturación basada en nodos. La ingesta de datos y la retención para Log Analytics y Application Insights ahora se notifican en un nuevo medidor común.

![Captura de pantalla de la página Uso y costos estimados del portal: precios de abril de 2018](./media/usage-estimated-costs/003.png)

## <a name="new-pricing-model"></a>Nuevo modelo de precios

En abril de 2018 se lanzó un [nuevo modelo de precios de supervisión](https://azure.microsoft.com/blog/introducing-a-new-way-to-purchase-azure-monitoring-services/).  Con características sencillas y en la nube y precios basados en el consumo. Solo paga por lo que usa, sin compromisos basados en nodos. Hay detalles del nuevo modelo de precios disponibles para [alertas, métricas y notificaciones](https://azure.microsoft.com/pricing/details/monitor/), [Log Analytics](https://azure.microsoft.com/pricing/details/log-analytics/) y [Application Insights](https://azure.microsoft.com/pricing/details/application-insights/). 

Para los clientes que se incorporan a Log Analytics o Application Insights después del 2 de abril de 2018, el nuevo modelo de precios es la única opción. Para los clientes que ya utilizan estos servicios, el traslado al nuevo modelo de precios es opcional.

## <a name="assessing-the-impact-of-the-new-pricing-model"></a>Evaluación del impacto del nuevo modelo de precios
El nuevo modelo de precios tendrá efectos diferentes en cada cliente en función de sus patrones de uso de la supervisión. Para los clientes que usaban Log Analytics o Application Insights antes del 2 de abril de 2018, la página **Uso y costos estimados** en Azure Monitor estima cualquier cambio en los costos si se trasladan al nuevo modelo de precios. Proporciona la forma de trasladar una suscripción al nuevo modelo. Para la mayoría de los clientes, el nuevo modelo de precios será ventajoso. Para clientes con patrones de uso de datos especialmente elevados o en regiones de mayor costo, podría no ser el caso.

Para ver una estimación de los costos de las suscripciones que eligió en la página **Uso y costos estimados**, seleccione el banner azul situado cerca de la parte superior de la página. Es mejor realizar este proceso con una suscripción cada vez, ya que es el nivel en el que se puede adoptar el nuevo modelo de precios.

![Captura de pantalla de la página Uso y costo estimado de Azure Monitor en el nuevo modelo de precios](./media/usage-estimated-costs/004.png)

En la nueva página se muestra una versión similar de la página anterior con un banner verde:

![Captura de pantalla de la página Uso y costo estimado de Azure Monitor en el modelo de precios actual](./media/usage-estimated-costs/005.png)

En la página también se muestra un conjunto diferente de medidores que se corresponden con el nuevo modelo de precios. Esta lista es un ejemplo:

- Insights y Analytics\Uso por encima del límite por nodo
- Insights y Analytics\Incluido por nodo
- Application Insights\Datos por encima del límite básico
- Application Insights\Datos incluidos

El nuevo modelo de precios no tiene asignaciones de datos incluidas basadas en nodos. Por lo tanto, estos medidores de ingesta de datos se combinan en un nuevo medidor de ingesta de datos común llamado **Shared Services\Data Ingestion**. 

Hay otro cambio en los datos ingeridos en Log Analytics o Application Insights en regiones con costos más elevados. Los datos para estas regiones con costos elevados se mostrarán con los nuevos medidores regionales. Un ejemplo es **Ingesta de datos (Centro-oeste de EE. UU.)**.

> [!NOTE]
> Los costos estimados según la suscripción no gestionan los derechos por nodo del nivel de cuenta de la suscripción de Operations Management Suite (OMS). Consulte con su representante de cuentas para obtener una explicación más detallada del nuevo modelo de precios en este caso.

## <a name="new-pricing-model-and-operations-management-suite-subscription-entitlements"></a>Nuevo modelo de precios y derechos de suscripción de Operations Management Suite

Los clientes que compraron Microsoft Operations Management Suite E1 y E2 son aptos para los derechos de ingesta de datos por nodo para [Log Analytics](https://www.microsoft.com/cloud-platform/operations-management-suite) y [Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-pricing). Para recibir estos derechos para las áreas de trabajo de Log Analytics o los recursos de Application Insights en una suscripción determinada: 

- El modelo de precios de la suscripción debe ser igual que el modelo anterior a abril de 2018.
- Las áreas de trabajo de Log Analytics deben usar el plan de tarifa "por nodo (OMS)".
- Los recursos de Application Insights deben utilizar el plan de tarifa "Enterprise".

En función del número de nodos del conjunto de aplicaciones que adquiriera la organización, puede ser de utilidad mover alguna suscripción al nuevo modelo de precios, pero le recomendamos que antes lo piense detenidamente. En general, es aconsejable quedarse en el modelo anterior a abril de 2018, tal como se describió anteriormente.

> [!WARNING]
> Si la organización compró Microsoft Operations Management Suite E1 y E2, es mejor mantener las suscripciones según el modelo de precios anterior a abril de 2018. 
>

## <a name="changes-when-youre-moving-to-the-new-pricing-model"></a>Cambios en el traslado al nuevo modelo de precios

El nuevo modelo de precios simplifica las opciones de precios que ofrecen Log Analytics y Application Insights en un solo nivel (o plan). Si lleva una suscripción al nuevo modelo de precios, podrá realizar lo siguiente:

- Cambiar el plan de tarifa de cada instancia de Log Analytics a un nuevo nivel por GB (denominado "pergb2018" en Azure Resource Manager).
- Cambiar cualquier recurso de Application Insights del plan Enterprise al plan Básico.

La estimación de costos muestra los efectos de estos cambios.

> [!WARNING]
> Tenga en cuenta lo siguiente si usa Azure Resource Manager o PowerShell para implementar [Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-template-workspace-configuration) o [Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-powershell) en una suscripción que haya trasladado al nuevo modelo de precios. Si especifica un plan de tarifa que no sea "pergb2018" para Log Analytics, o "Básico" para Application Insights, no se producirá un error en la implementación aunque haya especificado un plan de tarifa no válido; es más, la implementación se realizará correctamente, **pero usará solo el plan de tarifa válido** (esto no corresponde al plan Gratuito de Log Analytics, donde se genera un mensaje de plan de tarifa no válido).
>

## <a name="moving-to-the-new-pricing-model"></a>Traslado al nuevo modelo de precios

Si ha decidido adoptar el nuevo modelo de precios para una suscripción determinada, vaya a cada recurso de Application Insights, abra el **uso y costos estimados** y asegúrese de que se encuentra en el plan de tarifa básico y vaya a Log Analytics área de trabajo, abra el **tarifa** página y cambie a la **por GB (2018)** plan de tarifa. 

> [!NOTE]
> El requisito de que todos los recursos de Application Insights y áreas de trabajo de Log Analytics en una suscripción determinada adoptarán el modelo de precios más reciente se ha quitado, lo que permite mayor flexibilidad y facilita la configuración. 

## <a name="automate-moving-to-the-new-pricing-model"></a>Traslado automático al nuevo modelo de precios

Como se mencionó anteriormente, el ya no es un requisito para mover todos los recursos de supervisión en una suscripción al nuevo modelo de precios al mismo tiempo y por lo tanto, el ``migratetonewpricingmodel`` acción ya no tendrá ningún efecto. Ahora puede mover recursos de Application Insights y áreas de trabajo de Log Analytics por separado en los planes de tarifa más recientes.  

Automatización de este cambio se documenta para Application Insights con [conjunto AzureRmApplicationInsightsPricingPlan](https://docs.microsoft.com/powershell/module/azurerm.applicationinsights/set-azurermapplicationinsightspricingplan) con ``-PricingPlan "Basic"`` y el uso de Log Analytics [Set-AzureRmOperationalInsightsWorkspace](https://docs.microsoft.com/powershell/module/AzureRM.OperationalInsights/Set-AzureRmOperationalInsightsWorkspace) con ``-sku "PerGB2018"``. 

