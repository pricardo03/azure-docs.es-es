---
title: Diagnóstico y solución de problemas de consulta al usar Azure Cosmos DB
description: Aprenda a identificar, diagnosticar y solucionar problemas de consultas SQL de Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: troubleshooting
ms.date: 07/10/2019
ms.author: girobins
ms.subservice: cosmosdb-sql
ms.reviewer: sngun
ms.openlocfilehash: 079e8677febfe6683d4f0e60a0e7ba6b06ea549d
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2019
ms.locfileid: "67835833"
---
# <a name="troubleshoot-query-performance-for-azure-cosmos-db"></a>Solucionar problemas con el rendimiento de las consultas de Azure Cosmos DB
En este artículo se explica cómo identificar, diagnosticar y solucionar problemas de consultas SQL de Azure Cosmos DB. Para lograr un rendimiento óptimo para las consultas de Azure Cosmos DB, siga los pasos para la solución de problemas que tiene a continuación. 

## <a name="collocate-clients-in-same-azure-region"></a>Colocar clientes en la misma región de Azure 
Para conseguir la menor latencia posible, asegúrese de que la aplicación que llama se encuentra en la misma región de Azure que el punto de conexión de Azure Cosmos DB aprovisionado. Para obtener una lista de regiones disponibles, consulte el artículo referente a las [Regiones de Azure](https://azure.microsoft.com/global-infrastructure/regions/#services).

## <a name="check-consistency-level"></a>Comprobar el nivel de coherencia
El [nivel de coherencia](consistency-levels.md) puede afectar al rendimiento y a los cargos. Asegúrese de que su nivel de coherencia sea apropiado para el escenario proporcionado. Para obtener más detalles, consulte [Selección del nivel de coherencia adecuado](consistency-levels-choosing.md).

## <a name="log-query-metrics"></a>Registrar las métricas de consulta
Use `QueryMetrics` para solucionar problemas con consultas lentas o costosas. 

  * Establezca `FeedOptions.PopulateQueryMetrics = true` para obtener `QueryMetrics` en la respuesta.
  * La clase `QueryMetrics` tiene una función `.ToString()` sobrecargada que se puede invocar para obtener la representación de cadena de `QueryMetrics`. 
  * Las métricas se pueden usar para derivar los siguientes detalles, entre otros: 
  
      * Si algún componente específico de la canalización de consultas tardó demasiado en completarse (por ejemplo, cientos de milisegundos o más). 

          * Consulte `TotalExecutionTime`.
          * Si el elemento `TotalExecutionTime` de la consulta es menor que el tiempo de ejecución de un extremo a otro, el tiempo se emplea en el lado cliente o en la red. Compruebe que el cliente y la región de Azure estén colocados.
      
      * Si hay falsos positivos en los documentos analizados (si el recuento de documentos de salida es mucho menor que el recuento de documentos recuperados).  

          * Consulte `Index Utilization`.
          * `Index Utilization` = (número de documentos devueltos / Número de documentos cargados)
          * Si el número de documentos devueltos es mucho menor que el número de documentos cargados, es que se están analizando los falsos positivos.
          * Limite el número de documentos que se recuperan con filtros más específicos.  

      * Cómo se estableció el precio de los recorridos de ida y vuelta individuales (consulte el elemento `Partition Execution Timeline` de la representación de cadena de `QueryMetrics`). 
      * Si la consulta produjo un gasto elevado por solicitud. 

Para obtener más detalles, consulte el artículo [Obtención de métricas de ejecución de consultas SQL](profile-sql-api-query.md).
      
## <a name="tune-query-feed-options-parameters"></a>Ajuste de parámetros de opciones de fuentes de consulta 
El rendimiento de las consultas se puede optimizar a través de los parámetros de [opciones de fuentes](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.feedoptions?view=azure-dotnet) de la solicitud. Intente establecer las siguientes opciones:

  * Establezca `MaxDegreeOfParallelism` en-1 primero y, a continuación, compare el rendimiento entre distintos valores. 
  * Establezca `MaxBufferedItemCount` en-1 primero y, a continuación, compare el rendimiento entre distintos valores. 
  * Establezca `MaxItemCount` en -1.

Al comparar el rendimiento de valores diferentes, pruebe con valores como 2, 4, 8, 16 y otros.
 
## <a name="read-all-results-from-continuations"></a>Lea todos los resultados de las continuaciones.
Si cree que no obtiene todos los resultados, asegúrese de purgar la continuación por completo. En otras palabras, siga leyendo los resultados mientras el token de continuación tenga más documentos que generar.

La purga completa se puede lograr con cualquiera de los siguientes patrones:

  * Continúe procesando los resultados mientras la continuación no esté vacía.
  * Continúe con el procesamiento mientras la consulta tenga más resultados. 

    ```csharp
    // using AsDocumentQuery you get access to whether or not the query HasMoreResults
    // If it does, just call ExecuteNextAsync until there are no more results
    // No need to supply a continuation token here as the server keeps track of progress
    var query = client.CreateDocumentQuery<Family>(collectionLink, options).AsDocumentQuery();
    while (query.HasMoreResults)
    {
        foreach (Family family in await query.ExecuteNextAsync())
        {
            families.Add(family);
        }
    }
    ```

## <a name="choose-system-functions-that-utilize-index"></a>Elija las funciones del sistema que usan el índice.
Si la expresión se puede traducir en un intervalo de valores de cadena, puede usar el índice; de lo contrario, no puede. 

Esta es la lista de funciones de cadena que pueden utilizar el índice: 
    
  * STARTSWITH(str_expr, str_expr) 
  * LEFT(str_expr, num_expr) = str_expr 
  * SUBSTRING(str_expr, num_expr, num_expr) = str_expr, pero solo si el primer valor de num_expr es 0 
    
    Estos son algunos ejemplos de consulta: 
    
    ```sql

    -- If there is a range index on r.name, STARTSWITH will utilize the index while ENDSWITH won't 
    SELECT * 
    FROM c 
    WHERE STARTSWITH(c.name, 'J') AND ENDSWITH(c.name, 'n')

    ```
    
    ```sql

    -- LEFT will utilize the index while RIGHT won't 
    SELECT * 
    FROM c 
    WHERE LEFT(c.name, 2) = 'Jo' AND RIGHT(c.name, 2) = 'hn'

    ```

  * Evite las funciones del sistema en el filtro (o la cláusula WHERE) que no atienda el índice. Algunos ejemplos de tales funciones del sistema incluyen los elementos Contains, Upper, Lower.
  * Cuando sea posible, escriba consultas que usen un filtro por la clave de partición.
  * Para lograr consultas efectivas, evite llamar a los elementos UPPER/LOWER en el filtro. En cambio, normalice el uso de mayúsculas y minúsculas en los valores al realizar la inserción. Para cada uno de los valores, escriba el valor y use las mayúsculas y minúsculas que precise, o escriba tanto el valor original como el valor con las mayúsculas y minúsculas que quiera. 

    Por ejemplo:
    
    ```sql

    SELECT * FROM c WHERE UPPER(c.name) = "JOE"

    ```
    
    En este caso, almacene "JOE" en mayúsculas o almacene el valor original "Joe" y el valor "JOE". 
    
    Si se normaliza el uso de mayúsculas y minúsculas en los datos JSON, la consulta se convierte en lo siguiente:
    
    ```sql

    SELECT * FROM c WHERE c.name = "JOE"

    ```

    La segunda consulta será más eficaz, ya que no requiere transformaciones en cada uno de los valores para poder compararlos con "JOE".

Para obtener más detalles sobre la función del sistema, consulte el artículo sobre las [funciones del sistema](sql-query-system-functions.md).

## <a name="check-indexing-policy"></a>Comprobar la directiva de indexación
Para comprobar que la [directiva de indexación](index-policy.md) actual es la óptima:

  * Asegúrese de que todas las rutas de acceso JSON usadas en las consultas se incluyen en la directiva de indexación para lecturas más rápidas.
  * Excluya las rutas de acceso que no se usan en consultas para escrituras de mayor rendimiento.

Para obtener más detalles, consulte el artículo [Administración de directivas de indexación](how-to-manage-indexing-policy.md).

## <a name="spatial-data-check-ordering-of-points"></a>Datos espaciales: Comprobar el orden de los puntos
El orden de los puntos dentro de un elemento Polygon debe especificarse siguiendo el sentido contrario al de las agujas del reloj. Un elemento Polygon cuyos puntos se hayan especificado en el sentido de las agujas del reloj representa el inverso de la región dentro de él.

## <a name="optimize-join-expressions"></a>Optimizar las expresiones JOIN
Las expresiones `JOIN` pueden expandirse en grandes productos cruzados. Cuando sea posible, realice la consulta en un espacio de búsqueda más pequeño a través de un filtro más estrecho.

Las subconsultas multivalor pueden optimizar las expresiones `JOIN` mediante la inserción de predicados después de cada expresión select-many, en lugar de hacerlo después de todas las combinaciones cruzadas en la cláusula `WHERE`. Para obtener un ejemplo detallado, consulte el artículo [Optimizar las expresiones JOIN](https://docs.microsoft.com/azure/cosmos-db/sql-query-subquery#optimize-join-expressions).

## <a name="optimize-order-by-expressions"></a>Optimizar las expresiones ORDER BY 
El rendimiento de la consulta `ORDER BY` puede verse afectado si los campos son escasos o si no están incluidos en la directiva de indexación.

  * Para campos dispersos como el tiempo, disminuya el espacio de búsqueda tanto como sea posible con filtros. 
  * Para una propiedad única `ORDER BY`, incluya la propiedad en la directiva de indexación. 
  * Para múltiples expresiones de propiedad `ORDER BY`, defina un [índice compuesto](https://docs.microsoft.com/azure/cosmos-db/index-policy#composite-indexes) en los campos que se ordenan.  

## <a name="many-large-documents-being-loaded-and-processed"></a>Se cargan y procesan muchos documentos grandes
El tiempo y las RU que requiere una consulta no solo dependen del tamaño de la respuesta, sino que también dependen del trabajo que realiza la canalización de procesamiento de consultas. El tiempo y las RU aumentan proporcionalmente con la cantidad de trabajo que realiza toda la canalización de procesamiento de consultas. Se realiza más trabajo para documentos grandes, por lo tanto, se requiere más tiempo y RU para cargar y procesar esos documentos grandes.

## <a name="low-provisioned-throughput"></a>Poco rendimiento aprovisionado
Asegúrese de que el rendimiento aprovisionado pueda administrar la carga de trabajo. Aumente el presupuesto de RU en las colecciones que se vean afectadas.

## <a name="try-upgrading-to-the-latest-sdk-version"></a>Intentar actualizar a la última versión del SDK
Para determinar la versión más reciente del SDK, consulte el artículo [SDK Download and release notes](sql-api-sdk-dotnet.md) (Descarga y notas de la versión del SDK).

## <a name="next-steps"></a>Pasos siguientes
Consulte los siguientes documentos sobre cómo medir las RU por consulta, cómo obtener estadísticas de ejecución para optimizar las consultas y mucho más:

* [Obtención de métricas de ejecución de consultas SQL mediante el SDK de .NET](profile-sql-api-query.md)
* [Optimización del rendimiento de consultas con Azure Cosmos DB](sql-api-sql-query-metrics.md)
* [Sugerencias de rendimiento para el SDK de .NET](performance-tips.md)