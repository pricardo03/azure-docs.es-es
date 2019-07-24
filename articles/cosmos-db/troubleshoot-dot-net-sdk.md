---
title: Diagnóstico y solución de problemas al usar el SDK de .NET de Azure Cosmos DB
description: Use características como registro del lado cliente y otras herramientas de terceros para identificar, diagnosticar y solucionar problemas de Azure Cosmos DB cuando use el SDK de .NET.
author: j82w
ms.service: cosmos-db
ms.date: 05/28/2019
ms.author: jawilley
ms.devlang: c#
ms.subservice: cosmosdb-sql
ms.topic: troubleshooting
ms.reviewer: sngun
ms.openlocfilehash: 7e48809537acc21edbcf12d299a333df486c258f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66257149"
---
# <a name="diagnose-and-troubleshoot-issues-when-using-azure-cosmos-db-net-sdk"></a>Diagnóstico y solución de problemas al usar el SDK de .NET de Azure Cosmos DB
En este artículo se tratan problemas comunes, soluciones alternativas, pasos de diagnóstico y herramientas al usar el [SDK de .NET](sql-api-sdk-dotnet.md) con las cuentas de la API de SQL de Azure Cosmos DB.
El SDK de .NET proporciona la representación lógica del lado cliente para acceder a la API de SQL de Azure Cosmos DB. En este artículo se describen herramientas y enfoques para ayudarle si surge algún problema.

## <a name="checklist-for-troubleshooting-issues"></a>Lista de comprobación para la solución de problemas:
Tenga en cuenta la siguiente lista de comprobación antes de que la aplicación pase a la fase de producción. El uso de la lista de comprobación impedirá que se produzcan distintos problemas comunes que podrían surgir. Puede diagnosticar rápidamente cuándo se produce un problema:

*   Utilice el [SDK](https://github.com/Azure/azure-cosmos-dotnet-v2/blob/master/changelog.md) más reciente. No se debe utilizar la previsualización de SDK en la producción. Esto evitará problemas conocidos que ya están solucionados.
*   Revise los [consejos de rendimiento](performance-tips.md) y siga las prácticas sugeridas. Esto ayudará a evitar el escalado, la latencia y otros problemas de rendimiento.
*   Habilite el registro de SDK para ayudarle a solucionar un problema. La habilitación del registro puede afectar al rendimiento, por lo que es mejor hacerlo solo cuando se vayan a solucionar problemas. Puede habilitar los registros siguientes:
    *   Registre [métricas](monitor-accounts.md) mediante Azure Portal. Las métricas del portal muestran la telemetría de Azure Cosmos DB, que resulta útil para determinar si el problema corresponde a Azure Cosmos DB o es de parte del cliente.
    *   Registre la [cadena de diagnóstico](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.resourceresponsebase.requestdiagnosticsstring?view=azure-dotnet) de las respuestas de operación de punto.
    *   Registre las [métricas de consultas SQL](sql-api-query-metrics.md) desde todas las respuestas de consultas. 
    *   Siga la configuración del [registro del SDK]( https://github.com/Azure/azure-cosmos-dotnet-v2/blob/master/docs/documentdb-sdk_capture_etl.md).

Eche un vistazo a la sección [Problemas y soluciones](#common-issues-workarounds) de este artículo.

Compruebe que la [sección de problemas de GitHub](https://github.com/Azure/azure-cosmos-dotnet-v2/issues) se esté supervisando activamente. Compruebe si encuentra algún problema similar con una solución alternativa ya registrada. Si no logró solucionarlo, registre un problema de GitHub. Puede abrir una incidencia de soporte técnico para problemas urgentes.


## <a name="common-issues-workarounds"></a>Problemas comunes y soluciones alternativas

### <a name="general-suggestions"></a>Sugerencias generales
* Ejecute la aplicación en la misma región de Azure que su cuenta de Azure Cosmos DB, si es posible. 
* Es posible que experimente problemas de conectividad o disponibilidad debido a falta de recursos en el equipo cliente. Le recomendamos que supervise el uso de la CPU en los nodos que ejecutan el cliente de Azure Cosmos DB y la escalación vertical u horizontal si están ejecutando una carga alta.

### <a name="check-the-portal-metrics"></a>Comprobación de las métricas del portal
La comprobación de las [métricas del portal](monitor-accounts.md) le ayudarán a determinar si hay un problema por parte del cliente o si hay un problema con el servicio. Por ejemplo, si las métricas contienen una alta tasa de solicitudes de velocidad limitada (código de estado HTTP 429), lo que significa que la solicitud se ha limitado, consulte la sección [Tasa de solicitudes demasiado grande]. 

### <a name="request-timeouts"></a>Tiempos de espera de solicitudes
RequestTimeout se produce cuando se usa la conexión directa o TCP, pero puede producirse también en el modo de puerta de enlace. Estas son las causas conocidas comunes y las sugerencias sobre cómo corregir el problema.

* El uso de la CPU es elevado, lo que provocará latencia o tiempos de espera de la solicitud. El cliente puede escalar verticalmente el equipo host para proporcionarle más recursos, o se puede distribuir la carga entre más equipos.
* La disponibilidad de puertos/sockets puede ser reducida. Cuando se usan SDK de .NET anteriores a la versión 2.0, los clientes que se ejecutan en Azure pueden provocar el [agotamiento de puertos SNAT (PAT) de Azure]. Este un ejemplo de por qué se recomienda ejecutar siempre la versión más reciente del SDK.
* La creación de varias instancias de DocumentClient podría provocar problemas de tiempo de espera y la contención de la conexión. Siga los [consejos de rendimiento](performance-tips.md) y use una sola instancia de DocumentClient en un proceso completo.
* A veces, los usuarios verán tiempos de espera de solicitud o latencia elevados porque sus colecciones no se aprovisionan lo suficiente, el back-end limita las solicitudes y el cliente realiza reintentos de forma interna sin comunicarlo al autor de la llamada. Compruebe las [métricas del portal](monitor-accounts.md).
* Azure Cosmos DB distribuye el rendimiento general aprovisionado de forma uniforme entre las particiones físicas. Compruebe las métricas del portal para ver si la carga de trabajo detecta una [clave de partición](partition-data.md) frecuente. Esto hará que el rendimiento consumido agregado (RU/s) parezca estar por debajo de las RU aprovisionadas, pero un rendimiento consumido de una sola partición (RU/s) superará el rendimiento aprovisionado. 
* Además, el SDK 2.0 agrega semántica de canal la conexión directa o TCP. Se utiliza una conexión TCP para varias solicitudes al mismo tiempo. Esto puede provocar dos problemas en casos concretos:
    * Un alto grado de simultaneidad puede producir la contención en el canal.
    * Un gran número de solicitudes o respuestas puede provocar un bloqueo de encabezado de línea en el canal y agravar la contención, incluso con un nivel relativamente bajo de simultaneidad.
    * Si el caso se encuentra en alguna de estas dos categorías (o si se sospecha de un uso elevado de la CPU), estas son las posibles soluciones:
        * Intente escalar vertical u horizontalmente la aplicación.
        * Además, los registros de SDK se pueden capturar a través de [clientes de escucha de seguimiento](https://github.com/Azure/azure-cosmosdb-dotnet/blob/master/docs/documentdb-sdk_capture_etl.md) para obtener más información.

### <a name="connection-throttling"></a>Limitación de la conexión
La limitación de la conexión puede deberse a un límite de conexión en el equipo host. Antes de la versión 2.0, los clientes que se ejecutaban en Azure podían provocar el [Agotamiento de puertos SNAT (PAT) de Azure].

### <a name="snat"></a>Agotamiento de puertos SNAT (PAT) de Azure

Si la aplicación está implementada en Azure Virtual Machines sin una dirección IP pública, los [puertos SNAT de Azure](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections#preallocatedports) se usan de manera predeterminada para establecer conexiones con cualquier punto de conexión fuera de la máquina virtual. El número de conexiones permitidas desde la máquina virtual hasta el punto de conexión de Azure Cosmos DB está limitado por la [configuración de Azure SNAT](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections#preallocatedports).

 Los puertos SNAT de Azure se usan solo cuando la máquina virtual tiene una dirección IP privada y un proceso de la máquina virtual intenta conectarse con una dirección IP pública. Hay dos soluciones alternativas para evitar la limitación de Azure SNAT:

* Agregue el punto de conexión de servicio de Azure Cosmos DB a la subred de la red virtual de Azure Virtual Machines. Para obtener más información, consulte [puntos de conexión de servicio de red virtual de Azure](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview). 

    Cuando se habilita el punto de conexión de servicio, las solicitudes ya no se envían desde una dirección IP pública a Azure Cosmos DB. En su lugar, se envían la red virtual y la identidad de la subred. Este cambio puede producir caídas de firewall si solo se permiten direcciones IP públicas. Si usa un firewall, cuando se habilite el punto de conexión de servicio, agregue una subred al firewall mediante las [ACL de Virtual Network](https://docs.microsoft.com/azure/virtual-network/virtual-networks-acl).
* Asignar una dirección IP pública a la máquina virtual de Azure.

### <a name="http-proxy"></a>Proxy HTTP
Si usa un Proxy HTTP, asegúrese de que pueda admitir el número de conexiones configuradas en el SDK de `ConnectionPolicy`.
En caso contrario, se encontrará con problemas de conexión.

### Tasa de solicitudes demasiado grande<a name="request-rate-too-large"></a>
"Tasa de solicitudes demasiado grande" o el código de error 429 indican que sus solicitudes se han limitado porque el rendimiento consumido (RU/s) ha superado el rendimiento aprovisionado. El SDK reintentará automáticamente las solicitudes según la [directiva de reintentos](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.connectionpolicy.retryoptions?view=azure-dotnet) especificada. Si recibe este error con frecuencia, considere la posibilidad de aumentar el rendimiento de la colección. Compruebe las [métricas del portal](use-metrics.md) para ver si obtiene el error 429. Revise su [clave de partición](https://docs.microsoft.com/azure/cosmos-db/partitioning-overview#choose-partitionkey) para asegurarse de que se produce una distribución uniforme del volumen de solicitudes y almacenamiento. 

### <a name="slow-query-performance"></a>Rendimiento lento de las consultas
Las [métricas de consulta](sql-api-query-metrics.md) le ayudarán a determinar dónde está dedicando más tiempo la consulta. En las métricas de consulta, puede ver la cantidad que se dedica al back-end en comparación con el cliente.
* Si la consulta de back-end se devuelve rápidamente y se dedica una gran cantidad de tiempo al cliente, compruebe la carga en la máquina. Es probable que no haya suficientes recursos y el SDK esté esperando que haya recursos disponibles para gestionar la respuesta.
* Si la consulta de back-end es lenta, intente [optimizar la consulta](optimize-cost-queries.md) y examine la [directiva de indexación](index-overview.md). 

 <!--Anchors-->
[Common issues and workarounds]: #common-issues-workarounds
[Enable client SDK logging]: #logging
[Tasa de solicitudes demasiado grande]: #request-rate-too-large
[Request Timeouts]: #request-timeouts
[Agotamiento de puertos SNAT (PAT) de Azure]: #snat
[Production check list]: #production-check-list


