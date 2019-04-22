---
title: Límites de servicio para los niveles y SKU en Azure Search
description: Límites de servicio usados en la planeación de la capacidad y los límites máximos de solicitudes y respuestas de Azure Search.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 04/17/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: ff2b843e00ffdf005d952cf62eab6b93c9434913
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59699173"
---
# <a name="service-limits-in-azure-search"></a>Límites de servicio en Azure Search
Los límites máximos del almacenamiento, las cargas de trabajo y las cantidades de índices, documentos y otros objetos dependen de si se [aprovisiona Azure Search](search-create-service-portal.md) en **gratis**, **básica**,  **Estándar**, o **optimizadas para almacenamiento** planes de tarifa.

+ **Gratis** es un servicio multiinquilino compartido incluido en su suscripción de Azure.

+ El plan **Básico** proporciona recursos de proceso dedicados para cargas de trabajo de producción a escala más pequeña.

+ **Estándar** se ejecuta en máquinas dedicadas, con más almacenamiento y capacidad de procesamiento en cada nivel. Estándar incluye cuatro niveles: S1, S2, S3 y S3 HD.

+ **Almacenamiento optimizado** se ejecuta en máquinas dedicadas con más almacenamiento total, el ancho de banda de almacenamiento y memoria que **estándar**. Almacenamiento optimizado viene en dos niveles: L1 y L2

> [!NOTE]
> Los niveles de servicio de almacenamiento optimizado disponibles actualmente como versión preliminar a un precio con descuento para fines de pruebas y experimentación con el fin de recabar comentarios. El precio final se anunciará más adelante cuando estos niveles están disponibles con carácter general. Aconsejamos que no se usen estos niveles para las aplicaciones de producción.

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

| Recurso | Gratuito | Básico&nbsp;<sup>1</sup>  | S1 | S2 | S3 | S3&nbsp;HD | L1 | L2 |
| -------- | ---- | ------------------- | --- | --- | --- | --- | --- | --- |
| Índices máximos |3 |5 o 15 |50 |200 |200 |1000 GB por partición o 3000 por servicio |10 |10 |
| Campos máximos por índice |1000 |100 |1000 |1000 |1000 |1000 |1000 |1000 |
| [Proveedores de sugerencias](https://docs.microsoft.com/rest/api/searchservice/suggesters) máximos por índice |1 |1 |1 |1 |1 |1 |1 |1 |
| [Perfiles de puntuación](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index) máximos por índice |100 |100 |100 |100 |100 |100 |100 |100 |
| Funciones máximas por perfil |8 |8 |8 |8 |8 |8 |8 |8 |

<sup>1</sup> servicios básicos creados antes de diciembre de 2017 tienen límites inferiores (5 en lugar de 15) en los índices. El nivel básico es la única SKU con un límite inferior de 100 campos por índice.

<a name="document-limits"></a>

## <a name="document-limits"></a>Límites de documento 

A partir de octubre de 2018, ya no existen límites de documentos para ningún nuevo servicio creado en ningún nivel facturable (Básico, S1, S2, S3, S3 HD) en ninguna región. Aunque la mayoría de las regiones han tenido un número ilimitado de documentos desde noviembre o diciembre de 2017, hubo cinco regiones que continuaron imponiendo límites a los documentos. Dependiendo de cuándo y dónde haya creado un servicio de búsqueda, es posible que esté ejecutando un servicio que todavía esté sujeto a límites de documentos.

Para determinar si su servicio tiene límites de documentos, compruebe el icono de uso de la página de información general de su servicio. Los recuentos de documentos son ilimitados o están sujetos a un límite basado en el nivel.

  ![Icono de Uso](media/search-limits-quotas-capacity/portal-usage-tile.png)

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
|  10 000 |1&nbsp;millones |15 millones por partición, 180 millones por servicio |60 millones por partición, 720 millones por servicio |120 millones por partición, 1400 millones por servicio |1 millón por índice, 200 millones por partición |

Si el servicio tiene límites que lo bloquean, cree un nuevo servicio y vuelva a publicar todo el contenido de ese servicio. No existe ningún mecanismo para reaprovisionar el servicio en un nuevo hardware en segundo plano.

> [!Note] 
> Para los servicios de alta densidad S3 creados después de 2017, se ha quitado el límite de 200 millones de documentos por partición, pero el de 1 millón de documentos por índice se conserva.


### <a name="document-size-limits-per-api-call"></a>Límites de tamaño de documento por llamada API

El tamaño máximo del documento cuando se llama a una API de índice es de aproximadamente 16 megabytes.

El tamaño del documento es de hecho un límite en el tamaño del cuerpo de la solicitud de la API de índice. Dado que se puede pasar un lote de varios documentos a la API de índice de una vez, el límite de tamaño depende en realidad de cuántos documentos estén en el lote. Para un lote con un único documento, el tamaño máximo del documento es de 16 MB de JSON.

Para mantener el tamaño del documento reducido, no olvide excluir los datos no consultables de la solicitud. Las imágenes y otros datos binarios no son consultables directamente y no se deben almacenar en el índice. Para integrar los datos no consultables en los resultados de búsqueda, defina un campo que admita búsqueda que almacene una referencia de dirección URL al recurso.

## <a name="indexer-limits"></a>Límites de indexador

Tiempos de ejecución máximos existen para proporcionar equilibrio y la estabilidad al servicio como un todo, pero más grandes conjuntos de datos que tenga indización más tiempo que permite que el máximo. Si un trabajo de indexación no se puede completar en el tiempo máximo permitido, intente ejecutarlo en una programación. El programador realiza un seguimiento del estado de la indexación. Si un trabajo programado de indexación se interrumpe por cualquier motivo, el indizador puede reanudarlo donde se quedó en la siguiente ejecución programada.


| Recurso | Gratis&nbsp;<sup>1</sup> | Básico&nbsp;<sup>2</sup>| S1 | S2 | S3 | S3&nbsp;HD&nbsp;<sup>3</sup>|L1 |L2 |
| -------- | ----------------- | ----------------- | --- | --- | --- | --- | --- | --- |
| Indexadores máximos |3 |5 o 15|50 |200 |200 |N/D |10 |10 |
| Orígenes de datos máximos |3 |5 o 15 |50 |200 |200 |N/D |10 |10 |
| Conjuntos de habilidades máximos <sup>4</sup> |3 |5 o 15 |50 |200 |200 |N/D |10 |10 |
| Carga máxima de indexación por invocación |10 000 documentos |Limitado solamente por el número máximo de documentos |Limitado solamente por el número máximo de documentos |Limitado solamente por el número máximo de documentos |Limitado solamente por el número máximo de documentos |N/D |Sin límite |Sin límite |
| Programación mínimo | 5 minutos |5 minutos |5 minutos |5 minutos |5 minutos |5 minutos |5 minutos | 5 minutos |
| Tiempo de ejecución máximo <sup>5</sup> | 1-3 minutos |24 horas |24 horas |24 horas |24 horas |N/D  |24 horas |24 horas |
| Tiempo de ejecución máximo para conocimientos adicionales de búsqueda cognitiva o indización de blobs con análisis de imágenes <sup>5</sup> | 3-10 minutos |2 horas |2 horas |2 horas |2 horas |N/D  |2 horas |2 horas |
| Indexador de blobs: tamaño máximo de blob, MB |16 |16 |128 |256 |256 |N/D  |256 |256 |
| Indexador de blobs: número máximo de caracteres del contenido extraído de un blob |32 000 |64 000 |4&nbsp;millones |4&nbsp;millones |4&nbsp;millones |N/D |4&nbsp;millones |4&nbsp;millones |

<sup>1</sup> Los servicios gratuitos tienen un tiempo de ejecución máximo del indexador de 3 minutos para servicios de blob y de 1 minuto para todos los demás orígenes de datos. Para AI de indización que llama a Cognitive Services, servicios gratis se limitan a 20 transacciones gratuitas al día, donde una transacción se define como un documento que se pasa correctamente a través de la canalización de enriquecimiento.

<sup>2</sup> servicios básicos creados antes de diciembre de 2017 tienen límites inferiores (5 en lugar de 15) en los indexadores, orígenes de datos y conjuntos de habilidades.

<sup>3</sup> Los servicios S3 HD no incluyen compatibilidad con indexador.

<sup>4</sup> Máximo de 30 habilidades por conjunto de habilidades.

<sup>5</sup> Las cargas de trabajo de búsqueda cognitiva y análisis de imágenes en la indexación de blobs de Azure tienen tiempos de ejecución más cortos que la indexación de texto normal. El análisis de imágenes y el procesamiento de lenguaje natural consumen muchos recursos informáticos y una cantidad desproporcionada de la potencia de procesamiento disponible. Se ha reducido el tiempo de ejecución para que otros trabajos de la cola tengan la oportunidad de ejecutarse.  

## <a name="queries-per-second-qps"></a>Consultas por segundo (QPS)

Cada cliente debe desarrollar las estimaciones de QPS de manera independiente. El tamaño y la complejidad del índice, el tamaño y la complejidad de la consulta, y la cantidad de tráfico son los determinantes principales de las consultas por segundo. No hay manera de ofrecer estimaciones significativas cuando estos factores son desconocidos.

Las estimaciones son más predecibles cuando se calculan en los servicios que se ejecutan en recursos dedicados (niveles Básico y Estándar). Puede calcular el número de QPS con más precisión porque tiene control sobre más parámetros. Para obtener instrucciones sobre cómo encargarse del cálculo, consulte [Consideraciones sobre el rendimiento y la optimización de Azure Search](search-performance-optimization.md).

Los niveles de almacenamiento optimizado, debe esperar un rendimiento más bajo de consulta y una latencia superior que los niveles estándar.  La metodología para calcular el rendimiento de las consultas que puede experimentar es igual que los niveles estándar.

## <a name="data-limits-cognitive-search"></a>Límites de datos (búsqueda cognitiva)

Una [canalización de búsqueda cognitiva](cognitive-search-concept-intro.md) que realiza llamadas a un recurso de Text Analytics para el [reconocimiento de entidades](cognitive-search-skill-entity-recognition.md), la [extracción de frases clave](cognitive-search-skill-keyphrases.md), el [análisis de sentimiento ](cognitive-search-skill-sentiment.md) y la [detección de idioma](cognitive-search-skill-language-detection.md) está sujeta a los límites de datos. El tamaño máximo de un registro debe tener 50 000 caracteres según lo que mida `String.Length`. Si tiene que dividir los datos antes de enviarlos al analizador de opiniones, use la [aptitud División de texto](cognitive-search-skill-textsplit.md).

## <a name="api-request-limits"></a>Límites de solicitudes de API
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
Claves de API se usan para la autenticación de servicio. Hay dos tipos. Las claves de administración se especifican en el encabezado de solicitud y conceden acceso completo de lectura y escritura al servicio. Las claves de consulta son de solo lectura, se especifican en la dirección URL y normalmente se distribuyen a las aplicaciones cliente.

* Máximo de 2 claves de administración por servicio
* Máximo de 50 claves de consultas por servicio
