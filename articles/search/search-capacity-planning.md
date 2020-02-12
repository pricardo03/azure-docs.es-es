---
title: Capacidad de escalado para cargas de trabajo de consultas e índices
titleSuffix: Azure Cognitive Search
description: Ajuste los recursos de proceso de réplica y partición en Azure Cognitive Search, donde el precio de cada recurso se basa en unidades de búsqueda facturables.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 349587063c528fef1cbdb09d84e61e82443d45d1
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2020
ms.locfileid: "76906732"
---
# <a name="scale-up-partitions-and-replicas-to-add-capacity-for-query-and-index-workloads-in-azure-cognitive-search"></a>Escalado vertical de particiones y réplicas para agregar capacidad para cargas de trabajo de consultas e índices en Azure Cognitive Search

Después de [elegir un plan de tarifa](search-sku-tier.md) y [aprovisionar un servicio de búsqueda](search-create-service-portal.md), el siguiente paso es aumentar opcionalmente el número de réplicas o particiones utilizadas por el servicio. Cada nivel ofrece un número fijo de unidades de facturación. En este artículo se explica cómo asignar las unidades para lograr una configuración óptima que equilibra los requisitos para la ejecución de consulta, indexación y almacenamiento.

La configuración de recursos está disponible cuando se configura un servicio en un [nivel Básico](https://aka.ms/azuresearchbasic) o uno de los [niveles Estándar o Almacenamiento optimizado](search-limits-quotas-capacity.md). Para los servicios en estos niveles, la capacidad se adquiere en incrementos de *unidades de búsqueda* (SU), en las que cada partición y réplica cuentan como una SU. 

Uso de menos SU da lugar a una factura proporcionalmente menor. Mientras el servicio esté configurando, le seguiremos cobrando. Si va a estar un tiempo sin utilizar un servicio, la única forma de evitar que le cobremos será eliminando el servicio y creándolo de nuevo cuando lo necesite más adelante.

> [!Note]
> La eliminación de un servicio supone la eliminación de todo su contenido. Azure Cognitive Search no dispone de ningún recurso para realizar copias de seguridad de datos de búsqueda persistentes ni para restaurarlos. Para volver a implementar un índice existente en un nuevo servicio, debe ejecutar el programa usado para crearlo y cargarlo originalmente. 

## <a name="terminology-replicas-and-partitions"></a>Terminología: réplicas y particiones
Las réplicas y particiones son los principales recursos que respaldan a un servicio de búsqueda.

| Resource | Definición |
|----------|------------|
|*Particiones* | Proporciona almacenamiento de índices y E/S para realizar operaciones de lectura y escritura (por ejemplo, volver a generar o actualizar un índice).|
|*Réplicas* | Instancias del servicio de búsqueda, que se utilizan principalmente para equilibrar la carga de las operaciones de consulta. Cada réplica siempre hospeda una copia de un índice. Si hay 12 réplicas, tendrá 12 copias de todos los índices cargados en el servicio.|

> [!NOTE]
> No existe ninguna manera de manipular o administrar directamente los índices que se ejecutan en una réplica. Una copia de cada índice de cada réplica forma parte de la arquitectura del servicio.
>


## <a name="how-to-allocate-replicas-and-partitions"></a>Cómo asignar réplicas y particiones
En Azure Cognitive Search, un servicio se asigna inicialmente a un nivel mínimo de recursos que consta de una partición y una réplica. En los niveles donde se admita, puede ajustar de forma incremental los recursos informáticos aumentando las particiones si necesita más almacenamiento y E/S, o bien agregue réplicas para mejorar el rendimiento y aumentar los volúmenes de las consultas. Un único servicio debe tener recursos suficientes para controlar todas las cargas de trabajo (indexación y consultas). No se pueden subdividir las cargas de trabajo entre varios servicios.

Para aumentar o cambiar la asignación de réplicas y particiones, se recomienda usar Azure Portal. El portal aplica límites a las combinaciones permitidas que se mantengan por debajo de los límites máximos. Si necesita un enfoque de aprovisionamiento basado en script o en código, [Azure PowerShell](search-manage-powershell.md) o la [API de REST de administración](https://docs.microsoft.com/rest/api/searchmanagement/services) son soluciones alternativas.

Como norma general, las aplicaciones de búsqueda necesitan más réplicas que particiones, sobre todo cuando las operaciones de servicio están orientadas a las cargas de trabajo de consulta. En la siguiente sección sobre [alta disponibilidad](#HA) se explica el motivo.

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) y seleccione su servicio de búsqueda.

2. En **Configuración**, abra la página **Escala** para modificar réplicas y particiones. 

   En la siguiente captura de pantalla se muestra un servicio estándar aprovisionado con una réplica y una partición. La fórmula de la parte inferior indica cuántas unidades de búsqueda se usan (1). Si el precio por unidad era de 100 USD (no un precio real), el costo de ejecución de este servicio sería, de media, de 100 USD.

   ![Página de escala que muestra los valores actuales](media/search-capacity-planning/1-initial-values.png "Página de escala que muestra los valores actuales")

3. Use el control deslizante para aumentar o reducir el número de particiones. La fórmula de la parte inferior indica cuántas unidades de búsqueda se usan.

   En este ejemplo se duplica la capacidad, con dos réplicas y particiones de forma individual. Observe el recuento de unidades de búsqueda; ahora es cuatro porque la fórmula de facturación son las réplicas multiplicadas por las particiones (2 x 2). Cuanto más se duplica la capacidad, más se duplica el costo de ejecución del servicio. Si el costo de la unidad de búsqueda era de 100 USD, la nueva factura mensual sería ahora de 400 USD.

   Para ver los costos por unidad actuales de cada nivel, visite la [Página de precios](https://azure.microsoft.com/pricing/details/search/).

   ![Adición de réplicas y particiones](media/search-capacity-planning/2-add-2-each.png "Adición de réplicas y particiones")

3. Haga clic en **Guardar** para confirmar los cambios.

   ![Confirmación de cambios en la escala y facturación](media/search-capacity-planning/3-save-confirm.png "Confirmación de cambios en la escala y facturación")

   Los cambios en la capacidad tardan varias horas en completarse. No se puede cancelar una vez que se ha iniciado el proceso y no hay ningún tipo de supervisión en tiempo real de los ajustes de réplica y partición. Sin embargo, el siguiente mensaje se puede seguir viendo mientras se están realizando los cambios.

   ![Mensaje de estado del portal](media/search-capacity-planning/4-updating.png "Mensaje de estado del portal")


> [!NOTE]
> Una vez que se aprovisiona un servicio, no se puede actualizar en contexto a una SKU superior. Debe crear un servicio de búsqueda en el nivel nuevo y volver a cargar los índices. Consulte [Creación de un servicio Azure Cognitive Search mediante Azure Portal](search-create-service-portal.md) para obtener ayuda con el proceso de aprovisionamiento de servicios.
>
>

<a id="chart"></a>

## <a name="partition-and-replica-combinations"></a>Combinaciones de particiones y réplicas

Un servicio del nivel Básico puede tener exactamente 1 partición y hasta 3 réplicas para un límite máximo de 3 SU. El único recurso que puede ajustarse son las réplicas. Se necesita un mínimo de 2 réplicas para lograr una alta disponibilidad en las consultas.

Todos los servicios de búsqueda de los niveles Estándar y Almacenamiento optimizado pueden suponer las siguientes combinaciones de réplicas y particiones, con el límite de 36 unidades de búsqueda (SU). 

|   | **1 partición** | **2 particiones** | **3 particiones** | **4 particiones** | **6 particiones** | **12 particiones** |
| --- | --- | --- | --- | --- | --- | --- |
| **1 réplica** |1 unidad de búsqueda |2 unidades de búsqueda |3 unidades de búsqueda |4 unidades de búsqueda |6 unidades de búsqueda |12 unidades de búsqueda |
| **2 réplicas** |2 unidades de búsqueda |4 unidades de búsqueda |6 unidades de búsqueda |8 unidades de búsqueda |12 unidades de búsqueda |24 unidades de búsqueda |
| **3 réplicas** |3 unidades de búsqueda |6 unidades de búsqueda |9 unidades de búsqueda |12 unidades de búsqueda |18 unidades de búsqueda |36 unidades de búsqueda |
| **4 réplicas** |4 unidades de búsqueda |8 unidades de búsqueda |12 unidades de búsqueda |16 unidades de búsqueda |24 unidades de búsqueda |N/D |
| **5 réplicas** |5 unidades de búsqueda |10 unidades de búsqueda |15 unidades de búsqueda |20 unidades de búsqueda |30 unidades de búsqueda |N/D |
| **6 réplicas** |6 unidades de búsqueda |12 unidades de búsqueda |18 unidades de búsqueda |24 unidades de búsqueda |36 unidades de búsqueda |N/D |
| **12 réplicas** |12 unidades de búsqueda |24 unidades de búsqueda |36 unidades de búsqueda |N/D |N/D |N/D |

En el sitio web de Azure se explican con detalle la capacidad, los precios y las unidades de búsqueda. Para obtener más información, consulte [Detalles de precios](https://azure.microsoft.com/pricing/details/search/).

> [!NOTE]
> El número de réplicas y particiones se dividirse equitativamente en 12 (en concreto, 1, 2, 3, 4, 6, 12). Esto se debe a que Azure Cognitive Search divide previamente cada índice en 12 particiones para que se pueda repartir en porciones iguales entre todas las particiones. Por ejemplo, si su servicio tiene tres particiones y crea un nuevo índice, cada partición contendrá 4 particiones del índice. La manera en que Azure Cognitive Search particiona un índice es un detalle de implementación, sujeto a cambios en la futura versión. Aunque el número es 12 hoy, no debe esperar que ese número se siempre 12 en el futuro.
>


<a id="HA"></a>

## <a name="high-availability"></a>Alta disponibilidad
Dado que es sencillo y relativamente rápido escalar verticalmente, generalmente se recomienda que comience con una partición y una o dos réplicas, y que después escale verticalmente conforme se crean volúmenes de consulta. Las cargas de trabajo de consulta se ejecutan principalmente en réplicas. Es probable que necesite más réplicas si requiere más rendimiento o alta disponibilidad.

Las recomendaciones generales para alta disponibilidad son:

* Dos réplicas para alta disponibilidad de cargas de trabajo de solo lectura (consultas)

* Tres o más réplicas para lograr una alta disponibilidad en las cargas de trabajo de lectura y escritura (se agregan, actualizan o eliminan consultas e indexación como documentos individuales).

Los Acuerdos de Nivel de Servicio (SLA) de Azure Cognitive Search están destinados a las operaciones de consulta y actualizaciones de índices que constan de procesos de incorporación, actualización o eliminación de documentos.

El nivel básico alcanza el límite en una partición y tres réplicas. Si desea la flexibilidad de responder inmediatamente a las fluctuaciones en la demanda para el rendimiento de indexación y consulta, considere uno de los niveles Estándar.  Si descubre que el crecimiento de sus requisitos de almacenamiento es mucho más rápido que el del rendimiento de consultas, tenga en cuenta uno de los niveles Almacenamiento optimizado.

### <a name="index-availability-during-a-rebuild"></a>Disponibilidad de los índices durante un proceso de regeneración

La alta disponibilidad para Azure Cognitive Search se refiere a las consultas y actualizaciones de índices que no requieren volver a generar un índice. Si elimina un campo, cambia un tipo de datos o el nombre de un campo, debe volver a generar el índice. Para volver a crear el índice, debe eliminar el índice, volver a crearlo y cargar de nuevo los datos.

> [!NOTE]
> Puede agregar nuevos campos a un índice de Azure Cognitive Search sin volver a generar el índice. El valor del nuevo campo será Null en todos los documentos que estén en el índice.

Al recompilar el índice, habrá un período de tiempo en el que los datos se estarán agregando al nuevo índice. Si desea mantener la disponibilidad del índice antiguo durante este período de tiempo, debe contar con una copia del índice antiguo con un nombre distinto en el mismo servicio, o bien una copia del índice con el mismo nombre en un servicio diferente. Posteriormente, tendrá que proporcionar la lógica de conmutación por error o redireccionamiento en el código.

## <a name="disaster-recovery"></a>Recuperación ante desastres
En la actualidad no hay ningún mecanismo integrado para la recuperación ante desastres. La adición de particiones o réplicas sería la estrategia equivocada para cumplir los objetivos de recuperación ante desastres. El enfoque más común es agregar redundancia en el nivel de servicio mediante la configuración de un segundo servicio de búsqueda en otra región. Al igual que con la disponibilidad durante la regeneración de índices, la lógica de conmutación por error o redireccionamiento debe proporcionarse en el código.

## <a name="increase-query-performance-with-replicas"></a>Aumento del rendimiento de las consultas con réplicas
La latencia de consultas es un indicador de que se necesitan más réplicas. Por lo general, el primer paso para mejorar el rendimiento de las consultas consiste en agregar más réplicas. Conforme agrega réplicas, se ponen en línea copias adicionales del índice para admitir mayores cargas de trabajo de consultas y equilibrar la carga de las solicitudes por las diversas réplicas.

No ofrecemos estimaciones finales sobre consultas por segundo (QPS); el rendimiento de las consultas depende de la complejidad de la consulta y las cargas de trabajo competitivas. Si bien la adición de réplicas genera claramente un mejor rendimiento, el resultado final no será estrictamente lineal: la adición de tres réplicas no garantiza el triple rendimiento.

Para obtener una guía para la estimación de las QPS para las cargas de trabajo, consulte [Consideraciones sobre el rendimiento y la optimización de Azure Cognitive Search](search-performance-optimization.md).

## <a name="increase-indexing-performance-with-partitions"></a>Aumento del rendimiento de la indexación con particiones
Las aplicaciones de búsqueda que requieren una actualización de datos casi en tiempo real necesitan una proporción mayor de particiones que de réplicas. Al agregarse particiones, se distribuyen las operaciones de lectura y escritura entre un número mayor de recursos de proceso. Además, se cuenta con más espacio en disco para almacenar documentos e índices adicionales.

Las consultas en índices de mayor tamaño tardan más tiempo en realizarse. Por lo tanto, es posible que con cada aumento incremental de las particiones sea necesario también un aumento menor, pero proporcional, de las réplicas. La complejidad y el volumen de las consultas afectarán a la rapidez con que se ejecuta la consulta.


## <a name="next-steps"></a>Pasos siguientes

[Selección de un plan de tarifa de Azure Cognitive Search](search-sku-tier.md)
