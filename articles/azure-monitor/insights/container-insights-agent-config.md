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
ms.date: 06/22/2019
ms.author: magoedte
ms.openlocfilehash: 1e7506e311c38d87371dd1b65440b6fb41a7ce78
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/24/2019
ms.locfileid: "67341660"
---
# <a name="configure-agent-data-collection-for-azure-monitor-for-containers"></a>Configuración de la recopilación de datos del agente para Azure Monitor para contenedores

Azure Monitor para contenedores recopila stdout, stderr y las variables de entorno a partir de cargas de trabajo de contenedor implementadas en clústeres de Kubernetes administrado hospedados en Azure Kubernetes Service (AKS) desde el agente con contenedores. Puede definir la configuración de recopilación de datos del agente mediante la creación de ConfigMaps de Kubernetes personalizados para controlar esta experiencia. En este artículo se muestra cómo crear ConfigMap y configurar la recopilación de datos en función de sus requisitos.

## <a name="configure-your-cluster-with-custom-data-collection-settings"></a>Configuración del clúster con los parámetros de recopilación de datos personalizados

Se proporciona un archivo ConfigMap de plantilla que le permite editarlo fácilmente con sus personalizaciones sin tener que crearlo desde cero. Antes de comenzar, debe revisar la documentación de Kubernetes sobre [ConfigMaps](https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/) y familiarizarse con la forma de crear, configurar e implementar ConfigMaps. Esto le permitirá filtrar stderr y stdout por espacio de nombres o a través de todo el clúster, y las variables de entorno para cualquier contenedor que se ejecute en todos los pods o nodos del clúster.

>[!IMPORTANT]
>La versión mínima del agente compatible con esta función es microsoft/oms:ciprod06142019 o posterior. 

### <a name="overview-of-configurable-data-collection-settings"></a>Introducción a la configuración de la recopilación de datos configurable

A continuación figura la configuración que puede definir para controlar la recolección de datos.

|Clave |Tipo de datos |Valor |DESCRIPCIÓN |
|----|----------|------|------------|
|`schema-version` |Cadena (distingue mayúsculas de minúsculas) |v1 |Se trata de la versión del esquema que el agente utiliza al analizar este ConfigMap. La versión de esquema que se admite actualmente es v1. No se admite la modificación de este valor y se rechazará cuando ConfigMap se evalúe.|
|`config-version` |Cadena | | Permite realizar un seguimiento de la versión de este archivo de configuración en el sistema o repositorio de control de código fuente. El número máximo de caracteres permitido es 10 y todos los demás caracteres se truncan. |
|`[log_collection_settings.stdout] enabled =` |Boolean | true o false | Controla si está habilitada la recopilación de registros de contenedor de stdout. Cuando se establece en `true` y los espacios de nombres no se excluyen de la recopilación de registros de stdout (configuración `log_collection_settings.stdout.exclude_namespaces` más abajo), los registros de stdout se recopilarán a partir de todos los contenedores en todos los pods o nodos del clúster. Si no se especifica en ConfigMaps, el valor predeterminado es `enabled = true`. |
|`[log_collection_settings.stdout] exclude_namespaces =`|Cadena | Matriz separada por comas |Matriz de espacios de nombres de Kubernetes para la que no se recopilarán los registros de stdout. Esta configuración es efectiva únicamente si `log_collection_settings.stdout.enabled` se establece en `true`. Si no se especifica en ConfigMap, el valor predeterminado es `exclude_namespaces = ["kube-system"]`.|
|`[log_collection_settings.stderr] enabled =` |Boolean | true o false |Controla si está habilitada la recopilación de registros de contenedor de stderr. Cuando se establece en `true` y los espacios de nombres no se excluyen de la recopilación de registros de stdout (configuración `log_collection_settings.stderr.exclude_namespaces`), los registros de stderr se recopilarán a partir de todos los contenedores en todos los pods o nodos del clúster. Si no se especifica en ConfigMaps, el valor predeterminado es `enabled = true`. |
|`[log_collection_settings.stderr] exclude_namespaces =` |Cadena |Matriz separada por comas |Matriz de espacios de nombres de Kubernetes para la que no se recopilarán los registros de stderr. Esta configuración es efectiva únicamente si `log_collection_settings.stdout.enabled` se establece en `true`. Si no se especifica en ConfigMap, el valor predeterminado es `exclude_namespaces = ["kube-system"]`. |
| `[log_collection_settings.env_var] enabled =` |Boolean | true o false | Controla si se habilita la recopilación de variables de entorno. Cuando se establece en `false`, no se recopilan variables de entorno para ningún contenedor que se ejecuta en todos los pods o nodos del clúster. Si no se especifica en ConfigMap, el valor predeterminado es `enabled = true`. |

### <a name="configure-and-deploy-configmaps"></a>Configuración e implementación de ConfigMaps

Realice los pasos siguientes para configurar e implementar el archivo de configuración ConfigMap en el clúster.

1. [Descargue](https://github.com/microsoft/OMS-docker/blob/ci_feature_prod/Kubernetes/container-azm-ms-agentconfig.yaml) el archivo YAML ConfigMap de plantilla y guárdelo como container-azm-ms-agentconfig.yaml.  
1. Edite el archivo YAML ConfigMap con sus personalizaciones. 

    - Para excluir espacios de nombres específicos para la recopilación de registros de stdout, configure el par clave/valor en el ejemplo siguiente: `[log_collection_settings.stdout] enabled = true exclude_namespaces = ["my-namespace-1", "my-namespace-2"]`.
    - Para deshabilitar la recopilación de variables de entorno para un contenedor específico, establezca el par clave/valor `[log_collection_settings.env_var] enabled = true` para habilitar la recopilación de variables globalmente y luego siga [estos](container-insights-manage-agent.md#how-to-disable-environment-variable-collection-on-a-container) pasos para completar la configuración para el contenedor específico.
    - Para deshabilitar la recopilación de registros de stderr en todo el clúster, configure el par clave/valor mediante el siguiente ejemplo: `[log_collection_settings.stderr] enabled = false`.

1. Ejecute el siguiente comando kubectl para crear ConfigMap: `kubectl apply -f <configmap_yaml_file.yaml>`.
    
    Ejemplo: `kubectl apply -f container-azm-ms-agentconfig.yaml`. 
    
    El cambio de configuración puede tardar unos minutos en finalizar antes de que surta efecto, y todos los pods de omsagent del clúster se reiniciarán. El reinicio es un reinicio gradual para todos los pods de omsagent; no todos se reinician al mismo tiempo. Cuando los reinicios finalizan, se muestra un mensaje similar a lo siguiente e incluye el resultado: `configmap "container-azm-ms-agentconfig" created`.

Para comprobar que la configuración se aplicó correctamente, use el comando siguiente para revisar los registros de un pod de agente: `kubectl logs omsagent-fdf58 -n=kube-system`. Si hay errores de configuración de los pods de osmagent, la salida mostrará errores similares a lo siguiente:

``` 
***************Start Config Processing******************** 
config::unsupported/missing config schema version - 'v21' , using defaults
```

Los errores impiden que omsagent analice el archivo, lo que hace que se reinicie y use la configuración predeterminada. Después de corregir los errores en ConfigMap, guarde el archivo YAML y aplique ConfigMaps actualizados mediante la ejecución del comando: `kubectl apply -f <configmap_yaml_file.yaml`.

## <a name="applying-updated-configmap"></a>Aplicación de ConfigMap actualizado

Si ya ha implementado ConfigMap para el clúster y desea actualizarlo con una configuración más reciente, simplemente puede editar el archivo ConfigMap que usó anteriormente y luego aplicarlo usando el mismo comando que antes: `kubectl apply -f <configmap_yaml_file.yaml`.

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

## <a name="next-steps"></a>Pasos siguientes

- Para continuar aprendiendo a usar Azure Monitor y supervisar otros aspectos de su clúster de AKS, consulte [Comprender el rendimiento del clúster de AKS con Azure Monitor para contenedores](container-insights-analyze.md).
- Vea los [ejemplos de consultas de registro](container-insights-log-search.md#search-logs-to-analyze-data) para ver consultas predefinidas y ejemplos para evaluar o personalizar las alertas, la visualización o el análisis de los clústeres.