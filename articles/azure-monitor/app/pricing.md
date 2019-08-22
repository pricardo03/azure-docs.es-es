---
title: Administración del uso y los costos de Azure Application Insights | Microsoft Docs
description: Administre los volúmenes de telemetría y supervise los costos en Application Insights.
services: application-insights
documentationcenter: ''
author: DaleKoetke
manager: carmonm
ms.assetid: ebd0d843-4780-4ff3-bc68-932aa44185f6
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.reviewer: mbullwin
ms.date: 08/13/2019
ms.author: dalek
ms.openlocfilehash: abf23eda2474ecbcfcaf0dadb26327225213a9a6
ms.sourcegitcommit: 5b76581fa8b5eaebcb06d7604a40672e7b557348
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2019
ms.locfileid: "68989226"
---
# <a name="manage-usage-and-costs-for-application-insights"></a>Administración del uso y los costos de Application Insights

> [!NOTE]
> En este artículo se describe cómo analizar el uso de datos de Application Insights.  Consulte los artículos siguientes para obtener información relacionada.
> - En [Supervisión del uso y costos estimados](../../monitoring-and-diagnostics/monitoring-usage-and-estimated-costs.md) se describe cómo ver el uso y los costos estimados a través de varias características de supervisión de Azure para los distintos modelos de precios. También describe cómo cambiar el modelo de precios.

Si tiene preguntas sobre cómo funcionan los precios para Application Insights, no dude en publicar una pregunta en nuestro [foro](https://social.msdn.microsoft.com/Forums/home?forum=ApplicationInsights).

## <a name="pricing-model"></a>Modelo de precios

Los precios de [Azure Application Insights][start] se basan en el volumen de datos ingeridos. Cada recurso de Application Insights se cobra como un servicio independiente y contribuye a la factura de la suscripción a Azure.

### <a name="data-volume-details"></a>Detalles de volumen de datos

* El volumen de datos es el número de bytes de telemetría que recibe Application Insights. El volumen de datos se mide como el tamaño del paquete de datos JSON sin comprimir que recibe Application Insights de su aplicación. Para [los datos tabulares importados en Analytics](https://docs.microsoft.com/azure/application-insights/app-insights-analytics-import), el volumen de datos se mide como el tamaño descomprimido de los archivos enviados a Application Insights.
* Los cargos por volumen de datos de la aplicación se notifican a partir de abril de 2018 en un nuevo medidor de facturación denominado **Ingesta de datos**. Este nuevo medidor se comparte a través de tecnologías de supervisión, como Application Insights y Log Analytics, y su actual nombre de servicio es **Log Analytics**. 
* Los datos de la [secuencia de métricas en directo](../../azure-monitor/app/live-stream.md) no cuentan para calcular los precios.

> [!NOTE]
> Todos los precios que se muestran en las capturas de pantalla de este artículo son solo para fines de ejemplo. Para ver los precios vigentes actualmente en su moneda y región, consulte [Precios de Application Insights][pricing].

### <a name="multi-step-web-tests"></a>Pruebas web de varios pasos

Las [pruebas web de varios pasos](../../azure-monitor/app/availability-multistep.md) conllevan un cargo adicional. Las pruebas web de varios pasos son pruebas web que realizan una secuencia de acciones.

No hay ningún cargo aparte para las *pruebas de ping* de una sola página. La telemetría de las pruebas de ping y de las de varios pasos se carga igual que el resto de la telemetría de su aplicación.

## <a name="review-usage-and-estimate-costs"></a>Revisión del uso y costos estimados

Application Insights permite entender fácilmente cuáles serán los costos en función de patrones de uso reciente. Para comenzar, en Azure Portal, en el recurso de Application Insights, vaya a la página **Uso y costos estimados**:

![Elija el precio](./media/pricing/pricing-001.png)

A. Revise el volumen de datos para el mes. Esto incluye todos los datos recibidos y retenidos (después de cualquier [muestreo](../../azure-monitor/app/sampling.md)) de las aplicaciones de servidor y cliente, y de las pruebas de disponibilidad.  
B. Se realiza un cargo adicional para las [pruebas web de varios pasos](../../azure-monitor/app/availability-multistep.md). (Esto no incluye pruebas de disponibilidad simples, que se incluyen en la carga de volumen de datos).  
C. Vea las tendencias de volumen de datos durante el mes anterior.  
D. Habilite el [muestreo](../../azure-monitor/app/sampling.md) de ingesta de datos.   
E. Establezca el límite de volumen de datos diario.  

Para investigar el uso de Application Insights con más detalle, abra la página **Métricas**, agregue la métrica denominada "Volumen de puntos de datos" y, después, seleccione la opción *Aplicar división* para dividir los datos por "Tipo de elemento de telemetría". 

Los cargos de Application Insights se agregarán a la factura de Azure. Puede ver los detalles de su factura de Azure en la sección de **facturación** de Azure Portal o en el [Portal de facturación de Azure](https://account.windowsazure.com/Subscriptions). 

![En el menú de la izquierda, seleccione Facturación](./media/pricing/02-billing.png)

## <a name="data-rate"></a>Velocidad de datos
El volumen de datos que envía se limita de tres formas:

* **Muestreo**: puede usar el muestreo para reducir la cantidad de telemetría enviada desde las aplicaciones de servidor y de cliente, con mínima distorsión de las métricas. El muestreo es la principal herramienta que puede usar para ajustar la cantidad de datos que envía. Obtenga más información sobre las [características de muestreo](../../azure-monitor/app/sampling.md). 
* **Límite diario**: al crear un recurso de Application Insights en Azure Portal, el límite diario se establece en 100 GB/día. Al crear un recurso de Application Insights en Visual Studio, la capacidad predeterminada es pequeña (solo 32,3 MB/día). El límite diario predeterminado se establece para facilitar las pruebas. El objetivo es que el usuario aumente el límite diario antes de implementar la aplicación en producción. 

    El límite máximo es de 1,000 GB/día, a menos que solicite un máximo superior para una aplicación con mucho tráfico. 

    Tenga cuidado al establecer el límite diario. Su objetivo debe ser *no alcanzar nunca el límite diario*. Si lo alcanza, perderá datos durante el resto del día y no podrá supervisar su aplicación. Para cambiar el límite diario, use la opción **Límite de volumen diario**. Puede tener acceso a esta opción en el panel **Uso y costos estimados** (se describe con más detalle más adelante en el artículo).
    Hemos quitado la restricción en algunos tipos de suscripción con crédito que no se podía usar para Application Insights. Anteriormente, si la suscripción tenía un límite de gasto, el cuadro de diálogo de límite diario mostraba instrucciones sobre cómo quitarlo y habilitarlo para superar los 32,3 MB/día.
* **Limitación**: la limitación restringe la velocidad de datos a 32 000 eventos por segundo, promediados durante 1 minuto por clave de instrumentación.

*¿Qué ocurre si mi aplicación supera el porcentaje de limitación?*

* El volumen de datos que su aplicación envía se evalúa cada minuto. Si se supera la tasa por segundo promediada por minuto, el servidor rechazará algunas solicitudes. El SDK almacena en búfer los datos y, a continuación, intenta volver a enviarlos. Extiende un aumento durante varios minutos. Si su aplicación envía de forma consistente un volumen de datos que supera la tasa de limitación, es posible que algunos se eliminen (los SDK de ASP.NET, Java y JavaScript intentan volver a enviarlos de esta manera; otros SDK pueden simplemente eliminar los datos limitados). Si se produce la limitación, una notificación de advertencia le indica que esto ha sucedido.

*¿Cómo puedo saber cuántos datos envía mi aplicación?*

Puede usar una de las siguientes opciones para ver cuántos datos envía su aplicación:

* Vaya al panel **Uso y costos estimados** para ver el gráfico de volumen de datos diario. 
* En el Explorador de métricas, agregue un nuevo gráfico. En la métrica del gráfico, seleccione **volumen de punto de datos**. Activa la **agrupación** y, a continuación, agrupe por **Tipo de datos**.

## <a name="reduce-your-data-rate"></a>Reduzca la velocidad de datos
Estas son algunas cosas que puede hacer para reducir el volumen de datos:

* Use el [Muestreo](../../azure-monitor/app/sampling.md). Esta tecnología reduce la velocidad de datos sin sesgar las métricas. No perderá la capacidad de navegar entre elementos relacionados en Search. En las aplicaciones de servidor, el muestreo funciona automáticamente.
* [Limite el número de llamadas AJAX que se pueden notificar](../../azure-monitor/app/javascript.md#detailed-configuration) en cada vista de página o desactive los informes de AJAX.
* [Edite ApplicationInsights.config](../../azure-monitor/app/configuration-with-applicationinsights-config.md) para desactivar los módulos de recopilación que no necesite. Por ejemplo, podría decidir que los contadores de rendimiento o datos de dependencia no son esenciales.
* Divida la telemetría entre claves de instrumentación independientes. 
* Métricas agregadas previamente. Si coloca llamadas a TrackMetric en su aplicación, puede reducir el tráfico mediante la sobrecarga que acepta el cálculo de la media y la desviación estándar de un lote de medidas. O bien, puede usar un [paquete de agregación previa](https://www.myget.org/gallery/applicationinsights-sdk-labs).

## <a name="manage-the-maximum-daily-data-volume"></a>Administración del volumen de datos diario máximo

Puede usar el límite de volumen diario para restringir los datos recopilados. Sin embargo, si se alcanza el límite, se producirá una pérdida de todas las telemetrías enviadas desde su aplicación durante el resto del día. No *es aconsejable* hacer que su aplicación alcance el límite diario. No puede realizar un seguimiento del estado y rendimiento de su aplicación una vez que alcance el límite diario.

En lugar de usar el límite de volumen diario, use el [muestreo](../../azure-monitor/app/sampling.md) para ajustar el volumen de datos al nivel que desee. A continuación, use el límite diario solo como "último recurso" en caso de que su aplicación empiece a enviar de forma inesperada volúmenes de telemetría mucho más altos.

Para cambiar el límite diario, en la sección de **configuración** del recurso de Application Insights, en el panel **Uso y costos estimados**, seleccione  **Límite diario**.

![Ajuste del límite de volumen de telemetría diario](./media/pricing/pricing-003.png)

## <a name="sampling"></a>muestreo
[Muestreo](../../azure-monitor/app/sampling.md) es un método que permite reducir la velocidad a la que se envían datos de telemetría a la aplicación, al mismo tiempo que se conserva la capacidad de buscar eventos relacionados durante las búsquedas de diagnósticos. También retiene recuentos de eventos correctos.

El muestreo es una manera efectiva de reducir los gastos y permanecer dentro de la cuota mensual. El algoritmo de muestreo conserva elementos relacionados con la telemetría, de modo que, por ejemplo, al usar Search se puede buscar la solicitud relacionada con una excepción determinada. El algoritmo también conserva recuentos correctos, para que pueda ver en el Explorador de métricas los valores correctos de tasas de solicitudes, tasas de excepciones y otros recuentos.

Existen varias formas de muestreo.

* [Muestreo adaptable](../../azure-monitor/app/sampling.md) es el valor predeterminado del SDK de ASP.NET. El muestreo adaptable se ajusta automáticamente al volumen de telemetría que envía la aplicación. Funciona automáticamente en el SDK de la aplicación web, de modo que se reduce el tráfico de telemetría en la red. 
* *muestreo de ingesta* es una opción alternativa que funciona en el momento en que la telemetría procedente de su aplicación entra en el servicio Application Insights. El muestreo de ingesta no afecta al volumen de telemetría que envía la aplicación, pero reduce el volumen que retiene el servicio. Puede utilizar el muestreo de ingesta para reducir la cuota que utiliza la telemetría de exploradores y otros SDK.

Para establecer el muestreo de ingesta, vaya al panel **Precios**:

![En el panel Cuota y precios, seleccione el icono Ejemplos y, a continuación, seleccione una fracción de muestreo.](./media/pricing/pricing-004.png)

> [!WARNING]
> El panel **Muestreo de datos** solo controla el valor de muestreo de ingesta. No refleja la velocidad de muestreo que se aplica mediante el SDK de Application Insights en la aplicación. Si ya se ha muestreado la telemetría entrante en el SDK, no se aplica el muestreo de ingesta.
>

Para conocer la frecuencia de muestreo real independientemente de dónde se ha aplicado, use una [consulta de Analytics](analytics.md). La consulta tiene este aspecto:

    requests | where timestamp > ago(1d)
    | summarize 100/avg(itemCount) by bin(timestamp, 1h)
    | render areachart

En cada registro retenido, `itemCount` indica el número de registros originales que representa. Es igual a 1 + el número de registros descartados anteriores. 

## <a name="change-the-data-retention-period"></a>Cambio del período de retención de datos

Application Insights ahora incorpora un número limitado de clientes de Application Insights en nuestra versión preliminar de retención de variables. Puede encontrar información sobre cómo participar en este programa de versión preliminar [aquí](https://feedback.azure.com/forums/357324-azure-monitor-application-insights/suggestions/17454031).

La retención predeterminada de los recursos de Application Insights es de 90 días. Es posible seleccionar distintos períodos de retención para cada recurso de Application Insights. El conjunto completo de períodos de retención disponibles es de 30, 60, 120, 180, 270, 365, 550 o 730 días. 

Cuando la facturación está habilitada para una retención más prolongada, los datos que se mantengan más de 90 días se facturarán con la misma tarifa que actualmente se factura por la retención de datos de Azure Log Analytics. Obtenga más información sobre la [página de precios de Azure Monitor](https://azure.microsoft.com/pricing/details/monitor/).  Para mantener se al día con el progreso de la retención de variables, [vote por esta sugerencia](https://feedback.azure.com/forums/357324-azure-monitor-application-insights/suggestions/17454031). 

## <a name="limits-summary"></a>Resumen de límites

[!INCLUDE [application-insights-limits](../../../includes/application-insights-limits.md)]

## <a name="disable-daily-cap-e-mails"></a>Deshabilitación de los correos electrónicos de límite diario

Para deshabilitar los correos electrónicos de límite de volumen diario, en la sección de **Configuración** del recurso de Application Insights, en el panel **Uso y costos estimados**, seleccione **Límite diario**. Hay opciones para enviar correos electrónicos cuando se alcanza el límite, así como cuando se ha alcanzado un nivel de advertencia ajustable. Si quiere deshabilitar todos los correos electrónicos relacionados con el límite de volumen diario, desactive ambas casillas.

## <a name="legacy-enterprise-per-node-pricing-tier"></a>Plan de tarifa de Enterprise heredado (por nodo)

Para los usuarios pioneros de Azure Application Insights, todavía hay dos planes de tarifa posibles: Básico y Enterprise. El plan de tarifa Básico es el mismo que se describió anteriormente y es el predeterminado. Incluye todas las características del plan Enterprise sin ningún coste adicional. En el nivel Básico, la facturación se realiza principalmente en función del volumen de datos ingerido. 

> [!NOTE]
> Estos planes de tarifa heredados han cambiado de nombre. El plan de tarifa Enterprise ahora se llama **Por nodo** y el plan de tarifa Básico ahora se llama **Por GB**. Estos nuevos nombres se usan a continuación y en Azure Portal.  

El nivel Por nodo (anteriormente Enterprise) tiene un cargo por nodo y cada nodo recibe una concesión de datos diaria. En el plan de tarifa Por nodo, se le cobrará por los datos ingeridos por encima de la concesión incluida. Si usa Operations Management Suite, debería elegir el nivel Por nodo. 

Para ver los precios vigentes actualmente en su moneda y región, consulte [Precios de Application Insights](https://azure.microsoft.com/pricing/details/application-insights/).

> [!NOTE]
> En abril de 2018, [introdujimos](https://azure.microsoft.com/blog/introducing-a-new-way-to-purchase-azure-monitoring-services/) un nuevo modelo de precios para la supervisión de Azure. Este modelo adopta un modelo de "pago por uso" sencillo en toda la cartera de servicios de supervisión. Más información sobre el [nuevo modelo de precios](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-usage-and-estimated-costs), cómo [evaluar el impacto de pasar a este modelo](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-usage-and-estimated-costs#assessing-the-impact-of-the-new-pricing-model) en función de los patrones de uso y [cómo participar en el nuevo modelo](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-usage-and-estimated-costs#moving-to-the-new-pricing-model).

### <a name="per-node-tier-and-operations-management-suite-subscription-entitlements"></a>Derechos del nivel Por nodo y la suscripción a Operations Management Suite

Como se [anunció anteriormente](https://blogs.technet.microsoft.com/msoms/2017/05/19/azure-application-insights-enterprise-as-part-of-operations-management-suite-subscription/), los clientes que compran Operations Management Suite E1 y E2 pueden obtener Application Insights Por nodo como componente adicional sin coste alguno. Específicamente, cada unidad de Operations Management Suite E1 y E2 incluye el derecho de usar un nodo del nivel Application Insights Por nodo. Cada nodo de Application Insights incluye hasta 200 MB de datos ingeridos por día (independientes de la ingesta de datos de Log Analytics), con la retención de datos de 90 días sin costo adicional alguno. El nivel se describe con más detalle más adelante en el artículo. 

Dado que este nivel solo se aplica a clientes con una suscripción a Operations Management Suite, los clientes sin una suscripción no verán una opción para seleccionar este nivel.

> [!NOTE]
> Para garantizar que tiene este derecho, sus recursos de Application Insights deben estar en el plan de tarifa Por nodo. Este derecho se aplica solo como nodos. Los recursos de Application Insights del nivel Por GB no consiguen ningún beneficio. Este derecho no es visible en los costos estimados que se muestran en el panel **Uso y costos estimados**. Además, si traslada una suscripción al nuevo modelo de precios de supervisión de Azure en abril de 2018, el nivel Por GB será el único nivel disponible. El traslado de una suscripción al nuevo modelo de precios de supervisión de Azure no es aconsejable si tiene una suscripción a Operations Management Suite.

### <a name="how-the-per-node-tier-works"></a>Funcionamiento del nivel Por nodo

* En el nivel Por nodo, se paga por cada nodo que envíe datos de telemetría desde cualquier aplicación.
  * Un *nodo* es un servidor físico o virtual o una instancia del rol de plataforma como servicio que hospeda la aplicación.
  * Los equipos de desarrollo, los exploradores cliente y los dispositivos móviles no se cuentan como nodos.
  * Si la aplicación contiene varios componentes que envían datos telemetría, como un servicio web y un trabajo de back-end, estos componentes se cuentan por separado.
  * Los datos de la [secuencia de métricas en directo](../../azure-monitor/app/live-stream.md) no cuentan para calcular los precios. En una suscripción, se cobra por nodo, no por aplicación. Si tiene cinco nodos que envían telemetría desde 12 aplicaciones, se le cobrarán 5 nodos.
* Aunque los cargos se presupuestan mensualmente, solo se le cobrarán las horas en las que un nodo envíe telemetría de una aplicación. El cargo por hora es el cargo mensual presupuestado dividido entre 744 (el número de horas que tiene un mes de 31 días).
* Cada día, se asigna un volumen de datos de 200 MB por cada nodo detectado (con una granularidad por hora). Las asignaciones de datos que no se utilizan no se guardan de un día para otro.
  * Si elige el plan de tarifa Por nodo, cada suscripción recibirá una concesión de datos diaria en función del número de nodos que envíen datos de telemetría a los recursos de Application Insights de esa suscripción. Por tanto, si tiene cinco nodos que envían datos todo el día, tendrá una asignación global de 1 GB para todos los recursos de Application Insights de esa suscripción. No importa si algunos nodos envían más datos que otros, ya que los datos incluidos se comparten entre todos los nodos. Si, un día determinado, los recursos de Application Insights reciben más datos de los que se incluyen en la concesión de datos diaria para esta suscripción, se aplicarán los cargos de datos por encima del límite por cada GB. 
  * La asignación de datos diaria se calcula como el número de horas del día (de acuerdo con UTC) que cada nodo envía datos de telemetría dividido entre 24 y multiplicado por 200 MB. Es decir, si tiene cuatro nodos que envían datos de telemetría durante 15 de las 24 horas del día, los datos incluidos para ese día serán ((4 &#215; 15) / 24) &#215; 200 MB = 500 MB. A un precio de 2,30 USD por GB en el caso de uso por encima del límite de datos, el cargo sería de 1,15 USB si los nodos envían 1 GB de datos ese día.
  * La concesión diaria del nivel Por nodo no se comparte con las aplicaciones para las que haya elegido el nivel Por GB. La asignación que no se ha utilizado no se guarda de un día para otro. 

### <a name="examples-of-how-to-determine-distinct-node-count"></a>Ejemplos acerca de cómo determinar el número de nodos

| Escenario                               | Número de nodos total diario |
|:---------------------------------------|:----------------:|
| 1 aplicación utiliza 3 instancias de Azure App Service y 1 servidor virtual | 4 |
| 3 aplicaciones se ejecutan en 2 VM; los recursos de Application Insights de estas aplicaciones están en la misma suscripción y en el nivel Por nodo | 2 | 
| 4 aplicaciones cuyos recursos de Applications Insights están en la misma suscripción; cada aplicación ejecuta 2 instancias durante 16 horas de poca actividad y 4 instancias durante 8 horas punta | 13,33 | 
| Servicios en la nube con 1 rol de trabajo y 1 rol web; cada uno ejecuta 2 instancias. | 4 | 
| Un clúster de Azure Service Fabric con 5 nodos ejecuta 50 microservicios; cada microservicio ejecuta 3 instancias | 5|

* El número preciso de nodos depende del SDK de Application Insights que utilice la aplicación. 
  * En las versiones 2.2 y posteriores del SDK, tanto el [SDK básico](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) como el [SDK web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) de Application Insights cuentan cada host de aplicación como un nodo. Algunos ejemplos son el nombre de equipo del servidor físico y los hosts de máquina virtual, o el nombre de instancia de los servicios en la nube.  La única excepción es una aplicación que solamente usa [.NET Core](https://dotnet.github.io/) y el SDK básico de Application Insights. En ese caso, solo se cuenta un nodo para todos los hosts porque el nombre de host no está disponible. 
  * En las versiones anteriores del SDK, el [SDK web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) se comporta del mismo modo que las versiones más nuevas. Sin embargo, el [SDK básico](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) solo cuenta un nodo, independientemente del número real de hosts de la aplicación. 
  * Si la aplicación utiliza el SDK para establecer **roleInstance** en un valor personalizado, ese mismo valor se usará de manera predeterminada para calcular el número de nodos. 
  * Si utiliza una nueva versión del SDK con una aplicación que se ejecuta desde equipos cliente o dispositivos móviles, es posible que el recuento de nodos devuelva un número muy grande (debido a la gran cantidad de equipos cliente o de dispositivos móviles). 

## <a name="automation"></a>Automation

Puede escribir un script para establecer el plan de tarifa mediante Azure Resource Management. [Más información](powershell.md#price).


## <a name="next-steps"></a>Pasos siguientes

* [Muestreo](../../azure-monitor/app/sampling.md)

[api]: app-insights-api-custom-events-metrics.md
[apiproperties]: app-insights-api-custom-events-metrics.md#properties
[start]: ../../azure-monitor/app/app-insights-overview.md
[pricing]: https://azure.microsoft.com/pricing/details/application-insights/