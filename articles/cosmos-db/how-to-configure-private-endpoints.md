---
title: Configuración de Azure Private Link para una cuenta de Azure Cosmos
description: Aprenda a configurar Azure Private Link para acceder a una cuenta de Azure Cosmos mediante una dirección IP privada en una red virtual.
author: ThomasWeiss
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: thweiss
ms.openlocfilehash: 34b54459629560ba80e6a38d10edbab32ea44778
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2019
ms.locfileid: "73820150"
---
# <a name="configure-azure-private-link-for-an-azure-cosmos-account-preview"></a>Configuración de Azure Private Link para una cuenta de Azure Cosmos (versión preliminar)

Al usar Azure Private Link, puede conectarse a una cuenta de Azure Cosmos a través de un punto de conexión privado. El punto de conexión privado es un conjunto de direcciones IP privadas en una subred dentro de la red virtual. Al usar Private Link, puede limitar el acceso a una cuenta de Azure Cosmos determinada a través de direcciones IP privadas. Cuando se combina con directivas de NSG restringidas, Private Link ayuda a reducir el riesgo de la exfiltración de datos. Para más información sobre los puntos de conexión privados, consulte el artículo [Azure Private Link](../private-link/private-link-overview.md).

Además, Private Link permite que una cuenta de Azure Cosmos sea accesible desde dentro de la red virtual o cualquier red virtual emparejada. También se puede acceder a los recursos asignados a Private Link desde el entorno local a través de un emparejamiento privado a través de VPN o ExpressRoute. 

Puede conectarse a una cuenta de Azure Cosmos configurada con Private Link mediante los métodos de aprobación "automático" o "manual". Para obtener más información, consulte la sección [flujo de trabajo de aprobación](../private-link/private-endpoint-overview.md#access-to-a-private-link-resource-using-approval-workflow) de la documentación de Private Link. En este artículo se describen los pasos para crear una instancia de Private Link, siempre que se use el método de aprobación automática.

## <a name="create-a-private-link-using-the-azure-portal"></a>Creación de una instancia de Private Link mediante Azure Portal

Siga los pasos que se indican a continuación para crear una instancia de Private Link para una cuenta de Azure Cosmos existente mediante Azure Portal:

1. En el panel **Todos los recursos**, busque la cuenta de Azure Cosmos DB que desea proteger.

1. Seleccione **Conexión de punto de conexión privado** en el menú de configuración y seleccione la opción **Punto de conexión privado**:

   ![Creación de un punto de conexión privado mediante Azure Portal](./media/how-to-configure-private-endpoints/create-private-endpoint-portal.png)

1. En **Crear un punto de conexión privado (vista previa) - Conceptos básicos**, escriba o seleccione los detalles siguientes:

    | Configuración | Valor |
    | ------- | ----- |
    | **Detalles del proyecto** | |
    | Subscription | Seleccione su suscripción. |
    | Resource group | Seleccione un grupo de recursos.|
    | **Detalles de instancia** |  |
    | NOMBRE | Escriba cualquier nombre para el punto de conexión privado; si este nombre ya está tomado, cree uno exclusivo. |
    |Region| Seleccione la región en la que desea implementar la instancia de Private Link. El punto de conexión privado debe crearse en la misma ubicación donde existe la red virtual.|
    |||
1. Seleccione **Siguiente: Resource** (Siguiente: Recurso).
1. En **Create a private endpoint - Resource** (Crear un punto de conexión privado: recurso), escriba o seleccione esta información:

    | Configuración | Valor |
    | ------- | ----- |
    |Método de conexión  | Seleccione Connect to an Azure resource in my directory (Conectarse a un recurso de Azure en mi directorio). <br/><br/> Esta opción permite elegir uno de los recursos para configurar una instancia de Private Link, o conectarse al recurso de otra persona con un identificador o alias de recurso que haya compartido con usted.|
    | Subscription| Seleccione su suscripción. |
    | Tipo de recurso | Seleccione **Microsoft.AzureCosmosDB/databaseAccounts**. |
    | Resource |Seleccione la cuenta de Azure Cosmos. |
    |Recurso secundario de destino |Seleccione el tipo de API de Cosmos DB que desea asignar. Este valor predeterminado es solo una opción para las API de SQL, MongoDB y Cassandra. En el caso de las API de Gremlin y Table, también puede elegir *Sql*, ya que estas API son interoperables con la API de SQL. |
    |||

1. Seleccione **Siguiente: Configuration** (Siguiente: Configuración).
1. En **Create a private endpoint (Preview) - Configuración** (Crear un punto de conexión privado [versión preliminar]: Configuración), escriba o seleccione esta información:

    | Configuración | Valor |
    | ------- | ----- |
    |**Redes**| |
    | Virtual network| Seleccione la red virtual. |
    | Subnet | Seleccionar la subred. |
    |**Integración de DNS privado**||
    |Integración con una zona DNS privada |Seleccione **Sí**. <br><br/> Para conectar de forma privada con el punto de conexión privado, necesita un registro DNS. Se recomienda integrar el punto de conexión privado con una zona DNS privada. También se pueden usar los servidores DNS propios o bien crear registros DNS con los archivos de host de las máquinas virtuales. |
    |Zona DNS privada |Seleccione *privatelink.documents.azure.com* <br><br/> La zona DNS privada se determina de forma automática y actualmente no se puede cambiar mediante Azure Portal.|
    |||

1. Seleccione **Revisar + crear**. Se le remitirá a la página **Revisar y crear**, donde Azure validará la configuración.
1. Cuando reciba el mensaje **Validación superada**, seleccione **Crear**.

Cuando haya aprobado instancias de Private Link para una cuenta de Azure Cosmos, la opción **Todas las redes** en el panel **Firewall y redes virtuales** estará atenuada en Azure Portal.

En la tabla siguiente se muestra la asignación entre diferentes tipos de API de la cuenta de Azure Cosmos, los subrecursos admitidos y los nombres de zona privada correspondientes. También se puede acceder a las cuentas de Gremlin y Table API a través de la API de SQL, por lo que hay dos entradas para estas API:

|Tipo de API de la cuenta de Azure Cosmos  |Subrecursos (o identificadores de grupo) admitidos |Nombre de zona privada  |
|---------|---------|---------|
|Sql    |   Sql      | privatelink.documents.azure.com   |
|Cassandra    | Cassandra        |  privatelink.cassandra.cosmos.azure.com    |
|Mongo   |  MongoDB       |  privatelink.mongo.cosmos.azure.com    |
|Gremlin     | Gremlin        |  privatelink.gremlin.cosmos.azure.com   |
|Gremlin     |  Sql       |  privatelink.documents.azure.com    |
|Tabla    |    Tabla     |   privatelink.table.cosmos.azure.com    |
|Tabla     |   Sql      |  privatelink.documents.azure.com    |

### <a name="fetch-the-private-ip-addresses"></a>Capturar direcciones IP privadas

Una vez aprovisionado el punto de conexión privado, puede consultar las direcciones IP. Para ver las direcciones IP desde Azure Portal. Seleccione **Todos los recursos**, busque el punto de conexión privado que creó anteriormente, en este caso, es "dbPrivateEndpoint3", y seleccione la pestaña Información general para ver la configuración de DNS y las direcciones IP:

![Direcciones IP privadas en Azure Portal](./media/how-to-configure-private-endpoints/private-ip-addresses-portal.png)

Se crean varias direcciones IP por punto de conexión privado:

* Una para el punto de conexión global (independiente de la región) de la cuenta de Azure Cosmos.
* Una para cada región donde se ha implementado la cuenta de Azure Cosmos.

## <a name="create-a-private-link-using-azure-powershell"></a>Creación de una instancia de Private Link mediante Azure PowerShell

Ejecute el siguiente script de PowerShell para crear una instancia de Private Link denominada "MyPrivateEndpoint" para una cuenta de Azure Cosmos existente. Asegúrese de reemplazar los valores de las variables por valores específicos de su entorno.

```azurepowershell-interactive
Fill in these details, make sure to replace the variable values with the details specific to your environment.
$SubscriptionId = "<your Azure subscription ID>"
# Resource group where the Cosmos DB and VNet resources live
$ResourceGroupName = "myResourceGroup"
# Name of the Cosmos DB account
$CosmosDbAccountName = "mycosmosaccount"

# API type of the Cosmos DB account: Sql or MongoDB or Cassandra or Gremlin or Table
$CosmosDbApiType = "Sql"
# Name of the existing VNet
$VNetName = "myVnet"
# Name of the target subnet in the VNet
$SubnetName = "mySubnet"
# Name of the private endpoint to create
$PrivateEndpointName = "MyPrivateEndpoint"
# Location where the private endpoint can be created. The private endpoint should be created in the same location where your subnet or the virtual network exists
$Location = "westcentralus"

$cosmosDbResourceId = "/subscriptions/$($SubscriptionId)/resourceGroups/$($ResourceGroupName)/providers/Microsoft.DocumentDB/databaseAccounts/$($CosmosDbAccountName)"

$privateEndpointConnection = New-AzPrivateLinkServiceConnection -Name "myConnectionPS" -PrivateLinkServiceId $cosmosDbResourceId -GroupId $CosmosDbApiType
 
$virtualNetwork = Get-AzVirtualNetwork -ResourceGroupName  $ResourceGroupName -Name $VNetName  
 
$subnet = $virtualNetwork | Select -ExpandProperty subnets | Where-Object  {$_.Name -eq $SubnetName}  
 
$privateEndpoint = New-AzPrivateEndpoint -ResourceGroupName $ResourceGroupName -Name $PrivateEndpointName -Location "westcentralus" -Subnet  $subnet -PrivateLinkServiceConnection $privateEndpointConnection
```

### <a name="integrate-the-private-endpoint-with-private-dns-zone"></a>Integración del punto de conexión privado con la zona DNS privada

Después de crear el punto de conexión privado, puede integrarlo con una zona DNS privada mediante el siguiente script de PowerSehll:

```azurepowershell-interactive
Import-Module Az.PrivateDns
$zoneName = "privatelink.documents.azure.com"
$zone = New-AzPrivateDnsZone -ResourceGroupName $ResourceGroupName `
  -Name $zoneName

$link  = New-AzPrivateDnsVirtualNetworkLink -ResourceGroupName $ResourceGroupName `
  -ZoneName $zoneName `
  -Name "myzonelink" `
  -VirtualNetworkId $virtualNetwork.Id  
 
$pe = Get-AzPrivateEndpoint -Name $PrivateEndpointName `
  -ResourceGroupName $ResourceGroupName

$networkInterface = Get-AzResource -ResourceId $pe.NetworkInterfaces[0].Id `
  -ApiVersion "2019-04-01"
 
foreach ($ipconfig in $networkInterface.properties.ipConfigurations) { 
foreach ($fqdn in $ipconfig.properties.privateLinkConnectionProperties.fqdns) { 
Write-Host "$($ipconfig.properties.privateIPAddress) $($fqdn)"  
$recordName = $fqdn.split('.',2)[0] 
$dnsZone = $fqdn.split('.',2)[1] 
New-AzPrivateDnsRecordSet -Name $recordName `
  -RecordType A -ZoneName $zoneName  `
  -ResourceGroupName $ResourceGroupName -Ttl 600 `
  -PrivateDnsRecords (New-AzPrivateDnsRecordConfig `
  -IPv4Address $ipconfig.properties.privateIPAddress)  
}
}
```

### <a name="fetch-the-private-ip-addresses"></a>Capturar direcciones IP privadas

Después de haber aprovisionado el punto de conexión privado, puede consultar las direcciones IP y la asignación de FQDN mediante el siguiente script de PowerShell:

```azurepowershell-interactive

$pe = Get-AzPrivateEndpoint -Name MyPrivateEndpoint -ResourceGroupName myResourceGroup
$networkInterface = Get-AzNetworkInterface -ResourceId $pe.NetworkInterfaces[0].Id
foreach ($IPConfiguration in $networkInterface.IpConfigurations)
{
    Write-Host $IPConfiguration.PrivateIpAddress ":" $IPConfiguration.PrivateLinkConnectionProperties.Fqdns
}
```

## <a name="create-a-private-link-using-a-resource-manager-template"></a>Creación de una instancia de Private Link mediante una plantilla de Resource Manager

Para configurar Private Link, cree un punto de conexión privado en una subred de la red virtual. Para ello, use una plantilla de Azure Resource Manager. Cree una plantilla de Resource Manager denominada "PrivateEndpoint_template.json" con el código siguiente. Esta plantilla crea un punto de conexión privado para una cuenta existente de la API de Azure Cosmos SQL en una red virtual existente:

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "location": {
          "type": "string",
          "defaultValue": "[resourceGroup().location]",
          "metadata": {
            "description": "Location for all resources."
          }
        },
        "privateEndpointName": {
            "type": "string"
        },
        "resourceId": {
            "type": "string"
        },
        "groupId": {
            "type": "string"
        },
        "subnetId": {
            "type": "string"
        }
    },
    "resources": [
        {
            "name": "[parameters('privateEndpointName')]",
            "type": "Microsoft.Network/privateEndpoints",
            "apiVersion": "2019-04-01",
            "location": "[parameters('location')]",
            "properties": {
                "subnet": {
                    "id": "[parameters('subnetId')]"
                },
                "privateLinkServiceConnections": [
                    {
                        "name": "MyConnection",
                        "properties": {
                            "privateLinkServiceId": "[parameters('resourceId')]",
                            "groupIds": [ "[parameters('groupId')]" ],
                            "requestMessage": ""
                        }
                    }
                ]
            }
        }
    ],
    "outputs": {
        "privateEndpointNetworkInterface": {
          "type": "string",
          "value": "[reference(concat('Microsoft.Network/privateEndpoints/', parameters('privateEndpointName'))).networkInterfaces[0].id]"
        }
    }
}
```

### <a name="define-the-template-parameters-file"></a>Definir el archivo de parámetros de plantilla

Cree un archivo de parámetros para la plantilla y asígnele el nombre "PrivateEndpoint_parameters.json". Agregue el siguiente código al archivo de parámetros:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "privateEndpointName": {
            "value": ""
        },
        "resourceId": {
            "value": ""
        },
        "groupId": {
            "value": ""
        },
        "subnetId": {
            "value": ""
        }
    }
}
```

### <a name="deploy-the-template-by-using-a-powershell-script"></a>Implementar la plantilla mediante un script de PowerShell

A continuación, cree un script de PowerShell con el código siguiente. Antes de ejecutar el script, asegúrese de reemplazar el identificador de la suscripción, el nombre del grupo de recursos y otros valores de variables con los detalles específicos de su entorno:

```azurepowershell-interactive
### This script creates a private endpoint for an existing Cosmos DB account in an existing VNet

## Step 1: Fill in these details, make sure to replace the variable values with the details specific to your environment.
$SubscriptionId = "<your Azure subscription ID>"
# Resource group where the Cosmos DB and VNet resources live
$ResourceGroupName = "myResourceGroup"
# Name of the Cosmos DB account
$CosmosDbAccountName = "mycosmosaccount"
# API type of the Cosmos DB account. It can be one of the following: "Sql", "MongoDB", "Cassandra", "Gremlin", "Table"
$CosmosDbApiType = "Sql"
# Name of the existing VNet
$VNetName = "myVnet"
# Name of the target subnet in the VNet
$SubnetName = "mySubnet"
# Name of the private endpoint to create
$PrivateEndpointName = "myPrivateEndpoint"

$cosmosDbResourceId = "/subscriptions/$($SubscriptionId)/resourceGroups/$($ResourceGroupName)/providers/Microsoft.DocumentDB/databaseAccounts/$($CosmosDbAccountName)"
$VNetResourceId = "/subscriptions/$($SubscriptionId)/resourceGroups/$($ResourceGroupName)/providers/Microsoft.Network/virtualNetworks/$($VNetName)"
$SubnetResourceId = "$($VNetResourceId)/subnets/$($SubnetName)"
$PrivateEndpointTemplateFilePath = "PrivateEndpoint_template.json"
$PrivateEndpointParametersFilePath = "PrivateEndpoint_parameters.json"

## Step 2: Sign in to your Azure account and select the target subscription.
Login-AzAccount
Select-AzSubscription -SubscriptionId $subscriptionId

## Step 3: Make sure private endpoint network policies are disabled in the subnet.
$VirtualNetwork= Get-AzVirtualNetwork -Name "$VNetName" -ResourceGroupName "$ResourceGroupName"
($virtualNetwork | Select -ExpandProperty subnets | Where-Object  {$_.Name -eq "$SubnetName"} ).PrivateEndpointNetworkPolicies = "Disabled"
$virtualNetwork | Set-AzVirtualNetwork

## Step 4: Create the private endpoint.
Write-Output "Deploying private endpoint on $($resourceGroupName)"
$deploymentOutput = New-AzResourceGroupDeployment -Name "PrivateCosmosDbEndpointDeployment" `
    -ResourceGroupName $resourceGroupName `
    -TemplateFile $PrivateEndpointTemplateFilePath `
    -TemplateParameterFile $PrivateEndpointParametersFilePath `
    -SubnetId $SubnetResourceId `
    -ResourceId $CosmosDbResourceId `
    -GroupId $CosmosDbApiType `
    -PrivateEndpointName $PrivateEndpointName

$deploymentOutput
```

En el script de PowerShell, la variable "GroupId" solo puede contener un valor, que es el tipo de API de la cuenta. Los valores permitidos son: SQL, MongoDB, Cassandra, Gremlin y Table. Algunos tipos de cuenta de Azure Cosmos son accesibles a través de varias API. Por ejemplo:

* Se puede acceder a una cuenta de la API de Gremlin desde cuentas de API de Gremlin y de SQL.
* Se puede acceder a una cuenta de la Table API desde cuentas de Table API y de SQL.

Para estas cuentas, debe crear un punto de conexión privado para cada tipo de API, con el tipo de API correspondiente especificado en la matriz "GroupId".

Una vez que la plantilla se ha implementado correctamente, puede ver un resultado similar a la que se muestra en la siguiente imagen. Si los puntos de conexión privados están configurados correctamente, el valor de provisioningState es "Succeeded".

![Resultado de la implementación de la plantilla de Resource Manager](./media/how-to-configure-private-endpoints/resource-manager-template-deployment-output.png)

Una vez implementada la plantilla, las direcciones IP privadas se reservan dentro de la subred. La regla de firewall de la cuenta de Azure Cosmos está configurada para aceptar conexiones solo del punto de conexión privado.

## <a name="configure-custom-dns"></a>Configuración de DNS personalizado

Debe usar un DNS privado dentro de la subred en la que se ha creado el punto de conexión privado. Además, debe configurar los puntos de conexión para que cada una de las direcciones IP privadas se asigne a una entrada DNS (vea la propiedad "fqdns" en la respuesta mostrada anteriormente).

Al crear el punto de conexión privado, puede integrarlo con una zona DNS privada en Azure. Si decide no integrar el punto de conexión privado con la zona DNS privada en Azure y, en su lugar, usar un DNS personalizado, debe configurar el DNS de forma que se agreguen registros DNS para todas las direcciones IP privadas reservadas para el punto de conexión privado.

## <a name="firewall-configuration-with-private-link"></a>Configuración del firewall con Private Link

A continuación se muestran situaciones y resultados diferentes cuando se usa Private Link en combinación con las reglas de firewall:

* Si no hay configurada ninguna regla de firewall, de forma predeterminada, todo el tráfico tendrá acceso a una cuenta de Azure Cosmos.

* Si el tráfico público o el punto de conexión de servicio están configurados y se han creado puntos de conexión privados, los distintos tipos de tráfico entrante estarán autorizados por el tipo de regla de firewall correspondiente.

* Si no se configura ningún tráfico público ni punto de conexión de servicio y se crean puntos de conexión privados, la cuenta de Azure Cosmos solo es accesible a través de los puntos de conexión privados. Si no se configura ningún tráfico público ni punto de conexión de servicio, después de que se rechacen o eliminen todos los puntos de conexión privados aprobados, la cuenta estará abierta a toda la red.

## <a name="update-private-endpoint-when-you-add-or-remove-a-region"></a>Actualización del punto de conexión privado al agregar o quitar una región

La adición o eliminación de regiones a una cuenta de Azure Cosmos requiere que se agreguen o quiten las entradas DNS para esa cuenta. Estos cambios deben actualizarse en consecuencia en el punto de conexión privado. Actualmente, debe realizar este cambio manualmente mediante los pasos siguientes:

1. El administrador de Azure Cosmos DB agrega o quita regiones. A continuación, se notifica a los administradores de red acerca de los cambios pendientes. El punto de conexión privado asignado a una cuenta de Azure Cosmos ve sus propiedades "ActionsRequired" cambiadas de "None" a "Recreate". Después, el administrador de red actualiza el punto de conexión privado mediante la emisión de una solicitud PUT con la misma carga de Resource Manager que se usa para crearlo.

1. Después de esta operación, el DNS privado de la subred también tiene que actualizarse para reflejar las entradas de DNS agregadas o eliminadas, así como sus direcciones IP privadas correspondientes.

Por ejemplo, si implementa una cuenta de Azure Cosmos en 3 regiones: "Oeste de EE. UU.", "Centro de EE. UU." y "Oeste de Europa". Cuando crea un punto de conexión privado para la cuenta, se reservan 4 direcciones IP privadas en la subred. Una para cada región, lo que suma un total de 3, y otra para el punto de conexión independiente de la región o global.

Más adelante, si agrega una nueva región, por ejemplo, "Este de EE. UU." a la cuenta de Azure Cosmos, de forma predeterminada, no se puede acceder a la nueva región desde el punto de conexión privado existente. El administrador de la cuenta de Azure Cosmos debe actualizar la conexión al punto de conexión privado antes de tener acceso a él desde la nueva región. 

Al ejecutar el comando ` Get-AzPrivateEndpoint -Name <your private endpoint name> -ResourceGroupName <your resource group name>`, el resultado del comando contiene el parámetro `actionsRequired`, que se establece en "Recreate". Este valor indica que el punto de conexión privado debe actualizarse. Después, el administrador de la cuenta de Azure Cosmos ejecuta el comando `Set-AzPrivateEndpoint` para desencadenar la actualización del punto de conexión privado.

```powershell
$pe = Get-AzPrivateEndpoint -Name <your private endpoint name> -ResourceGroupName <your resource group name>

Set-AzPrivateEndpoint -PrivateEndpoint $pe
```

Una nueva dirección IP privada se reserva automáticamente en la subred de este punto de conexión privado, y el valor `actionsRequired` se convierte en `None`. Si no tiene ninguna integración de zona DNS privada (es decir, si usa un DNS privado personalizado), debe configurar el DNS privado para agregar un nuevo registro DNS para la dirección IP privada correspondiente a la nueva región.

Puede seguir los mismos pasos al quitar una región. La dirección IP privada de la región que se ha quitado se reclama automáticamente, y la marca `actionsRequired` se convierte en `None`. Si no tiene ninguna integración de zona DNS privada, debe configurar el DNS privado para quitar el registro DNS de la región que se ha quitado.

Los registros DNS de la zona DNS privada no se quitan automáticamente cuando se elimina un punto de conexión privado o se quita una región de la cuenta de Azure Cosmos. Debe quitar manualmente los registros DNS.

## <a name="current-limitations"></a>Limitaciones actuales

Al usar Private Link con una cuenta de Azure Cosmos se aplican las siguientes limitaciones:

* La compatibilidad de Private Link para las cuentas y redes virtuales de Azure Cosmos solo está disponible en determinadas regiones. Para obtener una lista de las regiones admitidas, consulte la sección [Regiones disponibles](../private-link/private-link-overview.md#availability) del artículo de Private Link. **Tanto la red virtual como la cuenta de Azure Cosmos deben estar en las regiones admitidas para poder crear un punto de conexión privado**.

* Al usar instancias de Private Link con una cuenta de Azure Cosmos mediante conexión de modo directo, solo puede usar el protocolo TCP. Aún no se admite el protocolo HTTP.

* Cuando se usa la API de Azure Cosmos DB para las cuentas de MongoDB, solo se admite el punto de conexión privado para las cuentas en el servidor versión 3.6 (es decir, las cuentas que usan el punto de conexión con el formato `*.mongo.cosmos.azure.com`). Private Link no se admite para cuentas en el servidor versión 3.2 (es decir, cuentas que usan el punto de conexión con el formato `*.documents.azure.com`). Para usar Private Link, debe migrar las cuentas anteriores a la nueva versión.

* Al usar la API de Azure Cosmos DB para las cuentas de MongoDB que tienen Private Link, no puede usar herramientas como Robo 3T, Studio 3T, Mongoose, etc. El punto de conexión puede tener compatibilidad con Private Link solo si se especifica el parámetro appName=<account name>. Por ejemplo: replicaSet=globaldb&appName=mydbaccountname. Dado que estas herramientas no pasan el nombre de la aplicación en la cadena de conexión al servicio, no puede usar Private Link. Sin embargo, todavía puede tener acceso a estas cuentas con controladores de SDK con la versión 3.6.

* No se puede mover ni eliminar una red virtual si contiene Private Link.

* No se puede eliminar una cuenta de Azure Cosmos si está asociada a un punto de conexión privado.

* Una cuenta de Azure Cosmos no puede conmutarse por error a una región que no esté asignada a todos los puntos de conexión privados conectados a esta cuenta. Para obtener más información, consulte Adición o eliminación de registros en la sección anterior.

* Un administrador debe conceder a un administrador de red al menos el permiso "*/PrivateEndpointConnectionsApproval" en el ámbito de la cuenta de Azure Cosmos para crear puntos de conexión privados aprobados automáticamente.

### <a name="private-dns-zone-integration-limitations"></a>Limitaciones de la integración de la zona DNS privada

Los registros DNS de la zona DNS privada no se quitan automáticamente cuando se elimina un punto de conexión privado o se quita una región de la cuenta de Azure Cosmos. Debe quitar manualmente los registros DNS antes de realizar las siguientes actividades:

* Agregar un nuevo punto de conexión privado conectado a esta zona DNS privada.
* Agregar una nueva región a cualquier cuenta de base de datos que tenga puntos de conexión privados conectados a esta zona DNS privada.

Si no se limpian los registros DNS, pueden producirse problemas inesperados en el plano de datos, como la interrupción de datos en las regiones agregadas después de la eliminación de puntos de conexión privados o de la eliminación de regiones.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre las demás características de seguridad de Azure Cosmos DB, consulte el siguiente artículo:

* Para configurar un firewall para Azure Cosmos DB, consulte [Compatibilidad con firewalls](firewall-support.md).

* [Configuración del punto de conexión de servicio de red virtual para la cuenta de Azure Cosmos](how-to-configure-vnet-service-endpoint.md).

* Para obtener más información sobre Private Link, consulte la documentación de [Azure Private Link](../private-link/private-link-overview.md).
