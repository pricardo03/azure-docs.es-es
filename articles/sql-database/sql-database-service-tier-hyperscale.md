---
title: Información general de Hiperescalado de Azure SQL Database | Microsoft Docs
description: En este artículo se describe el nivel de servicio Hiperescala en el modelo de compra basado en núcleos virtuales en Azure SQL Database y se explica cómo difiere de los niveles de servicio De uso general y Crítico para la empresa.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: 4db013b51657bb327c36d616a3743c46731cd19a
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "57903800"
---
# <a name="hyperscale-service-tier-preview-for-up-to-100-tb"></a>Nivel de servicio Hiperescalado (versión preliminar) para hasta 100 TB

Azure SQL Database se basa en la arquitectura del motor de base de datos de SQL Server que se ajusta al entorno en la nube, con el fin de garantizar una disponibilidad del 99,99 % incluso en los casos de error de la infraestructura. Hay tres modelos de arquitectura que se usan en Azure SQL Database:

- De uso general/Estándar 
- Crítico para la empresa/Premium
- Hiperescala

El nivel de servicio de Hiperescalado en Azure SQL Database es el nivel de servicio más reciente en el modelo de compra basado en núcleos virtuales. Este nivel de servicio es un nivel altamente escalable de almacenamiento y de rendimiento de proceso que aprovecha la arquitectura de Azure para escalar horizontalmente el almacenamiento y los recursos de proceso para una instancia de Azure SQL Database considerablemente más allá de los límites disponibles para los niveles de servicio Uso general y Crítico para la empresa.

> [!IMPORTANT]
> El nivel de servicio Hiperescala se encuentra actualmente en versión preliminar pública y está disponible en regiones de Azure limitadas. Para obtener la lista completa de regiones, consulte las [regiones disponibles del nivel de servicio Hiperescalado](#available-regions). No se recomienda ejecutar ninguna carga de trabajo de producción en las bases de datos de hiperescala todavía. No se puede actualizar una base de datos de hiperescala a otros niveles de servicio. Con fines de prueba, se recomienda realizar una copia de la base de datos actual y actualizar la copia al nivel de servicio Hiperescala.
> [!NOTE]
> Para más información acerca de los niveles de servicios Uso general y Crítico para la empresa en el modelo de compra basado en núcleos virtuales, consulte los niveles de servicio [de uso general](sql-database-service-tier-general-purpose.md) y [crítico para la empresa](sql-database-service-tier-business-critical.md). Para ver una comparación entre el modelo de compra basado en núcleos virtuales y el modelo de compra basado en DTU, consulte [Modelos de compra y recursos de Azure SQL Database](sql-database-purchase-models.md).
> [!IMPORTANT]
> El nivel de servicio Hiperescalado está actualmente en versión preliminar pública. No se recomienda ejecutar ninguna carga de trabajo de producción en las bases de datos de hiperescala todavía. No se puede actualizar una base de datos de hiperescala a otros niveles de servicio. Con fines de prueba, se recomienda realizar una copia de la base de datos actual y actualizar la copia al nivel de servicio Hiperescala.

## <a name="what-are-the-hyperscale-capabilities"></a>¿Cuáles son las funcionalidades de Hiperescala?

El nivel de servicio de Hiperescalado en Azure SQL Database proporciona las siguientes funcionalidades adicionales:

- Compatibilidad con bases de datos con un tamaño de hasta 100 TB
- Copias de seguridad de base de datos casi instantáneas (basadas en las instantáneas almacenadas en Azure Blob Storage) independientemente del tamaño sin efecto de la E/S en proceso   
- Restauraciones rápidas de base de datos (basadas en instantáneas de archivos) en minutos en lugar de horas o días (no el tamaño de la operación de datos)
- Mayor rendimiento general debido a un mayor rendimiento de los registros y tiempos más rápidos de confirmación de las transacciones, independientemente de los volúmenes de datos
- Rápido escalado horizontal: puede aprovisionar uno o varios de solo lectura nodos para la descarga de la carga de trabajo de lectura y para su uso como esperas activas
- Rápido escalado vertical: en tiempo constante, puede escalar verticalmente los recursos de proceso para dar cabida a cargas de trabajo pesadas como y cuando sea necesario y, a continuación, reducir verticalmente los recursos de proceso cuando no sean necesarios.

El nivel de servicio de Hiperescalado quita muchos de los límites prácticos que tradicionalmente se ven en las bases de datos en la nube. Donde la mayoría de las otras bases de datos están limitados por los recursos disponibles en un único nodo, las bases de datos en el nivel de servicio de Hiperescalado no tienen límites de este tipo. Con su arquitectura de almacenamiento flexible, el almacenamiento crece a medida que sea necesario. De hecho, las bases de datos de Hiperescalado no se crean con un tamaño máximo definido. Una base de datos de Hiperescalado aumenta según sea necesario, y se le cobrará solo la capacidad que use. Para cargas de trabajo de lectura intensiva, el nivel de servicio de Hiperescalado proporciona rápida escalabilidad horizontal mediante el aprovisionamiento de réplicas de lectura adicionales según sea necesario para descargar las cargas de trabajo de lectura.

Además, el tiempo necesario para crear copias de seguridad de bases de datos o para escalar o reducir verticalmente ya no está ligado al volumen de los datos en la base de datos. Pueden crearse copias de seguridad de las bases de datos de Hiperescalado de manera prácticamente instantánea. También puede escalar o reducir verticalmente una base de datos de decenas de terabytes en cuestión de minutos. Esta funcionalidad le libra de la preocupación de estar atado por las opciones de la configuración inicial.

Para más información sobre los tamaños de proceso para el nivel de servicio Hiperescalado, consulte [Características del nivel de servicios](sql-database-service-tiers-vcore.md#service-tier-characteristics).

## <a name="who-should-consider-the-hyperscale-service-tier"></a>Quién debe tener en cuenta el nivel de servicio de Hiperescalado

El nivel de servicio de Hiperescalado sirve principalmente para los clientes que tienen grandes bases de datos ya sea de forma local y quieren modernizar sus aplicaciones al moverlas a la nube, o para los clientes que ya están en la nube y están limitados por restricciones en el tamaño máximo de la base de datos (de 1 a 4 TB). También sirve para los clientes que buscan alto rendimiento y alta escalabilidad para el almacenamiento y los procesos.

El nivel de servicio de Hiperescalado es compatible con todas las cargas de trabajo de SQL Server, pero está optimizado principalmente para OLTP. El nivel de servicio de Hiperescalado también admite cargas de trabajo híbridas y analíticas (data mart).

> [!IMPORTANT]
> Los grupos elásticos no admiten el nivel de servicio de Hiperescalado.

## <a name="hyperscale-pricing-model"></a>Modelo de precios de Hiperescala

El nivel de servicio Hiperescalado solo está disponible en el [modelo de núcleos virtuales](sql-database-service-tiers-vcore.md). Para alinearse con la nueva arquitectura, el modelo de precios es ligeramente diferente de los niveles de servicio Se uso general o Crítico para la empresa:

- **Proceso**:

  El precio de la unidad de proceso de Hiperescala es por réplica. El precio de la [Ventaja híbrida de Azure](https://azure.microsoft.com/pricing/hybrid-benefit/) se aplica automáticamente a las réplicas de escalado de lectura. En la versión preliminar pública, se crean de forma predeterminada dos réplicas para bases de datos de Hiperescala.

- **Almacenamiento**:

  No es necesario especificar el tamaño máximo de datos al configurar una base de datos Hiperescala. En el nivel Hiperescala, se le cobra por el almacenamiento de su base de datos según el uso real. Como almacenamiento se asigna dinámicamente entre 5 GB y 100 TB, en incrementos de 1 GB.  

Para más información sobre los precios de Hiperescala, consulte [Precios de Azure SQL Database](https://azure.microsoft.com/pricing/details/sql-database/single/)

## <a name="distributed-functions-architecture"></a>Arquitectura de funciones distribuidas

A diferencia de los motores de base de datos tradicionales, que han centralizada todas las funciones de administración de datos en una ubicación o proceso (incluso las llamadas bases de datos distribuidas en producción actualmente tienen varias copias de un motor de datos monolítico), una base de datos de Hiperescalado separa el motor de procesamiento de consultas, donde la semántica de los diversos motores de datos difieren, de los componentes que proporcionan almacenamiento a largo plazo y durabilidad de los datos. De este modo, la capacidad de almacenamiento se puede escalar horizontalmente fácilmente en cuanto sea necesario (el destino inicial es de 100 TB). Las réplicas de solo lectura comparten los mismos componentes de proceso, por lo que no se requiere ninguna copia de datos para poner en marcha una nueva réplica legible. Durante la versión preliminar, se admite solo una réplica de solo lectura.

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

## <a name="create-a-hyperscale-database"></a>¿Qué es una base de datos de Hiperescala?

Puede crearse una base de datos a gran escala mediante la [portal Azure](https://portal.azure.com), [T-SQL](https://docs.microsoft.com/sql/t-sql/statements/create-database-transact-sql?view=azuresqldb-current), [PowerShell](https://docs.microsoft.com/powershell/module/az.sql/new-azsqldatabase) o [CLI](https://docs.microsoft.com/cli/azure/sql/db#az-sql-db-create). Las bases de datos de Hiperescala solo están disponibles con el [modelo de compra basado en núcleos virtuales](sql-database-service-tiers-vcore.md).

El siguiente comando de Transact-SQL crea una base de datos de Hiperescala. Debe especificar tanto la edición como el servicio objetivo en la instrucción `CREATE DATABASE`.

```sql
-- Create a HyperScale Database
CREATE DATABASE [HyperScaleDB1] (EDITION = 'HyperScale', SERVICE_OBJECTIVE = 'HS_Gen4_4');
GO
```

## <a name="migrate-an-existing-azure-sql-database-to-the-hyperscale-service-tier"></a>Migración de una base de datos Azure SQL existente al nivel de servicio Hiperescalado

Puede mover bases de datos SQL de Azure existentes a gran escala utilizando el [portal Azure](https://portal.azure.com), [T-SQL](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current), [PowerShell](https://docs.microsoft.com/powershell/module/az.sql/set-azsqldatabase) o [CLI](https://docs.microsoft.com/cli/azure/sql/db#az-sql-db-update). En la versión preliminar pública, esta es una migración unidireccional. No puede mover las bases de datos del nivel de servicio Hiperescalado a ningún otro. Es recomendable que haga una copia de las bases de datos de producción y migre a Hiperescalado las pruebas de concepto (POC).

El siguiente comando de T-SQL mueve una base de datos al nivel de servicio Hiperescalado. Debe especificar tanto la edición como el servicio objetivo en la instrucción `ALTER DATABASE`.

```sql
-- Alter a database to make it a HyperScale Database
ALTER DATABASE [DB2] MODIFY (EDITION = 'HyperScale', SERVICE_OBJECTIVE = 'HS_Gen4_4');
GO
```

## <a name="connect-to-a-read-scale-replica-of-a-hyperscale-database"></a>Conexión a una réplica de escalado de lectura de una base de datos de Hiperescala

En las bases de datos de Hiperescala, el argumento `ApplicationIntent` de la cadena de conexión proporcionado por el cliente dictamina si la conexión se enruta a la réplica de escritura o a una réplica de solo lectura secundaria. Si el argumento `ApplicationIntent` establecido en `READONLY` y la base de datos no tienen una réplica secundaria, la conexión se enrutará a la réplica principal y de forma predeterminada es el comportamiento `ReadWrite`.

```cmd
-- Connection string with application intent
Server=tcp:<myserver>.database.windows.net;Database=<mydatabase>;ApplicationIntent=ReadOnly;User ID=<myLogin>;Password=<myPassword>;Trusted_Connection=False; Encrypt=True;
```

## <a name="available-regions"></a>Regiones disponibles

El nivel de servicio Hiperescala se encuentra actualmente en versión preliminar pública y está disponible en las siguientes regiones de Azure: Este de Estados Unidos 1, East US 2, oeste UU.2, EE. UU., Norte CentralU S, Europa occidental, Europa del Norte, este de Australia, sudeste de Australia, sudeste de Asia, Japón oriental y centro de Corea

## <a name="known-limitations"></a>Limitaciones conocidas

| Problema | DESCRIPCIÓN |
| :---- | :--------- |
| El panel Administrar copias de seguridad de un servidor de SQL Database no muestra las bases de datos de hiperescala que se van a filtrar desde SQL server->  | Hiperescala tiene un método independiente para administrar las copias de seguridad y, por lo tanto, la configuración de la retención de copias de seguridad correspondiente a la retención a largo plazo y a un momento dado no se aplican o se invalidan. En consecuencia, las bases de datos de Hiperescala no aparecen en el panel Administración de copias de seguridad. |
| Restauración a un momento dado | Una vez que se migra una base de datos en el nivel de servicio Hiperescalado, no se admite la restauración a un momento dado antes de la migración.|
| Si un archivo de base de datos crece durante la migración debido a una carga de trabajo activa y cruza el límite de 1 TB por archivo, se produce un error en la migración. | Mitigaciones: <br> - Si es posible, debe migrar la base de datos cuando no haya ninguna carga de trabajo de actualización en ejecución.<br> - Vuelva a intentar la migración; se realizará correctamente siempre y cuando no se traspase el límite de 1 TB durante la migración.|
| Instancia administrada no se admite actualmente. | No se admite actualmente. |
| La migración a Hiperescala actualmente es una operación unidireccional. | Una vez que una base de datos se migra a Hiperescalado, no puede migrarse directamente a un nivel de servicio que no sea Hiperescalado. En este momento, la única forma de migrar una base de datos de Hiperescalado a otro nivel de servicio es con la exportación e importación mediante un archivo BACPAC.|
| En este momento, no se admite la migración de bases de datos con objetos en memoria. | Los objetos en memoria deben quitarse y volver a crearse como objetos que no sean en memoria antes de migrar una base de datos al nivel de servicio Hiperescalado.|
| Actualmente no se admite la opción para cambiar el seguimiento de datos. | No podrá usar la opción para cambiar el seguimiento de datos con las bases de datos de hiperescala.

## <a name="next-steps"></a>Pasos siguientes

- Para consultar preguntas frecuentes sobre Hiperescala, consulte [Preguntas más frecuentes acerca de Hiperescala](sql-database-service-tier-hyperscale-faq.md).
- Para más información sobre los niveles de servicio, consulte [Niveles de servicio](sql-database-purchase-models.md).
- Consulte [Overview of resource limits on a SQL Database server](sql-database-resource-limits-database-server.md) (Introducción a los límites de recursos de un servidor de SQL Database) para obtener información acerca de los límites en los niveles de servidor y suscripción.
- Para conocer los límites del modelo de compras para una base de datos única, consulte [Límites del modelo de compra basado en núcleos virtuales de Azure SQL Database para una base de datos única](sql-database-vcore-resource-limits-single-databases.md).
- Para obtener una lista de características y una comparación, consulte [Características comunes de SQL](sql-database-features.md).
