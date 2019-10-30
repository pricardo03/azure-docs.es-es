---
title: Estrategias de implementación y procedimientos recomendados para la optimización del rendimiento
titleSuffix: Azure Cognitive Search
description: Conozca las técnicas y los procedimientos recomendados para optimizar el rendimiento de Azure Cognitive Search y configurar la escala óptima.
manager: nitinme
author: LiamCavanagh
ms.author: liamca
ms.devlang: rest-api
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 15557a437732ee15c3c6dada7b2d9fe1d397dc5a
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2019
ms.locfileid: "72793420"
---
# <a name="deployment-strategies-and-best-practices-for-optimizing-performance-on-azure-cognitive-search"></a>Estrategias de implementación y procedimientos recomendados para la optimización del rendimiento en Azure Cognitive Search

En este artículo se describen las prácticas recomendadas para escenarios avanzados con requisitos sofisticados de escalabilidad y disponibilidad. 

## <a name="develop-baseline-numbers"></a>Desarrollo de números de línea de base
Al optimizar el rendimiento de la búsqueda, debe centrarse en los aspectos que reducen el tiempo de ejecución de consulta. Para ello, debe saber qué aspecto tiene una carga de consulta típica. Las instrucciones siguientes pueden ayudarle a llegar a números de consulta de línea de base.

1. Seleccionar una latencia objetivo (o cantidad máxima de tiempo) que una solicitud de búsqueda típica debería tardar en completarse.
2. Crear y probar una carga de trabajo real en su servicio de búsqueda con un conjunto de datos realista para medir estas tasas de latencia.
3. Comenzar con un número reducido de consultas por segundo (QPS) y aumentar gradualmente el número que se ejecuta en la prueba hasta que la latencia caiga por debajo de la latencia objetivo definida. Esta es una prueba comparativa importante que le ayudará a planear el escalado a medida que el uso de la aplicación crece.
4. Siempre que sea posible, reutilice las conexiones HTTP. Si usa el SDK para .NET de Azure Cognitive Search, debe reutilizar una instancia de [SearchIndexClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchindexclient), y si usa la API REST, debe reutilizar un única instancia de HttpClient.
5. Varíe el contenido de las solicitudes de consulta para que la búsqueda se realice a través de distintas partes del índice. La variación es importante porque si ejecuta continuamente las mismas solicitudes de búsqueda, el almacenamiento en caché de los datos empezará a ofrecer un rendimiento mejor que el que se obtendría con un conjunto de consultas más dispar.
6. Varíe la estructura de las solicitudes de consulta para obtener diferentes tipos de consultas. No todas las consultas de búsqueda se realizan en el mismo nivel. Por ejemplo, una sugerencia de búsqueda o la búsqueda de documentos normalmente es más rápida que una consulta con un número significativo de facetas y filtros. La composición de prueba debe incluir varias consultas en las mismas proporciones aproximadamente, tal como se esperaría en producción.  

Al crear estas cargas de trabajo de prueba, hay algunas características de Azure Cognitive Search que debe tener en cuenta:

+ Es posible sobrecargar el servicio mediante la inserción de demasiadas consultas de búsqueda al mismo tiempo. Cuando esto sucede, verá códigos de respuesta HTTP 503. Para evitar un código 503 durante las pruebas, empiece con varios intervalos de solicitudes de búsqueda para ver las diferencias en las tasas de latencia a medida que se agregan más solicitudes de búsqueda.

+ Azure Cognitive Search no ejecuta tareas de indexación en segundo plano. Si el servicio controla al mismo tiempo las cargas de trabajo de indexación y consulta, debe tener esto en cuenta mediante la introducción de trabajos de indexación en las pruebas de consulta o la exploración de opciones para ejecutar trabajos de indexación durante las horas de menor actividad.

> [!NOTE]
> Las [pruebas de carga de Visual Studio](https://www.visualstudio.com/docs/test/performance-testing/run-performance-tests-app-before-release) son una buena manera de realizar las pruebas comparativas porque permiten ejecutar solicitudes HTTP de la forma que necesite para ejecutar consultas en Azure Cognitive Search, y posibilita la paralelización de solicitudes.
> 
> 

## <a name="scaling-for-high-query-volume-and-throttled-requests"></a>Escalado para solicitudes limitadas y volumen elevado de consultas
Si recibe demasiadas solicitudes limitadas o se superan las tasas de latencia objetivo debido a una carga de consultas mayor, puede intentar reducir las tasas de latencia de dos maneras:

1. **Aumentar las réplicas:**  una réplica es como una copia de los datos que permite a Azure Cognitive Search equilibrar la carga de solicitudes entre las diversas copias.  Azure Cognitive Search administra el equilibrado de la carga y la replicación de los datos entre las réplicas, y puede modificar el número de réplicas asignado a su servicio en cualquier momento.  Puede asignar hasta 12 réplicas en un servicio de búsqueda estándar y 3 réplicas en un servicio de búsqueda básico. Las réplicas pueden ajustarse desde [Azure Portal](search-create-service-portal.md) o [PowerShell](search-manage-powershell.md).
2. **Aumentar el nivel de búsqueda:**  Azure Cognitive Search se ofrece con [diferentes niveles](https://azure.microsoft.com/pricing/details/search/), cada uno de los cuales proporciona distintos grados de rendimiento.  En algunos casos, el número de consultas es tan alto que su nivel actual no puede ofrecer tasas de latencia suficientemente bajas aunque se use el máximo de réplicas. En este caso, podría usar uno de los niveles de búsqueda más altos, como el nivel S3 de Azure Cognitive Search, muy conveniente para escenarios con un gran número de documentos y cargas de trabajo de consulta muy altas.

## <a name="scaling-for-slow-individual-queries"></a>Escalado para consultas individuales lentas
Otro motivo para las tasas de latencia altas es una única consulta que tarda demasiado tiempo en completarse. En este caso, agregar réplicas no ayudará. A continuación, se indican dos opciones posibles que podrían ayudar:

1. **Aumentar las particiones**: Una partición es un mecanismo para dividir los datos entre recursos adicionales. Agregar una segunda partición divide los datos en dos, una tercera partición los divide en tres, y así sucesivamente. Un efecto secundario positivo es que las consultas más lentas a veces se ejecutan más rápido debido a la computación en paralelo. Se observó la paralelización de consultas de selectividad baja, como las consultas que coinciden con muchos documentos, o facetas que proporcionan recuentos a través de un gran número de documentos. Como se necesita una gran cantidad de cálculo para puntuar la relevancia de los documentos o para contar el número de documentos, agregar particiones adicionales ayuda a que las consultas se completen más rápido.  
   
   Puede haber un máximo de 12 particiones en el servicio de búsqueda estándar y 1 partición en el servicio de búsqueda básico.  Las particiones pueden ajustarse desde [Azure Portal](search-create-service-portal.md) o [PowerShell](search-manage-powershell.md).

2. **Limitar los campos de alta cardinalidad:** Un campo de alta cardinalidad consta de un campo clasificable o filtrable que tiene un número significativo de valores únicos y, como resultado, consume recursos importantes al procesar los resultados. Por ejemplo, el establecimiento de un campo de identificador de producto o de descripción como filtrable o clasificable contaría como alta cardinalidad ya que la mayoría de los valores de un documento a otro son únicos. Siempre que sea posible, limite el número de campos de alta cardinalidad.

3. **Aumentar el nivel de búsqueda:**  moverse a un nivel superior de Azure Cognitive Search puede ser otra manera de mejorar el rendimiento de las consultas lentas. Cada nivel superior proporciona CPU más rápidas y más memoria, aspectos que tienen un impacto positivo en el rendimiento de las consultas.

## <a name="scaling-for-availability"></a>Escalado para disponibilidad
Las réplicas no solo ayudan a reducir la latencia de las consultas, sino que también permiten una alta disponibilidad. Con una sola réplica, puede esperar que se produzcan tiempos de inactividad periódicamente debido al reinicio del servidor después de las actualizaciones de software o por otros posibles eventos de mantenimiento.  Como resultado, es importante tener en cuenta si la aplicación requiere alta disponibilidad de búsquedas (consultas), así como de escrituras (eventos de indexación). Azure Cognitive Search ofrece opciones de Acuerdo de Nivel de Servicio en todas las ofertas de búsqueda de pago con los siguientes atributos:

* 2 réplicas para alta disponibilidad de cargas de trabajo de solo lectura (consultas)
* 3 o más réplicas para alta disponibilidad de cargas de trabajo de lectura-escritura (consultas e indización)

Para más información al respecto, visite el [Acuerdo de Nivel de Servicio de Azure Cognitive Search](https://azure.microsoft.com/support/legal/sla/search/v1_0/).

Como las réplicas son copias de los datos, tener varias réplicas permite que Azure Cognitive Search realice tareas de reinicio y mantenimiento de la máquina en una réplica mientras la ejecución de consultas continúa en otras. Por el contrario, si quita réplicas, provocará la degradación del rendimiento de consulta, suponiendo que esas réplicas eran un recurso infrautilizado.

## <a name="scaling-for-geo-distributed-workloads-and-geo-redundancy"></a>Escalado de cargas de trabajo distribuidas geográficamente y de redundancia geográfica
Los usuarios que se encuentran lejos del centro de datos que hospeda Azure Cognitive Search tendrán mayores índices de latencia en las cargas de trabajo distribuidas geográficamente. Una solución consiste en aprovisionar varios servicios de búsqueda en regiones más próximas a estos usuarios. Azure Cognitive Search no proporciona actualmente un método automatizado de replicación geográfica de índices de Azure Cognitive Search entre regiones, pero existen algunas técnicas que permiten simplificar la implementación y administración de este proceso. Se describen en las siguientes secciones.

El objetivo de un conjunto de servicios de búsqueda distribuido geográficamente es tener dos o más índices disponibles en dos o más regiones en las que un usuario se enruta al servicio de Azure Cognitive Search que proporciona la latencia más baja, tal como se muestra en este ejemplo:

   ![Tablas de referencias cruzadas de servicios por región][1]

### <a name="keeping-data-in-sync-across-multiple-azure-cognitive-search-services"></a>Sincronización de los datos entre varios servicios Azure Cognitive Search
Hay dos opciones para mantener sincronizados los servicios de búsqueda distribuidos: usar el [indexador de Azure Cognitive Search](search-indexer-overview.md) o la API de inserción (también denominada [API REST de Azure Cognitive Search](https://docs.microsoft.com/rest/api/searchservice/)).  

### <a name="use-indexers-for-updating-content-on-multiple-services"></a>Uso de indexadores para la actualización de contenido en varios servicios

Si ya usa un indexador en un servicio, puede configurar un segundo indexador en un segundo servicio para usar el mismo objeto de origen de datos, extrayendo datos de la misma ubicación. Cada servicio en cada región tiene su propio indexador y un índice de destino (el índice de búsqueda no se comparte, lo que significa que los datos se duplican), pero cada indexador hace referencia al mismo origen de datos.

Este es un objeto visual de alto nivel del aspecto que podría tener esa arquitectura.

   ![Origen de datos único con indexador distribuido y combinaciones de servicios][2]

### <a name="use-rest-apis-for-pushing-content-updates-on-multiple-services"></a>Uso de las API REST para insertar actualizaciones de contenido en varios servicios
Si usa la API REST de Azure Cognitive Search para [insertar contenido en el índice de Azure Cognitive Search](https://docs.microsoft.com/rest/api/searchservice/update-index) y quiere mantener sincronizados los distintos servicios de búsqueda, inserte los cambios en todos los servicios de búsqueda cada vez que se necesite una actualización. En el código, asegúrese de controlar los casos en los que una actualización en un servicio de búsqueda produce un error, pero se ejecuta correctamente en otros servicios de búsqueda.

## <a name="leverage-azure-traffic-manager"></a>Aprovechamiento de Azure Traffic Manager
[Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md) permite enrutar solicitudes a diversos sitios web con ubicación geográfica atendidos por distintos servicios Azure Cognitive Search. Una ventaja de Traffic Manager es que puede sondear Azure Cognitive Search para asegurarse de que está disponible y enrutar los usuarios a servicios de búsqueda alternativos en caso de tiempo de inactividad. Además, si va a enrutar solicitudes de búsqueda a través de Azure Web Sites, Azure Traffic Manager permite equilibrar la carga de aquellos casos en los que el sitio web está activo pero Azure Cognitive Search no. Este es un ejemplo de una arquitectura que utiliza el Administrador de tráfico.

   ![Tablas de referencias cruzadas de servicios por región con Administrador de tráfico central][3]

## <a name="next-steps"></a>Pasos siguientes
Para más información sobre los planes de tarifa y los límites de los servicios de cada uno de ellos, consulte [Límites de servicio en Azure Cognitive Search](search-limits-quotas-capacity.md).

Consulte [Escalado de niveles de recursos para cargas de trabajo de indexación y consulta en Búsqueda de Azure](search-capacity-planning.md) para obtener ayuda para elegir las combinaciones de particiones y réplicas.

Para más información sobre el rendimiento y para ver algunas demostraciones sobre cómo implementar las optimizaciones que se tratan en este artículo, vea el vídeo siguiente:

> [!VIDEO https://channel9.msdn.com/Events/Microsoft-Azure/AzureCon-2015/ACON319/player]
> 
> 

<!--Image references-->
[1]: ./media/search-performance-optimization/geo-redundancy.png
[2]: ./media/search-performance-optimization/scale-indexers.png
[3]: ./media/search-performance-optimization/geo-search-traffic-mgr.png
