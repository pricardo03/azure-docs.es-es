---
title: Configuración de la recopilación de datos del agente de Azure Monitor para contenedores | Microsoft Docs
description: En este artículo se describe cómo puede configurar el agente de Azure Monitor para contenedores para controlar la recopilación de registros de stdout y stderr y de las variables de entorno.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: ''
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/07/2019
ms.author: magoedte
ms.openlocfilehash: ada573cc919d775af52abc5a75004866aebbeddb
ms.sourcegitcommit: f9e81b39693206b824e40d7657d0466246aadd6e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/08/2019
ms.locfileid: "72033938"
---
# <a name="configure-agent-data-collection-for-azure-monitor-for-containers"></a>Configuración de la recopilación de datos del agente para Azure Monitor para contenedores

Azure Monitor para contenedores recopila stdout, stderr y las variables de entorno a partir de cargas de trabajo de contenedor implementadas en clústeres de Kubernetes administrado hospedados en Azure Kubernetes Service (AKS) desde el agente con contenedores. Este agente puede además recopilar datos de series temporales (también denominadas métricas) de Prometheus con el agente en contenedor, sin tener que configurar y administrar un servidor y una base de datos de Prometheus. Puede definir la configuración de recopilación de datos del agente mediante la creación de ConfigMaps de Kubernetes personalizados para controlar esta experiencia. 

En este artículo se muestra cómo crear ConfigMap y configurar la recopilación de datos en función de sus requisitos.

>[!NOTE]
>La compatibilidad con Prometheus es una característica que se encuentra en versión preliminar pública en este momento.
>

## <a name="configmap-file-settings-overview"></a>Información general de la configuración del archivo ConfigMap

Se proporciona un archivo ConfigMap de plantilla que le permite editarlo fácilmente con sus personalizaciones sin tener que crearlo desde cero. Antes de comenzar, debe revisar la documentación de Kubernetes sobre [ConfigMaps](https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/) y familiarizarse con la forma de crear, configurar e implementar ConfigMaps. Esto le permitirá filtrar stderr y stdout por espacio de nombres o a través de todo el clúster, y las variables de entorno para cualquier contenedor que se ejecute en todos los pods o nodos del clúster.

>[!IMPORTANT]
>La versión de agente mínima admitida para recopilar los registros stdout, stderr y las variables de entorno de las cargas de trabajo de contenedor es ciprod06142019 o posterior. La versión mínima del agente admitida para extraer las métricas de Prometheus es ciprod07092019 o posterior. Para comprobar la versión del agente, en la pestaña **Nodo** seleccione un nodo y, en el panel de propiedades, anote el valor de la propiedad **Etiqueta de imagen del agente**.  

### <a name="data-collection-settings"></a>Configuración de la colección de datos

A continuación figura la configuración que puede definir para controlar la recolección de datos.

|Clave |Tipo de datos |Valor |DESCRIPCIÓN |
|----|----------|------|------------|
|`schema-version` |Cadena (distingue mayúsculas de minúsculas) |v1 |Se trata de la versión del esquema que el agente utiliza al analizar este ConfigMap. La versión de esquema que se admite actualmente es v1. No se admite la modificación de este valor y se rechazará cuando ConfigMap se evalúe.|
|`config-version` |Cadena | | Permite realizar un seguimiento de la versión de este archivo de configuración en el sistema o repositorio de control de código fuente. El número máximo de caracteres permitido es 10 y todos los demás caracteres se truncan. |
|`[log_collection_settings.stdout] enabled =` |Boolean | true o false | Controla si está habilitada la recopilación de registros de contenedor de stdout. Cuando se establece en `true` y los espacios de nombres no se excluyen de la recopilación de registros de stdout (configuración `log_collection_settings.stdout.exclude_namespaces` más abajo), los registros de stdout se recopilarán a partir de todos los contenedores en todos los pods o nodos del clúster. Si no se especifica en ConfigMaps, el valor predeterminado es `enabled = true`. |
|`[log_collection_settings.stdout] exclude_namespaces =`|Cadena | Matriz separada por comas |Matriz de espacios de nombres de Kubernetes para la que no se recopilarán los registros de stdout. Esta configuración es efectiva únicamente si `log_collection_settings.stdout.enabled` se establece en `true`. Si no se especifica en ConfigMap, el valor predeterminado es `exclude_namespaces = ["kube-system"]`.|
|`[log_collection_settings.stderr] enabled =` |Boolean | true o false |Controla si está habilitada la recopilación de registros de contenedor de stderr. Cuando se establece en `true` y los espacios de nombres no se excluyen de la recopilación de registros de stdout (configuración `log_collection_settings.stderr.exclude_namespaces`), los registros de stderr se recopilarán a partir de todos los contenedores en todos los pods o nodos del clúster. Si no se especifica en ConfigMaps, el valor predeterminado es `enabled = true`. |
|`[log_collection_settings.stderr] exclude_namespaces =` |Cadena |Matriz separada por comas |Matriz de espacios de nombres de Kubernetes para la que no se recopilarán los registros de stderr. Esta configuración es efectiva únicamente si `log_collection_settings.stdout.enabled` se establece en `true`. Si no se especifica en ConfigMap, el valor predeterminado es `exclude_namespaces = ["kube-system"]`. |
| `[log_collection_settings.env_var] enabled =` |Boolean | true o false | Esta configuración controla la colección de variables de entorno en todos los pods/nodos del clúster y el valor predeterminado es `enabled = true` cuando no se especifica en ConfigMaps. Si la colección de variables de entorno está habilitada globalmente, puede deshabilitarla para un contenedor específico al establecer la variable de entorno `AZMON_COLLECT_ENV` en **False** con un valor de Dockerfile o en el [archivo de configuración para el Pod](https://kubernetes.io/docs/tasks/inject-data-application/define-environment-variable-container/) en la sección **env:** . Si la colección de variables de entorno está deshabilitada globalmente, no puede habilitar la colección para un contenedor específico (es decir, la única invalidación que se puede aplicar en el nivel de contenedor es deshabilitar la colección si ya está habilitada globalmente). |

### <a name="prometheus-scraping-settings"></a>Configuración de la recopilación de datos en Prometheus

![Arquitectura de supervisión de contenedores para Prometheus](./media/container-insights-agent-config/monitoring-kubernetes-architecture.png)

Azure Monitor para contenedores proporciona una experiencia provechosa para habilitar la recopilación de métricas de Prometheus mediante el uso de los siguientes mecanismos de recopilación de datos, tal como se muestra en la tabla siguiente. Las métricas se recopilan mediante un conjunto de opciones de configuración especificadas en un único archivo ConfigMap, el mismo que se usa para configurar la recopilación de stdout, stderr y variables de entorno a partir de cargas de trabajo de contenedor. 

La recopilación de datos activa de métricas de Prometheus se realiza desde una de estas dos perspectivas:

* En todo el clúster: la dirección URL de HTTP y detección de destinos en la lista de puntos de conexión de un servicio, servicios K8S como kube-dns y kube-state-metrics, y anotaciones pod específicas de una aplicación. Las métricas recopiladas en este contexto se definirán en la sección ConfigMap *[Prometheus data_collection_settings.cluster]* .
* En todo el nodo: la dirección URL de HTTP y detección de destinos en la lista de puntos de conexión de un servicio. Las métricas recopiladas en este contexto se definirán en la sección ConfigMap *[Prometheus data_collection_settings.node]* .

| Punto de conexión | Ámbito | Ejemplo |
|----------|-------|---------|
| Anotación de pod | En todo el clúster | anotaciones: <br>`prometheus.io/scrape: "true"` <br>`prometheus.io/path: "/mymetrics"` <br>`prometheus.io/port: "8000" <br>prometheus.io/scheme: "http"` |
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
| | `prometheus.io/port` | Cadena | 9102 | Especificar un puerto para escuchar. Si no se establece un puerto, se usará el valor predeterminado 9102. |
| En todo el nodo | `urls` | Cadena | Matriz separada por comas | Punto de conexión HTTP (dirección IP o ruta de acceso de dirección URL válida especificada). Por ejemplo: `urls=[$NODE_IP/metrics]`. ($NODE _IP es específico de Azure Monitor para el parámetro Containers y se puede usar en lugar de la dirección IP del nodo. Tiene que estar todo en mayúsculas). |
| En todo el nodo o en todo el clúster | `interval` | Cadena | 60 s | El valor predeterminado del intervalo de recopilación es de un minuto (60 segundos). Puede modificar la recopilación de *[prometheus_data_collection_settings. node]* y/o *[prometheus_data_collection_settings. cluster]* a unidades de tiempo como ns, us (o Âµs), ms, s, m, h. |
| En todo el nodo o en todo el clúster | `fieldpass`<br> `fielddrop`| Cadena | Matriz separada por comas | Puede especificar determinadas métricas para que se recopilen o no desde el punto de conexión estableciendo la lista de permitir (`fieldpass`) y no permitir (`fielddrop`). Tiene que establecer primero la lista de permitir. |

ConfigMaps es una lista global y solo puede haber una instancia de ConfigMap aplicada al agente. No puede tener otra instancia de ConfigMaps que anule las colecciones.

## <a name="configure-and-deploy-configmaps"></a>Configuración e implementación de ConfigMaps

Realice los pasos siguientes para configurar e implementar el archivo de configuración ConfigMap en el clúster.

1. [Descargue](https://github.com/microsoft/OMS-docker/blob/ci_feature_prod/Kubernetes/container-azm-ms-agentconfig.yaml) el archivo YAML ConfigMap de plantilla y guárdelo como container-azm-ms-agentconfig.yaml.  
1. Edite el archivo YAML ConfigMap con sus personalizaciones.

    - Para excluir espacios de nombres específicos para la recopilación de registros de stdout, configure el par clave/valor en el ejemplo siguiente: `[log_collection_settings.stdout] enabled = true exclude_namespaces = ["my-namespace-1", "my-namespace-2"]`.
    
    - Para deshabilitar la recopilación de variables de entorno para un contenedor específico, establezca el par clave/valor `[log_collection_settings.env_var] enabled = true` para habilitar la recopilación de variables globalmente y luego siga [estos](container-insights-manage-agent.md#how-to-disable-environment-variable-collection-on-a-container) pasos para completar la configuración para el contenedor específico.
    
    - Para deshabilitar la recopilación de registros de stderr en todo el clúster, configure el par clave/valor mediante el siguiente ejemplo: `[log_collection_settings.stderr] enabled = false`.
    
    - En los siguientes ejemplos se muestra cómo configurar las métricas del archivo ConfigMap a partir de una dirección URL en todo el clúster, desde el nodo DameonSet de un agente y mediante la especificación de una anotación de pod.

        - Recopilación de datos de las métricas de Prometheus a partir de una dirección URL específica en todo el clúster.

        ```
         prometheus-data-collection-settings: |- 
         # Custom Prometheus metrics data collection settings
         [prometheus_data_collection_settings.cluster] 
         interval = "1m"  ## Valid time units are ns, us (or µs), ms, s, m, h.
         fieldpass = ["metric_to_pass1", "metric_to_pass12"] ## specify metrics to pass through 
         fielddrop = ["metric_to_drop"] ## specify metrics to drop from collecting
         urls = ["http://myurl:9101/metrics"] ## An array of urls to scrape metrics from
        ```

        - Recopilación de datos de las métricas de Prometheus a partir de DaemonSet de un agente que se ejecuta en cada nodo del clúster.

        ```
         prometheus-data-collection-settings: |- 
         # Custom Prometheus metrics data collection settings 
         [prometheus_data_collection_settings.node] 
         interval = "1m"  ## Valid time units are ns, us (or µs), ms, s, m, h. 
         # Node level scrape endpoint(s). These metrics will be scraped from agent's DaemonSet running in every node in the cluster 
         urls = ["http://$NODE_IP:9103/metrics"] 
         fieldpass = ["metric_to_pass1", "metric_to_pass2"] 
         fielddrop = ["metric_to_drop"] 
        ```

        - Recopilación de datos de las métricas de Prometheus especificando una anotación de pod.

        ```
         prometheus-data-collection-settings: |- 
         # Custom Prometheus metrics data collection settings
         [prometheus_data_collection_settings.cluster] 
         interval = "1m"  ## Valid time units are ns, us (or µs), ms, s, m, h
         monitor_kubernetes_pods = true #replicaset will scrape Kubernetes pods for the following prometheus annotations: 
          - prometheus.io/scrape:"true" #Enable scraping for this pod 
          - prometheus.io/scheme:"http:" #If the metrics endpoint is secured then you will need to set this to `https`, if not default ‘http’
          - prometheus.io/path:"/mymetrics" #If the metrics path is not /metrics, define it with this annotation. 
          - prometheus.io/port:"8000" #If port is not 9102 use this annotation
        ```

1. Ejecute el siguiente comando kubectl para crear ConfigMap: `kubectl apply -f <configmap_yaml_file.yaml>`.
    
    Ejemplo: `kubectl apply -f container-azm-ms-agentconfig.yaml`. 
    
    El cambio de configuración puede tardar unos minutos en finalizar antes de que surta efecto, y todos los pods de omsagent del clúster se reiniciarán. El reinicio es un reinicio gradual para todos los pods de omsagent; no todos se reinician al mismo tiempo. Cuando los reinicios finalizan, se muestra un mensaje similar a lo siguiente e incluye el resultado: `configmap "container-azm-ms-agentconfig" created`.

Para comprobar que la configuración se aplicó correctamente, use el comando siguiente para revisar los registros de un pod de agente: `kubectl logs omsagent-fdf58 -n=kube-system`. Si hay errores de configuración de los pods de osmagent, la salida mostrará errores similares a los siguientes:

``` 
***************Start Config Processing******************** 
config::unsupported/missing config schema version - 'v21' , using defaults
```

Los errores relacionados con la aplicación de cambios de configuración para Prometheus también están disponibles para su revisión.  Bien desde los registros de un pod del agente que use el mismo comando `kubectl logs`, o desde los registros activos. Los registros activos muestran errores similares a los siguientes:

```
2019-07-08T18:55:00Z E! [inputs.prometheus]: Error in plugin: error making HTTP request to http://invalidurl:1010/metrics: Get http://invalidurl:1010/metrics: dial tcp: lookup invalidurl on 10.0.0.10:53: no such host
```

Los errores impiden que omsagent analice el archivo, lo que hace que se reinicie y use la configuración predeterminada. Después de corregir los errores en ConfigMap, guarde el archivo YAML y aplique ConfigMaps actualizados mediante la ejecución del comando: `kubectl apply -f <configmap_yaml_file.yaml`.

## <a name="applying-updated-configmap"></a>Aplicación de ConfigMap actualizado

Si ya ha implementado ConfigMap para el clúster y desea actualizarlo con una configuración más reciente, puede editar el archivo ConfigMap que usó antes y después aplicarlo usando el mismo comando, `kubectl apply -f <configmap_yaml_file.yaml`.

El cambio de configuración puede tardar unos minutos en finalizar antes de que surta efecto, y todos los pods de omsagent del clúster se reiniciarán. El reinicio es un reinicio gradual para todos los pods de omsagent; no todos se reinician al mismo tiempo. Cuando los reinicios finalizan, se muestra un mensaje similar a lo siguiente e incluye el resultado: `configmap "container-azm-ms-agentconfig" updated`.

## <a name="verifying-schema-version"></a>Comprobación de la versión del esquema

Las versiones de esquema de configuración admitidas están disponibles como anotación de pod (versiones de esquema) en el pod de omsagent. Puede verlas con el siguiente comando kubectl: `kubectl describe pod omsagent-fdf58 -n=kube-system`

La salida será similar a la siguiente con las versiones de esquema de anotación:

```
    Name:           omsagent-fdf58
    Namespace:      kube-system
    Node:           aks-agentpool-95673144-0/10.240.0.4
    Start Time:     Mon, 10 Jun 2019 15:01:03 -0700
    Labels:         controller-revision-hash=589cc7785d
                    dsName=omsagent-ds
                    pod-template-generation=1
    Annotations:    agentVersion=1.10.0.1
                  dockerProviderVersion=5.0.0-0
                    schema-versions=v1 
```

## <a name="review-prometheus-data-usage"></a>Revisión del uso de datos de Prometheus

Para ver las métricas de Prometheus que descartó Azure Monitor, especifique "prometheus" como espacio de nombres. A continuación, se muestra una consulta de ejemplo para ver las métricas de Prometheus desde el espacio de nombres `default` de Kubernetes.

```
InsightsMetrics 
| where Namespace contains "prometheus"
| extend tags=parse_json(Tags)
| where tostring(tags.namespace) == "default" 
```

Los datos de Prometheus también se pueden consultar directamente en función del nombre.

```
InsightsMetrics 
| where Name contains "some_prometheus_metric"
```

Para identificar el volumen de ingesta de cada tamaño de métricas en GB al día con el fin de comprender si es alto, se proporciona la siguiente consulta.

```
InsightsMetrics 
| where Namespace contains "prometheus"
| where TimeGenerated > ago(24h)
| summarize VolumeInGB = (sum(_BilledSize) / (1024 * 1024 * 1024)) by Name
| order by VolumeInGB desc
| render barchart
```
La salida mostrará resultados similares a los siguientes:

![Resultados de la consulta del registro del volumen de ingesta de datos](./media/container-insights-agent-config/log-query-example-usage-03.png)

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

![Resultados de la consulta del registro del volumen de ingesta de datos](./media/container-insights-agent-config/log-query-example-usage-02.png)

Puede encontrar más información sobre cómo supervisar el uso de datos y analizar el costo en [Administrar el uso y los costos con los registros de Azure Monitor](../platform/manage-cost-storage.md).

## <a name="next-steps"></a>Pasos siguientes

Azure Monitor para contenedores no incluye un conjunto de alertas predefinido. Consulte [Creación de alertas de rendimiento con Azure Monitor para contenedores](container-insights-alerts.md) para aprender a crear alertas recomendadas en caso de uso elevado de CPU y memoria a fin de permitir sus operaciones de desarrollo o sus procesos y procedimientos operativos.

- Para continuar aprendiendo a usar Azure Monitor y supervisar otros aspectos de su clúster de AKS, consulte [Comprender el rendimiento del clúster de AKS con Azure Monitor para contenedores](container-insights-analyze.md).

- En los [ejemplos de consultas de registro](container-insights-log-search.md#search-logs-to-analyze-data) encontrará consultas predefinidas y ejemplos para evaluar o personalizar las alertas, la visualización o el análisis de los clústeres.
