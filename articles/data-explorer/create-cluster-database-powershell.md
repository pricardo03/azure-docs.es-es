---
title: 'Inicio rápido: Creación de un clúster y una base de datos de Azure Data Explorer mediante PowerShell'
description: Aprenda a crear un clúster y una base de datos de Azure Data Explorer mediante PowerShell
services: data-explorer
author: oflipman
ms.author: oflipman
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: quickstart
ms.date: 03/17/2019
ms.openlocfilehash: 650bdc5cdf99645bc2be6c8e85737dacd10a6b27
ms.sourcegitcommit: 8a59b051b283a72765e7d9ac9dd0586f37018d30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2019
ms.locfileid: "58287386"
---
# <a name="create-an-azure-data-explorer-cluster-and-database-by-using-powershell"></a>Creación de un clúster y una base de datos de Azure Data Explorer mediante PowerShell

> [!div class="op_single_selector"]
> * [Portal](create-cluster-database-portal.md)
> * [CLI](create-cluster-database-cli.md)
> * [PowerShell](create-cluster-database-powershell.md)
> * [C#](create-cluster-database-csharp.md)
> * [Python](create-cluster-database-python.md)
>  


En este inicio rápido se describe cómo crear un clúster y una base de datos de Azure Data Explorer mediante PowerShell.

Puede ejecutar los scripts y cmdlets de PowerShell en Windows, Linux, o en [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) para crear y configurar [Azure Data Explorer](https://docs.microsoft.com/azure/kusto/ ).

[**Az.Kusto**](https://docs.microsoft.com/powershell/module/az.kusto/?view=azps-1.4.0#kusto ). Con Azure PowerShell y **Az.Kusto**, puede realizar las siguientes tareas:

## <a name="prerequisites"></a>Requisitos previos

Para completar esta guía de inicio rápido, necesita una suscripción de Azure. Si no tiene una, [cree una cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="configure-parameters"></a>Configuración de parámetros

Los pasos siguientes no son necesarios si ejecuta comandos en Azure Cloud Shell. Si ejecuta la CLI localmente, realice los pasos siguientes para iniciar sesión en Azure y establecer su suscripción actual:

1. Ejecute el siguiente comandos para iniciar sesión en Azure:

    ```azurepowershell-interactive
    Connect-AzAccount
    ```

2. Establezca la suscripción donde quiere que se cree el clúster.

    ```azurepowershell-interactive
     Set-AzContext -SubscriptionId "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ```
3. Instale el módulo Az.Kusto en el dispositivo:
    
    ```azurepowershell-interactive
     Install-Module -Name Az.Kusto  
    ```

## <a name="create-the-azure-data-explorer-cluster"></a>Creación del clúster de Azure Data Explorer

1. Cree el clúster mediante el siguiente comando:

    ```azurepowershell-interactive
     New-AzKustoCluster -ResourceGroupName testrg -Name mykustocluster -Location 'Central US' -Sku D13_v2 -Capacity 10
    ```

   |**Configuración** | **Valor sugerido** | **Descripción del campo**|
   |---|---|---|
   | NOMBRE | *mykustocluster* | Nombre que quiere para el clúster.|
   | SKU | *D13_v2* | La SKU que se usará para el clúster. |
   | ResourceGroupName | *testrg* | Nombre del grupo de recursos en el que se creará el clúster. |

    Hay varios parámetros opcionales que puede usar, como la capacidad del clúster, etcétera.

2. Ejecute el siguiente comando para comprobar si el clúster se creó correctamente:

    ```azurepowershell-interactive
    Get-AzKustoCluster -Name mykustocluster --ResourceGroupName testrg
    ```

Si el resultado contiene `provisioningState` con el valor `Succeeded`, significa que el clúster se ha creado correctamente.

## <a name="create-the-database-in-the-azure-data-explorer-cluster"></a>Creación de la base de datos en el clúster de Azure Data Explorer

1. Cree la base de datos con el siguiente comando:

    ```azurepowershell-interactive
    New-AzKustoDatabase -ResourceGroupName testrg -ClusterName mykustocluster -Name mykustodatabase -SoftDeletePeriod 3650:00:00:00 -HotCachePeriod 3650:00:00:00
    ```

   |**Configuración** | **Valor sugerido** | **Descripción del campo**|
   |---|---|---|
   | ClusterName | *mykustocluster* | Nombre del clúster donde se creará la base de datos.|
   | NOMBRE | *mykustodatabase* | Nombre de la base de datos.|
   | ResourceGroupName | *testrg* | Nombre del grupo de recursos en el que se creará el clúster. |
   | SoftDeletePeriod | *3650:00:00:00* | Cantidad de tiempo que los datos estarán disponibles para consulta. |
   | HotCachePeriod | *3650:00:00:00* | Cantidad de tiempo que los datos se conservarán en la caché. |

2. Ejecute el siguiente comando para ver la base de datos que ha creado:

    ```azurepowershell-interactive
    Get-AzKustoDatabase -ClusterName mykustocluster --ResourceGroupName testrg -Name mykustodatabase
    ```

Ahora cuenta con un clúster y una base de datos.

## <a name="clean-up-resources"></a>Limpieza de recursos

* Si tiene previsto seguir nuestros tutoriales y guías de inicio rápido, conserve los recursos que creó.
* Para limpiar los recursos, elimine el clúster. Cuando se elimina un clúster, también se eliminan todas las bases de datos en él. Use el siguiente comando para eliminar el clúster:

    ```azurepowershell-interactive
    Remove-AzKustoCluster -ResourceGroupName testrg -Name mykustocluster
    ```

## <a name="next-steps"></a>Pasos siguientes

[**Aquí**](https://docs.microsoft.com/powershell/module/az.kusto/?view=azps-1.4.0#kusto ) puede encontrar más comandos de Az.Kusto

> [!div class="nextstepaction"]
> [Inicio rápido: Ingesta de datos mediante el SDK de .NET Standard de Azure Data Explorer (versión preliminar)](net-standard-ingest-data.md)
