---
title: Solución de problemas de gráficos de métricas de Azure Monitor
description: Solucionar los problemas con la creación, personalizar o interpretar los gráficos de métricas
author: vgorbenko
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 04/23/2019
ms.author: vitalyg
ms.subservice: metrics
ms.openlocfilehash: cff1bbefc3c54f7f9c02c646bd9eef528fe28c73
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2019
ms.locfileid: "64939565"
---
# <a name="troubleshooting-metrics-charts"></a>Solución de problemas de gráficos de métricas

Use este artículo si experimenta problemas con la creación, personalizar o interpretar los gráficos en el Explorador de métricas de Azure. Si está familiarizado con las métricas, obtenga información sobre [Introducción al explorador de métricas](metrics-getting-started.md) y [características avanzadas del explorador de métricas](metrics-charts.md). También puede ver [ejemplos](metric-chart-samples.md) de los gráficos de métricas configurados.

## <a name="cant-find-your-resource-to-select-it"></a>No se encuentra el recurso para seleccionarlo

Hacer clic en el **seleccionar un recurso** botón, pero no ve el recurso en el cuadro de diálogo de selector de recursos.

**Solución:** Explorador de métricas, deberá seleccionar las suscripciones y grupos de recursos antes de enumerar los recursos disponibles. Si no ve el recurso:

1. Asegúrese de que ha seleccionado la suscripción correcta en el **suscripción** lista desplegable. Si su suscripción no aparece, haga clic en el **directorio + suscripción configuración** y agregar una suscripción con el recurso.

1. Asegúrese de que ha seleccionado el grupo de recursos correcto.
    > [!WARNING]
    > Para obtener el mejor rendimiento, al abrir el Explorador de métricas, el **grupo de recursos** desplegable no tiene ningún grupo de recursos seleccionado previamente. Debe seleccionar al menos un grupo para poder ver todos los recursos.

## <a name="chart-shows-no-data"></a>Gráfico no muestra ningún dato

En ocasiones, los gráficos no pueden mostrar ningún dato después de seleccionar las métricas y los recursos correctos. Este comportamiento puede deberse a varios de los siguientes motivos:

### <a name="microsoftinsights-resource-provider-isnt-registered-for-your-subscription"></a>Proveedor de recursos Microsoft.Insights no está registrado para la suscripción

Exploración de métricas requiere *Microsoft.Insights* registrado el proveedor de recursos en su suscripción. En muchos casos, se registra automáticamente (es decir, después de configurar una regla de alerta, personalizar la configuración de diagnóstico para cualquier recurso o configurar una regla de escalado automático). Si el proveedor de recursos Microsoft.Insights no está registrado, debe registrarlo en manualmente siguiendo los pasos descritos en [tipos y proveedores de recursos de Azure](../../azure-resource-manager/resource-manager-supported-services.md).

**Solución:** Abra **suscripciones**, **proveedores de recursos** pestaña y compruebe que *Microsoft.Insights* está registrado para su suscripción.

### <a name="you-dont-have-sufficient-access-rights-to-your-resource"></a>No tiene suficientes derechos de acceso al recurso

En Azure, el acceso a las métricas se controla mediante [control de acceso basado en roles (RBAC)](../../role-based-access-control/overview.md). Debe ser miembro del [lector de supervisión](../../role-based-access-control/built-in-roles.md#monitoring-reader), [colaborador de supervisión](../../role-based-access-control/built-in-roles.md#monitoring-contributor), o [colaborador](../../role-based-access-control/built-in-roles.md#contributor) para explorar las métricas de cualquier recurso.

**Solución:** Asegúrese de que tiene permisos suficientes para el recurso desde el que está explorando las métricas.

### <a name="your-resource-didnt-emit-metrics-during-the-selected-time-range"></a>El recurso no emite métricas durante el intervalo de tiempo seleccionado

Algunos recursos no emiten constantemente sus métricas. Por ejemplo, Azure no recopilará las métricas de máquinas virtuales detenidas. Otros recursos podrían emitir sus métricas solo cuando se produce alguna condición. Por ejemplo, una métrica que muestra el tiempo de procesamiento de una transacción requiere al menos una transacción. Si no hubiera ninguna transacción en el intervalo de tiempo seleccionado, el gráfico de forma natural estará vacío. Además, aunque la mayoría de las métricas en Azure se recopilan cada minuto, hay algunas que se recopilan con menos frecuencia. Consulte la documentación para obtener más detalles acerca de la métrica que desea explorar métrica.

**Solución:** Cambie la hora del gráfico a una gama más amplia. Puede iniciar desde "Últimos 30 días" mediante una mayor granularidad de tiempo (o basarse en la opción "granularidad de hora automática").

### <a name="you-picked-a-time-range-greater-than-30-days"></a>Seleccionar un intervalo de tiempo mayor que 30 días

[La mayoría de las métricas en Azure se almacenan para 93 días](data-platform-metrics.md#retention-of-metrics). Sin embargo, solo puede consultar para no más de 30 días de datos en un único gráfico. Esta limitación no se aplica a [métricas basadas en el registro](../app/pre-aggregated-metrics-log-metrics.md#log-based-metrics).

**Solución:** Si ve un gráfico en blanco o el gráfico sólo muestra parte de los datos métricos, compruebe que la diferencia entre start - y -fechas de finalización en el selector de hora no supera el intervalo de 30 días.

### <a name="all-metric-values-were-outside-of-the-locked-y-axis-range"></a>Todos los valores de métrica estaban fuera del intervalo del eje y bloqueado

Por [bloqueo de los límites del eje y del gráfico](metrics-charts.md#lock-boundaries-of-chart-y-axis), puede realizar involuntariamente el área de presentación de gráfico no muestra la línea del gráfico. Por ejemplo, si el eje y está bloqueado para un intervalo entre 0 y 50% y la métrica tiene un valor constante de 100%, la línea se representa siempre fuera del área visible, parezca que el gráfico en blanco.

**Solución:** Compruebe que no se bloquean los límites del eje y del gráfico fuera del intervalo de los valores de métrica. Si los límites del eje y se bloquean, desea restablecerlas temporalmente para asegurarse de que la métrica de valores no se encuentran fuera del rango del gráfico. Bloqueo del intervalo del eje y no se recomienda con granularidad automática para que los gráficos con **suma**, **min**, y **max** agregación porque sus valores cambiará con granularidad por cambiar el tamaño de la ventana del explorador o el paso de resolución de pantalla de uno a otro. Granularidad conmutación puede dejar el área de presentación de su vacío de gráfico.

### <a name="you-are-looking-at-a-guest-os-metric-but-didnt-enable-azure-diagnostic-extension"></a>Se trata de una métrica del SO invitado, pero no ha habilitado extensión de diagnóstico de Azure

Colección de **del SO invitado** métricas requieren la configuración de la extensión de diagnósticos de Azure o habilitarlo mediante el **configuración de diagnóstico** panel para el recurso.

**Solución:** Si está habilitada la extensión de diagnósticos de Azure, pero todavía no puede ver las métricas, siga los pasos descritos en [Guía de solución de problemas de extensión de diagnósticos de Azure](diagnostics-extension-troubleshooting.md#metric-data-doesnt-appear-in-the-azure-portal). Vea también los pasos de solución de problemas para [no se puede elegir el espacio de nombres de sistema operativo invitado y las métricas](metrics-troubleshoot.md#cannot-pick-guest-os-namespace-and-metrics)

## <a name="error-retrieving-data-message-on-dashboard"></a>Mensaje "Error al recuperar datos" en el panel

Este problema es común cuando se creó el panel con una métrica que estaba en desuso y se quita de Azure más adelante. Para comprobar que es el caso, abra el **métricas** pestaña del recurso y comprobación de las métricas disponibles en el selector de métricas. Si no se muestra la métrica, se quitó la métrica de Azure. Normalmente, cuando una métrica está en desuso, hay una métrica mejor nueva que brinda una perspectiva similar en el estado del recurso.

**Solución:** Actualizar el icono de error al seleccionar una métrica alternativa para el gráfico en el panel. También puede [revisar una lista de métricas disponibles para servicios de Azure](metrics-supported.md).

## <a name="chart-shows-dashed-line"></a>Gráfico muestra la línea discontinua

Gráficos de métricas de Azure utilizan el estilo de línea discontinua para indicar que hay un valor que falta (también conocido como "valor null") entre el grano de tiempo conocido dos puntos de datos. Por ejemplo, si en el selector de tiempo que ha elegido la granularidad de tiempo "minuto", pero la métrica se notificó en 07:26, 07:27, 07:29 y 07:30 (tenga en cuenta un intervalo entre puntos de datos segundo y tercero por minuto), a continuación, conectará una línea discontinua 07:27 y 07:29 y se conectará a una línea sólida todos los puntos de datos. Las entregas de línea discontinua hasta cero cuando se usa la métrica **recuento** y **suma** agregación. Para el **avg**, **min** o **max** agregaciones, la línea discontinua conecta dos puntos de datos conocidos más cercanos. Además, cuando falta los datos en el lado izquierdo o derecha del gráfico, la línea discontinua se expande a la dirección del punto de datos que faltan.
  ![imagen de métrica](./media/metrics-troubleshoot/missing-data-point-line-chart.png)

**Solución:** Este comportamiento es así por diseño. Resulta útil para identificar los puntos de datos que faltan. El gráfico de líneas es una opción superior para visualizar las tendencias de las métricas de alta densidad, pero puede ser difícil de interpretar las métricas con valores dispersos, especialmente cuando corelating valores con el detalle de tiempo es importante. La línea discontinua facilita la lectura de estos gráficos, pero si el gráfico está siendo claro, considere la posibilidad de ver las métricas con un tipo de gráfico diferente. Por ejemplo, un gráfico de dispersión de la misma métrica muestra claramente cada detalle de tiempo por solo visualización cuando hay un valor de un punto y omitir los datos de punto por completo cuando falta el valor: ![imagen de métrica](./media/metrics-troubleshoot/missing-data-point-scatter-chart.png)

   > [!NOTE]
   > Si prefiere un gráfico de líneas para la métrica, mover el mouse sobre el gráfico puede ayudarle a evaluar la granularidad de hora resaltando el punto de datos en la ubicación del puntero del mouse.

## <a name="chart-shows-unexpected-drop-in-values"></a>Gráfico muestra una caída inesperada en los valores

En muchos casos, la operación de colocar percibido en los valores de métrica es una falta de comprensión de los datos que se muestra en el gráfico. Puede se confunda con un descenso en sumas o recuentos cuando el gráfico muestra los minutos más recientes porque no se han recibido o procesado todavía Azure los últimos puntos de datos de métrica. Dependiendo del servicio, la latencia de las métricas de procesamiento puede ser dentro del intervalo unos minutos. Para los gráficos que muestran un intervalo de tiempo reciente con una granularidad de 1 o 5 minutos, se vuelve más evidente una disminución del valor a través de los últimos minutos: ![imagen de métrica](./media/metrics-troubleshoot/drop-in-values.png)

**Solución:** Este comportamiento es así por diseño. Creemos que es beneficioso que muestra los datos tan pronto como recibamos, incluso cuando los datos son *parcial* o *incompleta*. Si lo hace, podrá conclusión importante antes de realizar e iniciar inmediatamente la investigación. Por ejemplo, para una métrica que muestra el número de errores, aparece un valor parcial X indica que se produjeron al menos X errores en un momento determinado. Puede comenzar a investigar el problema inmediato, en lugar de esperar a ver el recuento exacto de los errores que se produjeron en este minuto, lo que podría no ser tan importante. El gráfico se actualizará una vez que se recibe todo el conjunto de datos, pero en ese momento pueden mostrar los nuevos puntos de datos incompletos desde minutos más recientes.

## <a name="cannot-pick-guest-os-namespace-and-metrics"></a>No se puede elegir las métricas y el espacio de nombres del SO invitado

Las máquinas virtuales y conjuntos de escalado de máquinas virtuales tienen dos categorías de métricas: **Host de máquina virtual** las métricas recopiladas por el entorno de hospedaje de Azure, y **del SO invitado** las métricas recopiladas por el [agente de supervisión](agents-overview.md) que se ejecutan en las máquinas virtuales. Instalar el agente de supervisión al habilitar [extensión de diagnóstico de Azure](diagnostics-extension-overview.md).

De forma predeterminada, las métricas de SO invitado se almacenan en la cuenta de almacenamiento de Azure, que elegir entre el **configuración de diagnóstico** pestaña del recurso. Si no se recopilan métricas de SO invitado o el Explorador de métricas no puede tener acceso a ellos, solo verá la **Host de máquina Virtual** espacio de nombres de métrica:

![imagen de métrica](./media/metrics-troubleshoot/cannot-pick-guest-os-namespace.png)

**Solución:** Si no ve **del SO invitado** espacio de nombres y las métricas en el Explorador de métricas:

1. Confirme que [extensión de diagnóstico de Azure](diagnostics-extension-overview.md) está habilitado y configurado para recopilar métricas.
    > [!WARNING]
    > No puede usar [agente de Log Analytics](agents-overview.md#log-analytics-agent) (también denominados el Microsoft Monitoring Agent, o "MMA") para enviar **del SO invitado** en una cuenta de almacenamiento.

1. Compruebe que la cuenta de almacenamiento no está protegida por el firewall.

1. Use la [Explorador de Azure storage](https://azure.microsoft.com/features/storage-explorer/) para validar que las métricas fluyen en la cuenta de almacenamiento. Si no se recopilan métricas, siga el [Guía de solución de problemas de extensión de diagnósticos de Azure](diagnostics-extension-troubleshooting.md#metric-data-doesnt-appear-in-the-azure-portal).

## <a name="next-steps"></a>Pasos siguientes

* [Obtenga información sobre cómo empezar con el Explorador de métricas](metrics-getting-started.md)
* [Obtenga información sobre las características avanzadas del explorador de métricas](metrics-charts.md)
* [Ver una lista de métricas disponibles para servicios de Azure](metrics-supported.md)
* [Vea los ejemplos de gráficos configurados](metric-chart-samples.md)