---
title: Creación de un clúster y una base de datos de Azure Data Explorer mediante Python
description: Aprenda a crear un clúster y una base de datos de Azure Data Explorer mediante Python.
author: oflipman
ms.author: oflipman
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
ms.date: 06/03/2019
ms.openlocfilehash: caac53aec08e234f320ee4278a5a58bbc62bff68
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66494576"
---
# <a name="create-an-azure-data-explorer-cluster-and-database-by-using-python"></a>Creación de un clúster y una base de datos de Azure Data Explorer mediante Python

> [!div class="op_single_selector"]
> * [Portal](create-cluster-database-portal.md)
> * [CLI](create-cluster-database-cli.md)
> * [PowerShell](create-cluster-database-powershell.md)
> * [C#](create-cluster-database-csharp.md)
> * [Python](create-cluster-database-python.md)
>  

Azure Data Explorer es un servicio de análisis de datos rápido y totalmente administrado para analizar en tiempo real grandes volúmenes de datos de que se transmiten desde aplicaciones, sitios web, dispositivos IoT, etc. Para usar Azure Data Explorer, cree primero un clúster y una o varias bases de datos en ese clúster. A continuación, ingerirá (cargará) los datos en una base de datos para que pueda ejecutar consultas en ella. En este artículo, se crean un clúster y una base de datos mediante Python.

## <a name="prerequisites"></a>Requisitos previos

Si no tiene una suscripción a Azure, cree una [cuenta gratuita de Azure](https://azure.microsoft.com/free/) antes de empezar.

## <a name="install-python-package"></a>Instalación del paquete de Python

Para instalar el paquete de Python correspondiente a Azure Data Explorer (Kusto), abra un símbolo del sistema que tenga Python en su ruta de acceso. Ejecute este comando:

```
pip install azure-mgmt-kusto
```

## <a name="create-the-azure-data-explorer-cluster"></a>Creación del clúster de Azure Data Explorer

1. Cree el clúster mediante el siguiente comando:

    ```Python
    from azure.mgmt.kusto.kusto_management_client import KustoManagementClient
    from azure.mgmt.kusto.models import Cluster, AzureSku

    credentials = xxxxxxxxxxxxxxx
    
    subscription_id = 'xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxx'
    location = 'Central US'
    sku = 'D13_v2'
    capacity = 5
    resource_group_name = 'testrg'
    cluster_name = 'mykustocluster'
    cluster = Cluster(location=location, sku=AzureSku(name=sku, capacity=capacity))
    
    kustoManagementClient = KustoManagementClient(credentials, subscription_id)
    
    cluster_operations = kustoManagementClient.clusters
    
    cluster_operations.create_or_update(resource_group_name, cluster_name, cluster)
    ```

   |**Configuración** | **Valor sugerido** | **Descripción del campo**|
   |---|---|---|
   | cluster_name | *mykustocluster* | Nombre que quiere para el clúster.|
   | sku | *D13_v2* | La SKU que se usará para el clúster. |
   | resource_group_name | *testrg* | Nombre del grupo de recursos en el que se creará el clúster. |

    Hay varios parámetros opcionales que puede usar, como la capacidad del clúster, etcétera.
    
1. Establecimiento de [*las credenciales*](https://docs.microsoft.com/python/azure/python-sdk-azure-authenticate?view=azure-python)

1. Ejecute el siguiente comando para comprobar si el clúster se creó correctamente:

    ```Python
    cluster_operations.get(resource_group_name = resource_group_name, cluster_name= clusterName, custom_headers=None, raw=False)
    ```

Si el resultado contiene `provisioningState` con el valor `Succeeded`, significa que el clúster se ha creado correctamente.

## <a name="create-the-database-in-the-azure-data-explorer-cluster"></a>Creación de la base de datos en el clúster de Azure Data Explorer

1. Cree la base de datos con el siguiente comando:

    ```Python
    from azure.mgmt.kusto.models import Database
    from datetime import timedelta
    
    softDeletePeriod = timedelta(days=3650)
    hotCachePeriod = timedelta(days=3650)
    databaseName="mykustodatabase"
    
    database_operations = kusto_management_client.databases 
    _database = Database(location=location,
                        soft_delete_period=softDeletePeriod,
                        hot_cache_period=hotCachePeriod)
    
    database_operations.create_or_update(resource_group_name = resource_group_name, cluster_name = clusterName, database_name = databaseName, parameters = _database)
    ```

   |**Configuración** | **Valor sugerido** | **Descripción del campo**|
   |---|---|---|
   | cluster_name | *mykustocluster* | Nombre del clúster donde se creará la base de datos.|
   | database_name | *mykustodatabase* | Nombre de la base de datos.|
   | resource_group_name | *testrg* | Nombre del grupo de recursos en el que se creará el clúster. |
   | soft_delete_period | *3650 days, 0:00:00* | Cantidad de tiempo que los datos estarán disponibles para consulta. |
   | hot_cache_period | *3650 days, 0:00:00* | Cantidad de tiempo que los datos se conservarán en la caché. |

1. Ejecute el siguiente comando para ver la base de datos que ha creado:

    ```Python
    database_operations.get(resource_group_name = resource_group_name, cluster_name = clusterName, database_name = databaseName)
    ```

Ahora cuenta con un clúster y una base de datos.

## <a name="clean-up-resources"></a>Limpieza de recursos

* Si tiene previsto seguir nuestros otros artículos, conserve los recursos que creó.
* Para limpiar los recursos, elimine el clúster. Cuando se elimina un clúster, también se eliminan todas las bases de datos en él. Use el siguiente comando para eliminar el clúster:

    ```Python
    cluster_operations.delete(resource_group_name = resource_group_name, cluster_name = clusterName)
    ```

## <a name="next-steps"></a>Pasos siguientes

* [Ingesta de datos mediante la biblioteca de Python de Azure Data Explorer](python-ingest-data.md)
