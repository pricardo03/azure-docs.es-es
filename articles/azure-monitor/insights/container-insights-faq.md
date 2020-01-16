---
title: Preguntas más frecuentes sobre Azure Monitor para contenedores | Microsoft Docs
description: Azure Monitor para contenedores es una solución que supervisa el estado de los clústeres de AKS y Container Instances en Azure. En este artículo se responden preguntas comunes.
ms.topic: conceptual
ms.date: 10/15/2019
ms.openlocfilehash: b0d2be8b573dbbf047f4a27ae9ac9f611b76dc51
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/15/2020
ms.locfileid: "75977759"
---
# <a name="azure-monitor-for-containers-frequently-asked-questions"></a>Preguntas más frecuentes sobre Azure Monitor para contenedores

En este artículo de preguntas frecuentes de Microsoft, se presenta una lista con las preguntas frecuentes sobre Azure Monitor para contenedores. Si tiene alguna otra pregunta sobre esta solución, vaya al [foro de discusión](https://feedback.azure.com/forums/34192--general-feedback) y publíquela. Si una pregunta es frecuente, se agrega a este artículo para que se pueda encontrar de forma rápida y sencilla.

## <a name="i-dont-see-image-and-name-property-values-populated-when-i-query-the-containerlog-table"></a>No veo que los valores de propiedad Name e Image se rellenen cuando consulto la tabla ContainerLog.

En el caso de la versión del agente ciprod12042019 y versiones posteriores, estas dos propiedades no se rellenan de forma predeterminada en cada línea de registro con el fin de reducir el costo generado en los datos de registro recopilados. Hay dos opciones para consultar la tabla que incluyen estas propiedades con sus valores:

### <a name="option-1"></a>Opción 1 

Combinar otras tablas para incluir estos valores de propiedad en los resultados.

Modifique las consultas para que incluyan las propiedades Image e ImageTag de la tabla ```ContainerInventory``` mediante la combinación de la propiedad ContainerID. Puede incluir la propiedad Name (como aparecía anteriormente en la tabla ```ContainerLog```) del campo ContaineName de la tabla KubepodInventory mediante la combinación de la propiedad ContainerID. Esta es la opción recomendada.

El ejemplo siguiente es una consulta detallada de ejemplo que explica cómo obtener estos valores de campo con combinaciones.

```
//lets say we are querying an hour worth of logs
let startTime = ago(1h);
let endTime = now();
//below gets the latest Image & ImageTag for every containerID, during the time window
let ContainerInv = ContainerInventory | where TimeGenerated >= startTime and TimeGenerated < endTime | summarize arg_max(TimeGenerated, *)  by ContainerID, Image, ImageTag | project-away TimeGenerated | project ContainerID1=ContainerID, Image1=Image ,ImageTag1=ImageTag;
//below gets the latest Name for every containerID, during the time window
let KubePodInv  = KubePodInventory | where ContainerID != "" | where TimeGenerated >= startTime | where TimeGenerated < endTime | summarize arg_max(TimeGenerated, *)  by ContainerID2 = ContainerID, Name1=ContainerName | project ContainerID2 , Name1;
//now join the above 2 to get a 'jointed table' that has name, image & imagetag. Outer left is safer in-case there are no kubepod records are if they are latent
let ContainerData = ContainerInv | join kind=leftouter (KubePodInv) on $left.ContainerID1 == $right.ContainerID2;
//now join ContainerLog table with the 'jointed table' above and project-away redundant fields/columns and rename columns that were re-written
//Outer left is safer so you dont lose logs even if we cannot find container metadata for loglines (due to latency, time skew between data types etc...)
ContainerLog
| where TimeGenerated >= startTime and TimeGenerated < endTime 
| join kind= leftouter (
   ContainerData
) on $left.ContainerID == $right.ContainerID2 | project-away ContainerID1, ContainerID2, Name, Image, ImageTag | project-rename Name = Name1, Image=Image1, ImageTag=ImageTag1 

```

### <a name="option-2"></a>Opción 2

Volver a habilitar la recopilación para estas propiedades en cada línea de registro de contenedor.

Si la primera opción no es conveniente debido a los cambios de consulta relacionados, puede volver a habilitar la recopilación de estos campos si habilita el valor ```log_collection_settings.enrich_container_logs``` en el mapa de configuración del agente, como se describe en los [valores de configuración de recopilación de datos](./container-insights-agent-config.md).

> [!NOTE]
> La segunda opción no se recomienda con clústeres de gran tamaño (más de 50 nodos), ya que genera llamadas del servidor de API desde cada nodo del clúster para realizar este enriquecimiento. Esta opción también aumenta el tamaño de los datos de cada línea de registro recopilada.

## <a name="can-i-view-metrics-collected-in-grafana"></a>¿Puedo ver las métricas recopiladas en Grafana?

Azure Monitor para contenedores admite la visualización de métricas almacenadas en el área de trabajo de Log Analytics en los paneles de Grafana. Hemos proporcionado una plantilla que puede descargar del [repositorio del panel](https://grafana.com/grafana/dashboards?dataSource=grafana-azure-monitor-datasource&category=docker) de Grafana para empezar a trabajar y como referencia para obtener información sobre cómo consultar datos adicionales de los clústeres supervisados para visualizarlos en paneles de Grafana personalizados. 

## <a name="can-i-monitor-my-aks-engine-cluster-with-azure-monitor-for-containers"></a>¿Puedo supervisar el clúster con el motor de AKS con Azure Monitor para contenedores?

Azure Monitor para contenedores admite la supervisión de cargas de trabajo de contenedor implementadas en clústeres con el motor de AKS (anteriormente conocido como motor de ACS) hospedados en Azure. Para más información y una introducción a los pasos necesarios para habilitar la supervisión de este escenario, consulte [Uso de Azure Monitor para contenedores de para el motor de AKS](https://github.com/microsoft/OMS-docker/tree/aks-engine).

## <a name="why-dont-i-see-data-in-my-log-analytics-workspace"></a>¿Por qué no veo datos en mi área de trabajo de Log Analytics?

Si no puede ver ningún dato en el área de trabajo de Log Analytics en un momento determinado cada día, puede deberse a que ha alcanzado el límite predeterminado de 500 MB, o bien el límite diario especificado para controlar la cantidad de datos que se recopila a diario. Cuando se alcanza el límite diario, la recopilación de datos se detiene y solo se reanuda al día siguiente. Para revisar el uso que hace de los datos y actualizar a un plan de tarifa distinto en función de los patrones de uso que tenga previstos, vea [Uso de datos de Log Analytics y costes](../platform/manage-cost-storage.md). 

## <a name="what-are-the-container-states-specified-in-the-containerinventory-table"></a>¿Cuáles son los estados de contenedor especificados en la tabla ContainerInventory?

La tabla ContainerInventory contiene información sobre los contenedores detenidos y en ejecución. La tabla se rellena con un flujo de trabajo dentro del agente que consulta el docker de todos los contenedores (en ejecución y detenidos) y reenvía dichos datos al área de trabajo de Log Analytics.
 
## <a name="how-do-i-resolve-missing-subscription-registration-error"></a>¿Cómo se resuelve el error que indica que *falta el registro de suscripción*?

Si recibe el error **Missing Subscription registration for Microsoft.OperationsManagement** (Falta el registro de suscripción de Microsoft.OperationsManagement), puede resolverlo registrando el proveedor de recursos **Microsoft.OperationsManagement** en la suscripción en la que está definida el área de trabajo. La documentación sobre cómo hacer esto se puede encontrar [aquí](../../azure-resource-manager/templates/error-register-resource-provider.md).

## <a name="is-there-support-for-rbac-enabled-aks-clusters"></a>¿Se admiten clústeres de AKS con RBAC habilitado?

La solución de supervisión de contenedores no admite RBAC, pero Azure Monitor para contenedores sí. La página de detalles de la solución no puede mostrar la información correcta en las hojas que muestran datos para estos clústeres.

## <a name="how-do-i-enable-log-collection-for-containers-in-the-kube-system-namespace-through-helm"></a>¿Cómo habilitar la recopilación de registros para contenedores en el espacio de nombres kube-system mediante Helm?

La recopilación de registros de contenedores en el espacio de nombres kube-system está deshabilitada de forma predeterminada. La recopilación de registros puede habilitarse mediante la configuración de una variable de entorno en omsagent. Para obtener más información, vea la página de GitHub sobre [Azure Monitor para contenedores](https://github.com/helm/charts/tree/master/incubator/azuremonitor-containers). 

## <a name="how-do-i-update-the-omsagent-to-the-latest-released-version"></a>¿Cómo se puede actualizar el complemento omsagent a la última versión?

Para obtener información sobre cómo actualizar el agente, vea la información sobre la [administración del agente](container-insights-manage-agent.md).

## <a name="how-do-i-enable-multi-line-logging"></a>¿Cómo se puede habilitar el registro de varias líneas?

Actualmente, Azure Monitor para contenedores no admite el registro de varias líneas, pero hay soluciones alternativas disponibles. Puede configurar todos los servicios para que escriban en formato JSON y, a continuación, Docker/Moby los escribirán como una sola línea.

Por ejemplo, puede ajustar el registro como un objeto JSON como se muestra en el ejemplo siguiente para una aplicación de Node.js de ejemplo:

```
console.log(json.stringify({ 
      "Hello": "This example has multiple lines:",
      "Docker/Moby": "will not break this into multiple lines",
      "and you will receive":"all of them in log analytics",
      "as one": "log entry"
      }));
```

Estos datos tendrán un aspecto similar al del siguiente ejemplo en Azure Monitor cuando se realiza una consulta en los registros:

```
LogEntry : ({“Hello": "This example has multiple lines:","Docker/Moby": "will not break this into multiple lines", "and you will receive":"all of them in log analytics", "as one": "log entry"}

```

Para obtener una visión detallada del problema, visite el siguiente [vínculo de GitHub](https://github.com/moby/moby/issues/22920).

## <a name="how-do-i-resolve-azure-ad-errors-when-i-enable-live-logs"></a>¿Cómo se resuelven los errores de Azure AD al habilitar los registros dinámicos? 

Puede ver el siguiente error: **la dirección URL de respuesta especificada en la solicitud no coincide con las direcciones URL de respuesta configuradas para la aplicación "<identificador de la aplicación\>"** . La solución para resolver esto puede encontrase en el artículo [Vista de los datos de contenedor en tiempo real con Azure Monitor para contenedores](container-insights-livedata-setup.md#configure-ad-integrated-authentication). 

## <a name="why-cant-i-upgrade-cluster-after-onboarding"></a>¿Por qué no se puede actualizar el clúster después de incorporarlo?

Si, después de habilitar Azure Monitor para contenedores en un clúster de AKS, elimina el área de trabajo de Log Analytics a la que el clúster enviaba datos, se producirá un error cuando intente actualizar dicho clúster. Para solucionar este problema, tendrá que deshabilitar la supervisión y, después, volver a habilitarla, pero de forma que haga referencia a otra área de trabajo válida de la suscripción. Tras ello, cuando intente llevar a cabo la actualización del clúster de nuevo, debería procesarse y completarse correctamente.  

## <a name="which-ports-and-domains-do-i-need-to-openwhitelist-for-the-agent"></a>¿Qué puertos y dominios es necesario abrir o incluir en la lista blanca del agente?

Consulte los [requisitos del firewall de red](container-insights-onboard.md#network-firewall-requirements) para obtener la información de configuración del proxy y el firewall necesaria para el agente en contenedor con las nubes Azure, Azure US Government y Azure China.

## <a name="next-steps"></a>Pasos siguientes

Para comenzar a supervisar el clúster de AKS, revise [Cómo incorporar Azure Monitor para contenedores](container-insights-onboard.md) para conocer los requisitos y los métodos disponibles para habilitar la supervisión. 
