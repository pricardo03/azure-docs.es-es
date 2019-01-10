---
title: Indexación de un conjunto de datos grande mediante indizadores integrados - Azure Search
description: Obtenga información sobre las estrategias de indexación de datos de gran tamaño o indexación de cálculo intensivo mediante modo por lotes, recursos y técnicas para indexación programada, paralela y distribuida.
services: search
author: HeidiSteen
manager: cgronlun
ms.service: search
ms.topic: conceptual
ms.date: 12/19/2018
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 2f3d08a32384cea815f096f51b24eea596d0d118
ms.sourcegitcommit: 21466e845ceab74aff3ebfd541e020e0313e43d9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/21/2018
ms.locfileid: "53742242"
---
# <a name="how-to-index-large-data-sets-in-azure-search"></a>Cómo indexar grandes conjuntos de datos en Azure Search

A medida que los volúmenes de datos aumentan o las necesidades de procesamiento cambian, es posible que las estrategias de indización predeterminadas ya no le parezcan prácticas. Para Azure Search, existen varios enfoques para adaptar los grandes conjuntos de datos, que van desde cómo estructurar una solicitud de carga de datos hasta usar un indizador específico del origen para las cargas de trabajo programadas y distribuidas.

Las mismas técnicas para datos de gran tamaño también se pueden aplicar a los procesos de larga ejecución. En concreto, los pasos que se describen en [indización paralela](#parallel-indexing) son útiles para la indización de cálculo intensivo, como el análisis de imágenes o el procesamiento de lenguaje natural en la [canalización de Cognitive Search](cognitive-search-concept-intro.md).

## <a name="batch-indexing"></a>Indización por lotes

Uno de los mecanismos más sencillos para la indización de un conjunto de datos más grande es enviar varios documentos o registros en una sola solicitud. Siempre y cuando la carga completa sea menor a 16 MB, una solicitud puede administrar hasta 1000 documentos en una operación de carga masiva. Suponiendo que se usa la [API REST para agregar o actualizar documentos](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents), podría empaquetar 1000 documentos en el cuerpo de la solicitud.

La indización por lotes se implementa para las solicitudes individuales con REST o. NET, o a través de indizadores. Algunos indizadores funcionan bajo distintos límites. En concreto, la indización de Azure Blob establece el tamaño de lote en 10 documentos en reconocimiento al tamaño máximo medio de los documentos. Para los indizadores basados en la [API REST para crear indizadores](https://docs.microsoft.com/rest/api/searchservice/Create-Indexer ), puede establecer el argumento `BatchSize` para personalizar esta configuración de modo que se adapte mejor a las características de los datos. 

> [!NOTE]
> Para mantener el tamaño del documento reducido, no olvide excluir los datos no consultables de la solicitud. Las imágenes y otros datos binarios no se pueden buscar directamente y no se deben almacenar en el índice. Para integrar los datos no consultables en los resultados de búsqueda, debe definir un campo que no admita búsquedas que almacene una referencia de dirección URL al recurso.

## <a name="add-resources"></a>Adición de recursos

Los servicios que se aprovisionan en uno de los [planes de tarifa Estándar](search-sku-tier.md) a menudo tienen una capacidad infrautilizada de almacenamiento y cargas de trabajo (consultas o indización), lo que hace que [aumentar los recuentos de particiones y réplicas ](search-capacity-planning.md) sea una solución obvia para adaptarse a los grandes conjuntos de datos. Para obtener mejores resultados, necesita dos recursos: las particiones para el almacenamiento y las réplicas para el trabajo de ingesta de datos.

Aumentar las réplicas y particiones son eventos facturables que aumentan el costo, pero a menos que indexe continuamente bajo la carga máxima, puede agregar escala para el tiempo que dure el proceso de indización y, a continuación, reducir los niveles de recursos cuando el proceso finalice.

## <a name="use-indexers"></a>Uso de indizadores

Los [indizadores](search-indexer-overview.md) sirven para rastrear los orígenes de datos externos en busca de contenido utilizable en búsquedas. Aunque no se diseñaron específicamente para el indexado a gran escala, varias funcionalidades de los indizadores son especialmente útiles para adaptar grandes conjuntos de datos:

+ Los programadores le permiten empaquetar la indexación en intervalos regulares para que se pueda distribuir con el tiempo.
+ Puede reanudar la indexación programada en el último punto de detención conocido. Si un origen de datos no se rastrea completamente en un plazo de 24 horas, el indizador reanudará la indexación en el segundo día donde se quedó.
+ Particionar los datos en orígenes de datos individuales más pequeños permite realizar procesamientos en paralelo. Puede dividir un conjunto de datos grande en conjuntos de datos más pequeños y, a continuación, crear varias definiciones de origen de datos que se pueden indizar en paralelo.

> [!NOTE]
> Los indizadores son específicos del origen de datos, por lo que un enfoque con indizadores solo es posible para algunos orígenes de datos en Azure: [SQL Database](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md), [Blob Storage](search-howto-indexing-azure-blob-storage.md), [Table Storage](search-howto-indexing-azure-tables.md) y [Cosmos DB](search-howto-index-cosmosdb.md).

## <a name="scheduled-indexing"></a>Indexación programada

La programación de indizadores es un mecanismo importante para procesar conjuntos de datos de gran tamaño y realizar procesos de ejecución lenta, como el análisis de imágenes en una canalización de Cognitive Search. El procesamiento de los indexadores funciona dentro de una ventana de 24 horas. Si el procesamiento no se completa en menos de 24 horas, los comportamientos de la programación de los indexadores pueden resultar beneficiosos. 

De manera predeterminada, la indexación programada se inicia en intervalos específicos, habitualmente con un trabajo que se completa antes de reanudar en el intervalo programado siguiente. Sin embargo, si el procesamiento no se completa dentro del intervalo, el indexador se detiene (porque se agotó el tiempo). En el intervalo siguiente, el procesamiento se reanuda donde quedó, porque el sistema lleva un registro de dónde sucedió eso. 

En la práctica, para cargas de índice que abarcan varios días, puede poner el indexador en una programación de 24 horas. Cuando la indexación se reanuda para el próximo ciclo de 24 horas, se reinicia en el último documento correcto conocido. De este modo, un indexador puede abrirse camino a través del trabajo pendiente de un documento en una serie de días hasta procesar todos los documentos no procesados. Para obtener más información sobre este enfoque, consulte la sección sobre la [indexación de conjuntos de datos de gran tamaño en Azure Blob Storage](search-howto-indexing-azure-blob-storage.md#indexing-large-datasets). Para obtener más información acerca de cómo configurar las programaciones en general, consulte la [API REST para crear indizadores](https://docs.microsoft.com/rest/api/searchservice/Create-Indexer#request-syntax).

<a name="parallel-indexing"></a>

## <a name="parallel-indexing"></a>Indexación en paralelo

La estrategia de indexación en paralelo se basa en la indexación de varios orígenes de datos al unísono, donde cada definición de origen de datos especifica un subconjunto de los datos. 

Para los requisitos de indexación que no son de rutina ni de cálculo intensivo (tales como el OCR en documentos digitalizados en una canalización de Cognitive Search, análisis de imagen o procesamiento de lenguaje natural), la estrategia de indexación en paralelo a menudo es el enfoque correcto para completar un proceso de larga ejecución en el tiempo más corto. Si puede eliminar o reducir las solicitudes de consulta, la indexación en paralelo en un servicio que no administra soluciones de consulta de manera simultánea es la mejor estrategia para trabajar en un gran volumen de contenido de procesamiento lento. 

El procesamiento paralelo tiene los siguientes elementos:

+ Subdivida los datos de origen entre varios contenedores o varias carpetas virtuales dentro del mismo contenedor. 
+ Asigne cada pequeño conjunto de datos a su propio [origen de datos](https://docs.microsoft.com/rest/api/searchservice/create-data-source), emparejado con su propio [indizador](https://docs.microsoft.com/rest/api/searchservice/create-indexer).
+ En el caso de Cognitive Search, haga referencia al mismo [conjunto de datos](https://docs.microsoft.com/rest/api/searchservice/create-skillset) en cada definición de indexador.
+ Escriba en el mismo índice de búsqueda de destino. 
+ Programe todos los indexadores para que se ejecuten al mismo tiempo.

> [!NOTE]
> Azure Search no permite dedicar réplicas o particiones a cargas de trabajo específicas. El riesgo de una indexación simultánea intensa sobrecarga el sistema y perjudica el rendimiento de las consultas. Si tiene un entorno de prueba, implemente la indexación en paralelo ahí primero para comprender las ventajas y los inconvenientes.

### <a name="how-to-configure-parallel-indexing"></a>Cómo configurar la indexación en paralelo

En el caso de los indexadores, la capacidad de procesamiento se basa parcialmente en un subsistema de indexador para cada unidad de servicio que el servicio de búsqueda usa. Es posible tener varios indexadores simultáneos en el servicio de Azure Search aprovisionado en los niveles Básico o Estándar con dos réplicas al menos. 

1. En [Azure Portal](https://portal.azure.com), en la página **Información general** del panel del servicio de búsqueda, compruebe el **plan de tarifa** para comprobar que puede permitir la indexación en paralelo. Los niveles Básico y Estándar ofrecen varias réplicas.

2. En **Configuración** > **Escala**, [aumente las réplicas](search-capacity-planning.md) para el procesamiento paralelo: una réplica adicional para cada carga de trabajo de indexador. Deje un número suficiente para el volumen de consultas existente. No se recomienda sacrificar las cargas de trabajo por la indexación.

3. Distribuya los datos en varios contenedores en un nivel que puedan alcanzar los indexadores de Azure Search. Esto podría tratarse de varias tablas en Azure SQL Database, varios contenedores en Azure Blob Storage o varias colecciones. Defina un objeto de origen de datos para cada tabla o contenedor.

4. Cree y programe varios indexadores para que se ejecuten en paralelo:

   + Considere un servicio con seis réplicas. Configure seis indexadores, cada uno asignado a un origen de datos con un sexto del conjunto de datos para dividir en seis todo el conjunto de datos. 

   + Dirija cada indexador al mismo índice. En el caso de las cargas de trabajo de Cognitive Search, dirija cada indexador al mismo conjunto de aptitudes.

   + Dentro de cada definición de indexador, programe el mismo patrón de ejecución del tiempo de ejecución. Por ejemplo, `"schedule" : { "interval" : "PT8H", "startTime" : "2018-05-15T00:00:00Z" }` crea una programación para el 15-05-2018 en todos los indexadores, que se ejecutará a intervalos de ocho horas.

A la hora programada, todos los indexadores empezarán a ejecutarse, cargarán los datos, aplicarán los enriquecimientos (si configuró una canalización de Cognitive Search) y escribirán en el índice. Azure Search no bloquea las actualizaciones del índice. Las escrituras simultáneas se administran y se realiza un reintento si una escritura determinada no se realizó correctamente la primera vez.

> [!Note]
> Cuando aumente las réplicas, considere aumentar el número de particiones si se espera que el tamaño del índice aumente de manera considerable. Las particiones almacenan segmentos del contenido indexado: cuantas más particiones tenga, más pequeño será el segmento que cada una debe almacenar.

## <a name="see-also"></a>Otras referencias

+ [Información general del indexador](search-indexer-overview.md)
+ [Indexing in the portal](search-import-data-portal.md) (Indexación en el portal)
+ [Azure SQL Database indexer](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md) (Indexador de Azure SQL Database)
+ [Indexador de Azure Cosmos DB](search-howto-index-cosmosdb.md)
+ [Indexador de Azure Blob Storage](search-howto-indexing-azure-blob-storage.md)
+ [Indexador de Azure Table Storage](search-howto-indexing-azure-tables.md)
+ [Security in Azure Search](search-security-overview.md) (Seguridad en Azure Search)