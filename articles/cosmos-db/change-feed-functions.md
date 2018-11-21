---
title: Uso de la fuente de cambios de Azure Cosmos DB con Azure Functions
description: Utilice la fuente de cambios de Azure Cosmos DB con Azure Functions
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/06/2018
ms.author: rimman
ms.openlocfilehash: 943cd79237290cd91d56cc1c51f5d773cdb16634
ms.sourcegitcommit: 542964c196a08b83dd18efe2e0cbfb21a34558aa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2018
ms.locfileid: "51636397"
---
# <a name="how-to-use-azure-cosmos-db-change-feed-with-azure-functions"></a>Uso de la fuente de cambios de Azure Cosmos DB con Azure Functions

Si usa Azure Functions, la manera más sencilla de conectarse a una fuente de cambios es agregar un [desencadenador de Azure Cosmos DB](../azure-functions/functions-bindings-cosmosdb-v2.md#trigger) a la aplicación Azure Functions. Cuando cree un desencadenador de Cosmos DB en una aplicación de Azure Functions, seleccione el contenedor de Cosmos al que conectarse, y la función se desencadena siempre que se cambie algo en el contenedor.

Los desencadenadores pueden crearse en el portal de Azure Functions, en el portal de Azure Cosmos DB o mediante programación. Para más información, consulte [Azure Cosmos DB: informática de base de datos sin servidor con Azure Functions](serverless-computing-database.md).

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

### <a name="how-can-i-configure-azure-functions-to-read-from-a-particular-region"></a>¿Cómo se puede configurar Azure Functions para que lea desde una región determinada?

Es posible definir la propiedad [PreferredLocations](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.connectionpolicy.preferredlocations?view=azure-dotnet#Microsoft_Azure_Documents_Client_ConnectionPolicy_PreferredLocations) cuando se utiliza el desencadenador de Azure Cosmos DB para especificar una lista de regiones. Es lo mismo que personalizar la propiedad ConnectionPolicy, para hacer que el desencadenador lea desde las regiones preferidas. Lo ideal es que se quiera leer desde la región más cercana donde se implementan las instancias de Azure Functions.

### <a name="what-is-the-default-size-of-batches-in-azure-functions"></a>¿Cuál es el tamaño predeterminado de lotes en Azure Functions?

El tamaño predeterminado es de 100 elementos por cada invocación de Azure Functions. Sin embargo, este número se puede configurar dentro del archivo function.json. Esta es una [lista completa de las opciones de configuración](../azure-functions/functions-bindings-cosmosdb-v2.md#trigger---configuration). Si está desarrollando de manera local, actualice la configuración de la aplicación dentro del archivo local.settings.json.

### <a name="i-am-monitoring-a-container-and-reading-its-change-feed-however-i-dont-get-all-the-inserted-documents-some-items-are-missing"></a>Estoy supervisando un contenedor y leyendo su fuente de cambios; sin embargo, veo que no recibo todos los documentos insertados y que faltan algunos documentos.

Asegúrese de que ninguna otra función de Azure lee el mismo contenedor con el mismo contenedor de concesión. Los documentos que faltan los procesan las otras instancias de Azure Functions que también usan el mismo contrato de arrendamiento.

Por lo tanto, si crea varias instancias de Azure Functions para leer la misma fuente de cambios, deben usar distintos contenedores de concesión o usar la configuración "leasePrefix" para compartir el mismo contenedor. Sin embargo, cuando usa la biblioteca de procesadores de fuente de cambios, puede iniciar varias instancias de la función de Azure y el SDK dividirá los documentos en distintas instancias de manera automática.

### <a name="azure-cosmos-item-is-updated-every-second-and-i-dont-get-all-the-changes-in-azure-functions-listening-to-change-feed"></a>El elemento de Azure Cosmos se actualiza a cada segundo y no recibo todos los cambios en Azure Functions donde se escucha la fuente de cambios.

Azure Functions sondea la fuente de cambios continuamente, con un retraso máximo de 5 segundos de manera predeterminada. Si no hay cambios pendientes de leer, o si hay cambios pendientes después de aplicar el desencadenador, la función los leerá de inmediato. Sin embargo, si no hay cambios pendientes, la función esperará cinco segundos y buscará más cambios.

Si el documento recibe varios cambios en el mismo intervalo que llevó al desencadenador a buscar nuevos cambios, es posible que reciba la última versión del documento y no la versión intermedia.

Si desea sondear la fuente de cambios durante menos de cinco segundos, por ejemplo, por cada segundo, puede configurar el tiempo de sondeo "feedPollDelay"; consulte [la configuración completa](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.connectionpolicy.preferredlocations?view=azure-dotnet#Microsoft_Azure_Documents_Client_ConnectionPolicy_PreferredLocations). Se define en milisegundos con un valor predeterminado de 5000. Es posible realizar sondeos durante menos de un segundo, pero no se recomienda, ya que comenzará a utilizar más memoria de CPU.

### <a name="can-multiple-azure-functions-read-one-containers-change-feed"></a>¿Varias instancias de Azure Functions pueden leer la fuente de cambios de contenedor?

Sí. Varias instancias de Azure Functions pueden leer la fuente de cambios del mismo contenedor. Sin embargo, las instancias de Azure Functions deben tener definido un "leaseCollectionPrefix" independiente.

### <a name="if-i-am-processing-change-feed-by-using-azure-functions-in-a-batch-of-10-documents-and-i-get-an-error-at-seventh-document-in-that-case-the-last-three-documents-are-not-processed-how-can-i-start-processing-from-the-failed-document-ie-seventh-document-in-my-next-feed"></a>Si estoy procesando una fuente de cambios con Azure Functions, como un lote de 10 documentos, y recibo un error en el séptimo documento. En ese caso, no se procesan los últimos tres documentos. ¿Cómo puedo empezar el procesamiento desde el documento con errores (es decir, el documento séptimo) en mi siguiente fuente?

Para tratar el error, el patrón recomendado es envolver el código con el bloque try-catch y, si está iterando sobre la lista de documentos, envuelva cada iteración en su propio bloque try-catch. Detecte el error y coloque ese documento en una cola (de mensajes fallidos) y defina la lógica para manejar los documentos que generaron el error. Con este método, si tiene un lote de 200 documentos y solo un documento con errores, no tiene que deshacerse de todo el lote.

Si se produce un error, no debe rebobinar al punto de control para comenzar otra acción. Puede seguir obteniendo esos documentos de la fuente de cambios. Recuerde que la fuente de cambios conserva la última instantánea final de los documentos, por lo que puede perder la instantánea anterior en el documento. La fuente de cambios solo conserva una última versión del documento y, entremedio, pueden venir otros procesos y cambiar el documento.

Mientras sigue corrigiendo el código, pronto no encontrará documentos en la cola de mensajes fallidos. La fuente de cambios llama automáticamente a Azure Functions y Azure Functions mantiene de manera interna el punto de control y el sistema. Si quiere revertir el punto de control y controlar cada uno de sus aspectos, debe considerar la posibilidad de usar el SDK del procesador de la fuente de cambios.

### <a name="are-there-any-extra-costs-for-using-the-azure-cosmos-db-trigger"></a>¿Hay algún costo adicional por usar el desencadenador de Azure Cosmos DB?

El desencadenador de Azure Cosmos DB aprovecha internamente la biblioteca de procesadores de fuente de cambios. Como tal, requiere una colección adicional, denominada colección de concesión, para mantener los puntos de control de estado y parciales. Esta administración de estados es necesaria para poder escalar dinámicamente y continuar en caso de que desee detener las instancias de Azure Functions y continuar el procesamiento en un momento posterior. Para más información, consulte el artículo sobre el [trabajo con la biblioteca de procesadores de fuente de cambios](change-feed-processor.md).

## <a name="next-steps"></a>Pasos siguientes

Ahora, puede obtener más información acerca de las fuentes de cambios en los siguientes artículos:

* [Introducción a la fuente de cambios](change-feed.md)
* [Maneras de leer la fuente de cambios](read-change-feed.md)
* [Uso de la biblioteca de procesadores de fuente de cambios](change-feed-processor.md)
* [Trabajo con la biblioteca de procesadores de fuente de cambios](change-feed-processor.md)
* [Informática de base de datos sin servidor con Azure Cosmos DB y Azure Functions](serverless-computing-database.md)
