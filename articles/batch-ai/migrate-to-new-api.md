---
title: Migrar a la API actualizada de Azure Batch AI | Microsoft Docs
description: Cómo actualizar el código y los scripts de Azure Batch AI para usar el área de trabajo y los recursos experimentales
services: batch-ai
documentationcenter: na
author: dlepow
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.custom: ''
ms.service: batch-ai
ms.workload: ''
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 06/08/2018
ms.author: danlep
ms.openlocfilehash: c5e4c1569464d2e204edf13fe7534d80780524e8
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36294967"
---
# <a name="migrate-to-the-updated-batch-ai-api"></a>Migrar a la API actualizada de Batch AI

En la API REST de Batch AI (versión 2018-05-01) y en los SDK y herramientas relacionados de Batch AI, se han introducido cambios significativos y características nuevas.

Si usó una versión anterior de la API de Batch AI, en este artículo se explica cómo modificar su código y scripts para que funcionen con la nueva API. 

## <a name="whats-changing"></a>¿Qué está cambiando?

En respuesta a los comentarios de los clientes, hemos eliminado los límites en la cantidad de trabajos y hemos simplificado la administración de los recursos de Batch AI. La nueva API presenta dos recursos nuevos: *área de trabajo*  y *experimento*. Reúna los trabajos de aprendizaje relacionados en un experimento y organice todos los recursos de Batch AI relacionados (clústeres, servidores de archivos, experimentos, trabajos) en la misma área de trabajo.  

* **Área de trabajo**: es una colección de alto nivel de todos los tipos de recursos de Batch AI. Los clústeres y servidores de archivos se encuentran en un área de trabajo. Las áreas de trabajo suelen separar el trabajo en función de los diferentes grupos o proyectos. Por ejemplo, puede tener un área de trabajo para el desarrollo y otra para las pruebas. Es probable que solo necesite un número limitado de áreas de trabajo por suscripción. 

* **Experimento**: es una colección de trabajos relacionados que se pueden consultar y administrar juntos. Por ejemplo, use un experimento para agrupar todos los trabajos que se realizan como parte de un barrido de optimización de hiperparámetros. 

En la imagen siguiente se muestra un ejemplo de jerarquía de recursos. 

![](./media/migrate-to-new-api/batch-ai-resource-hierarchy.png)

## <a name="monitor-and-manage-existing-resources"></a>Supervisar y administrar los recursos existentes
En cada grupo de recursos en el que ya creó clústeres, trabajos o servidores de archivos de Batch AI, el servicio Batch AI creará un área de trabajo denominado `migrated-<region>` (por ejemplo, `migrated-eastus`) y un experimento denominado `migrated`. Para obtener acceso a trabajos, clústeres o servidores de archivos previamente creados, debe usar el área de trabajo y experimento migrados. 

### <a name="portal"></a>Portal 
Para obtener acceso a trabajos, clústeres o servidores de archivos mediante el portal, primero debe seleccionar el área de trabajo `migrated-<region>`. Después de seleccionar el área de trabajo, realice operaciones como cambiar el tamaño o eliminar un clúster y ver el estado y las salidas del trabajo. 

### <a name="sdks"></a>SDK 
Para obtener acceso a trabajos, clústeres o servidores de archivos creados previamente a través de los SDK de Batch AI, escriba el nombre del área de trabajo y los parámetros del nombre del experimento. 

Si usa un SDK de Python, los cambios relevantes se muestran en los siguientes ejemplos. Estos cambios son similares en otros SDK de Batch AI. 


#### <a name="get-old-cluster"></a>Obtener el clúster antiguo 

```python
cluster = client.clusters.get(resource_group_name, 'migrated-<region>', cluster_name)
```

#### <a name="delete-old-cluster"></a>Eliminar el clúster antiguo 

```python
client.clusters.delete(resource_group_name, 'migrated-<region>', cluster_name)
```

#### <a name="get-old-file-server"></a>Obtener el antiguo servidor de archivos 

```python
cluster = client.fileservers.get(resource_group_name, 'migrated-<region>', fileserver_name)
```

#### <a name="delete-old-file-server"></a>Eliminar el antiguo servidor de archivos  

```python
client.fileservers.delete(resource_group_name, 'migrated-<region>', fileserver_name)
``` 


#### <a name="get-old-job"></a>Obtener el trabajo antiguo 

```python
cluster = client.jobs.get(resource_group_name, 'migrated-<region>', 'migrated', job_name)
```

#### <a name="delete-old-job"></a>Eliminar el trabajo antiguo

```python
client.jobs.delete(resource_group_name, 'migrated-<region>', 'migrated', job_name)
```
 
### <a name="azure-cli"></a>Azure CLI 
 
Cuando se usa la CLI de Azure para obtener acceso a trabajos, clústeres o servidores de archivos que se crearon anteriormente, use los parámetros `-w` y `-e` para proporcionar los nombres del área de trabajo y el experimento. 


#### <a name="get-old-cluster"></a>Obtener el clúster antiguo

```azurecli
az batchai cluster show -g resource-group-name -w migrated-<region> -n cluster-name
```


#### <a name="delete-old-cluster"></a>Eliminar el clúster antiguo 

```azurecli
az batchai cluster delete -g resource-group-name -w migrated-<region> -n cluster-name
```

#### <a name="get-old-file-server"></a>Obtener el antiguo servidor de archivos

```azurecli
az batchai file-server show -g resource-group-name -w migrated-<region> -n fileserver-name
```


#### <a name="delete-old-file-server"></a>Eliminar el antiguo servidor de archivos 

```azurecli
az batchai file-server delete -g resource-group-name -w migrated-<region> -n fileserver-name
``` 


#### <a name="get-old-job"></a>Obtener el trabajo antiguo

```azurecli
az batchai job show -g resource-group-name -w migrated-<region> -e migrated -n job-name
```


#### <a name="delete-old-job"></a>Eliminar el trabajo antiguo 

```azurecli
az batchai job delete -g resource-group-name -w migrated-<region> -e migrated -n job-name
``` 

## <a name="create-batch-ai-resources"></a>Crear recursos de Batch AI 
 
Si usa uno de los SDK de Batch AI existentes, puede continuar usándolo para crear recursos de Batch AI (trabajos, clústers o servidores de archivos) sin realizar cambios en el código. Sin embargo, después de actualizar al nuevo SDK, debe realizar los siguientes cambios.
 
### <a name="create-workspace"></a>Creación del espacio de trabajo 
En función del escenario, puede crear una sola vez un área de trabajo de forma manual a través del portal o la CLI. Si va a usar la CLI, cree un área de trabajo con el siguiente comando: 

```azurecli
az batchai workspace create -g resource-group-name -n workspace-name
```

### <a name="create-experiment"></a>Crear un experimento 


En función del escenario, puede crear una sola vez un experimento de forma manual a través del portal o la CLI. Si va a usar la CLI, cree un experimento con el siguiente comando: 

```azurecli
az batchai experiment create -g resource-group-name -w workspace-name -n experiment-name

```

### <a name="create-clusters-file-servers-and-jobs"></a>Crear clústeres, servidores de archivos y trabajos
 
Si usa el portal para crear trabajos, clústeres o servidores de archivos, el portal le ayudará con la creación y a la hora de proporcionar los parámetros de nombre del área de trabajo y el experimento.

Para crear trabajos, clústeres o servidores de archivos mediante el SDK actualizado, proporcione el parámetro de nombre del área de trabajo. Si usa un SDK de Python, los cambios relevantes se muestran en los siguientes ejemplos. Reemplace *workspace_name* y *experiment_name* con el área de trabajo y el experimento que creó anteriormente. Estos cambios son similares en otros SDK de Batch AI. 


#### <a name="create-cluster"></a>Crear clúster 

```python
_ = client.clusters.create(resource_group_name, workspace_name, cluster_name, cluster_create_parameters).result()
```

#### <a name="create-file-server"></a>Crear un servidor de archivos 

```python
_ = client.fileservers.create(resource_group_name, workspace_name, fileserver_name, fileserver_create_parameters).result()
```

#### <a name="create-job"></a>Creación del trabajo 

```python
_ = client.jobs.create(resource_group_name, workspace_name, experiment_name, job_name job_create_parameters).result()
```


## <a name="next-steps"></a>Pasos siguientes

* Consulte la referencia de API de Batch AI: [CLI](/cli/azure/batchai), [.NET](/dotnet/api/overview/azure/batchai), [Java](/java/api/overview/azure/batchai), [Node.js](/javascript/api/overview/azure/batchai), [Python](/python/api/overview/azure/batchai)y [REST](/rest/api/batchai)