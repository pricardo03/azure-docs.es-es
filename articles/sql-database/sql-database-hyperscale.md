---
title: Información general de Hiperescalado de Azure SQL Database | Microsoft Docs
description: En este tema se describe el nivel de servicio de Hiperescalado en el modelo de compra basado en núcleos virtuales en Azure SQL Database, y se explica cómo difiere de los niveles de servicio de Uso general y Crítico para la empresa.
services: sql-database
ms.service: sql-database
ms.subservice: ''
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 09/23/2018
ms.openlocfilehash: 314a2182c3be5c5f10e075cdbda27f4a716f2536
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2018
ms.locfileid: "47160407"
---
# <a name="what-is-the-hyperscale-service-tier-preview-in-azure-sql-database"></a>¿Qué es el nivel de servicio de Hiperescalado (versión preliminar) en Azure SQL Database?

El nivel de servicio de Hiperescalado en Azure SQL Database es el nivel de servicio más reciente en el modelo de compra basado en núcleos virtuales. Este nivel de servicio es un nivel altamente escalable de almacenamiento y de rendimiento de proceso que aprovecha la arquitectura de Azure para escalar horizontalmente el almacenamiento y los recursos de proceso para una instancia de Azure SQL Database considerablemente más allá de los límites disponibles para los niveles de servicio Uso general y Crítico para la empresa.

## <a name="what-are-the-capabilities-of-the-hyperscale-service-tier"></a>Cuáles son las funcionalidades del nivel de servicio de Hiperescalado

El nivel de servicio de Hiperescalado en Azure SQL Database proporciona las siguientes funcionalidades adicionales:

- Compatibilidad con bases de datos con un tamaño de hasta 100 TB 
- Copias de seguridad de bases de datos grandes más rápidas (basadas en instantáneas de archivos)
- Restauraciones de bases de datos más rápidas (basadas en instantáneas de archivos) 
- Mayor rendimiento general debido a un mayor rendimiento de los registros y tiempos más rápidos de confirmación de las transacciones, independientemente de los volúmenes de datos 
- Rápido escalado horizontal: puede aprovisionar uno o varios de solo lectura nodos para la descarga de la carga de trabajo de lectura y para su uso como esperas activas
- Rápido escalado vertical: en tiempo constante, puede escalar verticalmente los recursos de proceso para dar cabida a cargas de trabajo pesadas como y cuando sea necesario y, a continuación, reducir verticalmente los recursos de proceso cuando no sean necesarios. 

El nivel de servicio de Hiperescalado quita muchos de los límites prácticos que tradicionalmente se ven en las bases de datos en la nube. Donde la mayoría de las otras bases de datos están limitados por los recursos disponibles en un único nodo, las bases de datos en el nivel de servicio de Hiperescalado no tienen límites de este tipo. Con su arquitectura de almacenamiento flexible, puede escalar horizontalmente la capacidad de almacenamiento según sea necesario. De hecho, las bases de datos de Hiperescalado no se crean con un tamaño máximo definido. Una base de datos de Hiperescalado aumenta según sea necesario, y se le cobrará solo la capacidad que use. Para cargas de trabajo de lectura intensiva, el nivel de servicio de Hiperescalado proporciona rápida escalabilidad horizontal mediante el aprovisionamiento de réplicas de lectura adicionales según sea necesario para descargar las cargas de trabajo de lectura. 

Además, el tiempo necesario para crear copias de seguridad de bases de datos o para escalar o reducir verticalmente ya no está ligado al volumen de los datos en la base de datos. Pueden crearse copias de seguridad de las bases de datos de Hiperescalado de manera prácticamente instantánea. También puede escalar o reducir verticalmente una base de datos de decenas de terabytes en cuestión de minutos. Esta funcionalidad le libra de la preocupación de estar atado por las opciones de la configuración inicial. 

Para obtener más información sobre los tamaños de proceso para el nivel de servicio de Hiperescalado, consulte [Selección del nivel de servicio basado en núcleos virtuales, y los recursos de proceso, memoria, almacenamiento y E/S](sql-database-service-tiers-vcore.md).

## <a name="who-should-consider-the-hyperscale-service-tier"></a>Quién debe tener en cuenta el nivel de servicio de Hiperescalado

El nivel de servicio de Hiperescalado sirve principalmente para los clientes que tienen grandes bases de datos ya sea de forma local y quieren modernizar sus aplicaciones al moverlas a la nube, o para los clientes que ya están en la nube y están limitados por restricciones en el tamaño máximo de la base de datos (de 1 a 4 TB). También sirve para los clientes que buscan alto rendimiento y alta escalabilidad para el almacenamiento y los procesos.

El nivel de servicio de Hiperescalado es compatible con todas las cargas de trabajo de SQL Server, pero está optimizado principalmente para OLTP. El nivel de servicio de Hiperescalado también admite cargas de trabajo híbridas y analíticas (data mart). 

> [!IMPORTANT]
> Los grupos elásticos no admiten el nivel de servicio de Hiperescalado.

## <a name="architecture-distributing-functions-to-isolate-capabilities"></a>Arquitectura: distribución de funciones para aislar las funcionalidades

A diferencia de los motores de base de datos tradicionales, que han centralizada todas las funciones de administración de datos en una ubicación o proceso (incluso las llamadas bases de datos distribuidas en producción actualmente tienen varias copias de un motor de datos monolítico), una base de datos de Hiperescalado separa el motor de procesamiento de consultas, donde la semántica de los diversos motores de datos difieren, de los componentes que proporcionan almacenamiento a largo plazo y durabilidad de los datos. De este modo, la capacidad de almacenamiento se puede escalar horizontalmente fácilmente en cuanto sea necesario (el destino inicial es de 100 TB). Las réplicas de solo lectura comparten los mismos componentes de proceso, por lo que no se requiere ninguna copia de datos para poner en marcha una nueva réplica legible.

El siguiente diagrama ilustra los diferentes tipos de nodos en una base de datos de Hiperescalado:

![arquitectura](./media/sql-database-hyperscale/hyperscale-architecture.png)

Una base de datos de Hiperescalado contiene los distintos tipos de nodo siguientes:
 
### <a name="compute-node"></a>Nodo de ejecución

El nodo de ejecución es donde reside el motor relacional, donde ocurren todos los elementos de lenguaje, el procesamiento de consultas, etc. Todas las interacciones de usuario con una base de datos de Hiperescalado se producen a través de estos nodos de ejecución. Los nodos de ejecución tienen memorias caché basadas en SSD (con la etiqueta RBPEX, extensión del grupo de búferes resistentes, en el diagrama anterior) para minimizar el número de recorridos de ida y vuelta de red necesarios para capturar una página de datos. Hay un nodo de ejecución principal donde se procesan todas las transacciones y las cargas de trabajo de lectura y escritura. Hay uno o varios nodos de ejecución secundarios que actúan como nodos en espera activa para fines de conmutación por error, además de actuar como nodos de ejecución de solo lectura para la descarga de cargas de trabajo de lectura (si se desea esta funcionalidad).

### <a name="page-server-node"></a>Nodo de servidor de páginas

Los servidores de páginas son sistemas que representan un motor de almacenamiento escalado horizontalmente.  Cada servidor de páginas es responsable de un subconjunto de las páginas en la base de datos.  Nominalmente, cada servidor de páginas controla 1 terabyte de datos. No se comparte ningún dato en más de un servidor de páginas (fuera de las réplicas que se mantienen para ofrecer redundancia y disponibilidad). El trabajo de un servidor de páginas es servir las páginas de la base de datos a los nodos de ejecución a petición, y conservar las páginas actualizadas a medida que las transacciones actualizan los datos. Los servidores de páginas se mantienen actualizados mediante la reproducción de entradas del registro del servicio de registro. Los servidores de páginas también mantienen memorias caché basadas en SSD para mejorar el rendimiento. El almacenamiento a largo plazo de las páginas de datos se mantiene en Azure Storage para aumentar la confiabilidad.

### <a name="log-service-node"></a>Nodo de servicio de registros

El nodo de servicio de registros acepta entradas de registro desde el nodo de ejecución principal, los conserva en una memoria caché duradera y reenvía las entradas del registro al resto de los nodos de ejecución (para que puedan actualizar sus memorias caché), así como los servidores de páginas pertinentes, para que los datos puedan actualizarse allí. De este modo, todos los cambios en los datos desde el nodo de ejecución principal se propagan a través del servicio de registro a todos los nodos de ejecución secundarios y los servidores de páginas. Por último, las entradas de registro se insertan en el almacenamiento a largo plazo en Azure Storage, que es un repositorio de almacenamiento infinito. Este mecanismo elimina la necesidad de truncamiento frecuente del registro. El servicio de registro también tiene memoria caché local para acelerar el acceso.

### <a name="azure-storage-node"></a>Nodo de almacenamiento de Azure

El nodo de almacenamiento de Azure es el destino final de los datos a partir de servidores de páginas. Este almacenamiento se usa con fines de copia de seguridad, así como para la replicación entre regiones de Azure. Las copias de seguridad constan de instantáneas de archivos de datos. La operación de restauración es rápido a partir de estas instantáneas, y los datos se pueden restaurar en cualquier momento. 

## <a name="backup-and-restore"></a>Copia de seguridad y restauración

Las copias de seguridad son la base de instantáneas de archivos y, por tanto, son casi instantáneas. La separación del almacenamiento y los procesos permiten insertar la operación de copia de seguridad y restauración en la capa de almacenamiento para reducir la carga de procesamiento en el nodo de ejecución principal. Como resultado, la copia de seguridad de una base de datos de gran tamaño no afecta al rendimiento del nodo de ejecución principal. De forma similar, las restauraciones se realizan mediante la copia de la instantánea de archivos y, por tanto, no dependen de la operación de datos. Para restauraciones en la misma cuenta de almacenamiento, la operación de restauración es rápida.

## <a name="scale-and-performance-advantages"></a>Ventajas de escala y rendimiento

Con la capacidad de aumentar o disminuir rápidamente los nodos de ejecución adicionales de solo lectura, la arquitectura de Hiperescalado permite obtener importantes funcionalidades de escalado de lectura y también puede liberar el nodo de ejecución principal para atender más solicitudes de escritura. Además, los nodos de ejecución se pueden escalar o reducir verticalmente rápidamente debido a la arquitectura de almacenamiento compartido de la arquitectura de Hiperescalado. 


## <a name="next-steps"></a>Pasos siguientes

- Para más información sobre los niveles de servicio, consulte [Niveles de servicio](sql-database-service-tiers.md).
- Para más información sobre los límites de recursos, consulte los [límites de recursos de núcleos virtuales](sql-database-resource-limits.md).
- Para obtener una lista de características y una comparación, consulte [Características comunes de SQL](sql-database-features.md).
