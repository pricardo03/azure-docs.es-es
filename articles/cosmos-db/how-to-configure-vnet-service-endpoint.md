---
title: Configuración del acceso basado en red virtual y subred para la cuenta de Azure Cosmos
description: En este documento se describen los pasos necesarios para configurar el punto de conexión de servicio de Virtual Network en Azure Cosmos DB.
author: kanshiG
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 11/06/2018
ms.author: govindk
ms.openlocfilehash: a7c2d1e41fa4ac26854e2e6ab57184cd6ed0bd0c
ms.sourcegitcommit: 0b7fc82f23f0aa105afb1c5fadb74aecf9a7015b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2018
ms.locfileid: "51633689"
---
# <a name="how-to-access-azure-cosmos-db-resources-from-virtual-networks"></a>Acceso a los recursos de Azure Cosmos DB desde redes virtuales

Las cuentas de Azure CosmosDB se pueden configurar para permitir el acceso solo a una subred específica de Azure Virtual Network. Hay dos pasos necesarios para limitar el acceso a la cuenta de Azure Cosmos con conexiones desde una subred de una red virtual (VNET).
 
1. Habilite la subred enviar la identidad de la subred y de la red virtual a Azure Cosmos DB. Para lograrlo, habilite el punto de conexión de servicio para Azure Cosmos DB en la subred específica.

1. Agregue una regla en la cuenta de Cosmos Azure para especificar la subred como origen desde el cual se puede acceder a la cuenta.

> [!NOTE]
> Una vez que el punto de conexión de servicio para su cuenta de Azure Cosmos esté habilitada en una subred, el origen del tráfico que llegue a Azure Cosmos DB cambia de una dirección IP pública a la red virtual y subred. La conmutación de tráfico se aplica a cualquier cuenta de Azure Cosmos a la que se acceda desde esta subred. Si las cuentas de Azure Cosmos tienen firewall basado en IP para permitir esta subred, las solicitudes de la subred habilitada por el servicio ya no coincidirán con las reglas de firewall por IP y se rechazarán. Para más información, consulte los pasos descritos en la sección [Migración desde la regla de firewall por IP a la lista de control de acceso de la red virtual](#migrate-from-firewall-to-vnet) de este artículo. 

En las secciones siguientes se describe cómo configurar el punto de conexión de servicio de red virtual para una cuenta de Azure Cosmos DB.

## <a id="configure-using-portal"></a>Configuración del punto de conexión de servicio en Azure Portal

### <a name="configure-service-endpoint-for-an-existing-azure-virtual-network-and-subnet"></a>Configuración del punto de conexión de servicio para una subred y red virtual de Azure existentes

1. En la hoja **Todos los recursos**, busque la cuenta de Azure Cosmos que desea proteger.

1. Seleccione **Firewalls y redes virtuales** en el menú de configuración y elija Permitir el acceso en **Redes seleccionadas**.

1. Para conceder acceso a la subred de una red virtual existente, en **Redes virtuales**, seleccione **Add existing Azure virtual network** (Agregar red virtual de Azure existente).

1. Seleccione la **suscripción** en la que quiere agregar la red virtual de Azure. Seleccione las **Redes virtuales** y las **Subredes** de Azure a las que desea conceder acceso a la cuenta de Azure Cosmos. A continuación, seleccione **Habilitar** para habilitar las redes seleccionadas con los puntos de conexión de servicio para "Microsoft.AzureCosmosDB". Cuando haya finalizado, seleccione **Agregar**. 

   ![Selección de la red virtual y la subred](./media/how-to-configure-vnet-service-endpoint/choose-subnet-and-vnet.png)


1. Después de que la cuenta de Azure Cosmos esté habilitada para acceder desde una red virtual, solo permitirá el tráfico desde esta subred seleccionada. La red virtual y subred que agregó deben aparecer como se muestra en la captura de pantalla siguiente:

   ![red virtual y subred configuradas correctamente](./media/how-to-configure-vnet-service-endpoint/vnet-and-subnet-configured-successfully.png)

> [!NOTE]
> Para habilitar los puntos de conexión de servicio de red virtual, necesitará los siguientes permisos de suscripción:
  * Suscripción con una red virtual: colaborador de la red
  * Suscripción con la cuenta de Azure Cosmos: colaborador de la cuenta de DocumentDB

### <a name="configure-service-endpoint-for-a-new-azure-virtual-network-and-subnet"></a>Configuración del punto de conexión de servicio para una subred y red virtual de Azure nuevas

1. En la hoja **Todos los recursos**, busque la cuenta de Azure Cosmos DB que desea proteger.  

2. Antes de habilitar el punto de conexión de servicio de Virtual Network, copie la información del firewall para las direcciones IP asociada con la cuenta de Azure Cosmos DB para su futuro uso. Puede volver a habilitar el firewall para las direcciones IP después de configurar el punto de conexión de servicio.  

3. Seleccione **Firewalls y redes virtuales de Azure** en el menú de configuración y elija Permitir el acceso en **Redes seleccionadas**.  

4. Para conceder acceso a la subred de una red virtual de Azure nueva, en Redes virtuales, seleccione **Agregar nueva red virtual**.  

5. Proporcione los detalles necesarios para crear una red virtual y, a continuación, seleccione Crear. La subred se creará con un punto de conexión de servicio para "Microsoft.AzureCosmosDB" habilitado.

   ![Selección de la red virtual y la subred para la red virtual nueva](./media/how-to-configure-vnet-service-endpoint/choose-subnet-and-vnet-new-vnet.png)

Si otros servicios de Azure, como Azure Search, usan la cuenta de Azure Cosmos, o bien si se accede a ella desde Stream Analytics o Power BI, podrá permitir el acceso si marca la opción Permitir el acceso a servicios de Azure.

Para garantizar el acceso a las métricas de Azure Cosmos DB desde Azure Portal, debe habilitar las opciones **Allow access to Azure portal** (Permitir acceso a Azure Portal). Para obtener más información sobre estas opciones, consulte las solicitudes de Azure Portal y las solicitudes de las secciones de servicios de PaaS de Azure del artículo de configuración del [firewall por IP](how-to-configure-firewall.md). Después de seleccionar el acceso, seleccione **Guardar** para guardar la configuración.

## <a id="remove-vnet-or-subnet"></a>Eliminación de una red virtual o subred 

1. En la hoja **Todos los recursos**, busque la cuenta de Azure Cosmos DB a la que ha asignado los puntos de conexión de servicio.  

2. Seleccione **Firewalls y redes virtuales** en el menú de configuración.  

3. Para eliminar una regla de subred o red virtual, seleccione "..." junto a la red virtual o subred y luego **Eliminar**.

   ![Eliminación de una red virtual](./media/how-to-configure-vnet-service-endpoint/remove-a-vnet.png)

4.  Haga clic en **Guardar** para aplicar los cambios.

## <a id="configure-using-powershell"></a>Configuración del punto de conexión de servicio mediante Azure PowerShell 

Use los pasos siguientes para configurar el punto de conexión de servicio de una cuenta de Azure Cosmos DB mediante Azure PowerShell:  

1. Instale la última versión de [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) e [inicie sesión](https://docs.microsoft.com/powershell/azure/authenticate-azureps).  

1. Habilite el punto de conexión de servicio de una subred existente de una red virtual.  

   ```powershell
   $rgname= "<Resource group name>"
   $vnName = "<virtual network name>"
   $sname = "<Subnet name>"
   $subnetPrefix = "<Subnet address range>"

   Get-AzureRmVirtualNetwork `
    -ResourceGroupName $rgname `
    -Name $vnName | Set-AzureRmVirtualNetworkSubnetConfig `
    -Name $sname  `
    -AddressPrefix $subnetPrefix `
    -ServiceEndpoint "Microsoft.AzureCosmosDB" | Set-AzureRmVirtualNetwork
   ```

1. Prepárese para habilitar la ACL en la cuenta de Azure Cosmos; para ello, asegúrese de que la red virtual y la subred tienen el punto de conexión de servicio habilitado para la cuenta.

   ```powershell
   $vnProp = Get-AzureRmVirtualNetwork `
     -Name $vnName  -ResourceGroupName $rgName
   ```

1. Obtenga las propiedades de la cuenta de Azure Cosmos DB mediante la ejecución del siguiente cmdlet:  

   ```powershell
   $apiVersion = "2015-04-08"
   $acctName = "<Azure Cosmos DB account name>"

   $cosmosDBConfiguration = Get-AzureRmResource -ResourceType "Microsoft.DocumentDB/databaseAccounts" `
     -ApiVersion $apiVersion `
     -ResourceGroupName $rgName `
     -Name $acctName
   ```

1. Inicialice las variables para su uso posterior. Configure todas las variables de la definición de la cuenta existente; si tiene varias ubicaciones, debe agregarlas como parte de la matriz. En este paso, también configurará el punto de conexión de servicio de Virtual Network al definir la variable "accountVNETFilterEnabled" en "True". Este valor se asigna más adelante al parámetro "isVirtualNetworkFilterEnabled". 

   ```powershell
   $locations = @(@{})

   <# If you have read regions in addition to a write region, use the following code to set the $locations variable instead.

   $locations = @(@{"locationName"="<Write location>"; 
                 "failoverPriority"=0}, 
               @{"locationName"="<Read location>"; 
                  "failoverPriority"=1}) #>

   $consistencyPolicy = @{}
   $cosmosDBProperties = @{}

   $locations[0]['failoverPriority'] = $cosmosDBConfiguration.Properties.failoverPolicies.failoverPriority
   $locations[0]['locationName'] = $cosmosDBConfiguration.Properties.failoverPolicies.locationName

   $consistencyPolicy = $cosmosDBConfiguration.Properties.consistencyPolicy

   $accountVNETFilterEnabled = $True
   $subnetID = $vnProp.Id+"/subnets/" + $sname  
   $virtualNetworkRules = @(@{"id"=$subnetID})
   $databaseAccountOfferType = $cosmosDBConfiguration.Properties.databaseAccountOfferType
   ```

1. Actualice las propiedades de la cuenta de Azure Cosmos DB con la nueva configuración mediante la ejecución de los siguientes cmdlets: 

   ```powershell
   $cosmosDBProperties['databaseAccountOfferType'] = $databaseAccountOfferType
   $cosmosDBProperties['locations'] = $locations
   $cosmosDBProperties['consistencyPolicy'] = $consistencyPolicy
   $cosmosDBProperties['virtualNetworkRules'] = $virtualNetworkRules
   $cosmosDBProperties['isVirtualNetworkFilterEnabled'] = $accountVNETFilterEnabled

   Set-AzureRmResource `
     -ResourceType "Microsoft.DocumentDB/databaseAccounts" `
     -ApiVersion $apiVersion `
     -ResourceGroupName $rgName `
     -Name $acctName -Properties $CosmosDBProperties
   ```

1. Ejecute el siguiente comando para verificar que la cuenta de Azure Cosmos DB se ha actualizado con el punto de conexión de servicio de Virtual Network que configuró en el paso anterior:

   ```powershell
   $UpdatedcosmosDBConfiguration = Get-AzureRmResource `
     -ResourceType "Microsoft.DocumentDB/databaseAccounts" `
     -ApiVersion $apiVersion `
     -ResourceGroupName $rgName `
     -Name $acctName

   $UpdatedcosmosDBConfiguration.Properties
   ```

## <a id="configure-using-cli"></a>Configuración del punto de conexión de servicio con la CLI de Azure 

1. Habilite el punto de conexión de servicio para una subred en una red virtual.

1. Actualice la cuenta de Azure Cosmos existente con las ACL de subred.

   ```azurecli-interactive

   name="<Azure Cosmos account name>"
   resourceGroupName="<Resource group name>"

   az cosmosdb update \
      --name $name \
    --resource-group $resourceGroupName \
    --enable-virtual-network true \
    --virtual-network-rules "/subscriptions/testsub/resourceGroups/testRG/providers/Microsoft.Network/virtualNetworks/testvnet/subnets/frontend"
   ```

1. Cree una cuenta de Azure Cosmos con las ACL de subred.

   ```azurecli-interactive
   az cosmosdb create \
    --name $name \
    --kind GlobalDocumentDB \
    --resource-group $resourceGroupName \
    --max-interval 10 \
    --max-staleness-prefix 200 \
    --enable-virtual-network true \
    --virtual-network-rules "/subscriptions/testsub/resourceGroups/testRG/providers/Microsoft.Network/virtualNetworks/testvnet/subnets/default"
   ```

## <a id="migrate-from-firewall-to-vnet"></a>Migración desde la regla de firewall por IP a la ACL de la red virtual 

Los pasos siguientes solo son necesarios para las cuentas de Azure Cosmos con reglas de firewall por IP existentes que permiten una subred y si quiere usar las ACL basadas en la red virtual y la subred, en lugar de la regla de firewall por IP.

Una vez que el punto de conexión de servicio para la cuenta de Azure Cosmos esté activada para una subred, las solicitudes se enviarán con el origen que contiene información de la red virtual y de la subred, en lugar de la dirección IP pública. Por lo tanto, estas solicitudes no coinciden con un filtro de IP. Este cambio de origen se produce para todas las cuentas de Azure Cosmos que acceden desde la subred con el punto de conexión de servicio habilitado. Con el fin de evitar tiempos de inactividad, siga los pasos a continuación:

1. Para obtener las propiedades de la cuenta de Azure Cosmos, ejecute el siguiente cmdlet:

   ```powershell
   $apiVersion = "2015-04-08"
   $acctName = "<Cosmos account name>"

   $cosmosDBConfiguration = Get-AzureRmResource `
     -ResourceType "Microsoft.DocumentDB/databaseAccounts" `
     -ApiVersion $apiVersion `
     -ResourceGroupName $rgName `
     -Name $acctName
   ```

1. Inicialice las variables para su uso posterior. Configure todas las variables de la definición de la cuenta existente. Agregue la ACL de la red virtual a todas las cuentas de Cosmos Azure a las que se accede desde la subred con la marca `ignoreMissingVNetServiceEndpoint`.  

   Si tiene varias ubicaciones, deberá agregarlas como parte de la matriz. En este paso, también configurará el punto de conexión de servicio de Virtual Network al definir la variable "accountVNETFilterEnabled" en "True". Este valor se asigna más adelante al parámetro "isVirtualNetworkFilterEnabled".

   ```powershell
   $locations = @(@{})

   <# If you have read regions in addition to a write region, use the following code to set the $locations variable instead.

   $locations = @(@{"locationName"="<Write location>"; 
              "failoverPriority"=0}, 
            @{"locationName"="<Read location>"; 
               "failoverPriority"=1}) #>

   $consistencyPolicy = @{}
   $cosmosDBProperties = @{}

   $locations[0]['failoverPriority'] = $cosmosDBConfiguration.Properties.failoverPolicies.failoverPriority
   $locations[0]['locationName'] = $cosmosDBConfiguration.Properties.failoverPolicies.locationName
   $consistencyPolicy = $cosmosDBConfiguration.Properties.consistencyPolicy
   $accountVNETFilterEnabled = $True
   $subnetID = “Subnet ARM URL” e.g "/subscriptions/f7ddba26-ab7b-4a36-a2fa-7d01778da30b/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/testvnet/subnets/subnet1"

   $virtualNetworkRules = @(@{"id"=$subnetID, "ignoreMissingVNetServiceEndpoint"="True"})
   $databaseAccountOfferType = $cosmosDBConfiguration.Properties.databaseAccountOfferType
   ```

1. Para actualizar las propiedades de la cuenta de Azure Cosmos con la nueva configuración, ejecute los siguientes cmdlets:

   ```powershell
   $cosmosDBProperties['databaseAccountOfferType'] = $databaseAccountOfferType
   $cosmosDBProperties['locations'] = $locations
   $cosmosDBProperties['consistencyPolicy'] = $consistencyPolicy
   $cosmosDBProperties['virtualNetworkRules'] = $virtualNetworkRules
   $cosmosDBProperties['isVirtualNetworkFilterEnabled'] = $accountVNETFilterEnabled

   Set-AzureRmResource `
    -ResourceType "Microsoft.DocumentDB/databaseAccounts" `
    -ApiVersion $apiVersion `
    -ResourceGroupName $rgName `
    -Name $acctName -Properties $CosmosDBProperties
   ```

1. Repita los pasos 1 a 3 para todas las cuentas de Azure Cosmos que acceden desde la subred.

1.  Espere 15 minutos y, luego, actualice la subred para habilitar el punto de conexión de servicio.

1.  Habilite el punto de conexión de servicio de una subred existente de una red virtual.

   ```powershell
   $rgname= "<Resource group name>"
   $vnName = "<virtual network name>"
   $sname = "<Subnet name>"
   $subnetPrefix = "<Subnet address range>"

   Get-AzureRmVirtualNetwork `
    -ResourceGroupName $rgname `
    -Name $vnName | Set-AzureRmVirtualNetworkSubnetConfig `
    -Name $sname `
    -AddressPrefix $subnetPrefix `
    -ServiceEndpoint "Microsoft.AzureCosmosDB" | Set-AzureRmVirtualNetwork
   ```

1. Quite la regla de firewall por IP para la subred.

## <a name="next-steps"></a>Pasos siguientes

* Para configurar un firewall para Azure Cosmos DB, consulte el artículo sobre la [compatibilidad con un firewall](firewall-support.md).

