---
title: Diagnosticar y solucionar problemas cuando se usa el SDK de .NET de Azure Cosmos DB
description: Use características como el registro del lado cliente y otras herramientas de terceros para identificar, diagnosticar y solucionar problemas de Azure Cosmos DB mediante .NET SDK.
author: j82w
ms.service: cosmos-db
ms.date: 01/19/2019
ms.author: jawilley
ms.devlang: c#
ms.subservice: cosmosdb-sql
ms.topic: troubleshooting
ms.reviewer: sngun
ms.openlocfilehash: 7f969ab6059140ec32c9c5bf5045c546602a3c15
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60404723"
---
# <a name="diagnose-and-troubleshoot-issues-when-using-azure-cosmos-db-net-sdk"></a>Diagnosticar y solucionar problemas cuando se usa el SDK de .NET de Azure Cosmos DB
Este artículo tratan problemas comunes, soluciones alternativas, herramientas y pasos de diagnóstico cuando se usa el [.NET SDK](sql-api-sdk-dotnet.md) con cuentas de Azure Cosmos DB SQL API.
El SDK de .NET proporciona la representación lógica del lado cliente para tener acceso a la API de SQL de Azure Cosmos DB. En este artículo se describen herramientas y enfoques para ayudarle si surge algún problema.

## <a name="checklist-for-troubleshooting-issues"></a>Lista de comprobación para solucionar problemas:
Tenga en cuenta la siguiente lista de comprobación antes de mover la aplicación en producción. El uso de la lista de comprobación impedirá varios problemas comunes que podría ver. Puede diagnosticar rápidamente cuando se produce un problema:

*   Use la versión más reciente [SDK](https://github.com/Azure/azure-cosmos-dotnet-v2/blob/master/changelog.md). SDK de versión preliminar no deben usarse para la producción. Esto evitará problemas conocidos que ya se ha corregido.
*   Revise los [consejos de rendimiento](performance-tips.md) y siga las prácticas sugeridas. Esto ayudará a evitar el escalado, la latencia y otros problemas de rendimiento.
*   Habilitar el registro de SDK para ayudarle a solucionar un problema. Habilitar el registro puede afectar el rendimiento, por lo que es mejor habilitar la hora de solucionar problemas. Puede habilitar los registros siguientes:
    *   [Registrar las métricas](monitor-accounts.md) mediante el portal de Azure. Métricas del portal muestran la telemetría de Azure Cosmos DB, lo cual resulta útil para determinar si el problema se corresponde con Azure Cosmos DB o si está en el lado del cliente.
    *   Registro de la [cadena diagnóstico](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.resourceresponsebase.requestdiagnosticsstring?view=azure-dotnet) de las respuestas de operación de punto.
    *   Registro de la [métricas de consulta SQL](sql-api-query-metrics.md) de todas las respuestas de consulta 
    *   Siga el programa de instalación de [registro SDK]( https://github.com/Azure/azure-cosmos-dotnet-v2/blob/master/docs/documentdb-sdk_capture_etl.md)

Eche un vistazo a la sección [Problemas y soluciones](#common-issues-workarounds) de este artículo.

Compruebe el [sección problemas de GitHub](https://github.com/Azure/azure-cosmos-dotnet-v2/issues) que se supervisa activamente. Compruebe si encuentra algún problema similar con una solución alternativa ya registrada. Si no encuentra una solución, a continuación, notificar un problema en GitHub. Puede abrir un paso de soporte técnico para problemas urgentes.


## <a name="common-issues-workarounds"></a>Problemas comunes y soluciones alternativas

### <a name="general-suggestions"></a>Sugerencias generales
* Ejecute la aplicación en la misma región de Azure con su cuenta de Azure Cosmos DB, siempre que sea posible. 
* Es posible que experimenta problemas de conectividad o disponibilidad debido a falta de recursos en el equipo cliente. Le recomendamos que supervise el uso de CPU en los nodos que ejecutan al cliente de Azure Cosmos DB y el escalado vertical u horizontal si se está ejecutando una carga alta.

### <a name="check-the-portal-metrics"></a>Compruebe las métricas del portal
Comprobando la [métricas del portal](monitor-accounts.md) le ayudará a determinar si es un problema del lado cliente o si hay un problema con el servicio. Por ejemplo si las métricas contienen una alta tasa de solicitudes de velocidad limitada (código de estado HTTP 429) lo que significa que se esté limitando la solicitud, a continuación, compruebe el [tasa de solicitud demasiado grande] sección. 

### <a name="request-timeouts"></a>Tiempos de espera de solicitudes
RequestTimeout normalmente ocurre cuando se usa Direct/TCP, pero se puede producir en el modo de puerta de enlace. Estas son las causas conocidas comunes y sugerencias sobre cómo solucionar el problema.

* Uso de CPU es alto, que se producir latencia o tiempos de espera de solicitud. El cliente puede escalar verticalmente la máquina host para darle más recursos, o se puede distribuir la carga en varias máquinas.
* Socket o disponibilidad del puerto puede ser bajo. Cuando se usa el SDK de .NET que se lanzó antes que la versión 2.0, los clientes que se ejecutan en Azure pudieran acertar el [Agotamiento de puertos SNAT (PAT) de Azure]. Este un ejemplo de por qué se recomienda ejecutar siempre la versión más reciente del SDK.
* Creación de varias instancias de DocumentClient podría provocar problemas de tiempo de espera y la contención de la conexión. Siga el [sugerencias de rendimiento](performance-tips.md)y el uso de una sola instancia de DocumentClient en un proceso completo.
* A veces, los usuarios verán elevados tiempos de espera de solicitud o la latencia porque sus colecciones se aprovisionan insuficientemente, el back-end limita las solicitudes y el cliente lo reintenta internamente sin presentar esto al llamador. Compruebe el [métricas del portal](monitor-accounts.md).
* Azure Cosmos DB distribuye el rendimiento aprovisionado total uniformemente entre las particiones físicas. Compruebe las métricas del portal para ver si la carga de trabajo está detectando un activo [clave de partición](partition-data.md). Esto hará que el rendimiento consumido agregado (RU/s) y se parece estar bajo el RU aprovisionadas, pero un rendimiento de una sola partición consumido (RU/s) superará el rendimiento aprovisionado. 
* Además, el SDK 2.0 agrega semántica de canal para las conexiones de direct/TCP. Se usa una conexión TCP para varias solicitudes al mismo tiempo. Esto puede dar lugar a dos problemas en casos concretos:
    * Un alto grado de simultaneidad puede producir conflictos en el canal.
    * Grandes solicitudes o respuestas pueden producir un bloqueo de encabezado de la línea en el canal y agravar la contención, incluso con un grado de simultaneidad relativamente bajo.
    * Si el caso de que se encuentra en cualquiera de estas dos categorías (o si sospecha que el uso elevado de CPU), estos son los posibles remedios:
        * Pruebe a escalar la aplicación vertical u horizontalmente.
        * Además, se pueden capturar los registros del SDK mediante [escucha de seguimiento](https://github.com/Azure/azure-cosmosdb-dotnet/blob/master/docs/documentdb-sdk_capture_etl.md) para obtener más detalles.

### <a name="connection-throttling"></a>Limitación de la conexión
Conexión de limitación puede ocurrir debido a un límite de conexiones en un equipo host. Anterior a 2.0, los clientes que se ejecutan en Azure pudieran acertar el [Agotamiento de puertos SNAT (PAT) de Azure].

### <a name="snat"></a>Agotamiento de puertos SNAT (PAT) de Azure

Si la aplicación está implementada en Azure Virtual Machines sin una dirección IP pública, los [puertos SNAT de Azure](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections#preallocatedports) se usan de manera predeterminada para establecer conexiones con cualquier punto de conexión fuera de la máquina virtual. El número de conexiones permitidas desde la máquina virtual hasta el punto de conexión de Azure Cosmos DB está limitado por la [configuración de Azure SNAT](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections#preallocatedports).

 Los puertos SNAT de Azure se usan solo cuando la máquina virtual tiene una dirección IP privada y un proceso de la máquina virtual intenta conectarse con una dirección IP pública. Hay dos soluciones alternativas para evitar la limitación de Azure SNAT:

* Agregue el punto de conexión de servicio de Azure Cosmos DB a la subred de la red virtual de Azure Virtual Machines. Para obtener más información, consulte [puntos de conexión de servicio de red virtual de Azure](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview). 

    Cuando se habilita el punto de conexión de servicio, las solicitudes ya no se envían desde una dirección IP pública a Azure Cosmos DB. En su lugar, se envían la red virtual y la identidad de la subred. Este cambio puede producir caídas de firewall si solo se permiten direcciones IP públicas. Si usa un firewall, cuando se habilite el punto de conexión de servicio, agregue una subred al firewall mediante las [ACL de Virtual Network](https://docs.microsoft.com/azure/virtual-network/virtual-networks-acl).
* Asignar una dirección IP pública a la máquina virtual de Azure.

### <a name="http-proxy"></a>Proxy HTTP
Si usa un Proxy HTTP, asegúrese de que pueda admitir el número de conexiones configuradas en el SDK de `ConnectionPolicy`.
En caso contrario, se encontrará con problemas de conexión.

### Tasa de solicitud demasiado grande<a name="request-rate-too-large"></a>
'Demasiado grande de la frecuencia de solicitudes' o el código de error 429 indica que se está limitando las solicitudes, debido a que el rendimiento consumido (RU/s) ha superado el rendimiento aprovisionado. El SDK reintentará automáticamente las solicitudes según lo especificado [directiva de reintentos](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.connectionpolicy.retryoptions?view=azure-dotnet). Si recibe este error con frecuencia, considere la posibilidad de aumentar el rendimiento en la colección. Compruebe el [métricas del portal](use-metrics.md) para ver si obtiene 429 errores. Revise su [clave de partición](https://docs.microsoft.com/azure/cosmos-db/partitioning-overview#choose-partitionkey) para asegurarse de da como resultado una distribución uniforme del volumen de almacenamiento y la solicitud. 

### <a name="slow-query-performance"></a>Rendimiento de consultas lentas
El [consultar métricas](sql-api-query-metrics.md) le ayudará a determinar que la consulta pasan la mayor parte del tiempo. En las métricas de consulta, puede ver cuánto de eso se está invertido en el back-end en comparación con el cliente.
* Si la consulta de back-end devuelve rápidamente y emplea un tiempo largo en el cliente compruebe la carga en el equipo. Es probable que no hay suficientes recursos y el SDK está esperando recursos estén disponibles para controlar la respuesta.
* Si la consulta de back-end es lenta, intente [optimizar la consulta](optimize-cost-queries.md) y examinando actual [directiva de indexación](index-overview.md) 

 <!--Anchors-->
[Common issues and workarounds]: #common-issues-workarounds
[Enable client SDK logging]: #logging
[Tasa de solicitud demasiado grande]: #request-rate-too-large
[Request Timeouts]: #request-timeouts
[Agotamiento de puertos SNAT (PAT) de Azure]: #snat
[Production check list]: #production-check-list


