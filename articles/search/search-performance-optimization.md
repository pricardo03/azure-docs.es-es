---
title: 'Estrategias de implementación y procedimientos recomendados para optimizar el rendimiento: Azure Search'
description: Conozca las técnicas y los procedimientos recomendados para optimizar el rendimiento de Azure Search y configurar la escala óptima.
author: LiamCavanagh
manager: jlembicz
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.date: 03/02/2019
ms.author: liamca
ms.custom: seodec2018
ms.openlocfilehash: 8a07657d04cee57cb69c9f5f7862fed3e7965716
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61283598"
---
# <a name="deployment-strategies-and-best-practices-for-optimizing-performance-on-azure-search"></a>Estrategias de implementación y procedimientos recomendados para optimizar el rendimiento en Azure Search

En este artículo se describe las prácticas recomendadas para escenarios avanzados con requisitos sofisticados de escalabilidad y disponibilidad. 

## <a name="develop-baseline-numbers"></a>Desarrollar los números de línea base
Optimizar el rendimiento de la búsqueda, debe centrarse en lo que reduce el tiempo de ejecución de consulta. Para ello, debe saber qué aspecto tiene una carga de consulta típica. Las instrucciones siguientes pueden ayudarle a llegar a números de consulta de línea base.

1. Seleccionar una latencia objetivo (o cantidad máxima de tiempo) que una solicitud de búsqueda típica debería tardar en completarse.
2. Crear y probar una carga de trabajo real en su servicio de búsqueda con un conjunto de datos realista para medir estas tasas de latencia.
3. Comience con un número reducido de consultas por segundo (QPS) y aumentar gradualmente el número que se ejecutan en la prueba hasta que la latencia de consulta cae por debajo de la latencia objetivo definida. Esta es una prueba comparativa importante que le ayudará a planear el escalado a medida que el uso de la aplicación crece.
4. Siempre que sea posible, reutilice las conexiones HTTP. Si usa el SDK de .NET para Azure Search, deberá reutilizar una instancia o una instancia de [SearchIndexClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchindexclient) , y si usa la API de REST, deberá reutilizar un única instancia de HttpClient.
5. Variar la sustancia de solicitudes de consulta por lo que la búsqueda se realiza a través de distintas partes del índice. Variación es importante porque si ejecuta continuamente las mismas solicitudes de búsqueda, se iniciará el almacenamiento en caché de datos ofrecer un rendimiento mejor que se podría con una consulta más dispar establecido.
6. Variar la estructura de las solicitudes de consulta para que obtenga los diferentes tipos de consultas. No todas las consultas búsqueda se realiza en el mismo nivel. Por ejemplo, una sugerencia de búsqueda o la búsqueda de documentos es normalmente más rápida que una consulta con un número significativo de facetas y filtros. Composición de prueba debe incluir varias consultas en aproximadamente las mismas proporciones tal como se esperaría en producción.  

Al crear estas cargas de trabajo de prueba, hay algunas características de Azure Search que debe tener en cuenta:

+ Es posible sobrecargar el servicio mediante la inserción de demasiadas consultas de búsqueda al mismo tiempo. Cuando esto sucede, verá códigos de respuesta HTTP 503. Para evitar un 503 durante las pruebas, empezar con varios intervalos de solicitudes de búsqueda para ver las diferencias en las tasas de latencia a medida que agrega más solicitudes de búsqueda.

+ Azure Search no ejecuta tareas de indización en segundo plano. Si el servicio controla al mismo tiempo las cargas de trabajo de indexación y consulta, tenerlo en cuenta cualquier presentación de los trabajos de indexación en las pruebas de consulta, o bien explorar opciones para ejecutar trabajos de indexación durante las horas.

> [!NOTE]
> Las [pruebas de carga de Visual Studio](https://www.visualstudio.com/docs/test/performance-testing/run-performance-tests-app-before-release) son una buena manera de realizar las pruebas comparativas porque permite ejecutar solicitudes HTTP tal y como necesite para ejecutar consultas en Azure Search, y habilita la paralelización de solicitudes.
> 
> 

## <a name="scaling-for-high-query-volume-and-throttled-requests"></a>Escalado de volumen elevado de consultas y solicitudes limitadas
Cuando se están recibiendo demasiadas solicitudes limitadas o se superan las tasas de latencia de destino desde una carga de consultas mayor, puede intentar para reducir las tasas de latencia de dos maneras:

1. **Aumentar las réplicas:**  una réplica es como una copia de los datos que permite a Azure Search equilibrar la carga de solicitudes entre las diversas copias.  Azure Search administra el equilibrado de la carga y la replicación de los datos entre las réplicas, y puede modificar el número de réplicas asignado a su servicio en cualquier momento.  Puede asignar hasta 12 réplicas en un servicio de búsqueda estándar y 3 réplicas en un servicio de búsqueda básico. Las réplicas pueden ajustarse desde [Azure Portal](search-create-service-portal.md) o [PowerShell](search-manage-powershell.md).
2. **Aumentar el nivel de búsqueda:**  Azure Search se ofrece con [diferentes niveles](https://azure.microsoft.com/pricing/details/search/), cada uno de los cuales proporciona diferentes niveles de rendimiento.  En algunos casos, el número de consultas es tan alto que su nivel actual no puede ofrecer tasas de latencia suficientemente bajas aunque se use el máximo de réplicas. En este caso, desea considere la posibilidad de usar uno de los niveles más altos de búsqueda, como el nivel S3 de Azure Search que resulta ideal para escenarios con un gran número de documentos y las cargas de trabajo de consulta muy altas.

## <a name="scaling-for-slow-individual-queries"></a>Ajuste de escala para consultas individuales lentas
Otra razón para las tasas de latencia alta es una sola consulta tarda demasiado en completarse. En este caso, agregar réplicas no ayudará. Dos opciones posibles opciones que puedan ayudar a los siguientes:

1. **Aumentar las particiones**: Una partición es un mecanismo para dividir los datos entre recursos adicionales. Agregar una segunda partición divide los datos en dos, una tercera partición divide en tres y así sucesivamente. Un efecto secundario positivo es que las consultas más lentas a veces la realizar más rápido debido a la informática en paralelo. Se observó la paralelización de consultas de selectividad baja, como las consultas que coinciden con muchos documentos o facetas proporcionar recuentos a través de un gran número de documentos. Puesto que se requiere cálculo significativo para puntuar la pertinencia de los documentos, o contar el número de documentos, agregar particiones adicionales le ayuda a las consultas se completen más rápido.  
   
   Puede haber un máximo de 12 particiones en el servicio de búsqueda estándar y 1 partición en el servicio de búsqueda básico.  Las particiones pueden ajustarse desde [Azure Portal](search-create-service-portal.md) o [PowerShell](search-manage-powershell.md).

2. **Limitar los campos de alta cardinalidad:** Un campo de alta cardinalidad consta de una navegación por facetas o un campo filtrable que tiene un número significativo de valores únicos y como resultado, consume recursos considerables al calcular los resultados. Por ejemplo, establecer un campo de Id. de producto o la descripción como filtrable/facetable contaría como alta cardinalidad ya que la mayoría de los valores de documento a documento es única. Siempre que sea posible, limite el número de campos de alta cardinalidad.

3. **Aumentar el nivel de búsqueda:**  moverse a un nivel superior de Azure Search puede ser otra manera de mejorar el rendimiento de las consultas lentas. Cada nivel superior proporciona más rápido las CPU y más memoria, que tienen un impacto positivo en el rendimiento de las consultas.

## <a name="scaling-for-availability"></a>Escalado para disponibilidad
Las réplicas no solo ayudan a reducir la latencia de las consultas, sino que también permiten una alta disponibilidad. Con una sola réplica, puede esperar que se produzcan tiempos de inactividad periódicamente debido al reinicio del servidor después de las actualizaciones de software o por otros posibles eventos de mantenimiento.  Como resultado, es importante tener en cuenta si la aplicación requiere alta disponibilidad de búsquedas (consultas), así como de escrituras (eventos de indexación). Azure Search ofrece opciones de SLA en todas las ofertas de búsqueda de pago con los siguientes atributos:

* 2 réplicas para alta disponibilidad de cargas de trabajo de solo lectura (consultas)
* 3 o más réplicas para alta disponibilidad de cargas de trabajo de lectura-escritura (consultas e indización)

Para más información al respecto, visite el [Acuerdo de Nivel de Servicio de Azure Search](https://azure.microsoft.com/support/legal/sla/search/v1_0/).

Puesto que las réplicas son copias de los datos, tener varias réplicas permite que Azure Search en el equipo reinicios y mantenimiento de una réplica al tiempo que permite la ejecución de la consulta continuar en otras réplicas. Por el contrario, si aprovechar inmediatamente las réplicas, tendrás que pagar degradación del rendimiento de consulta, suponiendo que las réplicas eran un recurso infrautilizados.

## <a name="scaling-for-geo-distributed-workloads-and-geo-redundancy"></a>Ajuste de escala para cargas de trabajo distribuidas geográficamente y redundancia geográfica
Para cargas de trabajo distribuidas geográficamente, los usuarios que se encuentran lejos del centro de datos que hospeda Azure Search tendrán mayores tasas de latencia. Una solución consiste en aprovisionar varios servicios de búsqueda en regiones más próximas a estos usuarios. Azure Search no proporciona actualmente un método automatizado de índices de Azure Search con replicación geográfica entre regiones, pero existen algunas técnicas que permiten simplificar la implementación y administración de este proceso. Se describen en las siguientes secciones.

El objetivo de un conjunto distribuido geográficamente de servicios de búsqueda es tener dos o más índices disponibles en dos o más regiones donde un usuario se enruta al servicio Azure Search que proporciona la menor latencia, tal como se muestra en este ejemplo:

   ![Tablas de referencias cruzadas de servicios por región][1]

### <a name="keeping-data-in-sync-across-multiple-azure-search-services"></a>Sincronización de los datos entre varios servicios Azure Search
Hay dos opciones para mantener los servicios de búsqueda distribuidos sincronizados: usar el [indexador de Azure Search](search-indexer-overview.md) o la API de inserción (también denominada [API de REST de Azure Search](https://docs.microsoft.com/rest/api/searchservice/)).  

### <a name="use-indexers-for-updating-content-on-multiple-services"></a>Utilizar indizadores para actualizar el contenido en varios servicios

Si ya usas indexador en un servicio, puede configurar un indexador de segundo en un segundo servicio para usar el mismo objeto de origen de datos, extracción de datos de la misma ubicación. Cada servicio en cada región tiene su propio indizador y un índice de destino (el corpus de búsqueda no se comparten, lo que significa que los datos se duplicaban), pero cada indizador hace referencia el mismo origen de datos.

Este es un objeto visual de alto nivel del aspecto que podría tener esa arquitectura.

   ![Origen de datos único con indexador distribuido y combinaciones de servicios][2]

### <a name="use-rest-apis-for-pushing-content-updates-on-multiple-services"></a>Use las API de REST para insertar las actualizaciones de contenido en varios servicios
Si está utilizando la API de REST de Azure Search a [insertar contenido en el índice de Azure Search](https://docs.microsoft.com/rest/api/searchservice/update-index), puede mantener sincronizados los distintos servicios de búsqueda, inserte los cambios a todos los servicios de búsqueda cada vez que se requiere una actualización. En el código, asegúrese de que controlen los casos donde una actualización de un servicio de búsqueda se produce un error, pero produce un error por otros servicios de búsqueda.

## <a name="leverage-azure-traffic-manager"></a>Aproveche Azure Traffic Manager
[Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md) permite enrutar solicitudes a diversos sitios web con ubicación geográfica atendidos por distintos servicios Azure Search. Una ventaja de Traffic Manager es que puede sondear Azure Search para asegurarse de que está disponible y enrutar los usuarios a servicios de búsqueda alternativos en caso de tiempo de inactividad. Además, si va a enrutar solicitudes de búsqueda a través de Azure Websites, Azure Traffic Manager permite equilibrar la carga de aquellos casos en los que el sitio web está activo pero Azure Search no. Este es un ejemplo de una arquitectura que utiliza el Administrador de tráfico.

   ![Tablas de referencias cruzadas de servicios por región con Administrador de tráfico central][3]

## <a name="monitor-performance"></a>Supervisión del rendimiento
Azure Search ofrece la capacidad de analizar y supervisar el rendimiento de su servicio a través de [análisis de tráfico de búsqueda](search-traffic-analytics.md). Al habilitar esta funcionalidad y agregar instrumentación a la aplicación cliente, también puede registrar las operaciones de búsqueda individuales, así como las métricas agregadas a una cuenta de almacenamiento de Azure que puede procesarse para su análisis o visualizar en Power BI. Capturas de métricas de este modo ofrecen estadísticas de rendimiento, como el número promedio de tiempos de respuesta de consulta o consultas. Además, el registro de operaciones permite profundizar en los detalles de operaciones de búsqueda específicas.

Análisis de tráfico es útil para comprender las tasas de latencia desde ese punto de vista de Azure Search. Como las métricas de rendimiento de las consultas registradas se basan en el tiempo que una consulta tarda en procesarse por completo en Azure Search (desde el momento en que se solicita hasta que se envía), puede usarlas para determinar si los problemas de latencia se producen en el lado del servicio Azure Search o fuera de servicio, por ejemplo, la latencia de la red.  

## <a name="next-steps"></a>Pasos siguientes
Para obtener más información acerca de los planes de tarifa y los límites de los servicios de cada uno de ellos, consulte [Límites de servicio en Azure Search](search-limits-quotas-capacity.md).

Consulte [Escalado de niveles de recursos para cargas de trabajo de indexación y consulta en Búsqueda de Azure](search-capacity-planning.md) para obtener ayuda para elegir las combinaciones de particiones y réplicas.

Para más información sobre el rendimiento y para ver algunas demostraciones sobre cómo implementar las optimizaciones que se tratan en este artículo, vea el vídeo siguiente:

> [!VIDEO https://channel9.msdn.com/Events/Microsoft-Azure/AzureCon-2015/ACON319/player]
> 
> 

<!--Image references-->
[1]: ./media/search-performance-optimization/geo-redundancy.png
[2]: ./media/search-performance-optimization/scale-indexers.png
[3]: ./media/search-performance-optimization/geo-search-traffic-mgr.png
