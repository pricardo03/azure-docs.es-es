---
title: Configurar la integración de Prometheus con Azure Monitor para contenedores | Microsoft Docs
description: En este artículo se describe cómo puede configurar Azure Monitor para que el agente de contenedores extraiga las métricas de Prometheus con el clúster de Azure Kubernetes Service.
ms.service: azure-monitor
ms.subservice: ''
ms.topic: conceptual
author: mgoedtel
ms.author: magoedte
ms.date: 10/15/2019
ms.openlocfilehash: 51bdf0cfedb30fbd95f9a44e8f4a0efe4e857104
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73510749"
---
# <a name="configure-scraping-of-prometheus-metrics-with-azure-monitor-for-containers"></a>Configuración de la extracción de métricas de Prometheus con Azure Monitor para contenedores

[Prometheus](https://prometheus.io/) es una solución de supervisión de métricas de código abierto muy popular que forma parte de [Cloud Native Compute Foundation](https://www.cncf.io/). Azure Monitor para contenedores proporciona una experiencia de incorporación sin problemas para recopilar las métricas de Prometheus. Normalmente, para usar Prometheus es necesario configurar y administrar un servidor de Prometheus con un almacén. Al integrarse con Azure Monitor, no es necesario un servidor de Prometheus. Solo tiene que exponer el punto de conexión de métricas de Prometheus mediante exportadores o pods (aplicación) y el agente de contenedor de Azure Monitor para contenedores puede extraer las métricas por usted. 

![Arquitectura de supervisión de contenedores para Prometheus](./media/container-insights-prometheus-integration/monitoring-kubernetes-architecture.png)

>[!NOTE]
>La versión mínima del agente compatible con la extracción de métricas de Prometheus es ciprod07092019 o la más reciente, y la versión del agente compatible con la escritura de errores de configuración y del agente en la tabla `KubeMonAgentEvents` es ciprod10112019. Para más información sobre las versiones del agente y lo que se incluye en cada versión, vea las [notas de la versión del agente](https://github.com/microsoft/Docker-Provider/tree/ci_feature_prod). Para comprobar la versión del agente, en la pestaña **Nodo** seleccione un nodo y, en el panel de propiedades, anote el valor de la propiedad **Etiqueta de imagen del agente**.

### <a name="prometheus-scraping-settings"></a>Configuración de la recopilación de datos en Prometheus

La recopilación de datos activa de métricas de Prometheus se realiza desde una de estas dos perspectivas:

* En todo el clúster, dirección URL de HTTP y detección de destinos en la lista de puntos de conexión de un servicio. Por ejemplo, los servicios de K8S, como kube-dns y kube-state-metrics, y las anotaciones pod específicas de una aplicación. Las métricas recopiladas en este contexto se definirán en la sección ConfigMap *[Prometheus data_collection_settings.cluster]* .
* En todo el nodo: la dirección URL de HTTP y detección de destinos en la lista de puntos de conexión de un servicio. Las métricas recopiladas en este contexto se definirán en la sección ConfigMap *[Prometheus data_collection_settings.node]* .

| Punto de conexión | Ámbito | Ejemplo |
|----------|-------|---------|
| Anotación de pod | En todo el clúster | anotaciones: <br>`prometheus.io/scrape: "true"` <br>`prometheus.io/path: "/mymetrics"` <br>`prometheus.io/port: "8000"` <br>`prometheus.io/scheme: "http"` |
| Servicio Kubernetes | En todo el clúster | `http://my-service-dns.my-namespace:9100/metrics` <br>`https://metrics-server.kube-system.svc.cluster.local/metrics` |
| Punto de conexión/dirección URL | Por nodo o en todo el clúster | `http://myurl:9101/metrics` |

Cuando se especifica una dirección URL, Azure Monitor para contenedores solo extrae el punto de conexión. Cuando se especifica el servicio Kubernetes, el nombre del servicio se resuelve con el servidor DNS del clúster para obtener la dirección IP y, a continuación, se recopilan los datos del servicio resuelto.

|Ámbito | Clave | Tipo de datos | Valor | DESCRIPCIÓN |
|------|-----|-----------|-------|-------------|
| En todo el clúster | | | | Especifique uno de los tres métodos siguientes para extraer los puntos de conexión para las métricas. |
| | `urls` | Cadena | Matriz separada por comas | Punto de conexión HTTP (dirección IP o ruta de acceso de dirección URL válida especificada). Por ejemplo: `urls=[$NODE_IP/metrics]`. ($NODE _IP es específico de Azure Monitor para el parámetro Containers y se puede usar en lugar de la dirección IP del nodo. Tiene que estar todo en mayúsculas). |
| | `kubernetes_services` | Cadena | Matriz separada por comas | Una matriz de servicios de Kubernetes para extraer las métricas de las métricas de kube-state-metrics. Por ejemplo: `kubernetes_services = ["https://metrics-server.kube-system.svc.cluster.local/metrics", http://my-service-dns.my-namespace:9100/metrics]`.|
| | `monitor_kubernetes_pods` | Boolean | true o false | Cuando se establece en `true` en la configuración de todo el clúster, Azure Monitor para el agente de contenedores extraerá los pods de Kubernetes en todo el clúster para las siguientes anotaciones de Prometheus:<br> `prometheus.io/scrape:`<br> `prometheus.io/scheme:`<br> `prometheus.io/path:`<br> `prometheus.io/port:` |
| | `prometheus.io/scrape` | Boolean | true o false | Habilita la extracción del pod. `monitor_kubernetes_pods` se debe establecer en `true`. |
| | `prometheus.io/scheme` | Cadena | http o https | De manera predeterminada realiza la extracción mediante HTTP. Si es necesario, establézcalo en `https`. | 
| | `prometheus.io/path` | Cadena | Matriz separada por comas | La ruta de acceso del recurso HTTP en el que se van a capturar las métricas. Si la ruta de acceso de las métricas no es `/metrics`, defínala con esta anotación. |
| | `prometheus.io/port` | Cadena | 9102 | Especifique un puerto del que extraer. Si no se establece un puerto, se usará el valor predeterminado 9102. |
| | `monitor_kubernetes_pods_namespaces` | Cadena | Matriz separada por comas | Una lista de permitidos de espacios de nombres para extraer las métricas de los pods de Kubernetes.<br> Por ejemplo: `monitor_kubernetes_pods_namespaces = ["default1", "default2", "default3"]` |
| En todo el nodo | `urls` | Cadena | Matriz separada por comas | Punto de conexión HTTP (dirección IP o ruta de acceso de dirección URL válida especificada). Por ejemplo: `urls=[$NODE_IP/metrics]`. ($NODE _IP es específico de Azure Monitor para el parámetro Containers y se puede usar en lugar de la dirección IP del nodo. Tiene que estar todo en mayúsculas). |
| En todo el nodo o en todo el clúster | `interval` | Cadena | 60 s | El valor predeterminado del intervalo de recopilación es de un minuto (60 segundos). Puede modificar la recopilación de *[prometheus_data_collection_settings. node]* y/o *[prometheus_data_collection_settings. cluster]* a unidades de tiempo como as s, m, h. |
| En todo el nodo o en todo el clúster | `fieldpass`<br> `fielddrop`| Cadena | Matriz separada por comas | Puede especificar determinadas métricas para que se recopilen o no desde el punto de conexión estableciendo la lista de permitir (`fieldpass`) y no permitir (`fielddrop`). Tiene que establecer primero la lista de permitir. |

ConfigMaps es una lista global y solo puede haber una instancia de ConfigMap aplicada al agente. No puede tener otra instancia de ConfigMaps que anule las colecciones.

## <a name="configure-and-deploy-configmaps"></a>Configuración e implementación de ConfigMaps

Realice los pasos siguientes para configurar e implementar el archivo de configuración ConfigMap en el clúster.

1. [Descargue](https://github.com/microsoft/OMS-docker/blob/ci_feature_prod/Kubernetes/container-azm-ms-agentconfig.yaml) el archivo YAML ConfigMap de plantilla y guárdelo como container-azm-ms-agentconfig.yaml.

2. Edite el archivo YAML ConfigMap con sus personalizaciones para extraer métricas de Prometheus.

    - Para la colección de servicios de Kubernetes en todo el clúster, configure el archivo ConfigMap con el siguiente ejemplo.

        ```
        prometheus-data-collection-settings: |- 
        # Custom Prometheus metrics data collection settings
        [prometheus_data_collection_settings.cluster] 
        interval = "1m"  ## Valid time units are s, m, h.
        fieldpass = ["metric_to_pass1", "metric_to_pass12"] ## specify metrics to pass through 
        fielddrop = ["metric_to_drop"] ## specify metrics to drop from collecting
        kubernetes_services = ["http://my-service-dns.my-namespace:9102/metrics"]
        ```

    - Para configurar la extracción de métricas de Prometheus de una dirección URL específica en todo el clúster, configure el archivo ConfigMap mediante el siguiente ejemplo.

        ```
        prometheus-data-collection-settings: |- 
        # Custom Prometheus metrics data collection settings
        [prometheus_data_collection_settings.cluster] 
        interval = "1m"  ## Valid time units are s, m, h.
        fieldpass = ["metric_to_pass1", "metric_to_pass12"] ## specify metrics to pass through 
        fielddrop = ["metric_to_drop"] ## specify metrics to drop from collecting
        urls = ["http://myurl:9101/metrics"] ## An array of urls to scrape metrics from
        ```

    - Para configurar la extracción de métricas de Prometheus del DaemonSet de un agente para todos los nodos individuales del clúster, configure lo siguiente en el archivo ConfigMap:
    
        ```
        prometheus-data-collection-settings: |- 
        # Custom Prometheus metrics data collection settings 
        [prometheus_data_collection_settings.node] 
        interval = "1m"  ## Valid time units are s, m, h. 
        urls = ["http://$NODE_IP:9103/metrics"] 
        fieldpass = ["metric_to_pass1", "metric_to_pass2"] 
        fielddrop = ["metric_to_drop"] 
        ```

        >[!NOTE]
        >$NODE _IP es específico de Azure Monitor para el parámetro Containers y se puede usar en lugar de la dirección IP del nodo. Tiene que estar todo en mayúsculas. 

    - Para configurar la extracción de métricas de Prometheus especificando una anotación de pod, realice los siguientes pasos:

       1. En el archivo ConfigMap, especifique lo siguiente:

            ```
            prometheus-data-collection-settings: |- 
            # Custom Prometheus metrics data collection settings
            [prometheus_data_collection_settings.cluster] 
            interval = "1m"  ## Valid time units are s, m, h
            monitor_kubernetes_pods = true 
            ```

       2. Especifique la siguiente configuración para las anotaciones de pod:

           ```
           - prometheus.io/scrape:"true" #Enable scraping for this pod 
           - prometheus.io/scheme:"http:" #If the metrics endpoint is secured then you will need to set this to `https`, if not default ‘http’
           - prometheus.io/path:"/mymetrics" #If the metrics path is not /metrics, define it with this annotation. 
           - prometheus.io/port:"8000" #If port is not 9102 use this annotation
           ```
    
          Si quiere restringir la supervisión a espacios de nombres específicos para pods que tienen anotaciones, por ejemplo, incluya solo los pods dedicados a las cargas de trabajo de producción, establezca el `monitor_kubernetes_pod` en `true` en ConfigMap y agregue el filtro de espacio de nombres `monitor_kubernetes_pods_namespaces` especificando los espacios de nombres de los que se va a extraer. Por ejemplo: `monitor_kubernetes_pods_namespaces = ["default1", "default2", "default3"]`

3. Ejecute el siguiente comando kubectl para crear ConfigMap: `kubectl apply -f <configmap_yaml_file.yaml>`.
    
    Ejemplo: `kubectl apply -f container-azm-ms-agentconfig.yaml`. 
    
    El cambio de configuración puede tardar unos minutos en finalizar antes de que surta efecto, y todos los pods de omsagent del clúster se reiniciarán. El reinicio es un reinicio gradual para todos los pods de omsagent; no todos se reinician al mismo tiempo. Cuando los reinicios finalizan, se muestra un mensaje similar a lo siguiente e incluye el resultado: `configmap "container-azm-ms-agentconfig" created`.

## <a name="applying-updated-configmap"></a>Aplicación de ConfigMap actualizado

Si ya ha implementado ConfigMap para el clúster y desea actualizarlo con una configuración más reciente, puede editar el archivo ConfigMap que usó antes y después aplicarlo usando el mismo comando, `kubectl apply -f <configmap_yaml_file.yaml`.

El cambio de configuración puede tardar unos minutos en finalizar antes de que surta efecto, y todos los pods de omsagent del clúster se reiniciarán. El reinicio es un reinicio gradual para todos los pods de omsagent; no todos se reinician al mismo tiempo. Cuando los reinicios finalizan, se muestra un mensaje similar a lo siguiente e incluye el resultado: `configmap "container-azm-ms-agentconfig" updated`.

## <a name="verify-configuration"></a>Comprobación de la configuración 

Para comprobar que la configuración se aplicó correctamente, use el comando siguiente para revisar los registros de un pod de agente: `kubectl logs omsagent-fdf58 -n=kube-system`. Si hay errores de configuración de los pods de osmagent, la salida mostrará errores similares a los siguientes:

``` 
***************Start Config Processing******************** 
config::unsupported/missing config schema version - 'v21' , using defaults
```

Los errores relacionados con la aplicación de cambios de configuración también están disponibles para su revisión. Las siguientes opciones están disponibles para solucionar problemas adicionales de los cambios de configuración y la extracción de métricas de Prometheus:

- Desde los registros de pod del agente que usan el mismo comando `kubectl logs`. 

- Desde los registros en directo. Los registros activos muestran errores similares a los siguientes:

    ```
    2019-07-08T18:55:00Z E! [inputs.prometheus]: Error in plugin: error making HTTP request to http://invalidurl:1010/metrics: Get http://invalidurl:1010/metrics: dial tcp: lookup invalidurl on 10.0.0.10:53: no such host
    ```

- Desde la tabla **KubeMonAgentEvents** en el área de trabajo de Log Analytics. Los datos se envían cada hora con gravedad *Advertencia* para los errores de extracción y gravedad *Error* para los errores de configuración. Si no hay ningún error, la entrada de la tabla tendrá datos con gravedad *Info*, que no notifica errores. La propiedad **Tags** contiene más información sobre el pod y el identificador de contenedor en el que se produjo el error, así como la primera aparición, la última aparición y la cantidad en la última hora.

Los errores impiden que omsagent analice el archivo, lo que hace que se reinicie y use la configuración predeterminada. Después de corregir los errores en ConfigMap, guarde el archivo YAML y aplique ConfigMaps actualizados mediante la ejecución del comando: `kubectl apply -f <configmap_yaml_file.yaml`.

## <a name="query-prometheus-metrics-data"></a>Consulta de los datos de las métricas de Prometheus

Para ver las métricas de Prometheus que ha extraído Azure Monitor y los errores de configuración o de recopilación que el agente ha comunicado, revise [Consultar datos de métricas de Prometheus](container-insights-log-search.md#query-prometheus-metrics-data) y [Consultar errores de configuración o de recopilación](container-insights-log-search.md#query-config-or-scraping-errors).

## <a name="view-prometheus-metrics-in-grafana"></a>Ver métricas de Prometheus en Grafana

Azure Monitor para contenedores admite la visualización de métricas almacenadas en el área de trabajo de Log Analytics en los paneles de Grafana. Hemos proporcionado una plantilla que puede descargar del [repositorio del panel](https://grafana.com/grafana/dashboards?dataSource=grafana-azure-monitor-datasource&category=docker) de Grafana para empezar a trabajar y como referencia para obtener información sobre cómo consultar datos adicionales de los clústeres supervisados para visualizarlos en paneles de Grafana personalizados. 

## <a name="review-prometheus-data-usage"></a>Revisión del uso de datos de Prometheus

Para identificar el volumen de ingesta de cada tamaño de métricas en GB al día con el fin de comprender si es alto, se proporciona la siguiente consulta.

```
InsightsMetrics 
| where Namespace == "prometheus"
| where TimeGenerated > ago(24h)
| summarize VolumeInGB = (sum(_BilledSize) / (1024 * 1024 * 1024)) by Name
| order by VolumeInGB desc
| render barchart
```
La salida mostrará resultados similares a los siguientes:

![Resultados de la consulta del registro del volumen de ingesta de datos](./media/container-insights-prometheus-integration/log-query-example-usage-03.png)

Para calcular el tamaño de cada una de las métricas en GB en un mes, con el fin de comprender si el volumen de datos ingeridos recibidos en el área de trabajo es alto, se proporciona la siguiente consulta.

```
InsightsMetrics 
| where Namespace contains "prometheus"
| where TimeGenerated > ago(24h)
| summarize EstimatedGBPer30dayMonth = (sum(_BilledSize) / (1024 * 1024 * 1024)) * 30 by Name
| order by EstimatedGBPer30dayMonth desc
| render barchart
```

La salida mostrará resultados similares a los siguientes:

![Resultados de la consulta del registro del volumen de ingesta de datos](./media/container-insights-prometheus-integration/log-query-example-usage-02.png)

Puede encontrar más información sobre cómo supervisar el uso de datos y analizar el costo en [Administrar el uso y los costos con los registros de Azure Monitor](../platform/manage-cost-storage.md).

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre la configuración de la recopilación de agentes para las variables stdout, stderr y de entorno a partir de cargas de trabajo de contenedor [aquí](container-insights-agent-config.md). 