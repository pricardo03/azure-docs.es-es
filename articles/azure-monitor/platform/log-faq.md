---
title: Preguntas frecuentes de Log Analytics | Microsoft Docs
description: Respuestas a las preguntas frecuentes sobre el servicio Azure Log Analytics.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ad536ff7-2c60-4850-a46d-230bc9e1ab45
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 11/13/2018
ms.author: magoedte
ms.openlocfilehash: 08e915354df4f4aa1d9a183e78cbad47460b8d37
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66356221"
---
# <a name="log-analytics-faq"></a>Preguntas frecuentes sobre Log Analytics

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

En este artículo de preguntas frecuentes de Microsoft, se presenta una lista con las preguntas frecuentes sobre Log Analytics en Microsoft Azure. Si tiene alguna otra pregunta sobre Log Analytics, vaya al [foro de discusión](https://social.msdn.microsoft.com/Forums/azure/home?forum=opinsights) y publíquela. Si una pregunta es frecuente, se agrega a este artículo para que se pueda encontrar de forma rápida y sencilla.


## <a name="new-logs-experience"></a>Nueva experiencia de registros

### <a name="q-whats-the-difference-between-the-new-logs-experience-and-log-analytics"></a>P: ¿Cuál es la diferencia entre la nueva experiencia de registros y Log Analytics?

R: Son lo mismo. [Log Analytics se integra como una característica de Azure Monitor](../../azure-monitor/azure-monitor-rebrand.md) para proporcionar una experiencia de supervisión más unificada. La nueva experiencia de registros en Azure Monitor es exactamente igual que las consultas de Log Analytics que muchos clientes ya han usado.

### <a name="q-can-i-still-use-log-search"></a>P: ¿Puedo usar la búsqueda de registros? 

R: La búsqueda de registros todavía sigue estando disponible en el portal de OMS y en Azure Portal con el nombre **Registros (clásico)** . El portal de OMS se retirará oficialmente el 15 de enero de 2019. La experiencia de registros clásica en Azure Portal se retirará gradualmente y será reemplazada con la nueva experiencia de registros. 

### <a name="q-can-i-still-use-advanced-analytics-portal"></a>P: ¿puedo seguir usando el portal de análisis avanzado? 
La nueva experiencia de registros de Azure Portal se basa en el portal de análisis avanzado, pero todavía se tiene acceso a ella fuera de Azure Portal. Los pasos que se realizarán para retirar este portal externo se anunciará próximamente.

### <a name="q-why-cant-i-see-query-explorer-and-save-buttons-in-the-new-logs-experience"></a>P: ¿Por qué no puedo ver el Explorador de consultas y los botones Guardar en la nueva experiencia de registros?

Los botones **Explorador de consultas**, **Guardar** y **Establecer alerta** no están disponibles cuando se exploran los registros en el contexto de un recurso específico. Para crear alertas o guardar o cargar una consulta, los registros se deben enfocar a un área de trabajo. Para abrir los registros en el contexto del área de trabajo, seleccione **Todos los servicios** > **Supervisar** > **Registros**. Se selecciona la última área de trabajo usada, pero puede seleccionar cualquier otra área de trabajo. Consulte [Visualización y análisis de datos en Log Analytics](../log-query/portals.md) para obtener más información.

### <a name="q-how-do-i-extract-custom-fields-in-the-new-logs-experience"></a>P: ¿cómo puedo extraer los campos personalizados en la nueva experiencia de registros? 

R: La opción de extracción de campos personalizados se admiten actualmente en la experiencia clásica de registros. 

### <a name="q-where-do-i-find-list-view-in-the-new-logs"></a>P: ¿dónde encuentro la vista de lista en los nuevos registros? 

R: La vista de lista no está disponible en los nuevos registros. Hay una flecha a la izquierda de cada registro en la tabla de resultados. Haga clic en ella para abrir los detalles del registro en concreto. 

### <a name="q-after-running-a-query-a-list-of-suggested-filters-are-available-how-can-i-see-filters"></a>P: Después de ejecutar una consulta, aparece una lista de filtros sugeridos. ¿Cómo puedo verlos? 

R: Haga clic en "Filtros" en el panel izquierdo para obtener una versión preliminar de la nueva implementación de filtros. Esto ahora se basa en su conjunto de resultados completo, en lugar de estar limitado por el límite de 10 000 registros de la interfaz de usuario. Esta es actualmente una lista de los filtros más populares y los 10 valores más comunes para cada filtro. 

### <a name="q-why-am-i-getting-the-error-register-resource-provider-microsoftinsights-for-this-subscription-to-enable-this-query-in-logs-after-drilling-in-from-vm"></a>P: ¿Por qué recibo el error: "Debe registrar el proveedor de recursos 'Microsoft.Insights' de esta suscripción para habilitar esta consulta" en los registros, después de obtener detalles de la máquina virtual? 

R: De forma predeterminada, muchos proveedores de recursos se registran automáticamente. Sin embargo, debe registrar manualmente algunos de ellos. Esta opción configura su suscripción para así poder trabajar con el proveedor de recursos. El ámbito de registro es siempre la suscripción. Para más información, consulte [Proveedores de recursos y sus tipos](../../azure-resource-manager/resource-manager-supported-services.md#azure-portal).

### <a name="q-why-am-i-am-getting-no-access-error-message-when-accessing-logs-from-a-vm-page"></a>P: ¿por qué no recibo un mensaje de error de acceso cuando accedo a los registros desde una página de máquina virtual? 

R: Para ver los registros de la VM, debe tener permiso de lectura para aquellos espacios de trabajo que almacenan los registros de VM. En esos casos, su administrador debe otorgarle permisos en Azure.

### <a name="q-why-can-i-can-access-my-workspace-in-oms-portal-but-i-get-the-error-you-have-no-access-in-the-azure-portal"></a>P: ¿por qué puedo acceder a mi espacio de trabajo en el portal de OMS, pero aparece el error "No tiene acceso" en Azure Portal?  

R: Para obtener acceso a un área de trabajo en Azure, debe tener asignados permisos de Azure. Hay algunos casos donde es posible que no tenga los permisos de acceso adecuados. En ese caso, el administrador debe otorgarle permisos en Azure.Consulte [Traslado del portal de OMS a Azure](oms-portal-transition.md) para obtener más información.

### <a name="q-why-cant-i-cant-see-view-designer-entry-in-logs"></a>P: ¿por qué no puedo ver la entrada del Diseñador de vistas en los registros? 
R: El Diseñador de vistas solo está disponible en los registros de los usuarios asignados que tengan permiso de colaborador o superior.

### <a name="q-can-i-still-use-the-analytics-portal-outside-of-azure"></a>P: ¿Puedo seguir usando el portal de Analytics fuera de Azure?
A. Sí, la página de registros de Azure y el portal de análisis avanzado se basan en el mismo código. Log Analytics se integra como una característica de Azure Monitor para proporcionar una experiencia de supervisión más unificada. Todavía puede acceder al portal de Analytics mediante la dirección URL: https:\/\/portal.loganalytics.io/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/workspaces/{workspaceName}.



## <a name="general"></a>General

### <a name="q-how-can-i-see-my-views-and-solutions-in-azure-portal"></a>P: ¿cómo puedo ver mis vistas y soluciones en Azure Portal? 

R: La lista de vistas y soluciones instaladas está disponible en Azure Portal. Haga clic en **Todos los servicios**. En la lista de recursos, seleccione **Supervisar** y haga clic en **...Más**. Se selecciona la última área de trabajo usada, pero puede seleccionar cualquier otra área de trabajo. 

### <a name="q-why-i-cant-create-workspaces-in-west-central-us-region"></a>P: ¿por qué no puedo crear áreas de trabajo en la región Centro-oeste de EE. UU.? 

R: Esta región tiene un límite de capacidad temporal. Se espera que se solucione este límite a finales de septiembre de 2019.


### <a name="q-does-log-analytics-use-the-same-agent-as-azure-security-center"></a>P: ¿Log Analytics utiliza al mismo agente que Azure Security Center?

R: A principios de junio de 2017, Azure Security Center comenzó a usar Microsoft Monitoring Agent para recopilar y almacenar datos. Para más información, consulte [Preguntas frecuentes sobre la migración de la plataforma de Azure Security Center](../../security-center/security-center-enable-data-collection.md).

### <a name="q-what-checks-are-performed-by-the-ad-and-sql-assessment-solutions"></a>P: ¿Qué comprobaciones llevan a cabo las soluciones AD y SQL Assessment?

R: La consulta siguiente muestra una descripción de todas las comprobaciones que se llevan a cabo actualmente:

```
(Type=SQLAssessmentRecommendation OR Type=ADAssessmentRecommendation) | dedup RecommendationId | select FocusArea, ActionArea, Recommendation, Description | sort Type, FocusArea,ActionArea, Recommendation
```

Los resultados pueden exportarse a Excel para su revisión.

### <a name="q-why-do-i-see-something-different-than-oms-in-the-system-center-operations-manager-console"></a>P: P: ¿por qué aparece algo distinto a OMS en la consola de System Center Operations Manager?

R: En función del paquete acumulativo de actualizaciones de Operations Manager que use, es posible que vea un nodo para *System Center Advisor*, *Operational Insights* o *Log Analytics*.

La actualización de la cadena de texto de *OMS* se incluye en un módulo de administración, que debe importarse manualmente. Para ver el texto y la funcionalidad actuales, siga las instrucciones del último artículo de KB sobre el paquete acumulativo de actualizaciones de System Center Operations Manager y actualice la consola.

### <a name="q-is-there-an-on-premises-version-of-log-analytics"></a>P: ¿Existe alguna versión local de Log Analytics?

R:  No. Log Analytics es un servicio en la nube escalable que procesa y guarda grandes cantidades de datos. 

### <a name="q-how-do-i-troubleshoot-if-log-analytics-is-no-longer-collecting-data"></a>P: ¿Cómo se puede solucionar que Log Analytics ya no recopile datos?

R: En el caso de una suscripción y un área de trabajo creadas antes del 2 de abril de 2018, es decir, con el plan de tarifa *gratuito*, si se envían más de 500 MB de datos en el mismo día, la recopilación de datos se detiene el resto del día. Alcanzar el límite diario es un motivo frecuente de que Log Analytics deje de recopilar datos o de que parezca que faltan datos.  

Log Analytics crea un evento de tipo *Latido* y puede utilizarse para determinar si la recopilación de datos se detiene. 

Ejecute la siguiente consulta en la búsqueda para comprobar si ha alcanzado el límite diario y faltan datos: `Heartbeat | summarize max(TimeGenerated)`

Para comprobar un equipo específico, ejecute la consulta siguiente: `Heartbeat | where Computer=="contosovm" | summarize max(TimeGenerated)`

Cuando se detenga la recopilación de datos, según el intervalo de tiempo seleccionado, no verá ningún registro devuelto.   

La tabla siguiente describe los motivos por los que se detiene la recopilación de datos y una acción recomendada para reanudarla:

| Motivo por el que se detiene la recopilación de datos                       | Para reanudar la recopilación de datos |
| -------------------------------------------------- | ----------------  |
| Se ha alcanzado el límite de datos gratuitos<sup>1</sup>       | Espere hasta el mes siguiente para que la recopilación se reinicie automáticamente, o bien<br> Cambie a un plan de tarifa de pago |
| La suscripción de Azure está en estado suspendido debido a: <br> Prueba gratuita finalizada <br> Pase para Azure expirado <br> Se ha alcanzado el límite de gasto mensual (por ejemplo, en una suscripción de MSDN o Visual Studio)                          | Cambie a una suscripción de pago <br> Cambie a una suscripción de pago <br> Quite el límite o espere a que se restablezca |

<sup>1</sup> Si el área de trabajo tiene el plan de tarifa *gratuito*, el envío de datos diario se limita a 500 MB. Cuando se alcanza el límite diario, la recopilación de datos se detiene hasta el día siguiente. Los datos enviados mientras la recopilación de datos está detenida no se indexan ni están disponibles para las búsquedas. Cuando la recopilación de datos se reanuda, únicamente se procesan los nuevos datos enviados. 

Log Analytics usa la hora UTC y cada día se inicia a medianoche de esta hora. Si el área de trabajo alcanza el límite diario, el procesamiento se reanuda durante la primera hora del siguiente día UTC.

### <a name="q-how-can-i-be-notified-when-data-collection-stops"></a>P: ¿Cómo puedo recibir una notificación cuando se detiene la recopilación de datos?

R: Siga los pasos explicados en [Crear una nueva alerta de registro](../../azure-monitor/platform/alerts-metric.md) para recibir una notificación cuando se detenga la recopilación de datos.

Al crear la alerta para cuando se detenga la recopilación de datos, establezca:

- **Definición de la condición de alerta**: especifique el área de trabajo de Log Analytics como el destino del recurso.
- **Criterios de alerta**: especifique lo siguiente:
   - **Nombre de señal**: seleccione **Custom log search** (Búsqueda de registros personalizada).
   - **Consulta de búsqueda** en `Heartbeat | summarize LastCall = max(TimeGenerated) by Computer | where LastCall < ago(15m)`
   - **Lógica de alerta** está **basada en** *número de resultados* y **Condición** es *Mayor que* un **umbral**  de *0*
   - **Período de tiempo** de *30* minutos y **Frecuencia de alerta** cada *10* minutos
- **Definición de los detalles de la alerta**: especifique lo siguiente:
   - **Nombre** en *Recopilación de datos detenida*
   - **Gravedad** en *Advertencia*

Especifique un [Grupo de acciones](../../azure-monitor/platform/action-groups.md) existente o cree uno nuevo para que cuando la alerta de registro coincida con los criterios, se le notifique si faltan latidos durante más de 15 minutos.

## <a name="configuration"></a>Configuración
### <a name="q-can-i-change-the-name-of-the-tableblob-container-used-to-read-from-azure-diagnostics-wad"></a>P: ¿Se puede cambiar el nombre del contenedor de blobs o tablas usado para leer desde Azure Diagnostics (WAD)?

A. No, en este momento no es posible leer de tablas o contenedores arbitrarios en Azure Storage.

### <a name="q-what-ip-addresses-does-the-log-analytics-service-use-how-do-i-ensure-that-my-firewall-only-allows-traffic-to-the-log-analytics-service"></a>P: ¿Qué direcciones IP usa el servicio Log Analytics? ¿Cómo me aseguro de que el firewall solo permita el tráfico hacia el servicio Log Analytics?

A. El servicio Log Analytics se basa en Azure. Las direcciones IP de Log Analytics están en [Microsoft Azure Datacenter IP Ranges (Intervalos de IP de Microsoft Azure Datacenter)](https://www.microsoft.com/download/details.aspx?id=41653).

Cuando se realizan implementaciones de servicios, cambian las direcciones IP reales del servicio Log Analytics. Los nombres DNS que se permiten en el firewall se documentan en los [requisitos de red](../../azure-monitor/platform/log-analytics-agent.md#network-firewall-requirements).

### <a name="q-i-use-expressroute-for-connecting-to-azure-does-my-log-analytics-traffic-use-my-expressroute-connection"></a>P: Uso ExpressRoute para realizar la conexión con Azure. ¿El tráfico de Log Analytics usa la conexión ExpressRoute?

A. En la [documentación de ExpressRoute](../../expressroute/expressroute-faqs.md#supported-services), se describen los distintos tipos de tráfico de dicho servicio.

El tráfico dirigido a Log Analytics utiliza el circuito de ExpressRoute de emparejamiento público.

### <a name="q-is-there-a-simple-and-easy-way-to-move-an-existing-log-analytics-workspace-to-another-log-analytics-workspaceazure-subscription"></a>P: ¿Hay alguna manera fácil y directa de transferir un área de trabajo existente de Log Analytics a otra suscripción de Azure o a otra área de trabajo de Log Analytics?

A. El cmdlet `Move-AzResource` permite mover un área de trabajo de Log Analytics y también una cuenta de Automation de una suscripción de Azure a otra. Para obtener más información, consulte [Move-AzResource](https://msdn.microsoft.com/library/mt652516.aspx).

También es posible realizar este cambio en el Portal de Azure.

No se pueden mover datos de un área de trabajo de Log Analytics a otra, ni cambiar la región en la que se almacenan los datos de dicha solución.

### <a name="q-how-do-i-add-log-analytics-to-system-center-operations-manager"></a>P: ¿Cómo se agrega Log Analytics a System Center Operations Manager?

R:  La actualización al paquete acumulativo de actualizaciones más reciente y la importación de módulos de administración permiten conectar Operations Manager a Log Analytics.

>[!NOTE]
>La conexión de Operations Manager con Log Analytics solo está disponible para System Center Operations Manager 2012 SP1 y posterior.

### <a name="q-how-can-i-confirm-that-an-agent-is-able-to-communicate-with-log-analytics"></a>P: ¿Cómo puedo confirmar la comunicación entre un agente y Log Analytics?

R: Para asegurarse de que el agente pueda comunicarse con OMS, vaya a: Panel de Control, Configuración de seguridad, **Microsoft Monitoring Agent**.

En la pestaña **Azure Log Analytics (OMS)** , busque un icono de marca de verificación verde. Un icono de marca de verificación verde confirma que el agente puede comunicarse con el servicio de Azure.

Por su parte, un icono de advertencia amarillo significa que el agente está teniendo problemas de comunicación con Log Analytics. Una causa habitual es que el servicio Microsoft Monitoring Agent se ha detenido. Use el Administrador de control de servicios para reiniciar el servicio.

### <a name="q-how-do-i-stop-an-agent-from-communicating-with-log-analytics"></a>P: ¿Cómo detener la comunicación entre un agente y Log Analytics?

R: En System Center Operations Manager, quite el equipo de la lista de equipos que administra OMS. Operations Manager actualiza la configuración del agente para que ya no informe a Log Analytics. En el caso de los agentes conectados directamente a Log Analytics, puede detener la comunicación mediante: Panel de Control, Configuración de seguridad, **Microsoft Monitoring Agent**.
Quite todas las áreas de trabajo enumeradas en **Azure Log Analytics (OMS)** .

### <a name="q-why-am-i-getting-an-error-when-i-try-to-move-my-workspace-from-one-azure-subscription-to-another"></a>P: ¿Por qué recibo un error al tratar de mover mi área de trabajo de una suscripción de Azure a otra?

R: Para trasladar un área de trabajo a otra suscripción o grupo de recursos, debe primero desvincular la cuenta de Automation del área de trabajo. Desvincular una cuenta de Automation requiere la eliminación de estas soluciones si están instaladas en el área de trabajo: Update Management, Change Tracking o Start/Stop VMs during off-hours. Después de quitar estas soluciones, desvincule la cuenta de Automation seleccionando **Linked workspaces** (Áreas de trabajo vinculadas) en el panel izquierdo, en el recurso de cuenta de Automation, y haga clic en **Unlink workspace** (Desvincular área de trabajo) en la cinta.
 > Las soluciones que se han quitado se deben volver a instalar en el área de trabajo y se debe restablecer el vínculo de Automation al área de trabajo después del traslado.

Asegúrese de tener permisos en ambas suscripciones de Azure.

### <a name="q-why-am-i-getting-an-error-when-i-try-to-update-a-savedsearch"></a>P: ¿Por qué recibo un error al intentar actualizar un SavedSearch?

R: Tiene que agregar "etag" en el cuerpo de la API o las propiedades de plantilla de Azure Resource Manager:
```
"properties": {
   "etag": "*",
   "query": "SecurityEvent | where TimeGenerated > ago(1h) | where EventID == 4625 | count",
   "displayName": "An account failed to log on",
   "category": "Security"
}
```

## <a name="agent-data"></a>Datos del agente
### <a name="q-how-much-data-can-i-send-through-the-agent-to-log-analytics-is-there-a-maximum-amount-of-data-per-customer"></a>P: ¿Qué cantidad de datos puedo enviar a Log Analytics a través del agente? ¿Hay una cantidad máxima de datos por cliente?
A. El plan gratuito establece un límite diario de 500 MB por área de trabajo. Los planes estándar y premium carecen de límites en cuanto a la cantidad de datos cargada. Como servicio en la nube, Log Analytics se ha concebido para escalarse de forma automática con el fin de hacer frente al volumen procedente de un cliente (aun cuando se trate de varios terabytes diarios).

El agente de Log Analytics se ha diseñado de modo que se garantice que tenga una superficie mínima. El volumen de datos varía en función de las soluciones que se habiliten. Puede encontrar detalles sobre el volumen de datos y ver el desglose por solución en la página [Uso](../../azure-monitor/platform/data-usage.md).

Para más información, puede leer un [blog de cliente](https://thoughtsonopsmgr.blogspot.com/2015/09/one-small-footprint-for-server-one.html) que muestra los resultados después de evaluar el uso de recursos (consumo) del agente OMS.

### <a name="q-how-much-network-bandwidth-is-used-by-the-microsoft-management-agent-mma-when-sending-data-to-log-analytics"></a>P: ¿Cuánto ancho de banda de red usa el Agente de administración de Microsoft (MMA) al enviar datos a Log Analytics?

A. El ancho de banda equivale a una función de la cantidad de datos enviados. Los datos se comprimen al enviarse por la red.

### <a name="q-how-much-data-is-sent-per-agent"></a>P: ¿Qué cantidad de datos se envía por agente?

A. La cantidad de datos enviada por agente depende de:

* Las soluciones habilitadas
* El número de registros y contadores de rendimiento recopilados
* El volumen de datos de los registros

El plan de tarifas gratuito constituye una buena forma de incorporar varios servidores y estimar el volumen de datos típico. El uso general se muestra en la página [Uso](../../azure-monitor/platform/data-usage.md) .

En el caso de los equipos capaces de ejecutar el agente de WireData, use la siguiente consulta para ver la cantidad de datos enviada:

```
Type=WireData (ProcessName="C:\\Program Files\\Microsoft Monitoring Agent\\Agent\\MonitoringHost.exe") (Direction=Outbound) | measure Sum(TotalBytes) by Computer
```

## <a name="next-steps"></a>Pasos siguientes
* [Introducción a Log Analytics](../../azure-monitor/overview.md) , encontrará más información sobre esta solución y cómo empezar a utilizarla en cuestión de minutos.
