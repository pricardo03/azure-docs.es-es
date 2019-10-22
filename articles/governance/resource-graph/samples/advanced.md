---
title: Ejemplos de consultas avanzadas
description: Use Azure Resource Graph para ejecutar consultas avanzadas, lo entre las que se incluyen la capacidad del conjunto de escalado de máquinas virtuales, la enumeración de todas las etiquetas usadas y la coincidencia de las máquinas virtuales con expresiones regulares.
author: DCtheGeek
ms.author: dacoulte
ms.date: 10/18/2019
ms.topic: quickstart
ms.service: resource-graph
ms.openlocfilehash: 8b848eac338260340cb9160a72f33294e51101f2
ms.sourcegitcommit: bb65043d5e49b8af94bba0e96c36796987f5a2be
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2019
ms.locfileid: "72387573"
---
# <a name="advanced-resource-graph-queries"></a>Consultas avanzadas de Resource Graph

El primer paso para entender las consultas con Azure Resource Graph es el reconocimiento básico del [lenguaje de consultas](../concepts/query-language.md). Si aún no está familiarizado con [Azure Data Explorer](../../../data-explorer/data-explorer-overview.md), se recomienda repasar los conceptos básicos para entender cómo componer solicitudes para los recursos que está buscando.

Le guiaremos por las siguientes consultas avanzadas:

> [!div class="checklist"]
> - [Obtener la capacidad y el tamaño del conjunto de escalado de máquinas virtuales](#vmss-capacity)
> - [Eliminación de columnas de los resultados](#remove-column)
> - [Enumeración de todos los nombres de etiquetas](#list-all-tags)
> - [Máquinas virtuales que coinciden con regex](#vm-regex)
> - [Enumeración de Cosmos DB con ubicaciones de escritura concretas](#mvexpand-cosmosdb)
> - [Almacén de claves con el nombre de la suscripción](#join)
> - [Enumeración de bases de datos SQL y sus grupos elásticos](#join-sql)
> - [Enumeración de máquinas virtuales con su interfaz de red y dirección IP pública](#join-vmpip)
> - [Búsqueda de cuentas de almacenamiento con una etiqueta concreta en el grupo de recursos](#join-findstoragetag)
> - [Combinación de los resultados de dos consultas para formar un solo resultado](#unionresults)
> - [Inclusión de nombres de inquilinos y suscripciones en DisplayNames](#displaynames)

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free) antes de empezar.

## <a name="language-support"></a>Compatibilidad con idiomas

La CLI de Azure (mediante una extensión) y Azure PowerShell (mediante un módulo) admiten Azure Resource Graph. Antes de ejecutar cualquiera de las siguientes consultas, compruebe que el entorno está listo. Consulte la [CLI de Azure](../first-query-azurecli.md#add-the-resource-graph-extension) y [Azure PowerShell](../first-query-powershell.md#add-the-resource-graph-module) para conocer los pasos para instalar y validar el entorno de shell que prefiera.

## <a name="vmss-capacity"></a>Obtener la capacidad y el tamaño del conjunto de escalado de máquinas virtuales

Esta consulta busca los recursos del conjunto de escalado de máquinas virtuales y obtiene varios detalles, incluido el tamaño de la máquina virtual y la capacidad del conjunto de escalado. La consulta usa la función `toint()` para convertir la capacidad en un número para que se pueda ordenar. Por último, se cambia el nombre de las columnas por propiedades con nombre personalizadas.

```kusto
Resources
| where type=~ 'microsoft.compute/virtualmachinescalesets'
| where name contains 'contoso'
| project subscriptionId, name, location, resourceGroup, Capacity = toint(sku.capacity), Tier = sku.name
| order by Capacity desc
```

```azurecli-interactive
az graph query -q "Resources | where type=~ 'microsoft.compute/virtualmachinescalesets' | where name contains 'contoso' | project subscriptionId, name, location, resourceGroup, Capacity = toint(sku.capacity), Tier = sku.name | order by Capacity desc"
```

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where type=~ 'microsoft.compute/virtualmachinescalesets' | where name contains 'contoso' | project subscriptionId, name, location, resourceGroup, Capacity = toint(sku.capacity), Tier = sku.name | order by Capacity desc"
```

## <a name="remove-column"></a>Eliminación de columnas de los resultados

En la consulta siguiente se usa `summarize` para contar los recursos por suscripción, `join` para combinarlos con los detalles de la suscripción de la tabla _ResourceContainers_ y, después, `project-away` para quitar algunas de las columnas.

```kusto
Resources
| summarize resourceCount=count() by subscriptionId
| join (ResourceContainers | where type=='microsoft.resources/subscriptions' | project SubName=name, subscriptionId) on subscriptionId
| project-away subscriptionId, subscriptionId1
```

```azurecli-interactive
az graph query -q "Resources | summarize resourceCount=count() by subscriptionId | join (ResourceContainers | where type=='microsoft.resources/subscriptions' | project SubName=name, subscriptionId) on subscriptionId| project-away subscriptionId, subscriptionId1"
```

```azurepowershell-interactive
Search-AzGraph -Query "Resources | summarize resourceCount=count() by subscriptionId | join (ResourceContainers | where type=='microsoft.resources/subscriptions' | project SubName=name, subscriptionId) on subscriptionId| project-away subscriptionId, subscriptionId1"
```

## <a name="list-all-tags"></a>Enumeración de todos los nombres de etiquetas

Esta consulta comienza con la etiqueta y crea un objeto JSON que enumera todos los nombres de etiqueta únicos y sus tipos correspondientes.

```kusto
Resources
| project tags
| summarize buildschema(tags)
```

```azurecli-interactive
az graph query -q "Resources | project tags | summarize buildschema(tags)"
```

```azurepowershell-interactive
Search-AzGraph -Query "Resources | project tags | summarize buildschema(tags)"
```

## <a name="vm-regex"></a>Máquinas virtuales que coinciden con regex

Esta consulta busca las máquinas virtuales que coincidan con una [expresión regular](/dotnet/standard/base-types/regular-expression-language-quick-reference) (conocida como _regex_). **matches regex \@** nos permite definir regex para coincidir, que es `^Contoso(.*)[0-9]+$`.
Esa definición de regex se explica como:

- `^` - La coincidencia debe empezar al principio de la cadena.
- `Contoso`: la cadena distingue mayúsculas de minúsculas.
- `(.*)`: una coincidencia de subexpresión:
  - `.` - Coincide con cualquier carácter individual (excepto saltos de línea).
  - `*` - Coincide con el elemento anterior cero o más veces.
- `[0-9]` - Coincidencia de grupo caracteres para los números del 0 al 9.
- `+` - Coincide con el elemento anterior una o más veces.
- `$` - La coincidencia del elemento anterior se debe producir al final de la cadena.

Después de la coincidencia del nombre, la consulta proyecta el nombre y ordena por nombre de forma ascendente.

```kusto
Resources
| where type =~ 'microsoft.compute/virtualmachines' and name matches regex @'^Contoso(.*)[0-9]+$'
| project name
| order by name asc
```

```azurecli-interactive
az graph query -q "Resources | where type =~ 'microsoft.compute/virtualmachines' and name matches regex @'^Contoso(.*)[0-9]+$' | project name | order by name asc"
```

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where type =~ 'microsoft.compute/virtualmachines' and name matches regex @'^Contoso(.*)[0-9]+$' | project name | order by name asc"
```

## <a name="mvexpand-cosmosdb"></a>Enumeración de Cosmos DB con ubicaciones de escritura concretas

La siguiente consulta limita con los recursos de Cosmos DB, utiliza `mv-expand` para expandir el contenedor de propiedades para **properties.writeLocations** , después proyectar campos específicos y limitar los resultados a **properties.writeLocations.locationName** valores que coinciden con "Este de EE. UU." u "Oeste de EE. UU.".

```kusto
Resources
| where type =~ 'microsoft.documentdb/databaseaccounts'
| project id, name, writeLocations = (properties.writeLocations)
| mv-expand writeLocations
| project id, name, writeLocation = tostring(writeLocations.locationName)
| where writeLocation in ('East US', 'West US')
| summarize by id, name
```

```azurecli-interactive
az graph query -q "Resources | where type =~ 'microsoft.documentdb/databaseaccounts' | project id, name, writeLocations = (properties.writeLocations) | mv-expand writeLocations | project id, name, writeLocation = tostring(writeLocations.locationName) | where writeLocation in ('East US', 'West US') | summarize by id, name"
```

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where type =~ 'microsoft.documentdb/databaseaccounts' | project id, name, writeLocations = (properties.writeLocations) | mv-expand writeLocations | project id, name, writeLocation = tostring(writeLocations.locationName) | where writeLocation in ('East US', 'West US') | summarize by id, name"
```

## <a name="join"></a>Almacén de claves con el nombre de la suscripción

En la consulta siguiente se muestra un uso complejo de `join`. La consulta limita la tabla conectada a los recursos de suscripciones y con `project` para incluir solo el campo original _subscriptionId_  y el campo_name_  renombrado a _SubName_ . El cambio de nombre del campo evita `join` agregarlo como _name1_, ya que el campo ya existe en _resources_ . La tabla original se filtra con `where` y el `project` siguiente incluye columnas de ambas tablas. El resultado de la consulta es un solo almacén de claves que muestra el tipo, el nombre del almacén de claves y el nombre de la suscripción en que se encuentra.

```kusto
Resources
| join (ResourceContainers | where type=='microsoft.resources/subscriptions' | project SubName=name, subscriptionId) on subscriptionId
| where type == 'microsoft.keyvault/vaults'
| project type, name, SubName
| limit 1
```

```azurecli-interactive
az graph query -q "Resources | join (ResourceContainers | where type=='microsoft.resources/subscriptions' | project SubName=name, subscriptionId) on subscriptionId | where type == 'microsoft.keyvault/vaults' | project type, name, SubName| limit 1"
```

```azurepowershell-interactive
Search-AzGraph -Query "Resources | join (ResourceContainers | where type=='microsoft.resources/subscriptions' | project SubName=name, subscriptionId) on subscriptionId | where type == 'microsoft.keyvault/vaults' | project type, name, SubName| limit 1"
```

## <a name="join-sql"></a>Enumeración de bases de datos SQL y sus grupos elásticos

En la consulta siguiente se usa **leftouter**  `join` para reunir recursos de SQL Database y sus grupos elásticos relacionados, en caso de que tengan.

```kusto
Resources
| where type =~ 'microsoft.sql/servers/databases'
| project databaseId = id, databaseName = name, elasticPoolId = tolower(tostring(properties.elasticPoolId))
| join kind=leftouter (
    Resources
    | where type =~ 'microsoft.sql/servers/elasticpools'
    | project elasticPoolId = tolower(id), elasticPoolName = name, elasticPoolState = properties.state)
on elasticPoolId
| project-away elasticPoolId1
```

```azurecli-interactive
az graph query -q "Resources | where type =~ 'microsoft.sql/servers/databases' | project databaseId = id, databaseName = name, elasticPoolId = tolower(tostring(properties.elasticPoolId)) | join kind=leftouter ( Resources | where type =~ 'microsoft.sql/servers/elasticpools' | project elasticPoolId = tolower(id), elasticPoolName = name, elasticPoolState = properties.state) on elasticPoolId | project-away elasticPoolId1"
```

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where type =~ 'microsoft.sql/servers/databases' | project databaseId = id, databaseName = name, elasticPoolId = tolower(tostring(properties.elasticPoolId)) | join kind=leftouter ( Resources | where type =~ 'microsoft.sql/servers/elasticpools' | project elasticPoolId = tolower(id), elasticPoolName = name, elasticPoolState = properties.state) on elasticPoolId | project-away elasticPoolId1"
```

## <a name="join-vmpip"></a>Enumeración de máquinas virtuales con su interfaz de red y dirección IP pública

Esta consulta usa dos comandos **leftouter** `join` para reunir máquinas virtuales, sus interfaces de red relacionadas y cualquier dirección IP pública relacionada con esas interfaces de red.

```kusto
Resources
| where type =~ 'microsoft.compute/virtualmachines'
| extend nics=array_length(properties.networkProfile.networkInterfaces) 
| mvexpand nic=properties.networkProfile.networkInterfaces 
| where nics == 1 or nic.properties.primary =~ 'true' or isempty(nic) 
| project vmId = id, vmName = name, vmSize=tostring(properties.hardwareProfile.vmSize), nicId = tostring(nic.id) 
| join kind=leftouter (
    Resources
    | where type =~ 'microsoft.network/networkinterfaces'
    | extend ipConfigsCount=array_length(properties.ipConfigurations) 
    | mvexpand ipconfig=properties.ipConfigurations 
    | where ipConfigsCount == 1 or ipconfig.properties.primary =~ 'true'
    | project nicId = id, publicIpId = tostring(ipconfig.properties.publicIPAddress.id))
on nicId
| project-away nicId1
| summarize by vmId, vmName, vmSize, nicId, publicIpId
| join kind=leftouter (
    Resources
    | where type =~ 'microsoft.network/publicipaddresses'
    | project publicIpId = id, publicIpAddress = properties.ipAddress)
on publicIpId
| project-away publicIpId1
```

```azurecli-interactive
azure graph query -q "Resources | where type =~ 'microsoft.compute/virtualmachines' | extend nics=array_length(properties.networkProfile.networkInterfaces) | mvexpand nic=properties.networkProfile.networkInterfaces | where nics == 1 or nic.properties.primary =~ 'true' or isempty(nic) | project vmId = id, vmName = name, vmSize=tostring(properties.hardwareProfile.vmSize), nicId = tostring(nic.id) | join kind=leftouter ( Resources | where type =~ 'microsoft.network/networkinterfaces' | extend ipConfigsCount=array_length(properties.ipConfigurations) | mvexpand ipconfig=properties.ipConfigurations | where ipConfigsCount == 1 or ipconfig.properties.primary =~ 'true' | project nicId = id, publicIpId = tostring(ipconfig.properties.publicIPAddress.id)) on nicId | project-away nicId1 | summarize by vmId, vmName, vmSize, nicId, publicIpId | join kind=leftouter ( Resources | where type =~ 'microsoft.network/publicipaddresses' | project publicIpId = id, publicIpAddress = properties.ipAddress) on publicIpId | project-away publicIpId1"
```

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where type =~ 'microsoft.compute/virtualmachines' | extend nics=array_length(properties.networkProfile.networkInterfaces) | mvexpand nic=properties.networkProfile.networkInterfaces | where nics == 1 or nic.properties.primary =~ 'true' or isempty(nic) | project vmId = id, vmName = name, vmSize=tostring(properties.hardwareProfile.vmSize), nicId = tostring(nic.id) | join kind=leftouter ( Resources | where type =~ 'microsoft.network/networkinterfaces' | extend ipConfigsCount=array_length(properties.ipConfigurations) | mvexpand ipconfig=properties.ipConfigurations | where ipConfigsCount == 1 or ipconfig.properties.primary =~ 'true' | project nicId = id, publicIpId = tostring(ipconfig.properties.publicIPAddress.id)) on nicId | project-away nicId1 | summarize by vmId, vmName, vmSize, nicId, publicIpId | join kind=leftouter ( Resources | where type =~ 'microsoft.network/publicipaddresses' | project publicIpId = id, publicIpAddress = properties.ipAddress) on publicIpId | project-away publicIpId1"
```

## <a name="join-findstoragetag"></a>Búsqueda de cuentas de almacenamiento con una etiqueta concreta en el grupo de recursos

En la consulta siguiente se usa **innerunique** `join`para conectar las cuentas de almacenamiento con grupos de recursos que tienen un nombre de etiqueta y un valor de etiqueta especificados.

```kusto
Resources
| where type =~ 'microsoft.storage/storageaccounts'
| join kind=innerunique (
    ResourceContainers
    | where type =~ 'microsoft.resources/subscriptions/resourcegroups'
    | where tags['key1'] == 'value1'
    | project subscriptionId, resourceGroup)
on subscriptionId, resourceGroup
| project-away subscriptionId1, resourceGroup1
```

```azurecli-interactive
az graph query -q "Resources | where type =~ 'microsoft.storage/storageaccounts' | join kind=innerunique ( ResourceContainers | where type =~ 'microsoft.resources/subscriptions/resourcegroups' | where tags['key1'] == 'value1' | project subscriptionId, resourceGroup) on subscriptionId, resourceGroup | project-away subscriptionId1, resourceGroup1"
```

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where type =~ 'microsoft.storage/storageaccounts' | join kind=innerunique ( ResourceContainers | where type =~ 'microsoft.resources/subscriptions/resourcegroups' | where tags['key1'] == 'value1' | project subscriptionId, resourceGroup) on subscriptionId, resourceGroup | project-away subscriptionId1, resourceGroup1"
```

## <a name="unionresults"></a>Combinación de los resultados de dos consultas para formar un solo resultado

La siguiente consulta utiliza `union` para obtener los resultados de la tabla _ResourceContainers_ y agregarlos a los resultados de la tabla _Resources_.

```kusto
ResourceContainers
| where type=='microsoft.resources/subscriptions/resourcegroups' | project name, type  | limit 5
| union  (Resources | project name, type | limit 5)
```

```azurecli-interactive
az graph query -q "ResourceContainers | where type=='microsoft.resources/subscriptions/resourcegroups' | project name, type  | limit 5 | union  (Resources | project name, type | limit 5)"
```

```azurepowershell-interactive
Search-AzGraph -Query "ResourceContainers | where type=='microsoft.resources/subscriptions/resourcegroups' | project name, type  | limit 5 | union  (Resources | project name, type | limit 5)"
```

## <a name="displaynames"></a>Inclusión de nombres de inquilinos y suscripciones en DisplayNames

Esta consulta usa el nuevo parámetro**Include**con la opción _DisplayNames_ para agregar **subscriptionDisplayName** y **tenantDisplayName**a los resultados. Este parámetro solo está disponible para la CLI de Azure y Azure PowerShell.

```azurecli-interactive
az graph query -q "limit 1" --include displayNames
```

```azurepowershell-interactive
Search-AzGraph -Query "limit 1" -Include DisplayNames
```

> [!NOTE]
> Si la consulta no usa **project** para especificar las propiedades devueltas, **subscriptionDisplayName** y **tenantDisplayName** se incluyen automáticamente en los resultados.
> Si la consulta usa **project**, cada uno de los campos de _DisplayName_ se debe incluir explícitamente en **project**, o no se devolverán en los resultados, ni siquiera cuando se use el parámetro **Include**.

## <a name="next-steps"></a>Pasos siguientes

- Consulte ejemplos de [consultas de inicio](starter.md)
- Más información sobre el [lenguaje de consulta](../concepts/query-language.md)
- Más información sobre la [exploración de recursos](../concepts/explore-resources.md)