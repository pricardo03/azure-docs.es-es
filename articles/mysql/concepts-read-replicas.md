---
title: Réplicas de lectura en Azure Database for MySQL
description: En este artículo se describen las réplicas de lectura para Azure Database for MySQL.
services: mysql
author: ajlam
ms.author: andrela
editor: jasonwhowell
ms.service: mysql
ms.topic: conceptual
ms.date: 10/30/2018
ms.openlocfilehash: b4e79723072a19f2637bea16d0534cb85588e9e3
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/31/2018
ms.locfileid: "50412455"
---
# <a name="read-replicas-in-azure-database-for-mysql"></a>Réplicas de lectura en Azure Database for MySQL

La característica de réplica de lectura (versión preliminar pública) le permite replicar datos desde un servidor Azure Database for MySQL (maestro) en hasta un máximo de cinco servidores de solo lectura (réplicas) de la misma región de Azure. Las réplicas de solo lectura se actualizan asincrónicamente mediante la tecnología de replicación basada en la posición de los archivos de registros binarios nativos (binlog) del motor de MySQL. Para obtener más información acerca de la replicación de binlog, consulte la [Introducción a la replicación de binlog de MySQL](https://dev.mysql.com/doc/refman/5.7/en/binlog-replication-configuration-overview.html).

Las réplicas creadas en el servicio Azure Database for MySQL son nuevos servidores que pueden administrarse de la misma manera que los servidores de MySQL independientes o los habituales. Estos servidores se cargan a la misma velocidad que un servidor independiente.

Para obtener más información sobre los problemas y las características de replicación de MySQL, consulte la [documentación de replicación de MySQL](https://dev.mysql.com/doc/refman/5.7/en/replication-features.html).

## <a name="when-to-use-read-replicas"></a>Casos en los que utilizar las réplicas de lectura

Las réplicas de solo lectura pueden atender las aplicaciones y cargas de trabajo que se leen con mucha frecuencia. Las réplicas de lectura contribuyen a aumentar la capacidad de lectura disponible en comparación con la que tendría si usara simplemente un único servidor para lectura y escritura. Las cargas de trabajo de lectura se pueden aislar en las réplicas, mientras que las cargas de trabajo de escritura se pueden dirigir al servidor maestro.

Un caso habitual es que las cargas de trabajo de análisis e inteligencia empresarial utilicen la réplica de lectura como origen de datos para los informes.

## <a name="considerations-and-limitations"></a>Consideraciones y limitaciones

### <a name="pricing-tiers"></a>Planes de tarifa

Actualmente, las réplicas de lectura solo están disponibles en los planes de tarifa de uso general y optimizados para memoria.

### <a name="master-server-restart"></a>Reinicio del servidor maestro

Durante esta versión preliminar, cuando se crea una réplica para un servidor maestro que no tiene réplicas existentes, el maestro se reiniciará en primer lugar para prepararse para la replicación. Téngalo en cuenta y realice estas operaciones durante un período de poca actividad.

### <a name="stopping-replication"></a>Detención de la replicación

Puede detener la replicación entre un servidor maestro y un servidor de réplica. Al detener la replicación, se elimina la relación de replicación entre el servidor maestro y el de réplica.

Una vez que se ha detenido la replicación, el servidor de réplica se convierte en un servidor independiente. Los datos del servidor independiente son los datos que estaban disponibles en la réplica en el momento en que se inició el comando "detener la replicación". El servidor independiente no alcanza al servidor maestro. Este servidor no puede volver a convertirse en una réplica.

### <a name="replicas-are-new-servers"></a>Las réplicas son servidores nuevos

Las réplicas se crean como nuevos servidores de Azure Database for MySQL. Los servidores existentes no pueden convertirse en réplicas.

### <a name="replica-server-configuration"></a>Configuración de los servidores de réplica

Los servidores de réplica se crean utilizando las mismas opciones de configuración de servidor que el servidor maestro, que incluye las siguientes:

- Plan de tarifa
- Generación de procesos
- Núcleos virtuales
- Storage
- Período de retención de copia de seguridad
- Opción de redundancia de copia de seguridad
- Versión del motor MySQL

Una vez creada una réplica, puede cambiar el plan de tarifa (excepto hacia Basic y desde Basic), la generación informática, los núcleos virtuales, el almacenamiento y la retención de copias de seguridad por separado del servidor maestro.

### <a name="master-server-configuration"></a>Configuración del servidor maestro

Si se actualiza la configuración del servidor maestro (por ejemplo, los núcleos virtuales o el almacenamiento), la configuración de las réplicas también debe actualizarse a valores iguales o superiores. Sin esto, el servidor de réplicas no podrá mantener los cambios realizados en el servidor maestro y es posible que se bloqueen. 

### <a name="deleting-the-master-server"></a>Eliminación del servidor maestro

Al eliminar el servidor maestro, la replicación se detiene en todas las réplicas de lectura. Estas réplicas se convierten en servidores independientes. El propio servidor maestro se elimina.

### <a name="user-accounts"></a>Cuentas de usuario

Los usuarios del servidor principal se replican en las réplicas de lectura. Solo se puede conectar a una réplica de lectura utilizando las cuentas de usuario disponibles en el servidor maestro.

### <a name="other"></a>Otros

- No se admiten identificadores de transacción global (GTID).
- No permite crear réplicas de réplicas.
- Las tablas en memoria pueden provocar que las réplicas dejen de sincronizarse. Esto es una limitación de la tecnología de replicación de MySQL. Puede obtener más información en la [documentación de referencia de MySQL](https://dev.mysql.com/doc/refman/5.7/en/replication-features-memory.html).
- El hecho de ajustar el parámetro [`innodb_file_per_table`](https://dev.mysql.com/doc/refman/5.7/en/innodb-multiple-tablespaces.html) en un servidor maestro después de crear un servidor de réplica puede provocar que la réplica deje de sincronizarse. El servidor de réplica no tiene presente los espacios de tablas diferentes.
- Consulte la lista completa de las limitaciones de la replicación de MySQL en la [documentación de MySQL](https://dev.mysql.com/doc/refman/5.7/en/replication-features.html).


## <a name="next-steps"></a>Pasos siguientes

- Obtenga información sobre cómo [crear y administrar réplicas de lectura mediante Azure Portal](howto-read-replicas-portal.md).

<!--
- Learn how to [create and manage read replicas using the Azure CLI](howto-read-replicas-using-cli.md)
-->
