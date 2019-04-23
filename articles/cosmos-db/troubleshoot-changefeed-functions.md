---
title: Diagnosticar y solucionar problemas al usar el desencadenador de Azure Cosmos DB en Azure Functions
description: Problemas comunes, las soluciones alternativas y pasos de diagnóstico cuando se usa el desencadenador de Azure Cosmos DB con Azure Functions
author: ealsur
ms.service: cosmos-db
ms.date: 04/16/2019
ms.author: maquaran
ms.topic: troubleshooting
ms.reviewer: sngun
ms.openlocfilehash: 40d9aba4ff8fd78f6369729ddc16238e65bfc169
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/22/2019
ms.locfileid: "60010897"
---
# <a name="diagnose-and-troubleshoot-issues-when-using-azure-cosmos-db-trigger-in-azure-functions"></a>Diagnosticar y solucionar problemas al usar el desencadenador de Azure Cosmos DB en Azure Functions

Este artículo tratan problemas comunes, las soluciones alternativas y pasos de diagnóstico cuando se usa el [desencadenador de Azure Cosmos DB](change-feed-functions.md) con Azure Functions.

## <a name="dependencies"></a>Dependencias

El desencadenador de Azure Cosmos DB y los enlaces dependen de los paquetes de extensión a través de la base Azure Functions en tiempo de ejecución. Mantenga siempre estos paquetes actualizados, ya que es posible que incluyen correcciones y características nuevas que pueden solucionar los posibles problemas que pueden producirse:

* Para Azure Functions V2, consulte [Microsoft.Azure.WebJobs.Extensions.CosmosDB](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.CosmosDB).
* Para Azure Functions V1, consulte [Microsoft.Azure.WebJobs.Extensions.DocumentDB](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DocumentDB).

En este artículo siempre hará referencia a Azure Functions V2 cada vez que se menciona el tiempo de ejecución, a menos que especifique explícitamente.

## <a name="consuming-the-cosmos-db-sdk-separately-from-the-trigger-and-bindings"></a>Consume el SDK de Cosmos DB por separado de los desencadenadores y enlaces

Es la funcionalidad clave del paquete de extensión proporcionar compatibilidad con el desencadenador de Azure Cosmos DB y los enlaces. También incluye el [.NET SDK de Azure Cosmos DB](sql-api-sdk-dotnet-core.md), lo que resulta útil si desea interactuar con Azure Cosmos DB mediante programación sin usar el desencadenador y enlaces.

Si desea usar el SDK de Azure Cosmos DB, asegúrese de que no agregue al proyecto otra referencia de paquete de NuGet. En su lugar, **permiten la referencia del SDK resolver a través del paquete de extensión de Azure Functions**.

Además, si va a crear manualmente su propia instancia de la [cliente SDK de Azure Cosmos DB](./sql-api-sdk-dotnet-core.md), debe seguir el patrón de tener solo una instancia del cliente [mediante un enfoque de patrón Singleton](../azure-functions/manage-connections.md#documentclient-code-example-c) . Este proceso evita los problemas potenciales de socket en sus operaciones.

## <a name="common-known-scenarios-and-workarounds"></a>Escenarios comunes de conocidos y soluciones alternativas

### <a name="azure-function-fails-with-error-message-either-the-source-collection-collection-name-in-database-database-name-or-the-lease-collection-collection2-name-in-database-database2-name-does-not-exist-both-collections-must-exist-before-the-listener-starts-to-automatically-create-the-lease-collection-set-createleasecollectionifnotexists-to-true"></a>Se produce un error en la función de Azure con el mensaje de error "en la colección de origen 'nombre de la colección' (en la base de datos 'nombre de la base de datos') o la colección de concesión"colección2-name"(en la base de datos 'nombre de la base de datos2') no existe. Ambas colecciones deben existir antes de que se inicia el agente de escucha. Para crear automáticamente la colección de concesión, establezca 'CreateLeaseCollectionIfNotExists' en 'true' "

Esto significa que uno o ambos de los contenedores de Azure Cosmos necesarios para que el desencadenador de trabajo no existen o no son accesibles para la función de Azure. **El propio error le indicará qué base de datos de Azure Cosmos y contenedores es el desencadenador buscando** según su configuración.

1. Compruebe el `ConnectionStringSetting` atributo y que **hace referencia a una configuración que existe en la aplicación de función de Azure**. El valor de este atributo no debe ser la propia cadena de conexión, pero el nombre de la opción de configuración.
2. Compruebe que la `databaseName` y `collectionName` existe en su cuenta de Azure Cosmos. Si usas el reemplazo de valor automático (mediante `%settingName%` patrones), asegúrese de que existe el nombre de la configuración de la aplicación de función de Azure.
3. Si no especifica un `LeaseCollectionName/leaseCollectionName`, el valor predeterminado es "contratos". Compruebe que existe tal contenedor. Opcionalmente, puede establecer el `CreateLeaseCollectionIfNotExists` atributo en el desencadenador para `true` crearlo automáticamente.
4. Compruebe su [configuración del Firewall de la cuenta de Azure Cosmos](how-to-configure-firewall.md) para ver que no está no está bloqueando la función de Azure.

### <a name="azure-function-fails-to-start-with-shared-throughput-collection-should-have-a-partition-key"></a>Función de Azure no puede comenzar con "recopilación de rendimiento compartido debe tener una clave de partición"

Las versiones anteriores de la extensión de Azure Cosmos DB no admitía mediante un contenedor de las concesiones que se creó un [base de datos de rendimiento compartido](./set-throughput.md#set-throughput-on-a-database). Para resolver este problema, actualice el [Microsoft.Azure.WebJobs.Extensions.CosmosDB](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.CosmosDB) extensión para obtener la versión más reciente.

### <a name="azure-function-fails-to-start-with-the-lease-collection-if-partitioned-must-have-partition-key-equal-to-id"></a>Función de Azure no puede comenzar con "la colección de concesión, si tiene particiones, debe tener igual al identificador de clave de partición".

Este error significa que el contenedor de las concesiones actual tiene particiones, pero la ruta de acceso de clave de partición no es `/id`. Para resolver este problema, debe volver a crear el contenedor de concesiones con `/id` como clave de partición.

### <a name="you-see-a-value-cannot-be-null-parameter-name-o-in-your-azure-functions-logs-when-you-try-to-run-the-trigger"></a>Verá un "valor no puede ser null. Nombre de parámetro: o "en los registros de Azure Functions cuando se intenta ejecutar el desencadenador

Este problema aparece si utilizas el portal de Azure y se vuelva a seleccionar la **ejecutar** botón en la pantalla al inspeccionar una función de Azure que usa el desencadenador. El desencadenador no se requiere para que seleccionar ejecutar para iniciar, se iniciará automáticamente cuando se implementa la función de Azure. Si desea comprobar la secuencia de registro de la función de Azure en Azure portal, simplemente vaya al contenedor supervisado e insertar algunos nuevos elementos, verá automáticamente la ejecución del desencadenador.

### <a name="my-changes-take-too-long-be-received"></a>Recibir mi participación cambios demasiado largo

Este escenario puede haber varias causas y todos ellos deben comprobarse:

1. ¿La función de Azure se implementa en la misma región que la cuenta de Azure Cosmos? Óptimo latencia de red, la función de Azure y su cuenta de Azure Cosmos deben estar colocadas en la misma región de Azure.
2. ¿Se producen los cambios en el contenedor de Azure Cosmos continua o esporádico?
Si trata del último caso, podría haber algún retraso entre los cambios que se almacena y seleccionarlos la función de Azure. Esto es porque internamente, cuando el desencadenador comprueba si hay cambios en el contenedor de Azure Cosmos y no encuentra ninguno pendientes para leerse, estará en suspensión durante un periodo de tiempo (5 segundos, de forma predeterminada) configurable antes de comprobar los cambios nuevo (evitar el elevado consumo de RU). Puede configurar este tiempo de suspensión a través de la `FeedPollDelay/feedPollDelay` en el [configuración](../azure-functions/functions-bindings-cosmosdb-v2.md#trigger---configuration) del desencadenador (se espera el valor en milisegundos).
3. Podría ser el contenedor de Azure Cosmos [velocidad limitada](./request-units.md).
4. Puede usar el `PreferredLocations` atributo en el desencadenador para especificar una lista separada por comas de las regiones de Azure para definir un orden personalizado de conexión preferido.

### <a name="some-changes-are-missing-in-my-trigger"></a>Faltan algunos cambios en mi desencadenador

Si observa que algunos de los cambios producidos en el contenedor de Azure Cosmos no se recogen la función de Azure, hay un paso de investigación inicial que debe llevar a cabo.

Cuando la función de Azure recibe los cambios, a menudo los procesa y opcionalmente, enviar el resultado a otro destino. Cuando se está investigando los cambios que faltan, asegúrese de **medida los cambios que se reciben en el punto de ingesta** (cuando se inicia la función de Azure), no en el destino.

Si faltan algunos cambios en el destino, esto podría significar que es algún error que ocurre durante la ejecución de la función de Azure después de que los cambios se han recibido.

En este escenario, la mejor forma de acción consiste en agregar `try/catch blocks` en el código y dentro de los bucles que podrían estar procesando los cambios, para detectar cualquier error para un subconjunto determinado de elementos y controlarlos según corresponda (enviarlos a otro almacenamiento adicional análisis o vuelva a intentarlo). 

> **El desencadenador de Azure Cosmos DB, de forma predeterminada, no vuelva a intentar un lote de cambios si se ha producido una excepción no controlada** durante la ejecución del código. Esto significa que la razón por la que no han llegado los cambios en el destino es ya que se producen errores al procesarlos.

Si encuentra que algunos cambios no han recibido en absoluto el desencadenador, el escenario más común es que hay **se está ejecutando otra instancia de Azure Functions**. Es posible que otra función de Azure implementados en Azure o una función de Azure que se ejecuta localmente en un desarrollador de la máquina que tiene **exactamente la misma configuración** (mismo supervisados y contenedores de la concesión), y esta función de Azure se robe un subconjunto de los cambios que se esperaría que la función de Azure para procesar.

Además, se puede validar el escenario, si sabe cuántas instancias de Azure Function App tiene ejecutando. Si inspeccionar el contenedor de las concesiones y contar el número de elementos del arrendamiento dentro de los distintos valores de la `Owner` propiedad en ellos debe ser igual al número de instancias de la aplicación de función. Si hay más propietarios de las instancias de Azure Function App conocidos, significa que estos propietarios adicionales son el uno "adquiriendo" los cambios.

Una manera fácil para solucionar esta situación, es aplicar un `LeaseCollectionPrefix/leaseCollectionPrefix` a la función con un valor nuevo o diferente o, como alternativa, pruebe con un nuevo contenedor de las concesiones.

## <a name="next-steps"></a>Pasos siguientes

* [Habilitar la supervisión de las funciones de Azure](../azure-functions/functions-monitoring.md)
* [Azure Cosmos DB .NET SDK solución de problemas](./troubleshoot-dot-net-sdk.md)