---
title: Creación de un clúster y una base de datos de Azure Data Explorer mediante C#
description: Aprenda a crear un clúster y una base de datos de Azure Data Explorer mediante C#
author: oflipman
ms.author: oflipman
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
ms.date: 06/03/2019
ms.openlocfilehash: 64f16c2ad6fdeeb47b747eab24587b43f3df5130
ms.sourcegitcommit: 4b647be06d677151eb9db7dccc2bd7a8379e5871
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/19/2019
ms.locfileid: "68355944"
---
# <a name="create-an-azure-data-explorer-cluster-and-database-by-using-c"></a>Creación de un clúster y una base de datos de Azure Data Explorer mediante C#

> [!div class="op_single_selector"]
> * [Portal](create-cluster-database-portal.md)
> * [CLI](create-cluster-database-cli.md)
> * [PowerShell](create-cluster-database-powershell.md)
> * [C#](create-cluster-database-csharp.md)
> * [Python](create-cluster-database-python.md)

Azure Data Explorer es un servicio de análisis de datos rápido y totalmente administrado para analizar en tiempo real grandes volúmenes de datos de que se transmiten desde aplicaciones, sitios web, dispositivos IoT, etc. Para usar Azure Data Explorer, cree primero un clúster y una o varias bases de datos en ese clúster. A continuación, ingerirá (cargará) los datos en una base de datos para que pueda ejecutar consultas en ella. En este artículo, se crean un clúster y una base de datos con C#.

## <a name="prerequisites"></a>Requisitos previos

* Si no tiene Visual Studio 2019 instalado, puede descargar y usar la versión **gratis** de [Visual Studio 2019 Community Edition](https://www.visualstudio.com/downloads/). Asegúrese de que habilita **Desarrollo de Azure** durante la instalación de Visual Studio.

* Si no tiene una suscripción a Azure, cree una [cuenta gratuita de Azure](https://azure.microsoft.com/free/) antes de empezar.

## <a name="install-c-nuget"></a>Instalación de NuGet C#

1. Instale el [paquete NuGet de Azure Data Explorer (Kusto)](https://www.nuget.org/packages/Microsoft.Azure.Management.Kusto/).

1. Instale el [paquete NuGet Microsoft.IdentityModel.Clients.ActiveDirectory](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) para la autenticación.

## <a name="create-the-azure-data-explorer-cluster"></a>Creación del clúster de Azure Data Explorer

1. Cree el clúster mediante el siguiente código:

    ```csharp
    var resourceGroupName = "testrg";
    var clusterName = "mykustocluster";
    var location = "Central US";
    var sku = new AzureSku("D13_v2", 5);
    var cluster = new Cluster(location, sku);

    var authenticationContext = new AuthenticationContext("https://login.windows.net/{tenantName}");
    var credential = new ClientCredential(clientId: "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx", clientSecret: "xxxxxxxxxxxxxx");
    var result = await authenticationContext.AcquireTokenAsync(resource: "https://management.core.windows.net/", clientCredential: credential);

    var credentials = new TokenCredentials(result.AccessToken, result.AccessTokenType);

    var kustoManagementClient = new KustoManagementClient(credentials)
    {
        SubscriptionId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx"
    };

    kustoManagementClient.Clusters.CreateOrUpdate(resourceGroupName, clusterName, cluster);
    ```

   |**Configuración** | **Valor sugerido** | **Descripción del campo**|
   |---|---|---|
   | clusterName | *mykustocluster* | Nombre que quiere para el clúster.|
   | sku | *D13_v2* | La SKU que se usará para el clúster. |
   | resourceGroupName | *testrg* | Nombre del grupo de recursos en el que se creará el clúster. |

    Hay varios parámetros opcionales que puede usar, como la capacidad del clúster, etcétera.

1. Establecimiento de [las credenciales](https://docs.microsoft.com/dotnet/azure/dotnet-sdk-azure-authenticate?view=azure-dotnet)

1. Ejecute el siguiente comando para comprobar si el clúster se creó correctamente:

    ```csharp
    kustoManagementClient.Clusters.Get(resourceGroupName, clusterName);
    ```

Si el resultado contiene `ProvisioningState` con el valor `Succeeded`, significa que el clúster se ha creado correctamente.

## <a name="create-the-database-in-the-azure-data-explorer-cluster"></a>Creación de la base de datos en el clúster de Azure Data Explorer

1. Cree la base de datos mediante el siguiente código:

    ```csharp
    var hotCachePeriod = new TimeSpan(3650, 0, 0, 0);
    var softDeletePeriod = new TimeSpan(3650, 0, 0, 0);
    var databaseName = "mykustodatabase";
    var database = new Database(location: location, softDeletePeriod: softDeletePeriod, hotCachePeriod: hotCachePeriod);

    kustoManagementClient.Databases.CreateOrUpdate(resourceGroupName, clusterName, databaseName, database);
    ```

   |**Configuración** | **Valor sugerido** | **Descripción del campo**|
   |---|---|---|
   | clusterName | *mykustocluster* | Nombre del clúster donde se creará la base de datos.|
   | databaseName | *mykustodatabase* | Nombre de la base de datos.|
   | resourceGroupName | *testrg* | Nombre del grupo de recursos en el que se creará el clúster. |
   | softDeletePeriod | *3650:00:00:00* | Cantidad de tiempo que los datos estarán disponibles para consulta. |
   | hotCachePeriod | *3650:00:00:00* | Cantidad de tiempo que los datos se conservarán en la caché. |

2. Ejecute el siguiente comando para ver la base de datos que ha creado:

    ```csharp
    kustoManagementClient.Databases.Get(resourceGroupName, clusterName, databaseName);
    ```

Ahora cuenta con un clúster y una base de datos.

## <a name="clean-up-resources"></a>Limpieza de recursos

* Si tiene previsto seguir nuestros otros artículos, conserve los recursos que creó.
* Para limpiar los recursos, elimine el clúster. Cuando se elimina un clúster, también se eliminan todas las bases de datos en él. Use el siguiente comando para eliminar el clúster:

    ```csharp
    kustoManagementClient.Clusters.Delete(resourceGroupName, clusterName);
    ```

## <a name="next-steps"></a>Pasos siguientes

* [Ingesta de datos mediante el SDK de .NET Standard de Azure Data Explorer (versión preliminar)](net-standard-ingest-data.md)
