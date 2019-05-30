---
title: Réplicas de lectura en Azure Database for MySQL
description: En este artículo se describen las réplicas de lectura para Azure Database for MySQL.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 04/30/2019
ms.openlocfilehash: 2d70e1b5434b2fb263d1f4587888d4758fac2828
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66225357"
---
# <a name="read-replicas-in-azure-database-for-mysql"></a>Réplicas de lectura en Azure Database for MySQL

La característica de réplica de lectura le permite replicar datos desde un servidor Azure Database for MySQL a un servidor de solo lectura. Puede replicar desde el servidor maestro para hasta cinco réplicas. Las réplicas se actualizan de forma asincrónica mediante tecnología de replicación basada en la posición de archivos de registro binario nativo (binlog) del motor de MySQL. Para obtener más información acerca de la replicación de binlog, consulte la [Introducción a la replicación de binlog de MySQL](https://dev.mysql.com/doc/refman/5.7/en/binlog-replication-configuration-overview.html).

> [!IMPORTANT]
> Puede crear una réplica de lectura en la misma región que el servidor maestro o en cualquier otra región de Azure de su elección. La replicación entre regiones está actualmente en versión preliminar pública.

Las réplicas son nuevos servidores que administrar similar a normal-Azure Database for MySQL Server. En cada réplica de lectura, se le cobra por el proceso aprovisionado en núcleos virtuales y el almacenamiento aprovisionado en GB/mes.

Para obtener más información sobre los problemas y las características de replicación de MySQL, consulte la [documentación de replicación de MySQL](https://dev.mysql.com/doc/refman/5.7/en/replication-features.html).

## <a name="when-to-use-a-read-replica"></a>Casos en los que utilizar las réplicas de lectura

La finalidad de la característica de réplica de lectura es ayudar a mejorar el rendimiento y la escala de las cargas de trabajo que conllevan un gran número de operaciones de lectura. Las cargas de trabajo de lectura se pueden aislar en las réplicas, mientras que las cargas de trabajo de escritura se pueden dirigir al servidor maestro.

Un caso habitual es que las cargas de trabajo de análisis e inteligencia empresarial utilicen la réplica de lectura como origen de datos para los informes.

Dado que las réplicas son de solo lectura, no reducen directamente las cargas de capacidad de escritura en el servidor maestro. Esta característica no está destinada a cargas de trabajo intensivas de escritura.

La característica de réplica de lectura utiliza replicación asincrónica de MySQL. La característica no está diseñada para escenarios de replicación sincrónica. Habrá un retraso medible entre el servidor maestro y la réplica. Los datos de la réplica se vuelven finalmente coherentes con los datos del servidor maestro. Use esta característica con cargas de trabajo que puedan admitir este retraso.

Las réplicas de lectura pueden mejorar el plan de recuperación ante desastres. Si hay un desastre regional y el servidor maestro no está disponible, puede dirigir la carga de trabajo a una réplica en otra región. Para ello, primero permiten la réplica Aceptar operaciones de escritura mediante la función de replicación de detención. A continuación, puede redirigir la aplicación mediante la actualización de la cadena de conexión. Obtenga más información en el [detener la replicación](#stop-replication) sección.

## <a name="create-a-replica"></a>Creación de una réplica

Si un servidor maestro no tiene ningún servidor de réplica existente, el patrón se reiniciará en primer lugar para prepararse para la replicación.

Cuando se inicia el flujo de trabajo crear réplica, se crea una base de datos de Azure en blanco para el servidor MySQL. El nuevo servidor se rellena con los datos que estaban en el servidor maestro. El tiempo que se tarda en crear la réplica depende de la cantidad de datos en el servidor maestro y del tiempo desde la última copia de seguridad completa semanal. Puede oscilar desde unos minutos hasta varias horas.

Cada réplica está habilitada para el almacenamiento [crecimiento automático](concepts-pricing-tiers.md#storage-auto-grow). La característica de crecimiento permite a la réplica a mantenerse al día con los datos replicados a él y evitar una interrupción en la replicación causada por errores de almacenamiento insuficiente.

Aprenda a [crear una réplica de lectura en Azure Portal](howto-read-replicas-portal.md).

## <a name="connect-to-a-replica"></a>Conexión a una réplica

Al crear una réplica, no se heredan las reglas de firewall o el punto de conexión de servicio de red virtual del servidor maestro. Estas reglas se deben configurar de forma independiente para la réplica.

La réplica hereda su cuenta de administrador del servidor maestro. Todas las cuentas de usuario existentes en el servidor se replican en las réplicas de lectura. Solo se puede conectar a una réplica de lectura utilizando las cuentas de usuario disponibles en el servidor maestro.

Puede conectarse a la réplica mediante su nombre de host y una cuenta de usuario válida, como lo haría en una base de datos normal de Azure para el servidor MySQL. Para un servidor denominado **myreplica** con el nombre de usuario administrador **myadmin**, puede conectarse a la réplica mediante el uso de la CLI de mysql:

```bash
mysql -h myreplica.mysql.database.azure.com -u myadmin@myreplica -p
```

Cuando se le solicite, escriba la contraseña de la cuenta de usuario.

## <a name="monitor-replication"></a>Supervisión de la replicación

Azure Database for MySQL proporciona el **retraso de replicación en segundos** métricas en Azure Monitor. Esta métrica está disponible únicamente para las réplicas.

Esta métrica se calcula utilizando el `seconds_behind_master` métrica disponible en MySQL `SHOW SLAVE STATUS` comando.

Establecer una alerta que le informa cuando el retraso de replicación alcanza un valor que no es aceptable para la carga de trabajo.

## <a name="stop-replication"></a>Detención replicación

Puede decidir detener la replicación entre un servidor maestro y una réplica. Una vez que se ha detenido la replicación entre un servidor maestro y una réplica de lectura, la réplica se convierte en un servidor independiente. Los datos del servidor independiente son los datos que estaban disponibles en la réplica en el momento en que se inició el comando para detener la replicación. El servidor independiente no alcanza al servidor maestro.

Si decide detener la replicación en una réplica, pierde todos los vínculos a su principal anterior y otras réplicas. No hay ninguna conmutación por error automatizada entre un servidor maestro y su réplica.

> [!IMPORTANT]
> Este servidor independiente no puede volver a convertirse en una réplica.
> Asegúrese de que la réplica tenga todos los datos que necesita antes de detener la replicación en una réplica de lectura.

Aprenda a [detener la replicación en una réplica](howto-read-replicas-portal.md).

## <a name="considerations-and-limitations"></a>Consideraciones y limitaciones

### <a name="pricing-tiers"></a>Planes de tarifa

Actualmente, las réplicas de lectura solo están disponibles en los planes de tarifa de uso general y optimizados para memoria.

### <a name="master-server-restart"></a>Reinicio del servidor maestro

Cuando se crea una réplica para un servidor maestro que no tiene ninguna réplica existente, el patrón se reiniciará en primer lugar para prepararse para la replicación. Téngalo en cuenta y realice estas operaciones durante un período de poca actividad.

### <a name="new-replicas"></a>Nuevas réplicas

Se crea una réplica de lectura como un nuevo servidor Azure Database for MySQL. Un servidor no puede volver a convertirse en una réplica. No se puede crear una réplica de otra réplica de lectura.

### <a name="replica-configuration"></a>Configuración de réplicas

Las réplicas se crean con la misma configuración de servidor que el servidor maestro. Después de crea una réplica, se pueden cambiar varias configuraciones de forma independiente desde el servidor maestro: proceso de generación, núcleos virtuales, almacenamiento, período de retención de copia de seguridad y versión del motor de MySQL. El plan de tarifa también se puede cambiar de forma independiente, excepto si es con origen o destino en el nivel Básico.

> [!IMPORTANT]
> Antes de actualizar la configuración de un servidor maestro con nuevos valores, actualice la configuración de las réplicas a valores iguales o mayores. Esta acción garantiza que la réplica puede hacer frente a los cambios realizados en el servidor maestro.

### <a name="stopped-replicas"></a>Réplicas detenidas

Si detiene la replicación entre un servidor principal y una réplica de lectura, la réplica detenida se convierte en un servidor independiente que acepta las lecturas y escrituras. Este servidor independiente no puede volver a convertirse en una réplica.

### <a name="deleted-master-and-standalone-servers"></a>Servidores maestro e independiente eliminados

Al eliminar el servidor maestro, la replicación se detiene en todas las réplicas de lectura. Estas réplicas se convierten en servidores independientes. El propio servidor maestro se elimina.

### <a name="user-accounts"></a>Cuentas de usuario

Los usuarios del servidor principal se replican en las réplicas de lectura. Solo se puede conectar a una réplica de lectura utilizando las cuentas de usuario disponibles en el servidor maestro.

### <a name="server-parameters"></a>Parámetros del servidor

Para evitar que los datos deje de estar sincronizados y evitar posibles pérdidas o daños, se bloquean algunos parámetros del servidor se actualice al usar réplicas de lectura.

Los siguientes parámetros de servidor están bloqueados en servidores maestro y de réplica:
- [`innodb_file_per_table`](https://dev.mysql.com/doc/refman/5.7/en/innodb-multiple-tablespaces.html) 
- [`log_bin_trust_function_creators`](https://dev.mysql.com/doc/refman/5.7/en/replication-options-binary-log.html#sysvar_log_bin_trust_function_creators)

El [ `event_scheduler` ](https://dev.mysql.com/doc/refman/5.7/en/server-system-variables.html#sysvar_event_scheduler) parámetro está bloqueado en los servidores de réplica. 

### <a name="other"></a>Otros

- No se admiten identificadores de transacción global (GTID).
- No permite crear réplicas de réplicas.
- Las tablas en memoria pueden provocar que las réplicas dejen de sincronizarse. Esto es una limitación de la tecnología de replicación de MySQL. Puede obtener más información en la [documentación de referencia de MySQL](https://dev.mysql.com/doc/refman/5.7/en/replication-features-memory.html).
- Asegúrese de que las tablas del servidor maestro tienen claves principales. La falta de claves principales puede generar una latencia de replicación entre la tabla principal y la réplica.
- Consulte la lista completa de las limitaciones de la replicación de MySQL en la [documentación de MySQL](https://dev.mysql.com/doc/refman/5.7/en/replication-features.html).

## <a name="next-steps"></a>Pasos siguientes

- Obtenga información sobre cómo [crear y administrar réplicas de lectura mediante Azure Portal](howto-read-replicas-portal.md).
- Obtenga información sobre cómo [crear y administrar réplicas de lectura mediante la CLI de Azure](howto-read-replicas-cli.md).
