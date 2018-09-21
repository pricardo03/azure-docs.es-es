---
title: Preguntas frecuentes sobre Análisis de tráfico de Azure | Microsoft Docs
description: Conozca las respuestas a las preguntas más frecuentes sobre Análisis de tráfico.
services: network-watcher
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/08/2018
ms.author: jdial
ms.openlocfilehash: debefb329304235818b7b441122b90ae9a2834a9
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/14/2018
ms.locfileid: "45578317"
---
# <a name="traffic-analytics-frequently-asked-questions"></a>Preguntas frecuentes sobre Análisis de tráfico

En este artículo se recopila en un solo lugar muchas de las preguntas más frecuentes sobre Análisis de tráfico de Azure Network Watcher.

## <a name="what-are-the-prerequisites-to-use-traffic-analytics"></a>¿Cuáles son los requisitos previos para usar Análisis de tráfico?

Los requisitos previos de Análisis de tráfico son los siguientes:

- Una suscripción activa a Network Watcher.
- Registros de flujo habilitados para los grupos de seguridad de red que desea supervisar.
- Una cuenta de Azure Storage para almacenar los registros de flujo sin formato.
- Un área de trabajo de Azure Log Analytics con acceso de lectura y escritura.

Debe cumplir alguno de los siguientes requisitos para habilitar Análisis de tráfico:

- Se debe asignar su cuenta a uno de los siguientes roles en el nivel de suscripción: administrador de cuenta, administrador de servicios o coadministrador.
- Su cuenta debe tener cualquiera de los siguientes roles del control de acceso basado en rol (RBAC) en el ámbito de la suscripción: propietario, colaborador, lector o colaborador de red.
- Si la cuenta no está asignada a uno de los roles mencionados anteriormente, se debe asignar a un rol personalizado que tenga asignadas las acciones siguientes en el nivel de suscripción.
            
    - Microsoft.Network/applicationGateways/read
    - Microsoft.Network/connections/read
    - Microsoft.Network/loadBalancers/read 
    - Microsoft.Network/localNetworkGateways/read 
    - Microsoft.Network/networkInterfaces/read 
    - Microsoft.Network/networkSecurityGroups/read 
    - Microsoft.Network/publicIPAddresses/read
    - Microsoft.Network/routeTables/read
    - Microsoft.Network/virtualNetworkGateways/read 
    - Microsoft.Network/virtualNetworks/read
        
Para comprobar los roles asignados a un usuario para una suscripción:

1. Inicie sesión en Azure mediante **Login-AzureRmAccount**. 

2. Seleccione la suscripción necesaria con **Select-AzureRmSubscription**. 

3. Para enumerar todos los roles asignados a un usuario específico, use **Get-AzureRmRoleAssignment -SignInName [correo electrónico del usuario] -IncludeClassicAdministrators**. 

Si no se muestra ninguna salida, póngase en contacto con el administrador de la suscripción correspondiente para acceder para ejecutar los comandos. Para más información, consulte [Administración del control de acceso basado en rol con Azure PowerShell](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-powershell).


## <a name="in-which-azure-regions-are-traffic-analytics-available"></a>¿Qué regiones de Azure están disponibles en Análisis de tráfico?

Puede usar Análisis de tráfico para grupos de seguridad de red en cualquiera de las siguientes regiones admitidas: Centro-oeste de EE. UU., Este de EE. UU., Este de EE. UU. 2, Centro y norte de EE. UU., Centro y Sur de EE. UU., Centro de EE. UU., Oeste de EE. UU., Oeste de EE. UU. 2, Europa Occidental, Europa del Norte, Oeste de Reino Unido, Sur de Reino Unido, Este de Australia, Sudeste de Australia y Sudeste Asiático. El área de trabajo de Log Analytics debe encontrarse en las regiones Centro-oeste de EE. UU., Este de EE. UU., Europa Occidental, Sur de Reino Unido, Sudeste de Australia o Sudeste Asiático.

## <a name="can-the-nsgs-i-enable-flow-logs-for-be-in-different-regions-than-my-workspace"></a>¿Los grupos de seguridad de red en los que he habilitado los registros de flujo pueden estar en otras regiones que no sean las de mi área de trabajo?

Sí, estos grupos de seguridad de red pueden estar en regiones distintas a las del área de trabajo de Log Analytics.

## <a name="can-multiple-nsgs-be-configured-within-a-single-workspace"></a>¿Puede haber varios grupos de seguridad de red configurados en una única área de trabajo?

Sí.

## <a name="can-i-use-an-existing-workspace"></a>¿Puedo usar un área de trabajo existente?

Sí. Si selecciona un área de trabajo existente, asegúrese de que se ha migrado al nuevo lenguaje de consulta. Si no quiere actualizar el área de trabajo, debe crear una nueva. Para más información acerca del nuevo lenguaje de consulta, consulte [Actualización de Azure Log Analytics para la nueva búsqueda de registros](../log-analytics/log-analytics-log-search-upgrade.md).

## <a name="can-my-azure-storage-account-be-in-one-subscription-and-my-operations-management-suite-workspace-be-in-a-different-subscription"></a>¿La cuenta de Azure Storage y el área de trabajo de Operations Management Suite pueden estar en suscripciones diferentes?

Sí, la cuenta de Azure Storage y el área de trabajo de Operations Management Suite pueden estar en suscripciones diferentes.

## <a name="can-i-store-raw-logs-in-a-different-subscription"></a>¿Puedo almacenar registros sin formato en una cuenta diferente?

No. Puede almacenar registros sin formato en cualquier cuenta de almacenamiento en la que esté habilitado un grupo de seguridad de red para los registros de flujo. No obstante, la cuenta de almacenamiento y los registros sin formato se deben encontrar en la misma región y suscripción.

## <a name="what-if-i-cant-configure-an-nsg-for-traffic-analytics-due-to-a-not-found-error"></a>¿Qué sucede si no puedo configurar un grupo de seguridad de red para Análisis de tráfico debido a un error del tipo "No se encuentra"?

Seleccione una región compatible. Si selecciona una región que no es compatible, se producirá un error de tipo "No se encuentra". Las regiones compatibles se mencionaron anteriormente en este artículo.

## <a name="what-if-i-am-getting-the-status-failed-to-load-under-the-nsg-flow-logs-page"></a>¿Qué sucede si recibo el estado "No se pudo cargar" en la página de registros de flujo del grupo de seguridad de red?

Para que los registros de flujo funcionen correctamente, el proveedor Microsoft.Insight debe estar registrado. Si no sabe con seguridad si el proveedor Microsoft.Insights está registrado en su suscripción, sustituya *xxxxx-xxxxx-xxxxxx-xxxx* en el siguiente comando y ejecute los siguientes comandos en PowerShell:

```powershell-interactive
**Select-AzureRmSubscription** -SubscriptionId xxxxx-xxxxx-xxxxxx-xxxx
**Register-AzureRmResourceProvider** -ProviderNamespace Microsoft.Insights
```

## <a name="i-have-configured-the-solution-why-am-i-not-seeing-anything-on-the-dashboard"></a>He configurado la solución. ¿Por qué no veo nada en el panel?

La primera vez, el panel puede tardar hasta 30 minutos en aparecer. La solución tiene que agregar datos suficientes para poder inferir información detallada relevante. Después, genera informes. 

## <a name="what-if-i-get-this-message-we-could-not-find-any-data-in-this-workspace-for-selected-time-interval-try-changing-the-time-interval-or-select-a-different-workspace"></a>¿Qué sucede si aparece el siguiente mensaje: "No se encontró ningún dato en el área de trabajo para el intervalo de tiempo seleccionado. Intente cambiar el intervalo de tiempo o seleccione otra área de trabajo"?

Pruebe lo siguiente:
- Cambie el intervalo de tiempo en la barra superior.
- Seleccione otra área de trabajo de Log Analytics en la barra superior.
- Intente acceder a Análisis de tráfico después de 30 minutos, si se ha habilitado recientemente.
    
Si el problema persiste, comparta su experiencia en el [foro de UserVoice](https://feedback.azure.com/forums/217313-networking?category_id=195844).

## <a name="what-if-i-get-this-message-analyzing-your-nsg-flow-logs-for-the-first-time-this-process-may-take-20-30-minutes-to-complete-check-back-after-some-time-2-if-the-above-step-doesnt-work-and-your-workspace-is-under-the-free-sku-then-check-your-workspace-usage-here-to-validate-over-quota-else-refer-to-faqs-for-further-information"></a>¿Qué sucede si aparece este mensaje: "Analizando registros de flujo de grupos de seguridad de red por primera vez? Este proceso puede tardar de 20 a 30 minutos en completarse. Compruébelo de nuevo más tarde. 2) Si el paso anterior no funciona y el área de trabajo está en la SKU gratuita, compruebe aquí el uso del área de trabajo para validar el excedente de la cuota y consulte también las preguntas más frecuentes para más información.

Puede que aparezca este mensaje porque:
- Análisis de tráfico se ha habilitado hace poco tiempo y todavía no se han agregado datos suficientes para inferir información detallada relevante.
- Está utilizando la versión gratis del área de trabajo de Operations Management Suite y superó los límites de cuota. Puede que necesite usar un área de trabajo con una mayor capacidad.
    
Si el problema persiste, comparta su experiencia en el [foro de UserVoice](https://feedback.azure.com/forums/217313-networking?category_id=195844).
    
## <a name="what-if-i-get-this-message-looks-like-we-have-resources-data-topology-and-no-flows-information-meanwhile-click-here-to-see-resources-data-and-refer-to-faqs-for-further-information"></a>¿Qué sucede si aparece un mensaje similar al siguiente: "Parece que tenemos información sobre los datos del recurso (topología), pero no sobre los flujos. Mientras tanto, haga clic aquí para ver los datos de los recursos y consulte las preguntas más frecuentes para más información"?

Puede ver información sobre los recursos en el panel; sin embargo, no aparece ninguna estadística relacionada con los flujos. Es posible que estos datos no aparezcan porque no hay flujos de comunicación entre los recursos. Espere 60 minutos y vuelva a comprobar el estado. Si el problema continúa y sabe con seguridad que hay flujos de comunicación entre los recursos, comparta su experiencia en el [foro de UserVoice](https://feedback.azure.com/forums/217313-networking?category_id=195844).

## <a name="can-i-configure-traffic-analytics-using-powershell-or-an-azure-resource-manager-template-or-client"></a>¿Puedo configurar Análisis de tráfico mediante PowerShell o una plantilla o cliente de Azure Resource Manager?

Puede configurar Análisis de tráfico con Windows PowerShell desde la versión 6.2.1 en adelante. Para configurar un registro de flujo y Análisis de tráfico para un grupo de seguridad de red específico mediante el cmdlet Set, consulte [Set-AzureRmNetworkWatcherConfigFlowLog](https://docs.microsoft.com/powershell/module/azurerm.network/set-azurermnetworkwatcherconfigflowlog?view=azurermps-6.3.0). Para obtener un registro de flujo y Análisis de tráfico de un grupo de seguridad de red específico, consulte [Get-AzureRmNetworkWatcherFlowLogStatus](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermnetworkwatcherflowlogstatus?view=azurermps-6.3.0).

En la actualidad, no puede usar una plantilla de Azure Resource Manager para configurar Análisis de tráfico.

Para configurar Análisis de tráfico mediante un cliente de Azure Resource Manager, consulte los siguientes ejemplos.

**Ejemplo de cmdlet Set:**
```
#Requestbody parameters
$TAtargetUri ="/subscriptions/<NSG subscription id>/resourceGroups/<NSG resource group name>/providers/Microsoft.Network/networkSecurityGroups/<name of NSG>"
$TAstorageId = "/subscriptions/<storage subscription id>/resourcegroups/<storage resource group name> /providers/microsoft.storage/storageaccounts/<storage account name>"
$networkWatcherResourceGroupName = "<network watcher resource group name>"
$networkWatcherName = "<network watcher name>"

$requestBody = 
@"
{
    'targetResourceId': '${TAtargetUri}',
    'properties': 
    {
        'storageId': '${TAstorageId}',
        'enabled': '<true to enable flow log or false to disable flow log>',
        'retentionPolicy' : 
        {
            days: <enter number of days like to retail flow logs in storage account>,
            enabled: <true to enable retention or false to disable retention>
        }
    },
    'flowAnalyticsConfiguration':
    {
                'networkWatcherFlowAnalyticsConfiguration':
      {
        'enabled':,<true to enable traffic analytics or false to disable traffic analytics>
        'workspaceId':'bbbbbbbb-bbbb-bbbb-bbbb-bbbbbbbbbbbb',
        'workspaceRegion':'<workspace region>',
        'workspaceResourceId':'/subscriptions/<workspace subscription id>/resourcegroups/<workspace resource group name>/providers/microsoft.operationalinsights/workspaces/<workspace name>'
        
      }

    }
}
"@
$apiversion = "2016-09-01"

armclient login
armclient post "https://management.azure.com/subscriptions/<NSG subscription id>/resourceGroups/<network watcher resource group name>/providers/Microsoft.Network/networkWatchers/<network watcher name>/configureFlowlog?api-version=${apiversion}" $requestBody
```
**Ejemplo de cmdlet Get:**
```
#Requestbody parameters
$TAtargetUri ="/subscriptions/<NSG subscription id>/resourceGroups/<NSG resource group name>/providers/Microsoft.Network/networkSecurityGroups/<NSG name>"


$requestBody = 
@"
{
    'targetResourceId': '${TAtargetUri}'
}
“@


armclient login
armclient post "https://management.azure.com/subscriptions/<NSG subscription id>/resourceGroups/<network watcher resource group name>/providers/Microsoft.Network/networkWatchers/<network watcher name>/queryFlowLogStatus?api-version=${apiversion}" $requestBody
```



## <a name="how-is-traffic-analytics-priced"></a>¿Qué precio tiene Análisis de tráfico?

El análisis de tráfico se puede medir. La medición se basa en el procesamiento de los datos de registro de flujo que realiza el servicio y el almacenamiento de los registros mejorados resultantes en un área de trabajo de Log Analytics. 

Por ejemplo, según el [plan de tarifa](https://azure.microsoft.com/pricing/details/network-watcher/), teniendo en cuenta la región Centro-oeste de EE. UU., si los registros de flujo de datos almacenados en una cuenta de almacenamiento procesada por Análisis de tráfico es de 10 GB y los registros mejorados ingeridos en el área de trabajo de Log Analytics es de 1 GB los cargos aplicables son: 10 x 2,3 USD + 1 x 2,76 USD = 25,76 USD

## <a name="how-can-i-navigate-by-using-the-keyboard-in-the-geo-map-view"></a>¿Cómo puedo navegar con el teclado en la vista del mapa geográfico?

La página del mapa geográfico se compone de dos secciones principales:
    
- **Banner**: el banner de la parte superior del mapa geográfico proporciona botones para seleccionar los filtros de distribución de tráfico (por ejemplo, Implementación, Tráfico de países y Malintencionado). Si selecciona un botón, se aplica el filtro correspondiente en el mapa. Por ejemplo, si selecciona el botón Activo, el mapa resalta los centros de datos activos de la implementación.
- **Mapa**: la sección del mapa situada bajo el banner muestra la distribución del tráfico entre los centros de datos y los países de Azure.
    
### <a name="keyboard-navigation-on-the-banner"></a>Navegación por el banner con el teclado
    
- De forma predeterminada, el filtro seleccionado para el banner en la página del mapa geográfico es el filtro "Centros de datos de Azure".
- Para cambiar de filtro, use la tecla `Tab` o la tecla `Right arrow`. Para volver al filtro anterior, use la tecla `Shift+Tab` o la tecla `Left arrow`. La navegación hacia delante es de izquierda a derecha y de arriba abajo.
- Presione la tecla de dirección `Enter` o `Down` para aplicar el filtro seleccionado. En función del filtro y de la implementación, se resaltarán uno o varios nodos de la sección del mapa.
- Para cambiar entre el banner y el mapa, presione `Ctrl+F6`.
        
### <a name="keyboard-navigation-on-the-map"></a>Navegación por el mapa con el teclado
    
- Si hay un filtro seleccionado en el banner y se presiona `Ctrl+F6`, el foco se mueve a uno de los nodos resaltados (**Centro de datos de Azure** o **País o región**) de la vista del mapa.
- Para moverse a otros nodos resaltados del mapa, use la tecla `Tab` o `Right arrow` para avanzar hacia delante. Use la tecla `Shift+Tab` o `Left arrow` para el volver hacia atrás.
- Para seleccionar un nodo resaltado del mapa, utilice la tecla `Enter` o `Down arrow`.
- Cuando se selecciona uno de estos nodos, el foco se desplaza al **cuadro de herramientas de información** del nodo. De forma predeterminada, el foco se desplaza al botón Cerrar del **cuadro de herramientas de información**. Si desea moverse dentro de la vista del **cuadro**, utilice las teclas `Right arrow` y `Left arrow` para moverse hacia delante y hacia atrás, respectivamente. Si presiona `Enter`, conseguirá el mismo efecto que si selecciona el botón con el foco en el **cuadro de herramientas de información**.
- Si presiona `Tab` mientras el foco se encuentra en el **cuadro de herramientas de información**, el foco se desplazará a los puntos de conexión que están en el mismo continente que el nodo seleccionado. Use las teclas `Right arrow` y `Left arrow` para moverse por estos puntos de conexión.
- Si desea moverse a otros puntos de conexión del flujo o a otros clústeres del continente, utilice `Tab` para moverse hacia delante y `Shift+Tab` para moverse hacia atrás.
- Una vez que el foco esté en los **clústeres de continentes** utilice las teclas de dirección `Enter` o `Down` para resaltar los puntos de conexión del clúster de continentes. Si desea moverse por los puntos de conexión y el botón Cerrar del cuadro informativo del clúster de continentes, utilice la tecla `Right arrow` o `Left arrow` para moverse hacia delante y hacia atrás, respectivamente. Puede utilizar `Shift+L` en cualquier punto de conexión para cambiar a la línea de conexión que une el nodo seleccionado con el punto de conexión. Puede presionar `Shift+L` de nuevo para moverse al punto de conexión seleccionado.
        
### <a name="keyboard-navigation-at-any-stage"></a>Navegación mediante el teclado en cualquier etapa
    
- `Esc` contrae la selección expandida.
- La tecla `Up arrow` realiza la misma acción que `Esc`. La tecla `Down arrow` realiza la misma acción que `Enter`.
- Use `Shift+Plus` para acercar el zoom y `Shift+Minus` para alejarlo.

## <a name="how-can-i-navigate-by-using-the-keyboard-in-the-virtual-network-topology-view"></a>¿Cómo puedo navegar con el teclado en la vista de la topología de red virtual?

La página de topología de redes virtuales contiene dos secciones principales:
    
- **Banner**: el banner situado en la parte superior de la topología de redes virtuales proporciona botones para seleccionar los filtros de distribución de tráfico (por ejemplo, Redes virtuales conectadas, Redes virtuales desconectadas y Direcciones IP públicas). Si selecciona un botón, se aplica el filtro correspondiente en la topología. Por ejemplo, si selecciona el botón Activo, la topología resalta las redes virtuales activas de la implementación.
- **Topología**: la sección de topología, situada por debajo del banner, muestra la distribución del tráfico entre redes virtuales.
    
### <a name="keyboard-navigation-on-the-banner"></a>Navegación por el banner con el teclado
    
- De forma predeterminada, la opción seleccionada en la página de topología de redes virtuales para el banner es el filtro "Redes virtuales conectadas".
- Para cambiar de filtro, use la tecla `Tab` para moverse hacia delante. Para hacerlo hacia atrás, use la tecla `Shift+Tab`. La navegación hacia delante es de izquierda a derecha y de arriba abajo.
- Presione `Enter` para aplicar el filtro seleccionado. En función del filtro y de la implementación, se resaltarán uno o varios nodos (redes virtuales) en la sección de topología.
- Para cambiar entre el banner y la topología, presione `Ctrl+F6`.
        
### <a name="keyboard-navigation-on-the-topology"></a>Navegación por la topología con el teclado
    
- Si hay un filtro seleccionado en el banner y se presiona `Ctrl+F6`, el foco se mueve a uno de los nodos resaltados (**Red virtual**) de la vista de la topología.
- Para moverse a otros nodos resaltados de la vista de topología, use la tecla `Shift+Right arrow` para avanzar hacia delante. 
- Cuando se selecciona uno de estos nodos, el foco se desplaza al **cuadro de herramientas de información** del nodo. De forma predeterminada, el foco se desplaza al botón **Más información** del **cuadro de herramientas de información**. Si desea moverse dentro de la vista del **cuadro**, utilice las teclas `Right arrow` y `Left arrow` para moverse hacia delante y hacia atrás, respectivamente. Si presiona `Enter`, conseguirá el mismo efecto que si selecciona el botón con el foco en el **cuadro de herramientas de información**.
- En la selección de cualquiera de estos nodos, puede visitar todas sus conexiones, una a una, presionando la tecla `Shift+Left arrow`. El foco se mueve al **cuadro de herramientas de información** de esa conexión. En cualquier momento, el foco se puede desplazar hacia el nodo presionando `Shift+Right arrow` de nuevo.
    

## <a name="how-can-i-navigate-by-using-the-keyboard-in-the-subnet-topology-view"></a>¿Cómo puedo navegar con el teclado en la vista de la topología de subred?

La página de topología de subredes virtuales contiene dos secciones principales:
    
- **Banner**: el banner de la parte superior de la topología de subredes virtuales proporciona botones para seleccionar filtros de distribución de tráfico (por ejemplo, Activo, Medio y Subredes de puerta de enlace). Si selecciona un botón, se aplica el filtro correspondiente en la topología. Por ejemplo, si selecciona el botón Activo, la topología resalta las subredes virtuales activas de la implementación.
- **Topología**: la sección de topología, situada por debajo del banner, muestra la distribución del tráfico entre subredes virtuales.
    
### <a name="keyboard-navigation-on-the-banner"></a>Navegación por el banner con el teclado
    
- De forma predeterminada, la opción seleccionada en la página de topología de subredes virtuales para el banner es el filtro "Subredes virtuales".
- Para cambiar de filtro, use la tecla `Tab` para moverse hacia delante. Para hacerlo hacia atrás, use la tecla `Shift+Tab`. La navegación hacia delante es de izquierda a derecha y de arriba abajo.
- Presione `Enter` para aplicar el filtro seleccionado. En función del filtro y de la implementación, se resaltarán uno o varios nodos (subred) de la sección de topología.
- Para cambiar entre el banner y la topología, presione `Ctrl+F6`.
        
### <a name="keyboard-navigation-on-the-topology"></a>Navegación por la topología con el teclado
    
- Si hay un filtro seleccionado en el banner y se presiona `Ctrl+F6`, el foco se mueve a uno de los nodos resaltados (**Subred**) de la vista de la topología.
- Para moverse a otros nodos resaltados de la vista de topología, use la tecla `Shift+Right arrow` para avanzar hacia delante. 
- Cuando se selecciona uno de estos nodos, el foco se desplaza al **cuadro de herramientas de información** del nodo. De forma predeterminada, el foco se desplaza al botón **Más información** del **cuadro de herramientas de información**. Si desea moverse dentro de la vista del **cuadro**, utilice las teclas `Right arrow` y `Left arrow` para moverse hacia delante y hacia atrás, respectivamente. Si presiona `Enter`, conseguirá el mismo efecto que si selecciona el botón con el foco en el **cuadro de herramientas de información**.
- En la selección de cualquiera de estos nodos, puede visitar todas sus conexiones, una a una, presionando la tecla `Shift+Left arrow`. El foco se mueve al **cuadro de herramientas de información** de esa conexión. En cualquier momento, el foco se puede desplazar hacia el nodo presionando `Shift+Right arrow` de nuevo.    

