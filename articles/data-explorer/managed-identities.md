---
title: Configuración de identidades administradas para el clúster de Azure Data Explorer
description: Obtenga información sobre la configuración de identidades administradas para el clúster de Azure Data Explorer.
author: saguiitay
ms.author: itsagui
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
ms.date: 01/06/2020
ms.openlocfilehash: e76ae2e072bb780ac9788902e9157db871e4f09d
ms.sourcegitcommit: ef568f562fbb05b4bd023fe2454f9da931adf39a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/17/2020
ms.locfileid: "77373373"
---
# <a name="configure-managed-identities-for-your-azure-data-explorer-cluster"></a>Configuración de identidades administradas para el clúster de Azure Data Explorer

Una [identidad administrada de Azure Active Directory](/azure/active-directory/managed-identities-azure-resources/overview) permite al clúster acceder fácilmente a otros recursos protegidos por AAD, como Azure Key Vault. La plataforma Azure administra la identidad y no es necesario que lleve a cabo el aprovisionamiento ni la rotación de los secretos. En este artículo se muestra cómo crear una identidad administrada para los clústeres de Azure Data Explorer. La configuración de identidad administrada solo se admite actualmente para [permitir claves administradas por el cliente en el clúster](/azure/data-explorer/security#customer-managed-keys-with-azure-key-vault).

> [!Note]
> Las identidades administradas de Azure Data Explorer no se comportarán según lo previsto si la aplicación se migra entre suscripciones o inquilinos. La aplicación necesitará obtener una nueva identidad. Para ello, puede deshabilitar y volver a habilitar la característica mediante las indicaciones de [Quitar una identidad](#remove-an-identity). Las directivas de acceso y los recursos de nivel inferior también deberán actualizarse para utilizar la nueva identidad.

## <a name="add-a-system-assigned-identity"></a>Adición de una identidad asignada por el sistema

El clúster puede tener asignada una **identidad asignada por el sistema** que esté asociada al clúster y que se eliminará si se elimina el clúster. Un clúster solo puede tener una identidad asignada por el sistema. Para crear un clúster con una identidad asignada por el sistema se requiere la configuración de una propiedad adicional en el clúster.

### <a name="add-a-system-assigned-identity-using-c"></a>Adición de una identidad asignada por el sistema mediante C#

Para configurar una identidad administrada mediante el cliente de C# de Azure Data Explorer, haga lo siguiente:

* Instale el [paquete NuGet de Azure Data Explorer (Kusto)](https://www.nuget.org/packages/Microsoft.Azure.Management.Kusto/).
* Instale el [paquete NuGet Microsoft.IdentityModel.Clients.ActiveDirectory](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) para la autenticación.
* Para ejecutar el siguiente ejemplo, [cree una aplicación de Azure AD](/azure/active-directory/develop/howto-create-service-principal-portal) y una entidad de servicio con acceso a los recursos. Puede agregar una asignación de roles en el ámbito de la suscripción y obtener los objetos `Directory (tenant) ID`, `Application ID` y `Client Secret`necesarios.

#### <a name="create-or-update-your-cluster"></a>Creación o actualización del clúster

1. Cree o actualice el clúster mediante la propiedad `Identity`:

    ```csharp
    var tenantId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";//Directory (tenant) ID
    var clientId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";//Application ID
    var clientSecret = "xxxxxxxxxxxxxx";//Client Secret
    var subscriptionId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";
    var authenticationContext = new AuthenticationContext($"https://login.windows.net/{tenantId}");
    var credential = new ClientCredential(clientId, clientSecret);
    var result = await authenticationContext.AcquireTokenAsync(resource: "https://management.core.windows.net/", clientCredential: credential);
    
    var credentials = new TokenCredentials(result.AccessToken, result.AccessTokenType);
    
    var kustoManagementClient = new KustoManagementClient(credentials)
    {
        SubscriptionId = subscriptionId
    };
    
    var resourceGroupName = "testrg";
    var clusterName = "mykustocluster";
    var location = "Central US";
    var skuName = "Standard_D13_v2";
    var tier = "Standard";
    var capacity = 5;
    var sku = new AzureSku(skuName, tier, capacity);
    var identity = new Identity(IdentityType.SystemAssigned);
    var cluster = new Cluster(location, sku, identity: identity);
    await kustoManagementClient.Clusters.CreateOrUpdateAsync(resourceGroupName, clusterName, cluster);
    ```
    
2. Ejecute el siguiente comando para comprobar si el clúster se creó o actualizó correctamente con una identidad:

    ```csharp
    kustoManagementClient.Clusters.Get(resourceGroupName, clusterName);
    ```

    Si el resultado contiene `ProvisioningState` con el valor `Succeeded`, el clúster se creó o actualizó y debe tener las siguientes propiedades:
   
    ```csharp
    var principalId = cluster.Identity.PrincipalId;
    var tenantId = cluster.Identity.TenantId;
    ```

    `PrincipalId` y `TenantId` se reemplazan por GUID. La propiedad `TenantId` identifica el inquilino de AAD al que pertenece la identidad. `PrincipalId` es un identificador único para la nueva identidad del clúster. En AAD, la entidad de servicio tiene el mismo nombre que asignó a la instancia de App Service o Azure Functions.

### <a name="add-a-system-assigned-identity-using-an-azure-resource-manager-template"></a>Adición de identidad asignada por el sistema mediante una plantilla de Azure Resource Manager

Se puede utilizar una plantilla de Azure Resource Manager para automatizar la implementación de los recursos de Azure. Para obtener información sobre la implementación en Azure Data Explorer, consulte [Creación de un clúster y una base de datos de Azure Data Explorer mediante una plantilla de Azure Resource Manager](create-cluster-database-resource-manager.md).

Al agregar el tipo asignado por el sistema se indica a Azure que debe crear y administrar la identidad del clúster. Se puede crear cualquier recurso de tipo `Microsoft.Kusto/clusters` con una identidad mediante la inclusión de la siguiente propiedad en la definición de recursos: 

```json
"identity": {
    "type": "SystemAssigned"
}    
```

Por ejemplo:

```json
{
    "apiVersion": "2019-09-07",
    "type": "Microsoft.Kusto/clusters",
    "name": "[variables('clusterName')]",
    "location": "[resourceGroup().location]",
    "identity": {
        "type": "SystemAssigned"
    },
    "properties": {
        "trustedExternalTenants": [],
        "virtualNetworkConfiguration": null,
        "optimizedAutoscale": null,
        "enableDiskEncryption": false,
        "enableStreamingIngest": false,
    }
}
```

Cuando se crea el clúster, tiene las siguientes propiedades adicionales:

```json
"identity": {
    "type": "SystemAssigned",
    "tenantId": "<TENANTID>",
    "principalId": "<PRINCIPALID>"
}
```

`<TENANTID>` y `<PRINCIPALID>` se reemplazan por GUID. La propiedad `TenantId` identifica el inquilino de AAD al que pertenece la identidad. `PrincipalId` es un identificador único para la nueva identidad del clúster. En AAD, la entidad de servicio tiene el mismo nombre que asignó a la instancia de App Service o Azure Functions.

## <a name="remove-an-identity"></a>Quitar una identidad

Al quitar una identidad asignada por el sistema también se eliminará de AAD. Las identidades asignadas por el sistema también se quitan automáticamente de AAD cuando se elimina el recurso del clúster. Para quitar una identidad asignada por el sistema se puede deshabilitar la característica:

```json
"identity": {
    "type": "None"
}
```

## <a name="next-steps"></a>Pasos siguientes

* [Protección de clústeres de Azure Data Explorer en Azure](security.md)
* [Protección del clúster en Azure Data Explorer - Azure Portal](manage-cluster-security.md) mediante la habilitación del cifrado en reposo.
 * [Configuración de claves administradas por el cliente mediante C#](customer-managed-keys-csharp.md)
 * [Configuración de claves administradas por el cliente mediante la plantilla de Azure Resource Manager](customer-managed-keys-resource-manager.md)
