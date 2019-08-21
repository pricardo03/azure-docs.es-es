---
title: 'Réplicas de lectura en Azure Database for PostgreSQL: servidor único'
description: En este artículo se describe la característica de réplica de lectura de Azure Database for PostgreSQL de servidor único.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 08/12/2019
ms.openlocfilehash: 928a85c9d03148198fe3e965636740812ce732f7
ms.sourcegitcommit: 62bd5acd62418518d5991b73a16dca61d7430634
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2019
ms.locfileid: "68976281"
---
# <a name="read-replicas-in-azure-database-for-postgresql---single-server"></a>Réplicas de lectura en Azure Database for PostgreSQL: servidor único

La característica de réplica de lectura permite replicar datos de un servidor Azure Database for PostgreSQL en un servidor de solo lectura. Puede crear hasta cinco réplicas desde el servidor maestro. Las réplicas se actualizan de manera asincrónica mediante la tecnología de replicación nativa del motor de PostgreSQL.

Las réplicas son nuevos servidores que se administran de forma similar a los servidores de Azure Database for PostgreSQL normales. En cada réplica de lectura, se le cobra por el proceso aprovisionado en núcleos virtuales y el almacenamiento aprovisionado en GB/mes.

Vea cómo [crear y administrar réplicas](howto-read-replicas-portal.md).

## <a name="when-to-use-a-read-replica"></a>Casos en los que utilizar las réplicas de lectura
La finalidad de la característica de réplica de lectura es ayudar a mejorar el rendimiento y la escala de las cargas de trabajo que conllevan un gran número de operaciones de lectura. Las cargas de trabajo de lectura se pueden aislar en las réplicas, mientras que las cargas de trabajo de escritura se pueden dirigir al servidor maestro.

Un caso habitual es que las cargas de trabajo de análisis e inteligencia empresarial utilicen la réplica de lectura como origen de datos para los informes.

Dado que las réplicas son de solo lectura, no reducen directamente las cargas de capacidad de escritura en el servidor maestro. Esta característica no está destinada a cargas de trabajo intensivas de escritura.

Esta característica de réplica de lectura utiliza la replicación asincrónica nativa de PostgreSQL. La característica no está diseñada para escenarios de replicación sincrónica. Habrá un retraso medible entre el servidor maestro y la réplica. Los datos de la réplica se vuelven finalmente coherentes con los datos del servidor maestro. Use esta característica con cargas de trabajo que puedan admitir este retraso.

## <a name="cross-region-replication"></a>Replicación entre regiones
Puede crear una réplica de lectura en una región distinta del servidor maestro. La replicación entre regiones puede ser útil para escenarios como el planeamiento de la recuperación ante desastres o la incorporación de datos más cerca de los usuarios.

> [!IMPORTANT]
> La replicación entre regiones se encuentra actualmente en versión preliminar pública.

Puede tener un servidor maestro en cualquier [región de Azure Database for PostgreSQL](https://azure.microsoft.com/global-infrastructure/services/?products=postgresql).  Un servidor maestro puede tener una réplica emparejada en su región o en las regiones de la réplica universal.

### <a name="universal-replica-regions"></a>Regiones de réplica universal
Siempre puede crear una réplica de lectura en cualquiera de las siguientes regiones, con independencia de dónde se encuentre el servidor maestro. Estas son las regiones de réplica universal:

Este de Australia, Sudeste de Australia, Centro de EE. UU., Asia Oriental, Este de EE. UU. 2, Japón Oriental, Japón Occidental, Centro de Corea del Sur, Sur de Corea del Sur, Centro y norte de EE. UU., Norte de Europa, Centro y Sur de EE. UU., Asia Suroriental, Sur de Reino Unido, Oeste de Reino Unido, Oeste de Europa, Oeste de EE. UU., Oeste de EE. UU. 2.


### <a name="paired-regions"></a>Regiones emparejadas
Además de las regiones de réplica universal, puede crear una réplica de lectura en la región emparejada de Azure del servidor maestro. Si no conoce el par de la región, puede obtener más información en el [artículo sobre regiones emparejadas de Azure](https://docs.microsoft.com/azure/best-practices-availability-paired-regions).

Si usa réplicas entre regiones para planear la recuperación ante desastres, se recomienda que cree la réplica en la región emparejada en lugar de en una de las otras regiones. Las regiones emparejadas evitan actualizaciones simultáneas y priorizan el aislamiento físico y la residencia de datos.  

Sin embargo, existen limitaciones que deben considerarse: 

* Disponibilidad regional: Azure Database for PostgreSQL está disponible en Oeste de EE. UU. 2, Centro de Francia, Norte de Emiratos Árabes Unidos y Centro de Alemania. Sin embargo, sus regiones emparejadas no están disponibles.
    
* Pares unidireccionales: Algunas regiones de Azure se emparejan solo en una dirección. Estas regiones incluyen Oeste de la India, Sur de Brasil y US Gov Virginia. 
   Esto significa que un servidor maestro de Oeste de la India puede crear una réplica en India meridional. Sin embargo, un servidor maestro de India del Sur no puede crear una réplica en India occidental. Esto es debido a que la región secundaria del Oeste de la India es India meridional, pero la región secundaria de esta última no es India occidental.


## <a name="create-a-replica"></a>Creación de una réplica
El servidor maestro debe tener el `azure.replication_support` establecido en **RÉPLICA**. Cuando se cambia este parámetro, es necesario reiniciar el servidor para que el cambio surta efecto. Este parámetro `azure.replication_support` se aplica únicamente a los niveles De uso General y Optimizado para memoria.

Cuando se inicia el flujo de trabajo de creación de la réplica, se crea un servidor Azure Database for PostgreSQL en blanco. El nuevo servidor se rellena con los datos que estaban en el servidor maestro. El tiempo que se tarda en crear la réplica depende de la cantidad de datos en el servidor maestro y del tiempo desde la última copia de seguridad completa semanal. Puede oscilar desde unos minutos hasta varias horas.

Cada réplica se habilita para el [crecimiento automático](concepts-pricing-tiers.md#storage-auto-grow) del almacenamiento. La característica de crecimiento automático permite a la réplica mantenerse al día con los datos replicados en ella y evitar una interrupción en la replicación causada por errores de almacenamiento insuficiente.

La característica de réplica de lectura usa la replicación física de PostgreSQL (replicación no lógica). El modo de funcionamiento predeterminado es la transmisión de la replicación mediante espacios de replicación. Cuando es necesario, se usa el trasvase de registros para actualizarse.

Aprenda a [crear una réplica de lectura en Azure Portal](howto-read-replicas-portal.md).

## <a name="connect-to-a-replica"></a>Conexión a una réplica
Al crear una réplica, no se heredan las reglas de firewall o el punto de conexión de servicio de red virtual del servidor maestro. Estas reglas se deben configurar de forma independiente para la réplica.

La réplica hereda su cuenta de administrador del servidor maestro. Todas las cuentas de usuario existentes en el servidor se replican en las réplicas de lectura. Solo se puede conectar a una réplica de lectura utilizando las cuentas de usuario disponibles en el servidor maestro.

Puede conectarse a la réplica mediante su nombre de host y una cuenta de usuario válida, igual que haría en un servidor Azure Database for PostgreSQL normal. En un servidor denominado **myreplica** con el nombre de usuario administrador **myadmin**, puede conectarse a la réplica mediante psql:

```
psql -h myreplica.postgres.database.azure.com -U myadmin@myreplica -d postgres
```

Cuando se le solicite, escriba la contraseña de la cuenta de usuario.

## <a name="monitor-replication"></a>Supervisión de la replicación
Azure Database for PostgreSQL proporciona dos métricas para supervisar la replicación. Las dos métricas son **Max Lag Across Replicas** (Retraso máximo entre réplicas) y **Replica Lag** (Retraso entre réplicas). Para obtener información sobre cómo ver estas métricas, consulte la sección **Supervisión de una réplica** del [artículo de procedimientos de réplica de lectura](howto-read-replicas-portal.md).

La métrica **Retraso máximo entre réplicas** muestra el retardo en bytes entre la réplica con mayor retardo y el servidor maestro. Esta métrica está disponible únicamente en el servidor maestro.

La métrica **Replica Lag** muestra el tiempo desde la última transacción reproducida. Si no se produce ninguna transacción en el servidor maestro, la métrica refleja este retardo de tiempo. Esta métrica está disponible únicamente para los servidores de réplica. El retraso entre réplicas se calcula desde la vista `pg_stat_wal_receiver`:

```SQL
EXTRACT (EPOCH FROM now() - pg_last_xact_replay_timestamp());
```

Establezca una alerta que le informe cuando el retardo de la réplica alcance un valor que no sea aceptable para la carga de trabajo. 

Para más información, consulte el servidor maestro directamente para obtener el retardo de replicación en bytes en todas las réplicas.

En la versión 10 de PostgreSQL:

```SQL
select pg_wal_lsn_diff(pg_current_wal_lsn(), stat.replay_lsn) 
AS total_log_delay_in_bytes from pg_stat_replication;
```

En la versión 9.6 y versiones anteriores de PostgreSQL:

```SQL
select pg_xlog_location_diff(pg_current_xlog_location(), stat.replay_location) 
AS total_log_delay_in_bytes from pg_stat_replication;
```

> [!NOTE]
> Si se reinicia un servidor maestro o una réplica de lectura, el tiempo que tarda en reiniciarse y ponerse al día se reflejará en la métrica de retardo de la réplica.

## <a name="stop-replication"></a>Detención replicación
Puede decidir detener la replicación entre un servidor maestro y una réplica. La acción de detención reinicia la réplica para quitar su configuración de replicación. Una vez que se ha detenido la replicación entre un servidor maestro y una réplica de lectura, la réplica se convierte en un servidor independiente. Los datos del servidor independiente son los datos que estaban disponibles en la réplica en el momento en que se inició el comando para detener la replicación. El servidor independiente no alcanza al servidor maestro.

> [!IMPORTANT]
> Este servidor independiente no puede volver a convertirse en una réplica.
> Asegúrese de que la réplica tenga todos los datos que necesita antes de detener la replicación en una réplica de lectura.

Al detener la replicación, la réplica pierde todos los vínculos con su servidor maestro anterior y otras réplicas. No se produce ninguna conmutación por error automatizada entre un servidor maestro y la réplica. 

Aprenda a [detener la replicación en una réplica](howto-read-replicas-portal.md).


## <a name="considerations"></a>Consideraciones

En esta sección se resumen las consideraciones sobre la característica de réplica de lectura.

### <a name="prerequisites"></a>Requisitos previos
El parámetro `azure.replication_support` debe establecerse en **RÉPLICA** en el servidor maestro antes de crear una réplica de lectura. Cuando se cambia este parámetro, es necesario reiniciar el servidor para que el cambio surta efecto. El parámetro `azure.replication_support` se aplica únicamente a los niveles De uso General y Optimizado para memoria.

### <a name="new-replicas"></a>Nuevas réplicas
Las réplicas de lectura se crean como nuevos servidores Azure Database for PostgreSQL. Un servidor no puede volver a convertirse en una réplica. No se puede crear una réplica de otra réplica de lectura.

### <a name="replica-configuration"></a>Configuración de réplicas
Las réplicas se crean con la misma configuración de servidor que el servidor maestro. Después de crear una réplica, se pueden cambiar varias configuraciones independientemente del servidor maestro: generación de proceso, núcleos virtuales, almacenamiento y período de retención de copia de seguridad. El plan de tarifa también se puede cambiar de forma independiente, excepto si es con origen o destino en el nivel Básico.

> [!IMPORTANT]
> Antes de actualizar la configuración de un servidor maestro con nuevos valores, actualice la configuración de las réplicas a valores iguales o mayores. Esta acción garantiza que la réplica puede hacer frente a los cambios realizados en el servidor maestro.

PostgreSQL requiere que el valor del parámetro `max_connections` en la réplica de lectura sea mayor o igual que el valor principal; en caso contrario, no se iniciará la réplica. En Azure Database for PostgreSQL, el valor del parámetro `max_connections` se basa en la SKU. Para obtener más información, consulte el artículo de [límites de Azure Database for PostgreSQL](concepts-limits.md). 

Si trata de actualizar los valores del servidor, pero no cumple los límites, recibirá un error.

### <a name="max_prepared_transactions"></a>max_prepared_transactions
[PostgreSQL requiere](https://www.postgresql.org/docs/10/runtime-config-resource.html#GUC-MAX-PREPARED-TRANSACTIONS) que el valor del parámetro `max_prepared_transactions` en la réplica de lectura sea mayor o igual que el valor principal; en caso contrario, no se iniciará la réplica. Si quiere cambiar `max_prepared_transactions` en el servidor maestro, primero cámbielo en las réplicas.

### <a name="stopped-replicas"></a>Réplicas detenidas
Si decide detener la replicación entre un servidor maestro y una réplica de lectura, esta se reiniciará para aplicar el cambio. La réplica detenida se convierte en un servidor independiente que acepta las lecturas y escrituras. Este servidor independiente no puede volver a convertirse en una réplica.

### <a name="deleted-master-and-standalone-servers"></a>Servidores maestro e independiente eliminados
Cuando se elimina un servidor maestro, todas sus réplicas de lectura se convierten en servidores independientes. Las réplicas se reiniciarán para reflejar este cambio.

## <a name="next-steps"></a>Pasos siguientes
Aprenda a [crear y administrar réplicas de lectura en Azure Portal](howto-read-replicas-portal.md).
