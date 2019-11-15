---
title: Uso de la característica de base de datos del seguidor para adjuntar bases de datos en Azure Data Explorer
description: Obtenga información sobre cómo adjuntar bases de datos en Azure Data Explorer mediante la característica de base de datos del seguidor.
author: orspod
ms.author: orspodek
ms.reviewer: gabilehner
ms.service: data-explorer
ms.topic: conceptual
ms.date: 11/07/2019
ms.openlocfilehash: a46cf78d902ec8391d7dc3667a6d66daa78927ab
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2019
ms.locfileid: "73828387"
---
# <a name="use-follower-database-to-attach-databases-in-azure-data-explorer"></a>Uso de la base de datos del seguidor para adjuntar bases de datos en Azure Data Explorer

La característica de **base de datos del seguidor** le permite adjuntar una base de datos ubicada en otro clúster al clúster de Azure Data Explorer. La **base de datos del seguidor** se adjunta en modo de *solo lectura*, lo que permite ver los datos y ejecutar consultas en los datos que se han ingerido en la **base de datos del responsable**. La base de datos del seguidor sincroniza los cambios en las bases de datos del responsable. Debido a la sincronización, hay un retraso de entre unos segundos y varios minutos en la disponibilidad de los datos. La duración del retraso depende del tamaño total de los metadatos de la base de datos del responsable. Las bases de datos del responsable y el seguidor usan la misma cuenta de almacenamiento para capturar los datos. El almacenamiento pertenece a la base de datos del responsable. La base de datos del seguidor ve los datos sin necesidad de ingerirlos. Dado que la base de datos adjunta es de solo lectura, los datos, las tablas y las directivas de la base de datos no se pueden modificar, excepto en el caso de la [directiva de almacenamiento en caché](#configure-caching-policy), las [entidades de seguridad](#manage-principals) y los [permisos](#manage-permissions). Las bases de datos adjuntas no se pueden eliminar. Para poder eliminarlas, el responsable o el seguidor deben desasociarlas primero. 

Se adjunta una base de datos a otro clúster mediante la funcionalidad del seguidor como infraestructura para compartir datos entre organizaciones y equipos. La característica es útil para separar los recursos de proceso a fin de proteger un entorno de producción de los casos de uso que no son de producción. El seguidor también se puede usar para asociar el costo del clúster de Azure Data Explorer a la entidad que ejecuta consultas en los datos.

## <a name="which-databases-are-followed"></a>¿Qué bases de datos se siguen?

* Un clúster puede seguir una base de datos, varias o todas las del clúster del responsable. 
* Un solo clúster puede seguir bases de datos de varios clústeres de responsables. 
* Un clúster puede contener bases de datos del seguidor y del responsable.

## <a name="prerequisites"></a>Requisitos previos

1. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.
1. [Cree el clúster y la base de datos](/azure/data-explorer/create-cluster-database-portal) del responsable y el seguidor.
1. [Ingiera datos](/azure/data-explorer/ingest-sample-data) en la base de datos del responsable mediante uno de los diferentes métodos descritos en la [información general sobre la ingesta](/azure/data-explorer/ingest-data-overview).

## <a name="attach-a-database"></a>Adjuntar una base de datos

Puede usar varios métodos para adjuntar una base de datos. En este artículo, se describe cómo adjuntar una base de datos mediante C# o una plantilla de Azure Resource Manager. Para adjuntar una base de datos, debe tener permisos en el clúster del responsable y el clúster del seguidor. Para obtener más información sobre los permisos, consulte [Administración de permisos](#manage-permissions).

### <a name="attach-a-database-using-c"></a>Adjunción de una base de datos mediante C#

**Paquetes NuGet necesarios**

* Instale [Microsoft.Azure.Management.kusto](https://www.nuget.org/packages/Microsoft.Azure.Management.Kusto/).
* Instale [Microsoft.Rest.ClientRuntime.Azure.Authentication para llevar a cabo la autenticación](https://www.nuget.org/packages/Microsoft.Rest.ClientRuntime.Azure.Authentication).


```Csharp
var tenantId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";//Directory (tenant) ID
var clientId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";//Application ID
var clientSecret = "xxxxxxxxxxxxxx";//Client secret
var subscriptionId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";

var serviceCreds = await ApplicationTokenProvider.LoginSilentAsync(tenantId, clientId, clientSecret);
var resourceManagementClient = new ResourceManagementClient(serviceCreds);

var leaderResourceGroupName = "testrg";
var followerResourceGroupName = "followerResouceGroup";
var leaderClusterName = "leader";
var followerClusterName = "follower";
var attachedDatabaseConfigurationName = "adc";
var databaseName = "db" // Can be specific database name or * for all databases
var defaultPrincipalsModificationKind = "Union"; 
var location = "North Central US";

AttachedDatabaseConfiguration attachedDatabaseConfigurationProperties = new AttachedDatabaseConfiguration()
{
    ClusterResourceId = $"/subscriptions/{subscriptionId}/resourceGroups/{followerResourceGroupName}/providers/Microsoft.Kusto/Clusters/{followerClusterName}",
    DatabaseName = databaseName,
    DefaultPrincipalsModificationKind = defaultPrincipalsModificationKind,
    Location = location
};

var attachedDatabaseConfigurations = resourceManagementClient.AttachedDatabaseConfigurations.CreateOrUpdate(followerResourceGroupName, followerClusterName, attachedDatabaseConfigurationName, attachedDatabaseConfigurationProperties);
```

### <a name="attach-a-database-using-an-azure-resource-manager-template"></a>Adjunción de una base de datos mediante una plantilla de Azure Resource Manager

En esta sección, aprenderá a adjuntar una base de datos mediante una [plantilla de Azure Resource Manager](../azure-resource-manager/resource-group-overview.md). 

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "followerClusterName": {
            "type": "string",
            "defaultValue": "",
            "metadata": {
                "description": "Name of the follower cluster."
            }
        },
        "attachedDatabaseConfigurationsName": {
            "type": "string",
            "defaultValue": "",
            "metadata": {
                "description": "Name of the attached database configurations to create."
            }
        },
        "databaseName": {
            "type": "string",
            "defaultValue": "",
            "metadata": {
                "description": "The name of the database to follow. You can follow all databases by using '*'."
            }
        },
        "leaderClusterResourceId": {
            "type": "string",
            "defaultValue": "",
            "metadata": {
                "description": "Name of the leader cluster to create."
            }
        },
        "defaultPrincipalsModificationKind": {
            "type": "string",
            "defaultValue": "",
            "metadata": {
                "description": "The default principal modification kind."
            }
        },
        "location": {
            "type": "string",
            "defaultValue": "",
            "metadata": {
                "description": "Location for all resources."
            }
        }
    },
    "variables": {},
    "resources": [
        {
            "name": "[parameters('followerClusterName')]",
            "type": "Microsoft.Kusto/clusters",
            "sku": {
                "name": "Standard_D13_v2",
                "tier": "Standard",
                "capacity": 2
            },
            "apiVersion": "2019-09-07",
            "location": "[parameters('location')]"
        },
        {
            "name": "[concat(parameters('followerClusterName'), '/', parameters('attachedDatabaseConfigurationsName'))]",
            "type": "Microsoft.Kusto/clusters/attachedDatabaseConfigurations",
            "apiVersion": "2019-09-07",
            "location": "[parameters('location')]",
            "dependsOn": [
                "[resourceId('Microsoft.Kusto/clusters', parameters('followerClusterName'))]"
            ],
            "properties": {
                "databaseName": "[parameters('databaseName')]",
                "clusterResourceId": "[parameters('leaderClusterResourceId')]",
                "defaultPrincipalsModificationKind": "[parameters('defaultPrincipalsModificationKind')]"
            }
        }
    ]
}
```

### <a name="deploy-the-template"></a>Implementación de la plantilla 

Puede implementar la plantilla de Azure Resource Manager [mediante Azure Portal](https://portal.azure.com) o PowerShell.

   ![implementación de la plantilla](media/follower/template-deployment.png)


|**Configuración**  |**Descripción**  |
|---------|---------|
|Nombre del clúster del seguidor     |  Nombre del clúster del seguidor.       |
|Nombre de las opciones de configuración de la base de datos adjunta    |    Nombre del objeto de opciones de configuración de la base de datos adjunta. El nombre debe ser único en el nivel del clúster.     |
|Nombre de la base de datos     |      Nombre de la base de datos que se va a seguir. Si quiere seguir todas las bases de datos del responsable, use "*".   |
|Id. del recurso de clúster del responsable    |   Identificador del recurso de clúster del responsable.      |
|Tipo de modificación de las entidades de seguridad predeterminadas    |   Tipo de modificación de las entidades de seguridad predeterminadas. Puede ser `Union`, `Replace` o `None`. Para obtener más información sobre el tipo de modificación de las entidades de seguridad predeterminadas, consulte el [comando de control del tipo de modificación de entidades de seguridad](/azure/kusto/management/cluster-follower?branch=master#alter-follower-database-principals-modification-kind).      |
|Location   |   Ubicación de todos los recursos. El responsable y el seguidor deben estar en la misma ubicación.       |
 
### <a name="verify-that-the-database-was-successfully-attached"></a>Comprobación de que la base de datos se ha adjuntado correctamente

Para comprobar que la base de datos se ha adjuntado correctamente, busque las bases de datos adjuntas en [Azure Portal](https://portal.azure.com). 

1. Vaya al clúster del seguidor y seleccione **Bases de datos**.
1. Busque las nuevas bases de datos de solo lectura en la lista de bases de datos.

    ![Base de datos del seguidor de solo lectura](media/follower/read-only-follower-database.png)

O bien:

1. Vaya al clúster del responsable y seleccione **Bases de datos**.
2. Compruebe que las bases de datos relevantes estén marcadas como **COMPARTIDO CON OTROS** > **Sí**.

    ![Lectura y escritura de bases de datos adjuntas](media/follower/read-write-databases-shared.png)

## <a name="detach-the-follower-database-using-c"></a>Desasociación de la base de datos del seguidor mediante C# 

### <a name="detach-the-attached-follower-database-from-the-follower-cluster"></a>Desasociación de la base de datos del seguidor adjunta del clúster del seguidor

El clúster del seguidor puede desasociar cualquier base de datos adjunta de la siguiente manera:

```csharp
var tenantId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";//Directory (tenant) ID
var clientId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";//Application ID
var clientSecret = "xxxxxxxxxxxxxx";//Client secret
var subscriptionId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";

var serviceCreds = await ApplicationTokenProvider.LoginSilentAsync(tenantId, clientId, clientSecret);
var resourceManagementClient = new ResourceManagementClient(serviceCreds);

var followerResourceGroupName = "testrg";
//The cluster and database that are created as part of the prerequisites
var followerClusterName = "follower";
var attachedDatabaseConfigurationsName = "adc";

resourceManagementClient.AttachedDatabaseConfigurations.Delete(followerResourceGroupName, followerClusterName, attachedDatabaseConfigurationsName);
```

### <a name="detach-the-attached-follower-database-from-the-leader-cluster"></a>Desasociación de la base de datos del seguidor adjunta del clúster del responsable

El clúster del responsable puede desasociar cualquier base de datos adjunta de la siguiente manera:

```csharp
var tenantId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";//Directory (tenant) ID
var clientId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";//Application ID
var clientSecret = "xxxxxxxxxxxxxx";//Client secret
var subscriptionId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";

var serviceCreds = await ApplicationTokenProvider.LoginSilentAsync(tenantId, clientId, clientSecret);
var resourceManagementClient = new ResourceManagementClient(serviceCreds);

var leaderResourceGroupName = "testrg";
var followerResourceGroupName = "followerResouceGroup";
var leaderClusterName = "leader";
var followerClusterName = "follower";
//The cluster and database that are created as part of the Prerequisites
var followerDatabaseDefinition = new FollowerDatabaseDefinition()
    {
        AttachedDatabaseConfigurationName = "adc",
        ClusterResourceId = $"/subscriptions/{subscriptionId}/resourceGroups/{followerResourceGroupName}/providers/Microsoft.Kusto/Clusters/{followerClusterName}"
    };

resourceManagementClient.Clusters.DetachFollowerDatabases(leaderResourceGroupName, leaderClusterName, followerDatabaseDefinition);
```

## <a name="manage-principals-permissions-and-caching-policy"></a>Administración de entidades de seguridad, permisos y directiva de almacenamiento en caché

### <a name="manage-principals"></a>Administración de entidades de seguridad

Al adjuntar una base de datos, especifique el valor del **tipo de modificación de las entidades de seguridad predeterminadas**. El valor predeterminado es mantener la colección de bases de datos del responsable de [entidades de seguridad autorizadas](/azure/kusto/management/access-control/index.md#authorization).

|**Variante** |**Descripción**  |
|---------|---------|
|**Unión**     |   Las entidades de seguridad de las bases de datos adjuntas incluirán siempre las entidades de seguridad de las bases de datos originales, además de otras nuevas entidades de seguridad adicionales agregadas a la base de datos del seguidor.      |
|**Sustituya**   |    No se heredan las entidades de seguridad de la base de datos original. Se deben crear nuevas entidades de seguridad para la base de datos adjunta. Se debe agregar al menos una entidad de seguridad para bloquear la herencia de la entidad de seguridad.     |
|**None**   |   Las entidades de seguridad de bases de datos adjuntas incluyen solo las entidades de seguridad de la base de datos original sin entidades de seguridad adicionales.      |

Para obtener más información sobre el uso de comandos de control para configurar las entidades de seguridad autorizadas, vea [Comandos de control para administrar el clúster de un seguidor](/azure/kusto/management/cluster-follower.md).

### <a name="manage-permissions"></a>Administración de permisos

La administración de los permisos de bases de datos de solo lectura es igual que para todos los tipos de base de datos. Vea [Administración de permisos en Azure Portal](/azure/data-explorer/manage-database-permissions#manage-permissions-in-the-azure-portal).

### <a name="configure-caching-policy"></a>Configuración de la directiva de almacenamiento en caché

El administrador de la base de datos del seguidor puede modificar la [directiva de almacenamiento en caché](/azure/kusto/management/cache-policy) de la base de datos adjunta o de cualquiera de sus tablas en el clúster de hospedaje. El procedimiento predeterminado es mantener la colección de bases de datos del responsable de las directivas de almacenamiento en caché de nivel de tabla y base de datos. Por ejemplo, puede tener una directiva de almacenamiento en caché de 30 días en la base de datos del responsable para realizar informes mensuales y una directiva de almacenamiento en caché de tres días en la base de datos del seguidor para consultar solo los datos recientes si necesita solucionar problemas. Para obtener más información sobre el uso de comandos de control para configurar la directiva de almacenamiento en caché en la tabla o la base de datos del seguidor, vea [Comandos de control para administrar el clúster de un seguidor](/azure/kusto/management/cluster-follower.md).

## <a name="limitations"></a>Limitaciones

* Los clústeres del seguidor y el responsable deben estar en la misma región.
* No se puede usar la [ingesta de streaming](/azure/data-explorer/ingest-data-streaming) en una base de datos que se está siguiendo.
* No puede eliminar una base de datos que esté asociada a otro clúster antes de desasociarla.
* No puede eliminar un clúster que tenga una base de datos asociada a otro clúster antes de desasociarla.
* No puede detener un clúster que tenga bases de datos del seguidor o el responsable asociadas. 

## <a name="next-steps"></a>Pasos siguientes

* Para obtener información sobre la configuración del clúster del seguidor, consulte [Comandos de control para administrar el clúster de un seguidor](/azure/kusto/management/cluster-follower.md).