---
title: Registro de instancias de contenedor con Azure Log Analytics
description: Aprenda a enviar la salida de un contenedor (STDOUT y STDERR) a Azure Log Analytics.
services: container-instances
author: mmacy
manager: jeconnoc
ms.service: container-instances
ms.topic: overview
ms.date: 06/06/2018
ms.author: marsma
ms.openlocfilehash: a0772d1009021ca64b448710c5353407a5492fae
ms.sourcegitcommit: 6cf20e87414dedd0d4f0ae644696151e728633b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/06/2018
ms.locfileid: "34809876"
---
# <a name="container-instance-logging-with-azure-log-analytics"></a>Registro de instancias de contenedor con Azure Log Analytics

Las áreas de trabajo de Log Analytics proporcionan una ubicación centralizada para almacenar y consultar datos de registro no solo de los recursos de Azure, sino también de los recursos locales y de los recursos de otras nubes. Azure Container Instances incluye compatibilidad integrada para el envío de datos a Log Analytics.

Para enviar datos de instancias de contenedores a Log Analytics debe crear un grupo de contenedores mediante la CLI de Azure (o Cloud Shell) y un archivo YAML. En las secciones siguientes se describe tanto la creación de un grupo de contenedores con el registro habilitado como la consulta de registros.

## <a name="prerequisites"></a>Requisitos previos

Para habilitar el registro en las instancias de los contenedores, necesita lo siguiente:

* [Área de trabajo de Log Analytics](../log-analytics/log-analytics-quick-create-workspace.md)
* [CLI de Azure](/cli/azure/install-azure-cli) (o [Cloud Shell](/azure/cloud-shell/overview))

## <a name="get-log-analytics-credentials"></a>Obtención de las credenciales de Log Analytics

Azure Container Instances necesita permiso para enviar datos a su área de trabajo de Log Analytics. Para conceder este permiso y habilitar el registro, es preciso proporcionar el identificador del área de trabajo de Log Analytics y una de sus claves (la principal o la secundaria) al crear el grupo de contenedores.

Para obtener el identificador y la clave principal del área de trabajo de Log Analytics:

1. Vaya al área de trabajo de Log Analytics en Azure Portal
1. En **CONFIGURACIÓN**, seleccione **Configuración avanzada**
1. Seleccione **Orígenes conectados** > **Servidores de Windows** (o **Servidores de Linux**, ya que tanto el identificador como las claves son las mismas para ambos)
1. Anote el valor de:
   * **ID. DEL ÁREA DE TRABAJO**
   * **CLAVE PRINCIPAL**

## <a name="create-container-group"></a>Creación de un grupo de contenedores

Ahora que tiene el identificador del área de trabajo y la clave principal de Log Analytics, ya puede crear un grupo contenedor con el registro habilitado. En el siguiente ejemplo se crea un grupo de contenedores con un solo contenedor, [fluentd][fluentd]. El contenedor Fluentd genera varias líneas de salida en su configuración predeterminada. Dado que esta salida se envía a su área de trabajo de Log Analytics, sirve para mostrar la visualización y consulta de registros.

En primer lugar, copie el siguiente YAML, que define un grupo de contenedores con un único contenedor, en un nuevo archivo. Reemplace `LOG_ANALYTICS_WORKSPACE_ID` y `LOG_ANALYTICS_WORKSPACE_KEY` por los valores que obtuvo en el paso anterior y guarde el archivo como **deploy-aci.yaml**.

```yaml
apiVersion: 2018-06-01
location: eastus
name: mycontainergroup001
properties:
  containers:
  - name: mycontainer001
    properties:
      environmentVariables: []
      image: fluent/fluentd
      ports: []
      resources:
        requests:
          cpu: 1.0
          memoryInGB: 1.5
  osType: Linux
  restartPolicy: Always
  diagnostics:
    logAnalytics:
      workspaceId: LOG_ANALYTICS_WORKSPACE_ID
      workspaceKey: LOG_ANALYTICS_WORKSPACE_KEY
tags: null
type: Microsoft.ContainerInstance/containerGroups
```

A continuación, ejecute el siguiente comando para implementar el grupo de contenedores; reemplace `myResourceGroup` por un grupo de recursos de su suscripción (o bien cree antes un grupo de recursos denominado "myResourceGroup"):

```azurecli-interactive
az container create -g myResourceGroup -n mycontainergroup001 -f deploy-aci.yaml
```

Debería recibir una respuesta de Azure con detalles de implementación poco después de emitir el comando.

## <a name="view-logs-in-log-analytics"></a>Visualización de los registros de Log Analytics

Una vez que haya implementado el grupo de contenedores, las primeras entradas de registro pueden tardar varios minutos (hasta 10) en aparecer en Azure Portal. Para ver los registros del grupo de contenedores, abra el área de trabajo de Log Analytics y:

1. En la información general de **Área de trabajo de OMS**, seleccione **Búsqueda de registros**
1. En **A few more queries to try** (Intentar algunas consultas más), seleccione el vínculo **All collected data** (Todos los datos recopilados)

Debería ver que la consulta `search *` muestra varios resultados. Si no ve ninguno, espere unos minutos y seleccione el botón **EJECUTAR** para volver a ejecutar la consulta. De forma predeterminada, las entradas del registro se muestran en la vista de "Lista" (seleccione **Tabla** para ver las entradas del registro en un formato más condensado). Luego puede expandir una fila para ver el contenido de una entrada de registro individual.

![Resultados de Búsqueda de registros en Azure Portal][log-search-01]

## <a name="query-container-logs"></a>Consulta de registros de contenedores

Log Analytics incluyen un amplio [lenguaje de consulta][query_lang] para poder extraer información de miles de líneas de la salida del registro.

El agente de registro de Azure Container Instances envía las entradas a la tabla `ContainerInstanceLog_CL` de su área de trabajo de Log Analytics. La estructura básica de una consulta consiste en la tabla de origen (`ContainerInstanceLog_CL`) seguida de una serie de operadores separados por el carácter de barra vertical (`|`). Puede encadenar varios operadores para refinar los resultados y realizar funciones avanzadas.

Para ver los resultados de una consulta de ejemplo, pegue la siguiente consulta en el cuadro de texto de consulta (en "Mostrar convertidor de lenguaje heredado") y seleccione el botón **EJECUTAR** para ejecutar la consulta. Esta consulta muestra todas las entradas de registro cuyo campo "Message" contiene la palabra "warn":

```query
ContainerInstanceLog_CL
| where Message contains("warn")
```

También se admiten consultas más complejas. Por ejemplo, esta consulta muestra solo las entradas de registro del grupo de contenedores "mycontainergroup001" generadas en la última hora:

```query
ContainerInstanceLog_CL
| where (ContainerGroup_s == "mycontainergroup001")
| where (TimeGenerated > ago(1h))
```

## <a name="next-steps"></a>Pasos siguientes

### <a name="log-analytics"></a>Log Analytics

Para más información acerca de cómo realizar consultas en registros y configurar alertas en Azure Log Analytics, consulte:

* [Descripción de las búsquedas de registros en Log Analytics](../log-analytics/log-analytics-log-search.md)
* [Alertas unificadas en Azure Monitor](../monitoring-and-diagnostics/monitoring-overview-unified-alerts.md)

### <a name="monitor-container-cpu-and-memory"></a>Supervisión de la CPU y la memoria de los contenedores

Para obtener información acerca de la supervisión de los recursos de CPU y de memoria de un instancia de contenedor, consulte:

* [Supervisión de los recursos de los contenedores en Azure Container Instances](container-instances-monitor.md).

<!-- IMAGES -->
[log-search-01]: ./media/container-instances-log-analytics/portal-query-01.png

<!-- LINKS - External -->
[fluentd]: https://hub.docker.com/r/fluent/fluentd/
[query_lang]: https://docs.loganalytics.io/

<!-- LINKS - Internal -->