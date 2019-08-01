---
title: Alertas de registro en Azure Monitor
description: Desencadene correos electrónicos, notificaciones, llamadas a direcciones URL de sitios web (webhooks) o automatización cuando se cumplan las condiciones de consulta analítica especificadas para Alertas de Azure.
author: msvijayn
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 5/31/2019
ms.author: vinagara
ms.subservice: alerts
ms.openlocfilehash: ae35c735cffeb8cd85af1f32bb2d14ede6dc6b69
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66427416"
---
# <a name="log-alerts-in-azure-monitor"></a>Alertas de registro en Azure Monitor

En este artículo se proporcionan detalles sobre las alertas de registro, uno de los tipos de alerta que se admiten en [Alertas de Azure](../../azure-monitor/platform/alerts-overview.md) y permite que los usuarios usen la plataforma de análisis de Azure como base para las alertas.

Alertas de registro consiste en reglas de búsqueda de registros creadas para los [registros de Azure Monitor](../../azure-monitor/learn/tutorial-viewdata.md) o [Application Insights](../../azure-monitor/app/cloudservices.md#view-azure-diagnostics-events). Para obtener más información sobre su uso, consulte [creación de alertas de registro en Azure](../../azure-monitor/platform/alerts-log.md).

> [!NOTE]
> Ahora los datos de registro populares de los [registros de Azure Monitor](../../azure-monitor/learn/tutorial-viewdata.md) también están disponibles en la plataforma de métricas de Azure Monitor. Para obtener más detalles, consulte [Alerta de métricas de los registros](../../azure-monitor/platform/alerts-metric-logs.md).


## <a name="log-search-alert-rule---definition-and-types"></a>Regla de alertas de búsqueda de registros: definición y tipos

Alertas de Azure crea las reglas de búsqueda de registros para ejecutar automáticamente consultas de registros especificadas a intervalos regulares.  Si los resultados de la consulta de registros coinciden con determinados criterios, se crea un registro de alertas. La regla, luego, puede ejecutar de forma automática una o varias acciones con [grupos de acciones](../../azure-monitor/platform/action-groups.md). Puede que se necesite el rol de [Colaborador de supervisión de Azure](../../azure-monitor/platform/roles-permissions-security.md) para crear, modificar y actualizar las alertas de registro. Además, debe tener los permisos de acceso y ejecución de consultas para los destinos de análisis en la consulta o regla de alertas. Si el usuario que realice el proceso no tiene acceso a todos los destinos de análisis en la consulta o regla de alertas, puede que la creación de la regla devuelva un error o que la regla de alertas de registro se ejecute con resultados parciales.

Las reglas de búsqueda de registros se definen mediante los siguientes detalles:

- **Consulta de registro**.  La consulta que se ejecuta cada vez que se activa la regla de alertas.  Los registros devueltos por esta consulta se usan para determinar si se desencadena una alerta. La consulta de Analytics puede ser para un área de trabajo específica de Log Analytics o una aplicación de Application Insights, e incluso puede abarcar [varios recursos de Log Analytics y de Application Insights](../../azure-monitor/log-query/cross-workspace-query.md#querying-across-log-analytics-workspaces-and-from-application-insights) siempre que el usuario tenga derechos de acceso y de consulta a todos los recursos. 
    > [!IMPORTANT]
    > Se admiten [consultas entre recursos](../../azure-monitor/log-query/cross-workspace-query.md#querying-across-log-analytics-workspaces-and-from-application-insights) para alertas de registro de Application Insights y para alertas de registro de [Log Analytics que se configuran únicamente mediante la API scheduledQueryRules](../../azure-monitor/platform/alerts-log-api-switch.md).

    Algunos comandos y combinaciones de análisis son incompatibles con el uso de las alertas de registro. Para más información, consulte [Consultas de alertas de registro en Azure Monitor](../../azure-monitor/platform/alerts-log-query.md).

- **Período de tiempo**.  Especifica el intervalo de tiempo para la consulta. La consulta devuelve solo los registros que se crearon dentro de este intervalo de tiempo actual. El período de tiempo restringe los datos capturados para la consulta de registros, a fin de evitar abusos y cualquier comando de tiempo (como “ago”) utilizado en las consultas de registros. <br>*Por ejemplo, si el período de tiempo se establece en 60 minutos, y la consulta se ejecuta a las 13:15, se devuelven solo los registros creados entre las 12:15 y las 13:15 para ejecutar la consulta de registros. Ahora, si la consulta de registros utiliza un comando de tiempo, como ago (7d), la consulta de registro se ejecutaría solo para los datos entre las 12:15 y las 13:15, como si existieran datos solo para los últimos 60 minutos. Y no durante siete días de datos, tal como se especifica en la consulta de registros.*

- **Frecuencia**.  Especifica con qué frecuencia se debe ejecutar la consulta. Puede ser cualquier valor entre 5 minutos y 24 horas. Debe ser menor o igual que el período de tiempo.  Si el valor es mayor que el período de tiempo, se arriesga a que se pierdan registros.<br>*Por ejemplo, considere la posibilidad de un período de tiempo de 30 minutos y una frecuencia de 60 minutos.  Si la consulta se ejecuta a la 1:00, devolverá registros entre las 12:30 y la 1:00 p. m.  La próxima vez que se ejecute la consulta será a las 2:00 y devolverá los registros comprendidos entre la 1:30 y las 2:00.  Nunca se evaluarían los registros creados entre la 1:00 y las 13:30.*

- **Umbral**.  Los resultados de la búsqueda de registros se evalúan para determinar si se debe crear una alerta.  El umbral es diferente para los distintos tipos de reglas de alerta de búsqueda de registros.

Las reglas de búsqueda de [registros de Azure Monitor](../../azure-monitor/learn/tutorial-viewdata.md) o [Application Insights](../../azure-monitor/app/cloudservices.md#view-azure-diagnostics-events) pueden ser de dos tipos. Cada uno de estos tipos se describe en detalle en las secciones que aparecen a continuación.

- **[Número de resultados](#number-of-results-alert-rules)** . Alerta única creada cuando los registros de números devueltos por la búsqueda de registros superan un número especificado.
- **[Unidades métricas](#metric-measurement-alert-rules)** .  Alerta creada para cada objeto de los resultados de la búsqueda de registros con valores que superan el umbral especificado.

Las diferencias entre los tipos de reglas de alerta son las siguientes.

- La regla de alertas para *Número de resultados* crea siempre una alerta única mientras que la regla de alertas para *Unidades métricas* crea una alerta para cada objeto que supere el umbral.
- Las reglas de alerta para *número de resultados* crean una alerta cuando se supera el umbral una sola vez. Las reglas de alerta para *unidades métricas* pueden crear una alerta cuando se supera el umbral un número determinado de veces en un intervalo de tiempo determinado.

### <a name="number-of-results-alert-rules"></a>Reglas de alerta para número de resultados

Las reglas de alerta para **número de resultados** crean una única alerta cuando el número de registros devueltos por la consulta de búsqueda supera el umbral especificado. Este tipo de regla de alertas es perfecto para trabajar con eventos como, por ejemplo, registros de eventos de Windows, Syslog, respuesta de WebApp y registros personalizados.  Es posible que desee crear una alerta cuando se genere un evento de error concreto o cuando se generen varios eventos de error durante un período de tiempo establecido.

**Umbral**: el umbral de una regla de alertas para un número de resultados es mayor o menor que un valor determinado.  Se crea una alerta si el número de registros devueltos por la búsqueda de registros coincide con este criterio.

Para generar una alerta en un evento único, establezca el número de resultados en un valor mayor que 0 y compruebe la aparición de un evento único que se creó desde la última vez que se ejecutó la consulta. Algunas aplicaciones pueden registrar un error ocasional que no tiene por qué haber generado necesariamente una alerta.  Por ejemplo, la aplicación puede volver a intentar realizar el proceso que creó el evento de error y completarlo correctamente la vez siguiente.  En este caso, es posible que no desee crear la alerta, a menos que se creen varios eventos en un período de tiempo determinado.  

En algunos casos, quizá quiera crear una alerta sin que haya un evento.  Por ejemplo, un proceso puede registrar eventos regulares para indicar que está funcionando correctamente.  Si no se registra uno de estos eventos dentro de un período de tiempo determinado, debe crearse una alerta.  En este caso, tendría que establecer el umbral en un valor **less than 1** (menor que 1).

#### <a name="example-of-number-of-records-type-log-alert"></a>Ejemplo de alerta de registro del tipo número de registros

Considere un escenario donde desea saber cuándo la aplicación basada en web proporciona una respuesta a los usuarios con el código 500, un error interno del servidor. Crearía una regla de alertas con los detalles siguientes:  

- **Consulta:** solicitudes | donde resultCode == "500"<br>
- **Período de tiempo:** 30 minutos<br>
- **Frecuencia de alerta:** cinco minutos<br>
- **Valor del umbral:** mayor que 0<br>

De ese modo, la alerta ejecutaría la consulta cada 5 minutos, con 30 minutos de datos, para buscar cualquier registro con un código de resultado de 500. Si se encuentra aunque sea uno de estos registros, se desencadena la alerta y la acción configurada.

### <a name="metric-measurement-alert-rules"></a>Reglas de alertas para medición de métricas

Las reglas de alertas para **unidades métricas** crean una alerta para cada objeto de una consulta con un valor que supera un umbral especificado y una consulta de desencadenador especificada. A diferencia de las reglas de alertas de **número de resultados**, las de **unidades métricas** funcionan cuando los resultados de análisis proporcionan una serie temporal. Presentan las siguientes diferencias con respecto a las reglas de alerta para **número de resultados**.

- **Función de agregado**: Determina el cálculo que se lleva a cabo y potencialmente un campo numérico para agregar.  Por ejemplo, **count()** devuelve el número de registros de la consulta y **avg(CounterValue)** , el promedio del campo CounterValue durante el intervalo. La función de agregado en la consulta debe recibir el siguiente nombre: AggregatedValue y proporcionar un valor numérico. 

- **Campo de grupo**: Se crea un registro con un valor agregado para cada instancia de este campo y se puede generar una alerta para cada una.  Por ejemplo, si desea generar una alerta para cada equipo, usaría **by Computer** (por Equipo). En caso de que haya varios campos de grupos especificados en la consulta de alerta, el usuario puede especificar qué campo desea usar para clasificar los resultados mediante el parámetro **Aggregate On** (metricColumn).

    > [!NOTE]
    > El parámetro *Aggregate On* (metricColumn) está disponible para alertas de registro del tipo unidades métricas para Application Insights y para alertas de registro de [Log Analytics que se configuran únicamente mediante scheduledQueryRules API](../../azure-monitor/platform/alerts-log-api-switch.md).

- **Intervalo**:  Define el intervalo de tiempo durante el cual se agregan los datos.  Por ejemplo, si especifica **cinco minutos**, se crearía un registro para cada instancia del campo de grupo que se agrega a intervalos de cinco minutos en el período de tiempo especificado para la alerta.

    > [!NOTE]
    > La función Bin se debe usar en la consulta para especificar el intervalo. Como bin() puede generar intervalos de tiempo distintos, Alertas convertirá el comando bin automáticamente en el comando bin_at con un tiempo adecuado en el tiempo de ejecución para garantizar resultados con un punto fijo. Las alertas de registro del tipo unidades métricas están diseñadas para trabajar con consultas que tienen un máximo de tres instancias del comando bin().
    
- **Umbral**: El umbral de las reglas de alerta para unidades métricas se define por un valor agregado y un número de infracciones.  Si cualquier punto de datos de la búsqueda de registros supera este valor, se considera una infracción.  Si el número de infracciones para cualquier objeto de los resultados supera el valor especificado, se crea una alerta para ese objeto.

La configuración incorrecta de las opciones *Aggregate On* o *metricColumn* puede provocar que las reglas de alertas no funcionen. Para más información, consulte la sección sobre [solución de problemas cuando la regla de alertas de unidades métricas es incorrecta](alert-log-troubleshoot.md#metric-measurement-alert-rule-is-incorrect).

#### <a name="example-of-metric-measurement-type-log-alert"></a>Ejemplo de alerta de registro del tipo Unidades métricas

Considere la posibilidad de un escenario en el que desearía tener una alerta en caso de que cualquier equipo superara el uso del procesador en un 90 % tres veces en 30 minutos.  Crearía una regla de alertas con los detalles siguientes:  

- **Consulta:** Perf | where ObjectName == "Processor" and CounterName == "% Processor Time" | summarize AggregatedValue = avg(CounterValue) by bin(TimeGenerated, 5m), Computer<br>
- **Período de tiempo:** 30 minutos<br>
- **Frecuencia de alerta:** cinco minutos<br>
- **Lógica de alertas: estado y umbral:** mayor que 90<br>
- **Campo de grupo (Agregado en):** Equipo
- **Activación de alerta según:** infracciones totales mayores que 2<br>

La consulta crearía un valor medio para cada equipo a intervalos de cinco minutos.  Esta consulta se ejecutaría cada 5 minutos para los datos recopilados en los 30 minutos anteriores. Puesto que la opción Campo de grupo (Agregado en) seleccionada es la tabla "Computer" en columnas, el valor AggregatedValue se divide en varios valores de "Computer" y se determina la utilización media del procesador para cada equipo que viene determinada por un intervalo temporal de 5 minutos.  El resultado de la consulta de ejemplo para (supongamos) tres equipos sería el siguiente.


|TimeGenerated [UTC] |Equipo  |AggregatedValue  |
|---------|---------|---------|
|20xx-xx-xxT01:00:00Z     |   srv01.contoso.com      |    72     |
|20xx-xx-xxT01:00:00Z     |   srv02.contoso.com      |    91     |
|20xx-xx-xxT01:00:00Z     |   srv03.contoso.com      |    83     |
|...     |   ...      |    ...     |
|20xx-xx-xxT01:30:00Z     |   srv01.contoso.com      |    88     |
|20xx-xx-xxT01:30:00Z     |   srv02.contoso.com      |    84     |
|20xx-xx-xxT01:30:00Z     |   srv03.contoso.com      |    92     |

Si se realizara el trazado de los resultados de la consulta, este tendría el siguiente aspecto.

![Resultados de la consulta de ejemplo](media/alerts-unified-log/metrics-measurement-sample-graph.png)

En este ejemplo, hemos visto en intervalos de 5 minutos para cada uno de los tres equipos la utilización media del procesador calculada durante 5 minutos. srv01 ha infringido el umbral de 90 solo una vez en el minuto 1:25 del intervalo. En comparación, srv02 supera el umbral de 90 en los puntos 1:10, 1:15 y 1:25 del intervalo mientras que srv03 supera el umbral de 90 en los puntos 1:10, 1:15, 1:20 y 1:30.
Puesto que la alerta está configurada para desencadenarse si las infracciones son más de dos, vemos que srv02 y srv03 son los únicos que cumplen este criterio. Por tanto, se crearán alertas independientes para srv02 y srv03 porque han incumplido el umbral del 90 % dos veces en varios intervalos de tiempo.  Si, en vez de eso, el parámetro *Desencadenar alerta según:* se configuró para la opción *Continuous breaches* (Infracciones continuas), la alerta se desencadenaría **solo** para srv03 ya que infringió el umbral en tres intervalos de tiempo consecutivos, desde 1:10 a 1:20. Y **no** se desencadenaría para srv02, ya que este infringió el umbral en dos intervalos de tiempo consecutivos, de 1:10 a 1:15.

## <a name="log-search-alert-rule---firing-and-state"></a>Regla de alertas de búsqueda de registros: desencadenamiento y estado

La regla de alertas de búsqueda de registros funciona con la lógica dictada por el usuario según la configuración y la consulta personalizada de análisis que se use. Ya que la lógica de la supervisión, incluida la condición o razón exactas por las que la regla de alertas se debería desencadenar, se encapsula en una consulta de Analytics, puede ser diferente en cada regla de alertas de registro. Alertas de Azure cuenta con escasa información sobre la causa subyacente o escenario que se evalúa cuando se cumple o se supera la condición de umbral de la regla de alertas de búsqueda de registros. Por lo tanto, las alertas del registro se conocen como sin estado. Y las reglas de alertas de registro se seguirán activando periódicamente, siempre que el resultado de la consulta de análisis proporcionada cumpla la condición de la alerta. Sin la alerta, no se resuelve, ya que la lógica de la causa exacta del error de supervisión se enmascara dentro de la consulta de análisis proporcionada por el usuario. No existe en este momento ningún mecanismo para que Alertas de Azure Monitor deduzca de forma concluyente la causa raíz que se resuelve.

Veamos lo mismo con un ejemplo práctico. Supongamos que tenemos una regla de alerta de registro denominada *Contoso-Log-Alert*, según la configuración en el [ejemplo proporcionado para la alerta de registro de tipo Número de resultados](#example-of-number-of-records-type-log-alert), en el que se designa una consulta de alerta personalizada para buscar el código de resultado 500 en los registros.

- A la 1:05 p. m., cuando Alertas de Azure ejecutó Contoso-Log-Alert, el resultado de la búsqueda de registros no produjo ningún registro con el código de resultado 500. Puesto que cero está por debajo del umbral, la alerta no se desencadena.
- En la siguiente iteración a las 1:10 p. m., cuando Alertas de Azure ejecutó Contoso-Log-Alert, el resultado de la búsqueda de registros no produjo ningún registro con el código de resultado 500. Dado que el valor cinco supera el umbral, la alerta se desencadena con las acciones asociadas.
- A la 1:15 p. m., cuando Alertas de Azure ejecutó Contoso-Log-Alert, el resultado de la búsqueda de registros produjo dos registros con el código de resultado 500. Dado que el valor dos supera el umbral, la alerta se desencadena con las acciones asociadas.
- Ahora, en la siguiente iteración a la 1:20 p. m., cuando Alertas de Azure ejecutó Contoso-Log-Alert, el resultado de la búsqueda de registros de nuevo no produjo ningún registro con el código de resultado 500. Puesto que cero está por debajo del umbral, la alerta no se desencadena.

Pero en el caso enumerado anteriormente, a la 1:15 p. m., las alertas de Azure no pueden determinar que los problemas subyacentes de la 1:10 p. m. persisten y si hay nuevos errores. Como la consulta proporcionada por el usuario puede estar teniendo en cuenta los registros anteriores, las alertas de Azure pueden ser seguras. Puesto que la lógica de la alerta está encapsulada en la consulta de alerta, los dos registros con el código de resultado 500 visto a la 1:15 p. m. pueden o no haberse visto ya a la 1:10 p. m. Por tanto, para estar totalmente seguros, cuando Contoso-Log-Alert se ejecutó a la 1:15 p. m., la acción configurada se volvió a activar. Ahora a la 1:20 p. m., cuando no se ve ningún registro con el código de resultado 500, las alertas de Azure no pueden estar seguras de que la causa del código de resultado 500 visto a la 1:10 p. m. y a la 1:15 p. m. estén ahora resueltas, y las alertas de Azure Monitor pueden deducir con seguridad que los problemas de un error 500 no se repetirán por la misma razón de nuevo. Por lo tanto, el valor de Contoso-Log-Alert no cambiará a Resuelto en el panel de alertas de Azure ni en las notificaciones enviadas para afirmar que la alerta se resolvió. En su lugar, el usuario que comprenda la condición o el motivo exactos de la lógica incrustada en la consulta de análisis puede [marcar la alerta como cerrada](alerts-managing-alert-states.md) según sea necesario.

## <a name="pricing-and-billing-of-log-alerts"></a>Precios y facturación de las alertas de registro

Los precios que se aplican a las alertas de registro están disponibles en la página [Precios de Azure Monitor](https://azure.microsoft.com/pricing/details/monitor/). En las facturas de Azure, Alertas de registro se representa como tipo `microsoft.insights/scheduledqueryrules` con:

- Alertas de registro en Application Insights se muestra con el nombre exacto de la alerta junto con el grupo de recursos y las propiedades de la alerta
- Las alertas de registro de Log Analytics aparecen con el nombre exacto de la alerta junto con el grupo de recursos y las propiedades de la alerta cuando se crean mediante [scheduledQueryRules API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules).

La [API heredada de Log Analytics](../../azure-monitor/platform/api-alerts.md) dispone de acciones y programaciones de alertas como parte de la búsqueda guardada de Log Analytics y no [recursos de Azure](../../azure-resource-manager/resource-group-overview.md) propiamente dichos. Por tanto, para habilitar la facturación de tales alertas de registro heredadas creadas para Log Analytics mediante Azure Portal **sin** [cambiar a la nueva API](../../azure-monitor/platform/alerts-log-api-switch.md) o mediante la [API heredada de Log Analytics](../../azure-monitor/platform/api-alerts.md) se crearán pseudo reglas de alertas ocultas en `microsoft.insights/scheduledqueryrules` para la facturación en Azure. Las pseudo reglas de alertas ocultas creadas para la facturación en `microsoft.insights/scheduledqueryrules` aparecen como `<WorkspaceName>|<savedSearchId>|<scheduleId>|<ActionId>` junto con las propiedades de grupo de recursos y alerta.

> [!NOTE]
> Si hay caracteres no válidos como `<, >, %, &, \, ?, /`, se reemplazarán por `_` en el nombre de la pseudo regla de alertas oculta y, por tanto, también en la factura de Azure.

Para eliminar los recursos scheduleQueryRules ocultos que se crearon para la facturación de reglas de alertas mediante la [API heredada de Log Analytics](api-alerts.md), el usuario puede efectuar cualquiera de las siguientes acciones:

- El usuario puede [cambiar la preferencia de API para las reglas de alertas del área de trabajo de Log Analytics](../../azure-monitor/platform/alerts-log-api-switch.md) y sin perder sus reglas de alertas y sin supervisión puede moverlas a [scheduledQueryRules API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) compatible con Azure Resource Manager. Esto elimina la necesidad de crear pseudo reglas de alertas ocultas para la facturación.
- O si el usuario no desea cambiar la preferencia de API, deberá **eliminar** la programación original y la acción de alerta mediante la [API heredada de Log Analytics](api-alerts.md) o eliminar [la regla de alertas original en Azure Portal](../../azure-monitor/platform/alerts-log.md#view--manage-log-alerts-in-azure-portal)

Además, para los recursos scheduleQueryRules ocultos que se crearon para la facturación de reglas de alertas mediante la [API heredada de Log Analytics](api-alerts.md), cualquier operación de modificación como PUT dará error. Como las pseudo reglas de tipo `microsoft.insights/scheduledqueryrules` son para fines de facturación de las reglas de alerta creadas con la [API heredada de Log Analytics](api-alerts.md). Cualquier modificación de la regla de alerta se debe realizar mediante la [API heredada de Log Analytics](api-alerts.md) o el usuario puede [cambiar la preferencia de API para las reglas de alerta](../../azure-monitor/platform/alerts-log-api-switch.md) para usar en cambio la [API scheduledQueryRules](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules).

## <a name="next-steps"></a>Pasos siguientes

* Más información sobre la [creación de alertas de registro en Azure](../../azure-monitor/platform/alerts-log.md).
* Información sobre [webhooks en alertas de registro en Azure](alerts-log-webhook.md).
* Más información acerca de las [Alertas de Azure](../../azure-monitor/platform/alerts-overview.md).
* Más información sobre [Application Insights](../../azure-monitor/app/analytics.md).
* Más información sobre [Log Analytics](../../azure-monitor/log-query/log-query-overview.md).