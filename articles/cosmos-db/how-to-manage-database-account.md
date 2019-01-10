---
title: Aprenda a administrar cuentas de base de datos en Azure Cosmos DB
description: Aprenda a administrar cuentas de base de datos en Azure Cosmos DB
author: christopheranderson
ms.service: cosmos-db
ms.topic: sample
ms.date: 10/17/2018
ms.author: chrande
ms.openlocfilehash: d3ee6db382f848a029f3e0355abdfe5eecc80656
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/04/2019
ms.locfileid: "54035838"
---
# <a name="manage-database-accounts-in-azure-cosmos-db"></a>Administración de cuentas de base de datos en Azure Cosmos DB

Este artículo describe cómo administrar la cuenta de Azure Cosmos DB. Aprenderá a configurar el hospedaje múltiple, agregar o quitar una región, configurar varias regiones de escritura y configurar las prioridades de la conmutación por error. 

## <a name="create-a-database-account"></a>Creación de una cuenta de base de datos

### <a id="create-database-account-via-portal"></a>Azure Portal

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

### <a id="create-database-account-via-cli"></a>Azure CLI

```bash
# Create an account
az cosmosdb create --name <Azure Cosmos account name> --resource-group <Resource Group Name>
```

## <a name="configure-clients-for-multi-homing"></a>Configuración de los clientes para el hospedaje múltiple

### <a id="configure-clients-multi-homing-dotnet"></a>SDK para .NET

```csharp
// Create a new connection policy.
ConnectionPolicy policy = new ConnectionPolicy
    {
        // Note: These aren't required settings for multi-homing,
        // just suggested defaults.
        ConnectionMode = ConnectionMode.Direct,
        ConnectionProtocol = Protocol.Tcp,
        UseMultipleWriteLocations = true,
    };
// Add regions to preferred locations.
// The name of the location will match what you see in the portal, etc.
policy.PreferredLocations.Add("East US");
policy.PreferredLocations.Add("North Europe");

// Pass the connection policy with the preferred locations on it to the client.
DocumentClient client = new DocumentClient(new Uri(this.accountEndpoint), this.accountKey, policy);
```

### <a id="configure-clients-multi-homing-java-async"></a>SDK asincrónico para Java

```java
ConnectionPolicy policy = new ConnectionPolicy();
policy.setPreferredLocations(Collections.singleton("West US"));
AsyncDocumentClient client =
        new AsyncDocumentClient.Builder()
                .withMasterKey(this.accountKey)
                .withServiceEndpoint(this.accountEndpoint)
                .withConnectionPolicy(policy).build();
```

### <a id="configure-clients-multi-homing-java-sync"></a>SDK sincrónico para Java

```java
ConnectionPolicy connectionPolicy = new ConnectionPolicy();
Collection<String> preferredLocations = new ArrayList<String>();
preferredLocations.add("Australia East");
connectionPolicy.setPreferredLocations(preferredLocations);
DocumentClient client = new DocumentClient(accountEndpoint, accountKey, connectionPolicy);
```

### <a id="configure-clients-multi-homing-javascript"></a>SDK para Node.js/JavaScript/TypeScript

```javascript
// Set up the connection policy with your preferred regions.
const connectionPolicy: ConnectionPolicy = new ConnectionPolicy();
connectionPolicy.PreferredLocations = ["West US", "Australia East"];

// Pass that connection policy to the client.
const client = new CosmosClient({
  endpoint: config.endpoint,
  auth: { masterKey: config.key },
  connectionPolicy
});
```

### <a id="configure-clients-multi-homing-python"></a>SDK para Python

```python
connection_policy = documents.ConnectionPolicy()
connection_policy.PreferredLocations = ['West US', 'Japan West']
client = cosmos_client.CosmosClient(self.account_endpoint, {'masterKey': self.account_key}, connection_policy)

```

## <a name="addremove-regions-from-your-database-account"></a>Incorporación o eliminación de regiones de una cuenta de base de datos

### <a id="add-remove-regions-via-portal"></a>Azure Portal

1. Vaya a la cuenta de Azure Cosmos DB y abra el menú **Replicar datos globalmente**.

2. Para agregar regiones, seleccione los hexágonos en el mapa con la etiqueta **+** que corresponde a la región deseada. Para agregar una región, seleccione la opción **+ Agregar región** y elija una región en el menú desplegable.

3. Para quitar regiones, desactive una o varias regiones del mapa; para ello, seleccione los hexágonos azules con marcas de verificación. O seleccione el icono de "Papelera" (🗑) junto a la región en el lado derecho.

4. Seleccione **Aceptar** para guardar los cambios.

   ![Menú Agregar o eliminar regiones](./media/how-to-manage-database-account/add-region.png)

En el modo de escritura de una sola región, no puede eliminar la región de escritura. Debe realizar la conmutación por error a otra región antes de poder eliminar la región de escritura actual.

En el modo de escritura de varias regiones, puede agregar o quitar cualquier región, siempre que tenga al menos una región.

### <a id="add-remove-regions-via-cli"></a>Azure CLI

```bash
# Given an account created with 1 region like so
az cosmosdb create --name <Azure Cosmos account name> --resource-group <Resource Group name> --locations 'eastus=0'

# Add a new region by adding another region to the list
az cosmosdb update --name <Azure Cosmos account name> --resource-group <Resource Group name> --locations 'eastus=0 westus=1'

# Remove a region by removing a region from the list
az cosmosdb update --name <Azure Cosmos account name> --resource-group <Resource Group name> --locations 'westus=0'
```

## <a name="configure-multiple-write-regions"></a>Configuración de varias regiones de escritura

### <a id="configure-multiple-write-regions-portal"></a>Azure Portal

Cuando cree una cuenta de base de datos, asegúrese de que el valor **Multi-region Writes** (Operaciones de escritura en varias regiones) está habilitado.

![Captura de pantalla de creación de una cuenta de Azure Cosmos](./media/how-to-manage-database-account/account-create.png)

### <a id="configure-multiple-write-regions-cli"></a>Azure CLI

```bash
az cosmosdb create --name <Azure Cosmos account name> --resource-group <Resource Group name> --enable-multiple-write-locations true
```

### <a id="configure-multiple-write-regions-arm"></a>plantilla de Resource Manager

El siguiente código JSON es un ejemplo de plantilla de Azure Resource Manager. Puede usarlo para implementar una cuenta de Azure Cosmos DB con una directiva de coherencia de obsolescencia limitada. El intervalo de obsolescencia máxima se establece en 5 segundos. El número máximo de solicitudes obsoletas toleradas se establece en 100. Para obtener información sobre el formato y la sintaxis de una plantilla de Resource Manager, consulte [Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md).

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "name": {
            "type": "String"
        },
        "location": {
            "type": "String"
        },
        "locationName": {
            "type": "String"
        },
        "defaultExperience": {
            "type": "String"
        }
    },
    "resources": [
        {
            "type": "Microsoft.DocumentDb/databaseAccounts",
            "kind": "GlobalDocumentDB",
            "name": "[parameters('name')]",
            "apiVersion": "2015-04-08",
            "location": "[parameters('location')]",
            "tags": {
                "defaultExperience": "[parameters('defaultExperience')]"
            },
            "properties": {
                "databaseAccountOfferType": "Standard",
                "consistencyPolicy": {
                    "defaultConsistencyLevel": "BoundedStaleness",
                    "maxIntervalInSeconds": 5,
                    "maxStalenessPrefix": 100
                },
                "locations": [
                    {
                        "id": "[concat(parameters('name'), '-', parameters('location'))]",
                        "failoverPriority": 0,
                        "locationName": "[parameters('locationName')]"
                    }
                ],
                "isVirtualNetworkFilterEnabled": false,
                "enableMultipleWriteLocations": true,
                "virtualNetworkRules": [],
                "dependsOn": []
            }
        }
    ]
}
```


## <a id="manual-failover"></a>Habilitación de la conmutación por error manual de la cuenta de Azure Cosmos DB

### <a id="enable-manual-failover-via-portal"></a>Azure Portal

1. Vaya a la cuenta de Azure Cosmos DB y abra el menú **Replicar datos globalmente**.

2. En la parte superior del menú, seleccione **Conmutación por error manual**.

   ![Menú Replicar datos globalmente](./media/how-to-manage-database-account/replicate-data-globally.png)

3. En el menú **Conmutación por error manual**, seleccione la nueva región de escritura. Active la casilla de verificación para indicar que comprende que esta opción cambia la región de escritura.

4. Para desencadenar la conmutación por error, seleccione **Aceptar**.

   ![Menú Conmutación por error del portal](./media/how-to-manage-database-account/manual-failover.png)

### <a id="enable-manual-failover-via-cli"></a>Azure CLI

```bash
# Given your account currently has regions with priority like so: 'eastus=0 westus=1'
# Change the priority order to trigger a failover of the write region
az cosmosdb update --name <Azure Cosmos account name> --resource-group <Resource Group name> --locations 'eastus=1 westus=0'
```

## <a id="automatic-failover"></a>Habilitación de la conmutación automática por error de la cuenta de Azure Cosmos DB

### <a id="enable-automatic-failover-via-portal"></a>Azure Portal

1. En la cuenta de Azure Cosmos DB, abra el panel **Replicar datos globalmente**. 

2. En la parte superior del panel, seleccione **Conmutación automática por error**.

   ![Menú Replicar datos globalmente](./media/how-to-manage-database-account/replicate-data-globally.png)

3. En el panel **Conmutación automática por error**, asegúrese de que **Habilitar la conmutación automática por error** está establecido en **Activado**. 

4. Seleccione **Guardar**.

   ![Menú Conmutación por error automática del portal](./media/how-to-manage-database-account/automatic-failover.png)

En este menú también se pueden establecer las prioridades de la conmutación por error.

### <a id="enable-automatic-failover-via-cli"></a>Azure CLI

```bash
# Enable automatic failover on account creation
az cosmosdb create --name <Azure Cosmos account name> --resource-group <Resource Group name> --enable-automatic-failover true

# Enable automatic failover on an existing account
az cosmosdb update --name <Azure Cosmos account name> --resource-group <Resource Group name> --enable-automatic-failover true

# Disable automatic failover on an existing account
az cosmosdb update --name <Azure Cosmos account name> --resource-group <Resource Group name> --enable-automatic-failover false
```

## <a name="set-failover-priorities-for-your-azure-cosmos-db-account"></a>Establecimiento de las prioridades de la conmutación por error para la cuenta de Azure Cosmos DB

### <a id="set-failover-priorities-via-portal"></a>Azure Portal

1. En la cuenta de Azure Cosmos DB, abra el panel **Replicar datos globalmente**. 

2. En la parte superior del panel, seleccione **Conmutación automática por error**.

   ![Menú Replicar datos globalmente](./media/how-to-manage-database-account/replicate-data-globally.png)

3. En el panel **Conmutación automática por error**, asegúrese de que **Habilitar la conmutación automática por error** está establecido en **Activado**. 

4. Para modificar la prioridad de la conmutación por error, arrastre las regiones de lectura mediante los tres puntos del lado izquierdo de la fila que aparecen al mantener el puntero sobre ellos. 

5. Seleccione **Guardar**.

   ![Menú Conmutación por error automática del portal](./media/how-to-manage-database-account/automatic-failover.png)

En este menú no se puede modificar la región de escritura. Para cambiar la región de escritura de forma manual, es preciso realizar una conmutación por error manual.

### <a id="set-failover-priorities-via-cli"></a>Azure CLI

```bash
az cosmosdb failover-priority-change --name <Azure Cosmos account name> --resource-group <Resource Group name> --failover-policies 'eastus=0 westus=2 southcentralus=1'
```

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre cómo administrar los conflictos de datos y los niveles de coherencia en Azure Cosmos DB. Consulte los artículos siguientes:

* [Administración de la coherencia](how-to-manage-consistency.md)
* [Administración de conflictos entre regiones](how-to-manage-conflicts.md)

