---
title: Preguntas más frecuentes sobre Azure Monitor para contenedores | Microsoft Docs
description: Azure Monitor para contenedores es una solución que supervisa el estado de los clústeres de AKS y Container Instances en Azure. En este artículo se responden preguntas comunes.
services: azure-monitor
author: mgoedtel
manager: carmonm
editor: tysonn
ms.service: azure-monitor
ms.topic: article
ms.workload: infrastructure-services
ms.date: 08/02/2019
ms.author: magoedte
ms.openlocfilehash: 3644b40311c037df800eb89ca26d1285fbf1e082
ms.sourcegitcommit: d060947aae93728169b035fd54beef044dbe9480
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/02/2019
ms.locfileid: "68741504"
---
# <a name="azure-monitor-for-containers-frequently-asked-questions"></a>Preguntas más frecuentes sobre Azure Monitor para contenedores

En este artículo de preguntas frecuentes de Microsoft, se presenta una lista con las preguntas frecuentes sobre Azure Monitor para contenedores. Si tiene alguna otra pregunta sobre esta solución, vaya al [foro de discusión](https://feedback.azure.com/forums/34192--general-feedback) y publíquela. Si una pregunta es frecuente, se agrega a este artículo para que se pueda encontrar de forma rápida y sencilla.

## <a name="can-i-monitor-my-aks-engine-cluster-with-azure-monitor-for-containers"></a>¿Puedo supervisar el clúster con el motor de AKS con Azure Monitor para contenedores?

Azure Monitor para contenedores admite la supervisión de cargas de trabajo de contenedor implementadas en clústeres con el motor de AKS (anteriormente conocido como motor de ACS) hospedados en Azure. Para más información y una introducción a los pasos necesarios para habilitar la supervisión de este escenario, consulte [Uso de Azure Monitor para contenedores de para el motor de AKS](https://github.com/microsoft/OMS-docker/tree/aks-engine).

## <a name="why-dont-i-see-data-in-my-log-analytics-workspace"></a>¿Por qué no veo datos en mi área de trabajo de Log Analytics?

Si no puede ver ningún dato en el área de trabajo de Log Analytics en un momento determinado cada día, puede deberse a que ha alcanzado el límite predeterminado de 500 MB, o bien el límite diario especificado para controlar la cantidad de datos que se recopila a diario. Cuando se alcanza el límite diario, la recopilación de datos se detiene y solo se reanuda al día siguiente. Para revisar el uso que hace de los datos y actualizar a un plan de tarifa distinto en función de los patrones de uso que tenga previstos, vea [Uso de datos de Log Analytics y costes](../platform/manage-cost-storage.md). 

## <a name="what-are-the-container-states-specified-in-the-containerinventory-table"></a>¿Cuáles son los estados de contenedor especificados en la tabla ContainerInventory?

La tabla ContainerInventory contiene información sobre los contenedores detenidos y en ejecución. La tabla se rellena con un flujo de trabajo dentro del agente que consulta el docker de todos los contenedores (en ejecución y detenidos) y reenvía dichos datos al área de trabajo de Log Analytics.
 
## <a name="how-do-i-resolve-missing-subscription-registration-error"></a>¿Cómo se resuelve el error que indica que **falta el registro de suscripción**?

Si recibe el error **Missing Subscription registration for Microsoft.OperationsManagement** (Falta el registro de suscripción de Microsoft.OperationsManagement), puede resolverlo registrando el proveedor de recursos **Microsoft.OperationsManagement** en la suscripción en la que está definida el área de trabajo. La documentación sobre cómo hacer esto se puede encontrar [aquí](../../azure-resource-manager/resource-manager-register-provider-errors.md).

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

Puede ver el siguiente error: **la dirección URL de respuesta especificada en la solicitud no coincide con las direcciones URL de respuesta configuradas para la aplicación "<identificador de la aplicación\>"** . La solución para resolver esto puede encontrase en el artículo [Vista de los registros de contenedor en tiempo real con Azure Monitor para contenedores](container-insights-live-logs.md#configure-aks-with-azure-active-directory). 

## <a name="why-cant-i-upgrade-cluster-after-onboarding"></a>¿Por qué no se puede actualizar el clúster después de incorporarlo?

Si, después de habilitar Azure Monitor para contenedores en un clúster de AKS, elimina el área de trabajo de Log Analytics a la que el clúster enviaba datos, se producirá un error cuando intente actualizar dicho clúster. Para solucionar este problema, tendrá que deshabilitar la supervisión y, después, volver a habilitarla, pero de forma que haga referencia a otra área de trabajo válida de la suscripción. Tras ello, cuando intente llevar a cabo la actualización del clúster de nuevo, debería procesarse y completarse correctamente.  

## <a name="which-ports-and-domains-do-i-need-to-openwhitelist-for-the-agent"></a>¿Qué puertos y dominios es necesario abrir o incluir en la lista blanca del agente?
- *.ods.opinsights.azure.com   443
- *.oms.opinsights.azure.com   443
- *.blob.core.windows.net      443
- dc.services.visualstudio.com 443

## <a name="next-steps"></a>Pasos siguientes

Para comenzar a supervisar el clúster de AKS, revise [Cómo incorporar Azure Monitor para contenedores](container-insights-onboard.md) para conocer los requisitos y los métodos disponibles para habilitar la supervisión. 
