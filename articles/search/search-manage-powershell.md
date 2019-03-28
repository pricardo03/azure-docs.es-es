---
title: Scripts de PowerShell con el módulo Az.Search - Azure Search
description: Crear y configurar un servicio Azure Search con PowerShell. Puede escalar un servicio o reducir verticalmente, administrar la administración y las claves de api de consulta y consultar información del sistema.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.devlang: powershell
ms.topic: conceptual
ms.date: 03/11/2019
ms.author: heidist
ms.openlocfilehash: 7a91ad691089ac816b31ebe1fce202110e580f71
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2019
ms.locfileid: "58520571"
---
# <a name="manage-your-azure-search-service-with-powershell"></a>Administración del servicio Azure Search con PowerShell
> [!div class="op_single_selector"]
> * [Portal](search-manage.md)
> * [PowerShell](search-manage-powershell.md)
> * [API DE REST](https://docs.microsoft.com/rest/api/searchmanagement/)
> * [SDK de .NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.search)
> * [Python](https://pypi.python.org/pypi/azure-mgmt-search/0.1.0)> 

Puede ejecutar los scripts y cmdlets de PowerShell en Windows, Linux, o en [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) para crear y configurar [Azure Search](https://docs.microsoft.com/azure/search/). El [ **Az.Search** ](https://docs.microsoft.com/powershell/module/az.search/?view=azps-1.4.0#search) módulo amplía [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azps-1.4.0) con paridad completa a la [las API de REST de administración de Azure Search](https://docs.microsoft.com/rest/api/searchmanagement). Con Azure PowerShell y **Az.Search**, puede realizar las siguientes tareas:

> [!div class="checklist"]
> * [Lista de todos los servicios de búsqueda en su suscripción](#list-search-services)
> * [Obtenga información acerca de un servicio de búsqueda específica](#get-search-service-information)
> * [Crear o eliminar un servicio](#create-or-delete-a-service)
> * Volver a generar las claves de API de administración
> * [Crear o eliminar claves de api de consulta](#create-or-delete-query-keys)
> * [Escalar un servicio aumentando o reduciendo las réplicas y particiones](#scale-replicas-and-partitions)

No se puede usar PowerShell para cambiar el nombre, región o niveles de su servicio. Cuando se crea un servicio, se asignan recursos dedicados. Cambiar el hardware subyacente (tipo de nodo o ubicación), requiere un nuevo servicio. No hay ninguna API o herramienta de transferencia de contenido. Toda la administración de contenido es a través de [REST](https://docs.microsoft.com/rest/api/searchservice/) o [.NET](https://docs.microsoft.com/dotnet/api/?term=microsoft.azure.search) API, y si desea mover los índices, se deberá volver a crear y volver a cargarlos en un nuevo servicio. 

Aunque no hay ningún comando de PowerShell dedicado para administración de contenido, puede escribir el script de PowerShell que llama a REST o .NET para crear y cargar índices. El **Az.Search** módulo por sí solo no proporciona estas operaciones.

Otras tareas que no se admite a través de PowerShell o cualquier otra API (solo portal) incluyen:
+ [Asociar un recurso de cognitive services](cognitive-search-attach-cognitive-services.md) para [indexación enriquecida de inteligencia artificial](cognitive-search-concept-intro.md). Un servicio cognitive Services se adjunta a un conjunto de habilidades, no una suscripción o servicio.
+ [Soluciones de supervisión del complemento](search-monitor-usage.md#add-on-monitoring-solutions) o [análisis de tráfico de búsqueda](search-traffic-analytics.md) usada para la supervisión de Azure Search.

<a name="check-versions-and-load"></a>

## <a name="check-versions-and-load-modules"></a>Comprobar las versiones y carga los módulos

Los ejemplos de este artículo son interactivos y requieren permisos elevados. Azure PowerShell (el **Az** módulo) debe estar instalado. Para más información, vea [Instalar Azure PowerShell](/powershell/azure/overview).

### <a name="powershell-version-check-51-or-later"></a>Comprobación de la versión de PowerShell (5.1 o versiones posterior)

Debe ser local PowerShell 5.1 o una versión posterior, en cualquier sistema operativo compatible.

```azurepowershell-interactive
$PSVersionTable.PSVersion
```

### <a name="load-azure-powershell"></a>Carga de Azure PowerShell

Si no sabe si **Az** está instalado, ejecute el siguiente comando como un paso de verificación. 

```azurepowershell-interactive
Get-InstalledModule -Name Az
```

Algunos sistemas hacer no cargar automáticamente los módulos. Si se produce un error el comando anterior, intente cargar el módulo y si se produce un error, vuelva a las instrucciones de instalación para ver si se perdió un paso.

```azurepowershell-interactive
Import-Module -Name Az
```

### <a name="connect-to-azure-with-a-browser-sign-in-token"></a>Conectarse a Azure con un token de inicio de sesión de explorador

Puede usar sus credenciales de inicio de sesión de portal para conectarse a una suscripción en PowerShell. Como alternativa, puede [autenticarse de forma no interactiva con una entidad de servicio](../active-directory/develop/howto-authenticate-service-principal-powershell.md).

```azurepowershell-interactive
Connect-AzAccount
```

Si tiene varias suscripciones de Azure, establezca la suscripción de Azure. Para ver una lista de las suscripciones actuales, ejecute este comando.

```azurepowershell-interactive
Get-AzSubscription | sort SubscriptionName | Select SubscriptionName
```

Para especificar la suscripción, ejecute el siguiente comando. En el ejemplo siguiente, el nombre de la suscripción es `ContosoSubscription`.

```azurepowershell-interactive
Select-AzSubscription -SubscriptionName ContosoSubscription
```

<a name="list-search-services"></a>

## <a name="list-all-azure-search-services-in-your-subscription"></a>Enumerar todos los servicios de Azure Search en su suscripción

Son los siguientes comandos de [ **Az.Resources**](https://docs.microsoft.com/powershell/module/az.resources/?view=azps-1.4.0#resources), devolver información acerca de los recursos existentes y servicios ya aprovisionados en su suscripción. Si no sabe cuántos de los servicios de búsqueda se han creado, estos comandos devuelven esa información, lo que le ahorra un viaje en el portal.

El primer comando devuelve todos los servicios de búsqueda.

```azurepowershell-interactive
Get-AzResource -ResourceType Microsoft.Search/searchServices | ft
```

En la lista de servicios, devuelven información sobre un recurso específico.

```azurepowershell-interactive
Get-AzResource -ResourceName <service-name>
```

Resultados deben ser similares al siguiente resultado.

```
Name              : my-demo-searchapp
ResourceGroupName : demo-westus
ResourceType      : Microsoft.Search/searchServices
Location          : westus
ResourceId        : /subscriptions/<alpha-numeric-subscription-ID>/resourceGroups/demo-westus/providers/Microsoft.Search/searchServices/my-demo-searchapp
```

## <a name="import-azsearch"></a>Importar Az.Search

Los comandos de [ **Az.Search** ](https://docs.microsoft.com/powershell/module/az.search/?view=azps-1.4.0#search) no están disponibles hasta que se cargue el módulo.

```azurepowershell-interactive
Install-Module -Name Az.Search
```

### <a name="list-all-azsearch-commands"></a>Enumerar todos los comandos Az.Search

Como paso de verificación, devolver una lista de comandos proporcionados en el módulo.

```azurepowershell-interactive
Get-Command -Module Az.Search
```

Resultados deben ser similares al siguiente resultado.

```
CommandType     Name                                Version    Source
-----------     ----                                -------    ------
Cmdlet          Get-AzSearchAdminKeyPair            0.7.1      Az.Search
Cmdlet          Get-AzSearchQueryKey                0.7.1      Az.Search
Cmdlet          Get-AzSearchService                 0.7.1      Az.Search
Cmdlet          New-AzSearchAdminKey                0.7.1      Az.Search
Cmdlet          New-AzSearchQueryKey                0.7.1      Az.Search
Cmdlet          New-AzSearchService                 0.7.1      Az.Search
Cmdlet          Remove-AzSearchQueryKey             0.7.1      Az.Search
Cmdlet          Remove-AzSearchService              0.7.1      Az.Search
Cmdlet          Set-AzSearchService                 0.7.1      Az.Search
```

## <a name="get-search-service-information"></a>Obtener información del servicio de búsqueda

Después de **Az.Search** se importa y sabe que el grupo de recursos que contiene el servicio de búsqueda, ejecutar [Get AzSearchService](https://docs.microsoft.com/powershell/module/az.search/get-azsearchservice?view=azps-1.4.0) para devolver la definición de servicio, incluido el nombre, región, nivel y réplica y recuentos de particiones.

```azurepowershell-interactive
Get-AzSearchService -ResourceGroupName <resource-group-name>
```

Resultados deben ser similares al siguiente resultado.

```
Name              : my-demo-searchapp
ResourceGroupName : demo-westus
ResourceType      : Microsoft.Search/searchServices
Location          : West US
Sku               : Standard
ReplicaCount      : 1
PartitionCount    : 1
HostingMode       : Default
ResourceId        : /subscriptions/<alphanumeric-subscription-ID>/resourceGroups/demo-westus/providers/Microsoft.Search/searchServices/my-demo-searchapp
```

## <a name="create-or-delete-a-service"></a>Crear o eliminar un servicio

[**Nuevo AzSearchService** ](https://docs.microsoft.com/powershell/module/az.search/new-azsearchadminkey?view=azps-1.4.0) se usa para [crear un nuevo servicio de búsqueda](search-create-service-portal.md).

```azurepowershell-interactive
New-AzSearchService -ResourceGroupName "demo-westus" -Name "my-demo-searchapp" -Sku "Standard" -Location "West US" -PartitionCount 3 -ReplicaCount 3
``` 
Resultados deben ser similares al siguiente resultado.

```
ResourceGroupName : demo-westus
Name              : my-demo-searchapp
Id                : /subscriptions/<alphanumeric-subscription-ID>/demo-westus/providers/Microsoft.Search/searchServices/my-demo-searchapp
Location          : West US
Sku               : Standard
ReplicaCount      : 3
PartitionCount    : 3
HostingMode       : Default
Tags
```     

## <a name="regenerate-admin-keys"></a>Regeneración de claves de administración

[**Nuevo AzSearchAdminKey** ](https://docs.microsoft.com/powershell/module/az.search/new-azsearchadminkey?view=azps-1.4.0) se usa para sustituir admin [claves de API](search-security-api-keys.md). Se crean dos claves de administración con cada servicio para el acceso autenticado. Claves son necesarias en cada solicitud. Las dos claves de administración son funcionalmente equivalentes, conceder acceso de escritura completo a un servicio de búsqueda con la capacidad de recuperar toda la información, o crear y eliminar cualquier objeto. Dos claves existen para que puedan usar uno mientras se reemplaza el otro. 

Puede solo vuelve a generar una vez, especificado como el `primary` o `secondary` clave. Para un servicio ininterrumpido, no olvide actualizar todo el código de cliente para usar una clave secundaria mientras sustituir la clave principal. Evite cambiar las claves, mientras que las operaciones están en tránsito.

Como puede imaginar, si regenera las claves sin actualizar el código de cliente, se producirá un error en las solicitudes con la clave antigua. Volver a generar todas las nuevas claves no permanentemente bloquear el acceso a su servicio y aún puede acceder al servicio a través del portal. Después de regenerar claves principales y secundarias, puede actualizar el código de cliente para utilizar las nuevas claves y las operaciones se reanudarán en consecuencia.

El servicio genera valores de las claves de API. No puede proporcionar una clave personalizada para Azure Search usar. De forma similar, no hay ningún nombre definido por el usuario para la administración de claves de API. Las referencias a la clave se corrigen las cadenas, ya sea `primary` o `secondary`. 

```azurepowershell-interactive
New-AzSearchAdminKey -ResourceGroupName <resource-group-name> -ServiceName <search-service-name> -KeyKind Primary
```

Resultados deben ser similares al siguiente resultado. Aunque solo cambiar una vez, se devuelven las dos claves.

```
Primary                    Secondary
-------                    ---------
<alphanumeric-guid>        <alphanumeric-guid>  
```

## <a name="create-or-delete-query-keys"></a>Crear o eliminar las claves de consulta

[**Nuevo AzSearchQueryKey** ](https://docs.microsoft.com/powershell/module/az.search/new-azsearchquerykey?view=azps-1.4.0) se usa para crear consultas [claves de API](search-security-api-keys.md) para el acceso de solo lectura desde aplicaciones cliente a un índice de Azure Search. Las claves de consulta se utilizan para autenticar a un índice específico con el fin de recuperar los resultados de búsqueda. Las claves de consulta no conceden acceso de solo lectura a otros elementos en el servicio, como un índice, el origen de datos o el indizador.

No puede proporcionar una clave de búsqueda de Azure usar. El servicio genera claves de API.

```azurepowershell-interactive
New-AzSearchQueryKey -ResourceGroupName <resource-group-name> -ServiceName <search-service-name> -Name <query-key-name> 
```

## <a name="scale-replicas-and-partitions"></a>Escala réplicas y particiones

[**Conjunto AzSearchService** ](https://docs.microsoft.com/powershell/module/az.search/set-azsearchservice?view=azps-1.4.0) se usa para [aumentar o disminuir las réplicas y particiones](search-capacity-planning.md) reajustar recursos facturables dentro de su servicio. Aumentar las réplicas o particiones agrega a la factura, que los tiene gastos fijos y variables. Si tiene una necesidad temporal de potencia de procesamiento adicional, puede aumentar las réplicas y particiones para controlar la carga de trabajo. El área de supervisión en la página información general del portal tiene iconos en la latencia de consulta, consultas por segundo y la limitación, que indica si la capacidad actual es adecuada.

Puede tardar un rato en Agregar o quitar recursos. Los ajustes de capacidad se producen en segundo plano, lo que permite las cargas de trabajo existentes continuar. Capacidad adicional se usa para las solicitudes entrantes en cuanto esté listo, sin ninguna configuración adicional necesaria. 

Eliminación de capacidad puede ser perjudiciales. Detener todos los trabajos de indexación y el indizador antes de reducir la capacidad se recomienda evitar solicitudes interrumpidas. Si esto no es factible, es posible que considere la posibilidad de reducir la capacidad de forma incremental, una réplica y partición a la vez, hasta que se alcancen los nuevos niveles de destino.

Una vez que envíe el comando, no hay ninguna manera de finalización a. Tendrá que esperar hasta que finalice el comando antes de revisar los recuentos.

```azurepowershell-interactive
Set-AzSearchService -ResourceGroupName <resource-group-name> -Name <search-service-name> -PartitionCount 6 -ReplicaCount 6
```

Resultados deben ser similares al siguiente resultado.

```
ResourceGroupName : demo-westus
Name              : my-demo-searchapp
Location          : West US
Sku               : Standard
ReplicaCount      : 6
PartitionCount    : 6
HostingMode       : Default
Id                : /subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/demo-westus/providers/Microsoft.Search/searchServices/my-demo-searchapp
```


## <a name="next-steps"></a>Pasos siguientes

Crear un [índice](search-what-is-an-index.md), [consultar un índice](search-query-overview.md) mediante el portal, las API de REST o el SDK. NET.

* [Creación de un índice de Azure Search en Azure Portal](search-create-index-portal.md)
* [Configurar un indexador para cargar datos de otros servicios](search-indexer-overview.md)
* [Consulta de un índice de Azure Search mediante el Explorador de búsqueda de Azure Portal](search-explorer.md)
* [Cómo usar Azure Search en .NET](search-howto-dotnet-sdk.md)
