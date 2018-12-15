---
title: Supervisión del uso y costos estimados en Azure Monitor
description: Introducción al proceso de uso de la página Uso y costos estimados de Azure Monitor
author: dalekoetke
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 08/11/2018
ms.author: mbullwin
ms.reviewer: Dale.Koetke
ms.component: ''
ms.openlocfilehash: 072201dd552f5d68a9b6621b2ca7c83acd76a1f5
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/15/2018
ms.locfileid: "53439572"
---
# <a name="monitoring-usage-and-estimated-costs"></a>Supervisión del uso y costos estimados

> [!NOTE]
> En este artículo se describe cómo ver el uso y los costos estimados a través de varias características de supervisión de Azure para los distintos modelos de precios.  Consulte los artículos siguientes para obtener información relacionada.
> - En [Administración de los costos mediante el control del volumen de datos y la retención en Log Analytics](../../azure-monitor/platform/manage-cost-storage.md) se describe cómo controlar los costos al cambiar el período de retención de datos.
> - En [Análisis del uso de datos en Log Analytics](../../azure-monitor/platform/data-usage.md) se describe cómo analizar y alertar sobre el uso de datos.
> - En [Administración de precios y volúmenes de datos de Application Insights](../../application-insights/app-insights-pricing.md) se describe cómo analizar el uso de datos en Application Insights.

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

Los clientes que compraron Microsoft Operations Management Suite E1 y E2 son aptos para los derechos de ingesta de datos por nodo para [Log Analytics](https://www.microsoft.com/cloud-platform/operations-management-suite) y [Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-pricing#the-price-plans). Para recibir estos derechos para las áreas de trabajo de Log Analytics o los recursos de Application Insights en una suscripción determinada: 

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

Si ha decidido adoptar el nuevo modelo de precios para una suscripción, seleccione la opción **Selección del modelo de precios** en la parte superior de la página **Uso y costos estimados**:

![Captura de pantalla de la página Uso y costo estimado de Azure Monitor en el nuevo modelo de precios](./media/usage-estimated-costs/006.png)

La página **Selección del modelo de precios** se abrirá. Muestra una lista de cada una de las suscripciones que vio en la página anterior:

![Captura de pantalla de la selección del modelo de precios](./media/usage-estimated-costs/007.png)

Para trasladar una suscripción al nuevo modelo de precios, simplemente active la casilla y seleccione **Guardar**. Puede volver al modelo de precios anterior de la misma manera. Tenga en cuenta que se requieren permisos de propietario o colaborador de la suscripción para cambiar el modelo de precios.

## <a name="automate-moving-to-the-new-pricing-model"></a>Traslado automático al nuevo modelo de precios

Los scripts siguientes requieren el módulo de Azure PowerShell. Para comprobar si tiene la versión más reciente, consulte [Instalación del módulo de Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-6.1.0).

Una vez que tenga la versión más reciente de Azure PowerShell, primero debería ejecutar ``Connect-AzureRmAccount``.

``` PowerShell
# To check if your subscription is eligible to adjust pricing models.
$ResourceID ="/subscriptions/<Subscription-ID-Here>/providers/microsoft.insights"
Invoke-AzureRmResourceAction `
 -ResourceId $ResourceID `
 -ApiVersion "2017-10-01" `
 -Action listmigrationdate `
 -Force
```

Si isGrandFatherableSubscription es True, el modelo de precios de esta suscripción se puede mover entre modelos de precios. Si falta un valor en optedInDate significa que esta suscripción actualmente está establecida en el modelo de precios anterior.

```
isGrandFatherableSubscription optedInDate
----------------------------- -----------
                         True            
```

Para migrar esta suscripción al nuevo modelo de precios, ejecute:

```PowerShell
$ResourceID ="/subscriptions/<Subscription-ID-Here>/providers/microsoft.insights"
Invoke-AzureRmResourceAction `
 -ResourceId $ResourceID `
 -ApiVersion "2017-10-01" `
 -Action migratetonewpricingmodel `
 -Force
```

Para confirmar que el cambio se realizó correctamente, vuelva a ejecutar:

```PowerShell
$ResourceID ="/subscriptions/<Subscription-ID-Here>/providers/microsoft.insights"
Invoke-AzureRmResourceAction `
 -ResourceId $ResourceID `
 -ApiVersion "2017-10-01" `
 -Action listmigrationdate `
 -Force
```

Si la migración se realizó correctamente, el resultado ahora debería ser similar a:

```
isGrandFatherableSubscription optedInDate                      
----------------------------- -----------                      
                         True 2018-05-31T13:52:43.3592081+00:00
```

optInDate ahora incluye una marca de tiempo que indica cuándo esta suscripción decidió usar el modelo de precios nuevo.

Si tiene que revertir el cambio y volver al modelo de precios anterior, debería ejecutar:

```PowerShell
 $ResourceID ="/subscriptions/<Subscription-ID-Here>/providers/microsoft.insights"
Invoke-AzureRmResourceAction `
 -ResourceId $ResourceID `
 -ApiVersion "2017-10-01" `
 -Action rollbacktolegacypricingmodel `
 -Force
```

Si vuelve a ejecutar el script anterior que tiene ``-Action listmigrationdate``, ahora debería ver un valor optedInDate vacío que indica que la suscripción volvió al modelo de precios heredado.

Si tiene varias suscripciones que quiere migrar y que están hospedadas en el mismo inquilino, puede crear su propia variante con partes de los scripts siguientes:

```PowerShell
#Query tenant and create an array comprised of all of your tenants subscription ids
$TenantId = <Your-tenant-id>
$Tenant =Get-AzureRMSubscription -TenantId $TenantId
$Subscriptions = $Tenant.Id
```

Para comprobar si todas las suscripciones del inquilino son elegibles para el modelo de precios nuevo, puede ejecutar:

```PowerShell
Foreach ($id in $Subscriptions)
{
$ResourceID ="/subscriptions/$id/providers/microsoft.insights"
Invoke-AzureRmResourceAction `
 -ResourceId $ResourceID `
 -ApiVersion "2017-10-01" `
 -Action listmigrationdate `
 -Force
}
```

El script se puede perfeccionar si se crea un script que genere tres matrices. Una matriz constará de todos los identificadores de todas las suscripciones que tienen ```isGrandFatherableSubscription``` establecido en True y optedInDate actualmente no tiene un valor. La segunda matriz de cualquier suscripción actualmente en el modelo de precios nuevo. Y una tercera matriz rellenada solo con los identificadores de suscripción del inquilino que no son elegibles para el modelo de precios nuevo:

```PowerShell
[System.Collections.ArrayList]$Eligible= @{}
[System.Collections.ArrayList]$NewPricingEnabled = @{}
[System.Collections.ArrayList]$NotEligible = @{}

Foreach ($id in $Subscriptions)
{
$ResourceID ="/subscriptions/$id/providers/microsoft.insights"
$Result= Invoke-AzureRmResourceAction `
 -ResourceId $ResourceID `
 -ApiVersion "2017-10-01" `
 -Action listmigrationdate `
 -Force

     if ($Result.isGrandFatherableSubscription -eq $True -and [bool]$Result.optedInDate -eq $False)
     {
     $Eligible.Add($id)
     }

     elseif ($Result.isGrandFatherableSubscription -eq $True -and [bool]$Result.optedInDate -eq $True)
     {
     $NewPricingEnabled.Add($id)
     }

     elseif ($Result.isGrandFatherableSubscription -eq $False)
     {
     $NotEligible.add($id)
     }
}
```

> [!NOTE]
> Según el número de suscripción, el script anterior puede tardar en ejecutarse. Debido al uso del método .add(), la ventana de PowerShell reflejará los valores en incremento cuando se agreguen elementos a cada matriz.

Ahora que las suscripciones están divididas en las tres matrices, debe revisar cuidadosamente los resultados. Es posible que quiera crear una copia de seguridad del contenido de las matrices para poder revertir fácilmente los cambio si tiene que hacerlo en el futuro. Si decidió que quiere convertir todas las suscripciones elegibles que actualmente tienen el modelo de precios anterior al modelo de precios nuevo, puede realizar esta tarea con:

```PowerShell
Foreach ($id in $Eligible)
{
$ResourceID ="/subscriptions/$id/providers/microsoft.insights"
Invoke-AzureRmResourceAction `
 -ResourceId $ResourceID `
 -ApiVersion "2017-10-01" `
 -Action migratetonewpricingmodel `
 -Force
}

```