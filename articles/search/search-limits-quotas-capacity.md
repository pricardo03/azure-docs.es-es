---
title: Límites de servicio para los niveles y SKU en Azure Search
description: Límites de servicio usados en la planeación de la capacidad y los límites máximos de solicitudes y respuestas de Azure Search.
author: HeidiSteen
manager: nitinme
services: search
ms.service: search
ms.topic: conceptual
ms.date: 07/01/2019
ms.author: heidist
ms.openlocfilehash: fd65bb134d9057246a1b8c5cc2986e979713d20b
ms.sourcegitcommit: e9936171586b8d04b67457789ae7d530ec8deebe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71327161"
---
# <a name="service-limits-in-azure-search"></a>Límites de servicio en Azure Search
Los límites máximos del almacenamiento, las cargas de trabajo y las cantidades de índices, documentos y otros objetos dependen de si [aprovisiona Azure Search](search-create-service-portal.md) conforme a un plan de tarifa **Gratis**, **Básico**, **Estándar** o **Almacenamiento optimizado**.

+ **Gratis** es un servicio multiinquilino compartido incluido en su suscripción de Azure. Las solicitudes de indexación y consulta se ejecutan en las réplicas y las particiones que usan otros inquilinos.

+ **Básico** proporciona recursos informáticos dedicados para cargas de trabajo de producción en una escala menor, pero comparte algunas infraestructuras de red con otros inquilinos.

+ **Estándar** se ejecuta en máquinas dedicadas, con más almacenamiento y capacidad de procesamiento en cada nivel. Estándar incluye cuatro niveles: S1, S2, S3 y S3 HD.

+ El plan de tarifa **Almacenamiento optimizado** se ejecuta en máquinas dedicadas con más almacenamiento total, ancho de banda de almacenamiento y memoria que el plan **Estándar**. Dicho plan se ofrece en dos niveles: L1 y L2.

> [!NOTE]
> A partir del 1 de julio, todos los niveles tendrán disponibilidad general, incluido el nivel optimizado para almacenamiento. Puede encontrar más información en la [página de precios](https://azure.microsoft.com/pricing/details/search/).

  S3 High Density (S3 HD) está diseñado para cargas de trabajo específicas: [multiinquilino](search-modeling-multitenant-saas-applications.md) y grandes cantidades de índices pequeños (un millón de documentos por índice, tres mil índices por servicio). Este nivel no proporciona la [característica de indexador](search-indexer-overview.md). En S3 HD, la ingesta de datos debe aprovechar el enfoque de inserción, mediante llamadas API para insertar los datos del origen en el índice. 

> [!NOTE]
> Un servicio se aprovisiona en un nivel específico. Saltar niveles para obtener capacidad implica el aprovisionamiento de un nuevo servicio (no hay ninguna actualización local). Para más información, vea [Selección de SKU o plan de tarifa](search-sku-tier.md). Para información sobre cómo ajustar la capacidad dentro de un servicio que ya ha aprovisionado, vea [Escalado de niveles de recursos para cargas de trabajo de indexación y consulta en Azure Search](search-capacity-planning.md).
>

## <a name="subscription-limits"></a>Límites de suscripción
[!INCLUDE [azure-search-limits-per-subscription](../../includes/azure-search-limits-per-subscription.md)]

## <a name="storage-limits"></a>Límites de Storage
[!INCLUDE [azure-search-limits-per-service](../../includes/azure-search-limits-per-service.md)]

<a name="index-limits"></a>

## <a name="index-limits"></a>Límites de índice

| Resource | Gratuito | Básico&nbsp;<sup>1</sup>  | S1 | S2 | S3 | S3&nbsp;HD | L1 | L2 |
| -------- | ---- | ------------------- | --- | --- | --- | --- | --- | --- |
| Índices máximos |3 |5 o 15 |50 |200 |200 |1000 GB por partición o 3000 por servicio |10 |10 |
| Campos simples máximos por índice |1000 |100 |1000 |1000 |1000 |1000 |1000 |1000 |
| Campos de colección complejos máximos por índice |40 |40 |40 |40 |40 |40 |40 |40 |
| Elementos máximos en todas las colecciones complejas por documento |3000 |3000 |3000 |3000 |3000 |3000 |3000 |3000 |
| Profundidad máxima de campos complejos |10 |10 |10 |10 |10 |10 |10 |10 |
| [Proveedores de sugerencias](https://docs.microsoft.com/rest/api/searchservice/suggesters) máximos por índice |1 |1 |1 |1 |1 |1 |1 |1 |
| [Perfiles de puntuación](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index) máximos por índice |100 |100 |100 |100 |100 |100 |100 |100 |
| Funciones máximas por perfil |8 |8 |8 |8 |8 |8 |8 |8 |

<sup>1</sup> Los servicios básicos creados antes de diciembre de 2017 tienen límites inferiores (5 en lugar de 15) en los índices. El nivel básico es la única SKU con un límite inferior de 100 campos por índice.

<a name="document-limits"></a>

## <a name="document-limits"></a>Límites de documento 

A partir de octubre de 2018, ya no existen límites de documentos<sup>1</sup> para ningún nuevo servicio creado en ningún nivel facturable (Básico, S1, S2, S3, S3 HD) en ninguna región. Aunque la mayoría de las regiones han tenido un número ilimitado de documentos desde noviembre o diciembre de 2017, hubo cinco regiones que continuaron imponiendo límites a los documentos. Dependiendo de cuándo y dónde haya creado un servicio de búsqueda, es posible que esté ejecutando un servicio que todavía esté sujeto a límites de documentos.

Para determinar si su servicio tiene límites de documentos, compruebe el icono de uso de la página de información general de su servicio. Los recuentos de documentos son ilimitados o están sujetos a un límite basado en el nivel.

  ![Icono de Uso](media/search-limits-quotas-capacity/portal-usage-tile.png)

<sup>1</sup> Aunque no hay ningún límite de documentos específico de SKU, cada índice sigue sujeto a un límite máximo de seguridad para garantizar la estabilidad del servicio. Este límite proviene de Lucene. Cada documento de Azure Search se indexa internamente como uno o más documentos Lucene. El número de documentos Lucene por documento de Azure Search depende del número total de elementos en campos de colección complejos. Cada elemento se indexa como un documento Lucene independiente. Por ejemplo, un documento con tres elementos en un campo de colección complejo se indexará como cuatro documentos Lucene: uno para el documento en sí y tres para los elementos. El número máximo de documentos Lucene es aproximadamente 25 mil millones por índice.

### <a name="regions-previously-having-document-limits"></a>Regiones que anteriormente tenían límites de documentos

Si el portal indica un límite de documentos, su servicio se creó antes de finales de 2017, o se creó en un centro de datos mediante clústeres de menor capacidad para hospedar servicios de Azure Search:

+ Este de Australia
+ Asia oriental
+ India Central
+ Oeste de Japón
+ Centro occidental de EE.UU.

A los servicios sujetos a límites de documento se les aplican los límites máximos siguientes:

|  Gratuito | Básica | S1 | S2 | S3 | S3&nbsp;HD |
|-------|-------|----|----|----|-------|
|  10 000 |1&nbsp;millón |15 millones por partición, 180 millones por servicio |60 millones por partición, 720 millones por servicio |120 millones por partición, 1400 millones por servicio |1 millón por índice, 200 millones por partición |

Si el servicio tiene límites que lo bloquean, cree un nuevo servicio y vuelva a publicar todo el contenido de ese servicio. No existe ningún mecanismo para reaprovisionar el servicio en un nuevo hardware en segundo plano.

> [!Note] 
> Para los servicios de alta densidad S3 creados después de 2017, se ha quitado el límite de 200 millones de documentos por partición, pero el de 1 millón de documentos por índice se conserva.


### <a name="document-size-limits-per-api-call"></a>Límites de tamaño de documento por llamada API

El tamaño máximo del documento cuando se llama a una API de índice es de aproximadamente 16 megabytes.

El tamaño del documento es de hecho un límite en el tamaño del cuerpo de la solicitud de la API de índice. Dado que se puede pasar un lote de varios documentos a la API de índice de una vez, el límite de tamaño depende en realidad de cuántos documentos estén en el lote. Para un lote con un único documento, el tamaño máximo del documento es de 16 MB de JSON.

Para mantener el tamaño del documento reducido, no olvide excluir los datos no consultables de la solicitud. Las imágenes y otros datos binarios no son consultables directamente y no se deben almacenar en el índice. Para integrar los datos no consultables en los resultados de búsqueda, defina un campo que admita búsqueda que almacene una referencia de dirección URL al recurso.

## <a name="indexer-limits"></a>Límites de indexador

Los tiempos de ejecución máximos existen para proporcionar equilibrio y estabilidad al servicio como un todo, pero es posible que los conjuntos de datos más grandes necesiten más tiempo de indexación que el que permite el máximo. Si un trabajo de indexación no se puede completar en el tiempo máximo permitido, intente ejecutarlo en una programación. El programador realiza un seguimiento del estado de la indexación. Si un trabajo programado de indexación se interrumpe por cualquier motivo, el indizador puede reanudarlo donde se quedó en la siguiente ejecución programada.


| Resource | Gratis&nbsp;<sup>1</sup> | Básico&nbsp;<sup>2</sup>| S1 | S2 | S3 | S3&nbsp;HD&nbsp;<sup>3</sup>|L1 |L2 |
| -------- | ----------------- | ----------------- | --- | --- | --- | --- | --- | --- |
| Indexadores máximos |3 |5 o 15|50 |200 |200 |N/D |10 |10 |
| Orígenes de datos máximos |3 |5 o 15 |50 |200 |200 |N/D |10 |10 |
| Conjuntos de habilidades máximos <sup>4</sup> |3 |5 o 15 |50 |200 |200 |N/D |10 |10 |
| Carga máxima de indexación por invocación |10 000 documentos |Limitado solamente por el número máximo de documentos |Limitado solamente por el número máximo de documentos |Limitado solamente por el número máximo de documentos |Limitado solamente por el número máximo de documentos |N/D |Sin límite |Sin límite |
| Programación mínima | 5 minutos |5 minutos |5 minutos |5 minutos |5 minutos |5 minutos |5 minutos | 5 minutos |
| Tiempo de ejecución máximo <sup>5</sup> | 1-3 minutos |24 horas |24 horas |24 horas |24 horas |N/D  |24 horas |24 horas |
| Tiempo de ejecución máximo para conocimientos adicionales de búsqueda cognitiva o indización de blobs con análisis de imágenes <sup>5</sup> | 3-10 minutos |2 horas |2 horas |2 horas |2 horas |N/D  |2 horas |2 horas |
| Indexador de blobs: tamaño máximo de blob, MB |16 |16 |128 |256 |256 |N/D  |256 |256 |
| Indexador de blobs: número máximo de caracteres del contenido extraído de un blob |32 000 |64 000 |4&nbsp;millones |4&nbsp;millones |4&nbsp;millones |N/D |4&nbsp;millones |4&nbsp;millones |

<sup>1</sup> Los servicios gratuitos tienen un tiempo de ejecución máximo del indexador de 3 minutos para servicios de blob y de 1 minuto para todos los demás orígenes de datos. Para la indexación de IA que llama a Cognitive Services, los servicios gratuitos se limitan a 20 transacciones gratuitas por día, donde una transacción se define como un documento que pasa correctamente a través de la canalización enriquecimiento.

<sup>2</sup> Los servicios básicos creados antes de diciembre de 2017 tienen límites inferiores (5 en lugar de 15) en los indexadores, los orígenes de datos y los conjuntos de aptitudes.

<sup>3</sup> Los servicios S3 HD no incluyen compatibilidad con indexador.

<sup>4</sup> Máximo de 30 habilidades por conjunto de habilidades.

<sup>5</sup> Las cargas de trabajo de búsqueda cognitiva y análisis de imágenes en la indexación de blobs de Azure tienen tiempos de ejecución más cortos que la indexación de texto normal. El análisis de imágenes y el procesamiento de lenguaje natural consumen muchos recursos informáticos y una cantidad desproporcionada de la potencia de procesamiento disponible. Se ha reducido el tiempo de ejecución para que otros trabajos de la cola tengan la oportunidad de ejecutarse.  

## <a name="synonym-limits"></a>Límites de sinónimos

El número máximo de asignaciones de sinónimos permitidas varía según el nivel de precios. Cada regla puede tener hasta 20 expansiones, donde una expansión es un término equivalente. Por ejemplo, dado el término "gato", la asociación con "gatito", "felino" y "felis" (el género de gatos) contaría como 3 expansiones.

| Resource | Gratuito | Básica | S1 | S2 | S3 | S3-HD |L1 | L2 |
| -------- | -----|------ |----|----|----|-------|---|----|
| Asignaciones máximas de sinónimos |3 |3|5 |10 |20 |20 | 10 | 10 |
| Número máximo de reglas por asignación |5000 |20000|20000 |20000 |20000 |20000 | 20000 | 20000  |

## <a name="queries-per-second-qps"></a>Consultas por segundo (QPS)

Cada cliente debe desarrollar las estimaciones de QPS de manera independiente. El tamaño y la complejidad del índice, el tamaño y la complejidad de la consulta, y la cantidad de tráfico son los determinantes principales de las consultas por segundo. No hay manera de ofrecer estimaciones significativas cuando estos factores son desconocidos.

Las estimaciones son más predecibles cuando se calculan en los servicios que se ejecutan en recursos dedicados (niveles Básico y Estándar). Puede calcular el número de QPS con más precisión porque tiene control sobre más parámetros. Para obtener instrucciones sobre cómo encargarse del cálculo, consulte [Consideraciones sobre el rendimiento y la optimización de Azure Search](search-performance-optimization.md).

En el caso de los niveles de Almacenamiento optimizado, debe esperar un rendimiento más bajo de las consultas y una latencia superior que en los niveles Estándar.  La metodología para calcular el rendimiento de las consultas que experimentará es igual que en los niveles Estándar.

## <a name="data-limits-cognitive-search"></a>Límites de datos (búsqueda cognitiva)

Una [canalización de búsqueda cognitiva](cognitive-search-concept-intro.md) que realiza llamadas a un recurso de Text Analytics para el [reconocimiento de entidades](cognitive-search-skill-entity-recognition.md), la [extracción de frases clave](cognitive-search-skill-keyphrases.md), el [análisis de sentimiento ](cognitive-search-skill-sentiment.md) y la [detección de idioma](cognitive-search-skill-language-detection.md) está sujeta a los límites de datos. El tamaño máximo de un registro debe ser de 50 000 caracteres, medidos por [`String.Length`](https://docs.microsoft.com/dotnet/api/system.string.length). Si tiene que dividir los datos antes de enviarlos al analizador de opiniones, use la [aptitud División de texto](cognitive-search-skill-textsplit.md).

## <a name="api-request-limits"></a>Límites de solicitud de API
* Máximo de 16 MB por solicitud <sup>1</sup>
* Longitud máxima de dirección URL de 8 KB
* Máximo de 1000 documentos por lote del índice de cargas de índices, combinaciones o eliminaciones
* Máximo de 32 campos en cláusula $orderby
* El tamaño máximo del término de búsqueda es de 32 766 bytes (32 KB menos 2 bytes) de texto con codificación UTF-8

<sup>1</sup> En Azure Search, el cuerpo de una solicitud está sujeto a un límite superior de 16 MB, que impone un límite práctico en el contenido de campos individuales o colecciones que no esté restringido de algún modo por límites teóricos (consulte [Supported data types](https://docs.microsoft.com/rest/api/searchservice/supported-data-types) (Tipos de datos admitidos) para más información sobre composición de campos y restricciones).

## <a name="api-response-limits"></a>Límites de respuesta de API
* Máximo de 1000 documentos devueltos por página de resultados de búsqueda
* Máximo de 100 sugerencias devueltas por solicitud de Sugerir API

## <a name="api-key-limits"></a>Límites de clave de API
Las claves de API se usan para la autenticación del servicio. Hay dos tipos. Las claves de administración se especifican en el encabezado de solicitud y conceden acceso completo de lectura y escritura al servicio. Las claves de consulta son de solo lectura, se especifican en la dirección URL y normalmente se distribuyen a las aplicaciones cliente.

* Máximo de 2 claves de administración por servicio
* Máximo de 50 claves de consultas por servicio
