---
title: Registro y auditoría de Azure | Microsoft Docs
description: Aprenda a usar datos de registro para obtener un conocimiento más profundo sobre su aplicación.
services: security
documentationcenter: na
author: UnifyCloud
manager: barbkess
editor: TomSh
ms.assetid: ''
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/14/2019
ms.author: TomSh
ms.openlocfilehash: 80f90f1788e798261f77bb7a4147763e7ca6cec0
ms.sourcegitcommit: 124c3112b94c951535e0be20a751150b79289594
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/10/2019
ms.locfileid: "68946506"
---
# <a name="azure-logging-and-auditing"></a>Registro y auditoría de Azure

Azure proporciona una amplia gama de opciones de registro y auditoría de seguridad configurables para ayudarle a identificar vacíos en las directivas y mecanismos de seguridad. Este artículo describe la generación, recopilación y análisis de los registros de seguridad provenientes de los servicios hospedados en Azure.

> [!Note]
> Algunas de las recomendaciones de este artículo pueden provocar un aumento del uso de datos, de la red o de los recursos de proceso, lo que podría incrementar los costos de las licencias o las suscripciones.

## <a name="types-of-logs-in-azure"></a>Tipos de registros de Azure

Las aplicaciones en la nube son complejas y tienen muchas partes en movimiento. Los registros proporcionan datos para ayudar a mantener las aplicaciones en ejecución. Los registros ayudan a solucionar problemas pasados y prevenir los potenciales. También pueden ayudarle a mejorar el rendimiento o mantenimiento de la aplicación o a automatizar acciones que de lo contrario requerirían intervención manual.

Los registros de Azure se clasifican en los tipos siguientes:
* Los **registros de control y administración** proporcionan información sobre las operaciones CREATE, UPDATE y DELETE de Azure Resource Manager. Para más información, consulte [Registros de actividad de Azure](../../azure-monitor/platform/activity-logs-overview.md).

* Los **registros del plano de datos** proporcionan información sobre eventos desencadenados como parte del uso de los recursos de Azure. Ejemplos de este tipo de registro son los registros de eventos del sistema de Windows, de seguridad y de aplicaciones en una máquina virtual, así como los [registros de diagnóstico](../../azure-monitor/platform/diagnostic-logs-overview.md) que se han configurado mediante Azure Monitor.

* Los **eventos procesados** proporcionan información sobre eventos y alertas analizados que se han procesado en su nombre. Ejemplos de este tipo son las [alertas de Azure Security Center](../../security-center/security-center-managing-and-responding-alerts.md), donde [Azure Security Center](../../security-center/security-center-intro.md) ha procesado y analizado su suscripción y proporciona unas alertas de seguridad concisas.

En la tabla siguiente se enumeran los tipos más importante de registros disponibles en Azure.

| Categoría del registro | Tipo de registro | Uso | Integración |
| ------------ | -------- | ------ | ----------- |
|[Registros de actividad](../../azure-monitor/platform/activity-logs-overview.md)|Eventos de plano de control de los recursos de Azure Resource Manager|  Proporciona información detallada sobre las operaciones que se realizaron en los recursos de la suscripción.|    API REST y [Azure Monitor](../../azure-monitor/platform/activity-logs-overview.md)|
|[Registros de Azure Diagnostics](../../azure-monitor/platform/diagnostic-logs-overview.md)|Datos frecuentes acerca del funcionamiento de los recursos de Azure Resource Manager de la suscripción|  Proporciona información detallada sobre las operaciones que el mismo recurso realiza.| Azure Monitor, [Stream](../../azure-monitor/platform/diagnostic-logs-overview.md)|
|[Informes de Azure AD](../../active-directory/reports-monitoring/overview-reports.md)|Registros e informes | Informa sobre las actividades de inicio de sesión de usuario e información de actividades del sistema acerca de la administración de grupos y usuarios.|[Graph API](../../active-directory/develop/active-directory-graph-api-quickstart.md)|
|[Máquinas virtuales y servicios en la nube](../../azure-monitor/learn/quick-collect-azurevm.md)|Servicio de Registro de eventos de Windows y Syslog de Linux|  Captura los datos del sistema y los datos de registro en las máquinas virtuales, y transfiere estos datos a la cuenta de almacenamiento que elija.|   Windows con [WAD](../../monitoring-and-diagnostics/azure-diagnostics.md) (almacenamiento de Windows Azure Diagnostics) y Linux en Azure Monitor|
|[Análisis de Azure Storage](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics)|El registro de almacenamiento proporciona datos de métricas de una cuenta de almacenamiento|Proporciona información detallada sobre seguimiento de solicitudes, análisis de tendencias de uso y diagnóstico de problemas con la cuenta de almacenamiento.|   API de REST o [biblioteca de cliente](https://msdn.microsoft.com/library/azure/mt347887.aspx)|
|[Registros de flujo de los grupos de seguridad de red (NSG)](../../network-watcher/network-watcher-nsg-flow-logging-overview.md)|Tiene formato JSON y muestra flujos entrantes y salientes por cada regla|Muestra información sobre el tráfico IP de entrada y salida a través de un grupo de seguridad de red.|[Azure Network Watcher](../../network-watcher/network-watcher-monitoring-overview.md)|
|[Application Insights](../../azure-monitor/app/app-insights-overview.md)|Registros, excepciones y diagnósticos personalizados|  Proporciona un servicio de supervisión de rendimiento de aplicaciones (APM) para desarrolladores web en varias plataformas.| API de REST, [Power BI](https://powerbi.microsoft.com/documentation/powerbi-azure-and-power-bi/)|
|Datos de proceso y alertas de seguridad|    Alertas de Azure Security Center, alertas de registro de Azure Monitor|    Proporciona información y alertas de seguridad.|  API de REST, JSON|

### <a name="activity-logs"></a>Registros de actividad

Los [registros de actividad de Azure](../../azure-monitor/platform/activity-logs-overview.md) proporcionan información detallada sobre las operaciones que se realizaron en los recursos de la suscripción. Los registros de actividad se conocían antes como "Registros de auditoría" o "Registros operativos", ya que notifican [eventos del plano de control](https://driftboatdave.com/2016/10/13/azure-auditing-options-for-your-custom-reporting-needs/) para las suscripciones. 

Los registros de actividad le ayudan a determinar el "qué, quién y cuándo" para las operaciones de escritura (es decir, PUT, POST o DELETE). Los registros de actividad también le ayudan a conocer el estado de la operación y otras propiedades apropiadas. Los registros de actividad no incluyen operaciones de lectura (GET).

En este artículo, PUT, POST y DELETE hacen referencia a todas las operaciones de escritura que el registro de actividad contiene sobre los recursos. Por ejemplo, puede usar los registros de actividad para encontrar errores al solucionar problemas o para supervisar cómo ha modificado un recurso un usuario de su organización.

![Diagrama del registro de actividad](./media/log-audit/azure-log-audit-fig1.png)

Puede recuperar los eventos del registro de actividad mediante Azure Portal, la [CLI de Azure](../../storage/common/storage-azure-cli.md), los cmdlets de PowerShell y la [API REST de Azure Monitor](../../azure-monitor/platform/rest-api-walkthrough.md). Los registros de actividad tienen un período de retención de datos de noventa días.

Escenarios de integración para un evento del registro de actividad:

* [Crear una alerta de correo electrónico o webhook que se desencadene con un evento del registro de actividad](../../monitoring-and-diagnostics/monitor-alerts-unified-log-webhook.md).

* [Transmitirlo al Centro de eventos](../../azure-monitor/platform/activity-logs-stream-event-hubs.md) para la ingesta en un servicio de terceros o una solución de análisis personalizado como Power BI.

* Analizarlo en Power BI con el [paquete de contenido de PowerBI](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs/).

* [Guardarlo en una cuenta de almacenamiento para su archivo o inspección manual](../../azure-monitor/platform/archive-activity-log.md). Puede especificar el tiempo de retención (en días) mediante perfiles de registro.

* Verlo y realizar consultas en él en Azure Portal.

* Puede consultarlo mediante un cmdlet de PowerShell, la CLI de Azure o la API REST.

* Exportar el registro de actividad con perfiles de registro a [registros de Azure Monitor](../../log-analytics/log-analytics-queries.md).

Puede usar una cuenta de almacenamiento o un [espacio de nombres de centro de eventos](../../event-hubs/event-hubs-resource-manager-namespace-event-hub-enable-capture.md) que no esté en la misma suscripción que la que emite el registro. El usuario que configura los ajustes debe tener el acceso del [control de acceso basado en rol (RBAC)](../../role-based-access-control/role-assignments-portal.md) adecuado para ambas suscripciones.

### <a name="azure-diagnostics-logs"></a>Registros de Azure Diagnostics

Un recurso emite registros de diagnóstico de Azure que proporcionan datos exhaustivos y frecuentes acerca del funcionamiento de ese recurso. El contenido de estos registros varía según el tipo de recurso. Por ejemplo, los [registros del sistema de eventos de Windows](../../azure-monitor/platform/data-sources-windows-events.md) son una categoría de registro de diagnóstico para máquinas virtuales y los [registros de blob, tabla y cola](../../storage/common/storage-monitor-storage-account.md) son categorías de los registros de diagnóstico para cuentas de almacenamiento. Los registros de diagnóstico son diferentes de los registros de actividad, que proporcionan información sobre las operaciones que se han realizado en los recursos de la suscripción.

![Diagramas de los registros de diagnóstico de Azure](./media/log-audit/azure-log-audit-fig2.png)

Los registros de diagnóstico de Azure ofrecen varias opciones de configuración, como Azure Portal, PowerShell, la CLI de Azure y la API REST.

**Escenarios de integración**

* Guardarlos en una [cuenta de almacenamiento](../../azure-monitor/platform/archive-diagnostic-logs.md) para auditarlos o para inspeccionarlos manualmente. Puede especificar el tiempo de retención (en días) mediante la configuración de diagnóstico.

* [Transmitirlos a Event Hubs](../../azure-monitor/platform/diagnostic-logs-stream-event-hubs.md) para la ingesta en un servicio de terceros o una solución de análisis personalizado como [Power BI](https://powerbi.microsoft.com/documentation/powerbi-azure-and-power-bi/).

* Analícelos con [registros de Azure Monitor](../../log-analytics/log-analytics-queries.md).

**Servicios admitidos, esquema de los registros de diagnóstico y categorías de registro admitidas por tipo de recurso**


| Servicio | Esquema y documentación | Tipo de recurso | Categoría |
| ------- | ------------- | ------------- | -------- |
|Azure Load Balancer| [Registros de Azure Monitor para Load Balancer (versión preliminar)](../../load-balancer/load-balancer-monitor-log.md)|Microsoft.Network/loadBalancers<br>Microsoft.Network/loadBalancers|    LoadBalancerAlertEvent<br>LoadBalancerProbeHealthStatus|
|Grupos de seguridad de red|[Registros de Azure Monitor para grupos de seguridad de red](../../virtual-network/virtual-network-nsg-manage-log.md)|Microsoft.Network/networksecuritygroups<br>Microsoft.Network/networksecuritygroups|NetworkSecurityGroupEvent<br>NetworkSecurityGroupRuleCounter|
|Azure Application Gateway|[Registro de diagnóstico para Application Gateway](../../application-gateway/application-gateway-diagnostics.md)|Microsoft.Network/applicationGateways<br>Microsoft.Network/applicationGateways<br>Microsoft.Network/applicationGateways|ApplicationGatewayAccessLog<br>ApplicationGatewayPerformanceLog<br>ApplicationGatewayFirewallLog|
|Azure Key Vault|[Registros de Key Vault](../../key-vault/key-vault-logging.md)|Microsoft.KeyVault/vaults|AuditEvent|
|Azure Search|[Habilitación y uso de Análisis de tráfico de búsqueda](../../search/search-traffic-analytics.md)|Microsoft.Search/searchServices|OperationLogs|
|Azure Data Lake Store|[Acceso a los registros de diagnóstico de Azure Data Lake Store](../../data-lake-store/data-lake-store-diagnostic-logs.md)|Microsoft.DataLakeStore/accounts<br>Microsoft.DataLakeStore/accounts|Auditoría<br>Requests|
|Análisis con Azure Data Lake|[Acceso a los registros de diagnóstico de Azure Data Lake Analytics](../../data-lake-analytics/data-lake-analytics-diagnostic-logs.md)|Microsoft.DataLakeAnalytics/accounts<br>Microsoft.DataLakeAnalytics/accounts|Auditoría<br>Requests|
|Azure Logic Apps|[Esquema de seguimiento personalizado de Logic Apps B2B](../../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md)|Microsoft.Logic/workflows<br>Microsoft.Logic/integrationAccounts|WorkflowRuntime<br>IntegrationAccountTrackingEvents|
|Azure Batch|[Registros de diagnóstico de Azure Batch](../../batch/batch-diagnostics.md)|Microsoft.Batch/batchAccounts|ServiceLog|
|Azure Automation|[Registros de Azure Monitor para Azure Automation](../../automation/automation-manage-send-joblogs-log-analytics.md)|Microsoft.Automation/automationAccounts<br>Microsoft.Automation/automationAccounts|JobLogs<br>JobStreams|
|Azure Event Hubs|[Registros de diagnóstico de Event Hubs](../../event-hubs/event-hubs-diagnostic-logs.md)|Microsoft.EventHub/namespaces<br>Microsoft.EventHub/namespaces|ArchiveLogs<br>OperationalLogs|
|Azure Stream Analytics|[Registros de diagnóstico de trabajos](../../stream-analytics/stream-analytics-job-diagnostic-logs.md)|Microsoft.StreamAnalytics/streamingjobs<br>Microsoft.StreamAnalytics/streamingjobs|Ejecución<br>Creación|
|Azure Service Bus|[Registros de diagnóstico de Service Bus](../../service-bus-messaging/service-bus-diagnostic-logs.md)|Microsoft.ServiceBus/namespaces|OperationalLogs|

### <a name="azure-active-directory-reporting"></a>Informes de Azure Active Directory

Azure Active Directory (Azure AD) incluye informes de seguridad, actividad y auditoría del directorio de un usuario. El [informe de auditoría de Azure AD](../../active-directory/active-directory-reporting-azure-portal.md) le ayuda a identificar acciones con privilegios que han ocurrido en la instancia de Azure AD del usuario. Las acciones con privilegios incluyen cambios de elevación (por ejemplo, creación de roles o restablecimientos de contraseña), cambios de configuraciones de directiva (por ejemplo, directivas de contraseña) o cambios de configuración de directorio (por ejemplo, cambios en la configuración de la federación de dominio).

Los informes proporcionan el registro de auditoría con el nombre del evento, el usuario que realizó la acción, el recurso de destino que se ve afectado por el cambio y la fecha y hora (en formato UTC). Los usuarios pueden recuperar la lista de eventos de auditoría de Azure AD desde [Azure Portal](https://portal.azure.com/), como se describe en [Visualización de los registros de auditoría](../../active-directory/reports-monitoring/overview-reports.md). 

Los informes incluidos se enumeran en la siguiente tabla:

| Informes de seguridad | Informes de actividad | Informes de auditoría |
| :--------------- | :--------------- | :------------ |
|Inicios de sesión desde orígenes desconocidos| Uso de la aplicación: resumen| Informe de auditoría de directorio|
|Inicios de sesión tras varios errores|  Uso de la aplicación: detallado||
|Inicios de sesión desde varias ubicaciones geográficas|    Panel de la aplicación||
|Inicios de sesión desde direcciones IP con actividad sospechosa|   Errores de aprovisionamiento de cuentas||
|Actividad de inicio de sesión irregular|    Dispositivos de usuario individuales||
|Inicios de sesión desde dispositivos posiblemente infectados|   Actividad de usuario individual||
|Usuarios con actividad de inicio de sesión anómala| Informe de actividad de grupos||
||Informe de actividad de registro de restablecimiento de contraseña||
||Actividad de restablecimiento de contraseña||

Los datos de estos informes pueden resultar útiles para las aplicaciones, como los sistemas de administración de información y eventos de seguridad (SIEM), las auditorías y las herramientas de inteligencia empresarial. Las API de generación de informes de Azure AD proporcionan acceso mediante programación a los datos a través de un conjunto de API de REST. Se puede llamar a estas [API](../../active-directory/active-directory-reporting-api-getting-started-azure-portal.md) desde diversos lenguajes de programación y herramientas.

La retención de eventos del informe de auditoría de Azure AD varía entre siete y noventa días según el tipo de licencia asociada al inquilino. 

> [!Note]
> Para más información acerca de la retención de los informes, consulte [Directivas de retención de informes de Azure AD](../../active-directory/reports-monitoring/reference-reports-data-retention.md).

Si está interesado en conservar los eventos de auditoría más tiempo, utilice la API de informes para extraer con periodicidad los [eventos de auditoría](../../active-directory/active-directory-reporting-activity-audit-logs.md) en un almacén de datos independiente.

### <a name="virtual-machine-logs-that-use-azure-diagnostics"></a>Registros de máquina virtual que usan Azure Diagnostics

[Azure Diagnostics](../../monitoring-and-diagnostics/azure-diagnostics.md) es la funcionalidad de Azure que habilita la recopilación de datos de diagnóstico en una aplicación implementada. Puede utilizar la extensión de diagnóstico desde varios orígenes distintos. Actualmente se admiten los [roles web y de trabajo de Azure Cloud Services](../../cloud-services/cloud-services-choose-me.md).

![Registros de máquina virtual que usan Azure Diagnostics](./media/log-audit/azure-log-audit-fig3.png)

### <a name="azure-virtual-machineslearnpathsdeploy-a-website-with-azure-virtual-machines-that-are-running-microsoft-windows-and-service-fabricservice-fabricservice-fabric-overviewmd"></a>[Máquinas virtuales de Azure](/learn/paths/deploy-a-website-with-azure-virtual-machines/) con Microsoft Windows y [Service Fabric](../../service-fabric/service-fabric-overview.md).

Puede habilitar Azure Diagnostics en una máquina virtual mediante alguna de las siguientes opciones:

* [Uso de Visual Studio para realizar el seguimiento de las máquinas virtuales de Azure](/visualstudio/azure/vs-azure-tools-debug-cloud-services-virtual-machines)

* [Configuración remota de Azure Diagnostics en una máquina virtual de Azure](../../virtual-machines/virtual-machines-dotnet-diagnostics.md)

* [Uso de PowerShell para configurar los diagnósticos en máquinas virtuales de Azure](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-ps-extensions-diagnostics?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

* [Creación de una máquina virtual Windows con supervisión y diagnóstico mediante una plantilla de Azure Resource Manager](../../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

### <a name="storage-analytics"></a>Storage Analytics

[Azure Storage Analytics](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics) registra y proporciona datos de métricas para una cuenta de almacenamiento. Puede usar estos datos para hacer un seguimiento de solicitudes, analizar tendencias de uso y diagnosticar problemas con la cuenta de almacenamiento. El registro de Storage Analytics está disponible para los [servicios de almacenamiento Azure Blob, Azure Queue y Azure Table](../../storage/common/storage-introduction.md). Storage Analytics registra información detallada sobre las solicitudes correctas y erróneas realizadas a un servicio de almacenamiento.

Esta información se puede utilizar para supervisar solicitudes concretas y para diagnosticar problemas con un servicio de almacenamiento. Las solicitudes se registran en función de la mejor opción. Las entradas del registro se crean solo si se presentan solicitudes al punto de conexión de servicio. Por ejemplo, si una cuenta de almacenamiento tiene actividad en el punto de conexión de Blob service pero no en los puntos de conexión de Table o Queue service, solo se crean los registros correspondientes a Blob service.

Para utilizar Storage Analytics, debe habilitarlo para cada servicio que desee supervisar. Puede habilitarlo en [Azure Portal](https://portal.azure.com/). Para más información, consulte [Supervisión de una cuenta de almacenamiento en Azure Portal](../../storage/common/storage-monitor-storage-account.md). También puede habilitar Storage Analytics mediante programación a través de la API de REST o la biblioteca de cliente. Use la operación Set Service Properties para habilitar Storage Analytics de forma individual para cada servicio.

Los datos agregados se almacenan en un blob conocido (para el registro) y en tablas conocidas (para las métricas), a los que se puede acceder mediante las API de Blob service y de Table service.

Storage Analytics tiene un límite de 20 terabytes (TB) en cuanto a la cantidad de datos almacenados, que es independiente del límite total de la cuenta de almacenamiento. Todos los registros se almacenan en [blobs en bloques](../../storage/common/storage-analytics.md) en un contenedor llamado $logs, que se crea automáticamente cuando se habilita Storage Analytics para una cuenta de almacenamiento.

> [!Note]
> * Para más información sobre las directivas de retención de datos y facturación, consulte [Facturación de Storage Analytics](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-and-billing).
> * Para más información sobre los límites de las cuentas de almacenamiento, consulte [Objetivos de escalabilidad y rendimiento de Azure Storage](../../storage/common/storage-scalability-targets.md).

Storage Analytics registra los siguientes tipos de solicitudes autenticadas y anónimas:

| Autenticadas  | Anónimas|
| :------------- | :-------------|
| Solicitudes correctas | Solicitudes correctas |
|Solicitudes erróneas, incluidos errores de tiempo de espera, de limitación, de red, de autorización y de otro tipo | Solicitudes que usan una firma de acceso compartido, incluidas las correctas y las erróneas |
| Solicitudes que usan una firma de acceso compartido, incluidas las correctas y las erróneas |Errores de tiempo de espera para el cliente y el servidor |
|   Solicitudes de datos de análisis |    Solicitudes GET erróneas con el código de error 304 (No modificado) |
| Las solicitudes realizadas por el propio Storage Analytics, como la creación o eliminación del registro, no se registran. Puede encontrar una lista completa de los datos registrados en [Operaciones registradas y mensajes de estado de Storage Analytics](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-logged-operations-and-status-messages) y [Formato de registro de Storage Analytics](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-log-format). | El resto de solicitudes anónimas erróneas no se registran. Puede encontrar una lista completa de los datos registrados en [Operaciones registradas y mensajes de estado de Storage Analytics](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-logged-operations-and-status-messages) y [Formato de registro de Storage Analytics](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-log-format). |

### <a name="azure-networking-logs"></a>Registros de redes de Azure

El registro y supervisión de redes en Azure es completa y cubre dos amplias categorías:

* [Network Watcher](../../network-watcher/network-watcher-monitoring-overview.md): la supervisión de red basada en un escenario se proporciona con las características de Network Watcher. Este servicio incluye captura de paquetes, próximo salto, comprobación de flujo de IP, vista de grupos de seguridad y registros de flujo de NSG. La supervisión en el nivel de escenario, ofrece una visión global de los recursos de la red que contrasta con la supervisión de recursos de red individuales.

* [Supervisión de recursos](../../network-watcher/network-watcher-monitoring-overview.md): la supervisión en el nivel de recurso consta de cuatro características: registros de diagnóstico, métricas, solución de problemas y mantenimiento de recursos. Todas estas funciones se integran en el nivel de recursos de red.

![Registros de redes de Azure](./media/log-audit/azure-log-audit-fig4.png)

Network Watcher es un servicio regional que permite supervisar y diagnosticar problemas en un nivel de escenario de red mediante Azure. Las herramientas de visualización y diagnóstico de red que incluye Network Watcher le ayudan a conocer, diagnosticar y obtener información acerca de cualquier red de Azure.

### <a name="network-security-group-flow-logging"></a>Registro de flujo de grupo de seguridad de red

[Los registros de flujo de NSG](../../network-watcher/network-watcher-nsg-flow-logging-overview.md) son una característica de Network Watcher que puede usar para visualizar información acerca del tráfico IP de entrada y de salida en un grupo de seguridad de red. Estos registros de flujo se escriben en formato JSON y muestran:
* Flujos de entrada y de salida por regla.
* La NIC a la que se aplica el flujo.
* Información de 5 tuplas sobre el flujo (dirección IP de origen o destino, puerto de origen o destino y protocolo).
* Información sobre si se permitió o denegó el tráfico.

Aunque los registros de flujo tienen como destino los NSG, no se muestran como los demás registros. Los registros de flujos solo se almacenan en una cuenta de almacenamiento.

Las mismas directivas de retención vistas en otros registros se aplican a los registros de flujo. Los registros tienen una directiva de retención que se puede establecer entre 1 y 365 días. Si no se establece una directiva de retención, los registros se mantendrán indefinidamente.

**Registros de diagnóstico**

Los recursos de red crean eventos periódicos y espontáneos y estos se registran en las cuentas de almacenamiento y se envían a un centro de eventos o a registros de Azure Monitor. Estos registros proporcionan información acerca del mantenimiento de un recurso. Estos registros se pueden visualizar en herramientas como Power BI y registros de Azure Monitor. Para aprender a visualizar registros de diagnóstico, consulte [Registros de Azure Monitor](../../azure-monitor/insights/azure-networking-analytics.md).

![Registros de diagnóstico](./media/log-audit/azure-log-audit-fig5.png)

Los registros de diagnóstico están disponibles para [Load Balancer](../../load-balancer/load-balancer-monitor-log.md), los [grupos de seguridad de red](../../virtual-network/virtual-network-nsg-manage-log.md), las rutas y [Application Gateway](../../application-gateway/application-gateway-diagnostics.md).

Network Watcher proporciona una vista de los registros de diagnóstico. Esta vista contiene todos los recursos de redes que admiten el registro de diagnóstico. Desde esta vista, puede habilitar y deshabilitar los recursos de red de forma cómoda y rápida.


Además de las funcionalidades de registro anteriores, Network Watcher tiene actualmente las siguientes funcionalidades:
- [Topología](../../network-watcher/view-network-topology.md): proporciona una vista de nivel de red que muestra las diversas interconexiones y asociaciones entre los recursos de red de un grupo de recursos.

- [Captura de paquetes variable](../../network-watcher/network-watcher-packet-capture-overview.md): captura datos de paquetes dentro y fuera de una máquina virtual. Las opciones de filtrado avanzadas y controles optimizados, con los que se pueden establecer límites de tiempo y de tamaño, proporcionan una gran versatilidad. Los datos de paquete se pueden almacenar en un almacén de blobs o en el disco local en formato de archivo *.cap*.

- [Comprobación de flujo IP](../../network-watcher/network-watcher-ip-flow-verify-overview.md): comprueba si un paquete está permitido o denegado en función de los parámetros de paquete de información de 5 tuplas sobre el flujo (IP de destino, IP de origen, puerto de destino, puerto de origen y protocolo). Si un grupo de seguridad deniega un paquete, se devuelve el nombre de la regla y del grupo que lo deniega.

- [Próximo salto](../../network-watcher/network-watcher-next-hop-overview.md): determina el próximo salto para los paquetes que se enrutan en el tejido de red de Azure, lo que le permite diagnosticar cualquier ruta definida por el usuario que se haya configurado incorrectamente.

- [Vista de grupo de seguridad](../../network-watcher/network-watcher-security-group-view-overview.md): Obtiene las reglas de seguridad eficaces que se aplican en una máquina virtual.

- [Solución de problemas de las conexiones y la puerta de enlace de Virtual Network](../../network-watcher/network-watcher-troubleshoot-manage-rest.md): ayuda a solucionar problemas de las conexiones y las puertas de enlace de Virtual Network.

- [Límites de suscripción de red](../../network-watcher/network-watcher-monitoring-overview.md): permite ver el uso de los recursos de la red en comparación con los límites.

### <a name="application-insights"></a>Application Insights

[Azure Application Insights](../../azure-monitor/app/app-insights-overview.md) es un servicio de APM extensible para desarrolladores web en varias plataformas. Úselo para supervisar las aplicaciones web en directo. Se detectan automáticamente las anomalías de rendimiento. Incluye herramientas de análisis eficaces que le ayudan a diagnosticar problemas y comprender lo que hacen realmente los usuarios con la aplicación.

Application Insights está diseñado para ayudarle a mejorar continuamente el rendimiento y la facilidad de uso.

Funciona con diversas aplicaciones y en una amplia variedad de plataformas, como .NET, Node.js o Java EE, tanto hospedadas localmente como en la nube. Se integra con el proceso de DevOps y tiene puntos de conexión para diversas herramientas de desarrollo.

![Diagrama de Application Insights](./media/log-audit/azure-log-audit-fig6.png)

Application Insights está dirigido al equipo de desarrollo y sirve ayudarle a conocer el rendimiento de una aplicación y cómo se utiliza. Supervisa:

* **Tasas de solicitudes, tiempos de respuesta y tasas de error**: averigüe qué páginas son las más populares, en qué momento del día y dónde están los usuarios. Vea qué páginas presentan mejor rendimiento. Si los tiempos de respuesta y las tasas de error aumentan cuando hay más solicitudes, quizás tiene un problema de recursos.

* **Tasas de dependencias, tiempos de respuesta y tasas de error**: averigüe si los servicios externos producen ralentización.

* **Excepciones**: analice las estadísticas agregadas o seleccione instancias concretas y profundice en el seguimiento de la pila y las solicitudes relacionadas. Se notifican tanto las excepciones de servidor como las de explorador.

* **Rendimiento de carga y vistas de página**: obtenga informes de los exploradores de los usuarios.

* **Llamadas AJAX**: obtenga tasas de páginas web, tiempos de respuesta y tasas de error.

* **Número de usuarios y sesiones**.

* **Contadores de rendimiento**: obtenga datos de las máquinas de servidor Windows o Linux, como CPU, memoria y uso de la red.

* **Diagnóstico de host**: obtenga datos de Docker o Azure.

* **Registros de seguimiento de diagnóstico**: obtenga datos de la aplicación para poder correlacionar eventos de seguimiento con las solicitudes.

* **Métricas y eventos personalizados**: obtenga datos que usted mismo escribe en el código de cliente o servidor para realizar un seguimiento de eventos empresariales, como artículos vendidos o partidas ganadas.

La tabla siguiente enumera y describe escenarios de integración:

| Escenario de integración | DESCRIPCIÓN |
| --------------------- | :---------- |
|[Mapa de aplicación](../../azure-monitor/app/app-map.md)|Los componentes de la aplicación, con alertas y métricas clave.|
|[Búsqueda de diagnóstico para datos de instancia](../../azure-monitor/app/diagnostic-search.md)| Busque y filtre eventos como solicitudes, excepciones, llamadas de dependencia, seguimientos de registro y vistas de páginas.|
|[Explorador de métricas para datos agregados](../../azure-monitor/app/metrics-explorer.md)|Explore, filtre y segmente datos agregados, como los índices de solicitudes, errores y excepciones; los tiempos de respuesta y los tiempos de carga de página.|
|[Paneles](../../azure-monitor/app/overview-dashboard.md)|Combine datos de varios recursos y compártalos con otros. Ideal para aplicaciones de varios componentes y para la presentación continua en la sala de reuniones.|
|[Secuencia de métricas en directo](../../azure-monitor/app/live-stream.md)|Al implementar una nueva compilación, fíjese en estos indicadores de rendimiento casi en tiempo real para asegurarse de que todo funciona según lo esperado.|
|[Analytics](../../azure-monitor/app/analytics.md)|Responda preguntas complejas acerca del uso y el rendimiento de su aplicación mediante este eficaz lenguaje de consulta.|
|[Alertas automáticas y manuales](../../azure-monitor/app/alerts.md)|Las alertas automáticas se adaptan a los patrones normales de telemetría de la aplicación y se desencadenan cuando algo no responde al patrón habitual. También puede establecer alertas sobre niveles de métricas estándares o personalizadas.|
|[Visual Studio](../../azure-monitor/app/visual-studio.md)|Visualice los datos de rendimiento en el código. Vaya al código desde los seguimientos de la pila.|
|[Power BI](../../azure-monitor/app/export-power-bi.md)|Integre métricas de uso con otra inteligencia empresarial.|
|[API DE REST](https://dev.applicationinsights.io/)|Escriba código para ejecutar consultas sobre las métricas y los datos sin procesar.|
|[Exportación continua](../../azure-monitor/app/export-telemetry.md)|Exportación masiva de datos sin procesar al almacenamiento cuando llegan.|

### <a name="azure-security-center-alerts"></a>Alertas de Azure Security Center

La detección de amenazas de Azure Security Center recopila automáticamente información de seguridad de los recursos de Azure, de la red y de soluciones de asociados conectadas. Después, analiza estos datos (a menudo, relacionando la información de diferentes orígenes) para identificar las amenazas. En Security Center, las alertas de seguridad están clasificadas por prioridad y se incluyen recomendaciones para solucionar la amenaza. Para más información, consulte [Azure Security Center](../../security-center/security-center-intro.md).

![Diagrama de Azure Security Center](./media/log-audit/azure-log-audit-fig7.png)

Security Center utiliza análisis avanzados que superan con creces los enfoques basados en firmas. Aplica innovaciones en datos de gran tamaño y tecnologías de [aprendizaje automático](https://azure.microsoft.com/blog/machine-learning-in-azure-security-center/) para evaluar eventos en todo el tejido en la nube. De este modo, detecta amenazas que sería imposible identificar mediante enfoques manuales y predice la evolución de los ataques. Estas técnicas de análisis son:

* **Información integrada sobre amenazas**: busca actores malintencionados conocidos aplicando la información global sobre amenazas de los productos y servicios de Microsoft, la Unidad de crímenes digitales de Microsoft (DCU), Microsoft Security Response Center (MSRC) y otras fuentes externas.

* **Análisis del comportamiento**: aplica patrones conocidos para identificar comportamientos malintencionados.

* **Detección de anomalías**: usa la generación de perfiles estadísticos para crear una base de referencia histórica. Alerta de las desviaciones de las referencias que se ajustan a un posible vector de ataque.

Muchos equipos de respuesta a incidentes y operaciones de seguridad confían en una solución SIEM como punto de partida para clasificar e investigar alertas de seguridad. Gracias a la integración de registro de Azure, puede sincronizar alertas de Security Center, además de los eventos de seguridad de máquina virtual recopilados por los diagnósticos y los registros de auditoría de Azure, con registros de Azure Monitor o una solución SIEM casi en tiempo real.

## <a name="azure-monitor-logs"></a>Registros de Azure Monitor

Los registros de Azure Monitor son un servicio de Azure que le ayuda a recopilar y analizar los datos generados por los recursos en los entornos locales o en la nube. Proporciona información en tiempo real mediante el uso de paneles personalizados y de búsqueda integrados para analizar fácilmente millones de registros en todas las cargas de trabajo y los servidores, independientemente de su ubicación física.

![Diagrama de registros de Azure Monitor](./media/log-audit/azure-log-audit-fig8.png)

En el centro de registros de Azure Monitor se encuentra el área de trabajo de Log Analytics, que está hospedada en Azure. Registros de Azure Monitor recopila datos en el área de trabajo desde los orígenes conectados mediante la configuración de orígenes de datos y la incorporación de soluciones a la suscripción. Los orígenes de datos y las soluciones crean distintos tipos de registros, cada uno con su propio conjunto de propiedades. No obstante, los orígenes y las soluciones se pueden analizar de forma conjunta en las consultas al área de trabajo. Esta funcionalidad le permite usar las mismas herramientas y los mismos métodos para trabajar con distintas variantes de datos recopilados por distintos orígenes.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

Los orígenes conectados son los equipos y otros recursos que generan los datos que recopila el servicio de registros de Azure Monitor. Los orígenes pueden incluir los agentes instalados en equipos [Windows](../../log-analytics/log-analytics-agent-windows.md) y [Linux](../../log-analytics/log-analytics-quick-collect-linux-computer.md) que se conectan directamente o agentes de un [grupo de administración de System Center Operations Manager conectado](../../azure-monitor/platform/om-agents.md). Registros de Azure Monitor también puede recopilar datos de una [cuenta de Azure Storage](../../azure-monitor/platform/diagnostic-logs-stream-log-store.md).

Los [orígenes de datos](../../azure-monitor/platform/agent-data-sources.md) son las distintas variantes de datos que se recopilan desde cada origen conectado. Los orígenes incluyen eventos y [datos de rendimiento](../../azure-monitor/platform/data-sources-performance-counters.md) de agentes de [Windows](../../azure-monitor/platform/data-sources-windows-events.md) y Linux, además de orígenes como [registros de IIS](../../azure-monitor/platform/data-sources-iis-logs.md) y [registros de texto personalizado](../../azure-monitor/platform/data-sources-custom-logs.md). Usted configura cada origen de datos que desea recopilar y la configuración se entrega automáticamente a cada origen conectado.

Hay cuatro maneras de [recopilar registros y métricas de los servicios de Azure](../../azure-monitor/platform/diagnostic-logs-stream-log-store.md):

* Azure Diagnostics directo a registros de Azure Monitor (**Diagnósticos** en la tabla siguiente)

* Azure Diagnostics a Azure Storage y luego a registros de Azure Monitor (**Storage** en la tabla siguiente)

* Conectores para servicios de Azure (**Conector** en la tabla siguiente)

* Scripts para recopilar y después publicar datos en registros de Azure Monitor (espacios en blanco en la tabla siguiente y para servicios que no aparecen)

| Servicio | Tipo de recurso | Registros | Métricas | Solución |
| :------ | :------------ | :--- | :------ | :------- |
|Azure Application Gateway| Microsoft.Network/<br>applicationGateways|  Diagnóstico|Diagnóstico|    [Azure Application](../../azure-monitor/insights/azure-networking-analytics.md)[Gateway Analytics](../../azure-monitor/insights/azure-networking-analytics.md#azure-application-gateway-analytics-solution-in-azure-monitor)|
|Application Insights||     Conector|  Conector|  [Application Insights](https://blogs.technet.microsoft.com/msoms/2016/09/26/application-insights-connector-in-oms/)[Connector (versión preliminar)](https://blogs.technet.microsoft.com/msoms/2016/09/26/application-insights-connector-in-oms/)|
|Cuentas de Azure Automation| Microsoft.Automation/<br>AutomationAccounts|    Diagnóstico||       [Más información](../../automation/automation-manage-send-joblogs-log-analytics.md)|
|Cuentas de Azure Batch|  Microsoft.Batch/<br>batchAccounts|  Diagnóstico|    Diagnóstico||
|Servicios en la nube clásica||       Storage||       [Más información](../../azure-monitor/platform/azure-storage-iis-table.md)|
|Cognitive Services|    Microsoft.CognitiveServices/<br>accounts|       Diagnóstico|||
|Análisis con Azure Data Lake| Microsoft.DataLakeAnalytics/<br>accounts|   Diagnóstico|||
|Azure Data Lake Store| Microsoft.DataLakeStore/<br>accounts|   Diagnóstico|||
|Espacio de nombres del centro de eventos de Azure| Microsoft.EventHub/<br>namespaces|  Diagnóstico|    Diagnóstico||
|Azure IoT Hub| Microsoft.Devices/<br>IotHubs||     Diagnóstico||
|Azure Key Vault|   Microsoft.KeyVault/<br>vaults|  Diagnóstico  || [Análisis de Key Vault](../../azure-monitor/insights/azure-key-vault.md)|
|Azure Load Balancer|   Microsoft.Network/<br>loadBalancers|    Diagnóstico|||
|Azure Logic Apps|  Microsoft.Logic/<br>workflows|  Diagnóstico|    Diagnóstico||
||Microsoft.Logic/<br>integrationAccounts||||
|Grupos de seguridad de red|   Microsoft.Network/<br>networksecuritygroups|Diagnóstico||   [Análisis de los grupos de seguridad de red de Azure](../../azure-monitor/insights/azure-networking-analytics.md#azure-application-gateway-and-network-security-group-analytics)|
|Almacenes de recuperación|   Microsoft.RecoveryServices/<br>vaults|||[Azure Recovery Services Analytics (versión preliminar)](https://github.com/krnese/AzureDeploy/blob/master/OMS/MSOMS/Solutions/recoveryservices/)|
|Servicios de búsqueda|   Microsoft.Search/<br>searchServices|    Diagnóstico|    Diagnóstico||
|Espacio de nombres de Service Bus| Microsoft.ServiceBus/<br>namespaces|    Diagnóstico|Diagnóstico|    [Service Bus Analytics (versión preliminar)](https://github.com/Azure/azure-quickstart-templates/tree/master/oms-servicebus-solution)|
|Service Fabric||       Storage||    [Service Fabric Analytics (versión preliminar)](../../service-fabric/service-fabric-diagnostics-oms-setup.md)|
|SQL (v12)| Microsoft.Sql/<br>servers/<br>databases||       Diagnóstico||
||Microsoft.Sql/<br>servers/<br>elasticPools||||
|Storage|||         Script| [Azure Storage Analytics (versión preliminar)](https://github.com/Azure/azure-quickstart-templates/tree/master/oms-azure-storage-analytics-solution)|
|Azure Virtual Machines|    Microsoft.Compute/<br>virtualMachines|  Extensión|  Extensión||
||||Diagnóstico||
|Conjuntos de escalado de máquinas virtuales|    Microsoft.Compute/<br>virtualMachines    ||Diagnóstico||
||Microsoft.Compute/<br>virtualMachineScaleSets/<br>virtualMachines||||
|Granjas de servidores web|Microsoft.Web/<br>serverfarms||   Diagnóstico
|Websites|  Microsoft.Web/<br>sites ||      Diagnóstico|    [Más información](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webappazure-oms-monitoring)|
||Microsoft.Web/<br>sites/<br>slots||||


## <a name="log-integration-with-on-premises-siem-systems"></a>Integración de registros con sistemas locales de SIEM

Con Azure Log Integration, puede integrar los registros sin procesar de los recursos de Azure con el sistema local de Administración de eventos e información de seguridad (SIEM). Las descargas de AzLog se deshabilitaron el 27 de junio de 2018. Para obtener orientación sobre cómo avanzar, consulte el artículo [Use Azure monitor to integrate with SIEM tools](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/) (Uso de Azure Monitor para realizar la integración con herramientas SIEM)

![Diagrama de Log Integration](./media/log-audit/azure-log-audit-fig9.png)

Log Integration recopila diagnósticos de Azure de las máquinas virtuales de Windows, los registros de actividad de Azure, las alertas de Azure Security Center y los registros del proveedor de recursos de Azure. Esta integración proporciona un panel unificado para todos los recursos, tanto locales como en la nube, de forma que puede agregar, correlacionar, analizar y alertar sobre los eventos de seguridad.

Azure Log Integration admite actualmente la integración de registros de actividad de Azure, registros de eventos de Windows de las máquinas virtuales Windows de la suscripción de Azure, alertas de Azure Security Center, registros de diagnósticos de Azure y registros de auditoría de Azure AD.

| Tipo de registro | Registros de Azure Monitor admite JSON (Splunk, ArcSight e IBM QRadar) |
| :------- | :-------------------------------------------------------- |
|Registros de auditoría de Azure AD|   Sí|
|Registros de actividad| Sí|
|Alertas de Security Center |Sí|
|Registros de diagnóstico (registros de recursos)|  Sí|
|Registros de VM|   Sí, mediante eventos reenviados y no mediante JSON|

[Introducción a Azure Log Integration](azure-log-integration-get-started.md): este tutorial le guía por la instalación de Azure Log Integration y la integración de los registros de Azure Storage, los registros de actividad de Azure, las alertas de Azure Security Center y los registros de auditoría de Azure AD.

Escenarios de integración para SIEM:

* [Pasos de configuración de asociados](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/): Esta entrada de blog muestra cómo configurar la integración de registro de Azure para trabajar con soluciones de asociados como Splunk, HP ArcSight e IBM QRadar.

* [Preguntas más frecuentes sobre Azure Log Integration](azure-log-integration-faq.md): Este artículo responde a las preguntas sobre Azure Log Integration.

* [Integración de las alertas de Security Center con Azure Log Integration](../../security-center/security-center-export-data-to-siem.md): este artículo describe cómo sincronizar las alertas de Security Center, además de los eventos de seguridad de máquina virtual recopilados por los registros de Azure Diagnostics y los registros de auditoría de Azure, con registros de Azure Monitor o una solución SIEM.

## <a name="next-steps"></a>Pasos siguientes

- [Auditoría y registro](management-monitoring-overview.md): proteja los datos mediante el mantenimiento de la visibilidad y la rápida respuesta a las alertas de seguridad puntuales.

- [Registro de seguridad y recopilación de registros de auditoría en Azure](https://azure.microsoft.com/resources/videos/security-logging-and-audit-log-collection/): aplique esta configuración para asegurarse de que las instancias de Azure están recopilando los registros de seguridad y auditoría correctos.

- [Configuración de auditoría para una colección de sitios](https://support.office.com/article/Configure-audit-settings-for-a-site-collection-A9920C97-38C0-44F2-8BCB-4CF1E2AE22D2?ui=&rs=&ad=US): si es el administrador de una colección de sitios, recupere el historial de las acciones de un usuario individua y el historial de las acciones realizadas durante un intervalo de fechas concreto. 

- [Búsqueda en el registro de auditoría en el Centro de seguridad y cumplimiento de Office 365](https://support.office.com/article/Search-the-audit-log-in-the-Office-365-Security-Compliance-Center-0d4d0f35-390b-4518-800e-0c7ec95e946c?ui=&rs=&ad=US): utilice el Centro de seguridad y cumplimiento de Office 365 para buscar en el registro de auditoría unificado y ver la actividad de usuario y de administrador en la organización de Office 365.


