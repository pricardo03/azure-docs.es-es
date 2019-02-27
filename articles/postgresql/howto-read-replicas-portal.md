---
title: Administración de réplicas de lectura en Azure Portal para Azure Database for PostgreSQL
description: Aprenda a administrar réplicas de lectura de Azure Database for PostgreSQL desde Azure Portal.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 02/19/2019
ms.openlocfilehash: b34b103d3b710b90fd7b396f2c8d0e7adc27aaca
ms.sourcegitcommit: 75fef8147209a1dcdc7573c4a6a90f0151a12e17
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/20/2019
ms.locfileid: "56454674"
---
# <a name="create-and-manage-read-replicas-from-the-azure-portal"></a>Creación y administración de réplicas de lectura en Azure Portal

En este artículo, obtendrá información sobre cómo crear y administrar las réplicas de lectura en el servicio Azure Database for PostgreSQL mediante Azure Portal. Para más información acerca de las réplicas de lectura, consulte la [introducción](concepts-read-replicas.md).

> [!IMPORTANT]
> La característica de réplica de lectura está en versión preliminar pública.

## <a name="prerequisites"></a>Requisitos previos
Un [servidor de Azure Database for PostgreSQL](quickstart-create-server-database-portal.md) que se usará como servidor maestro.

## <a name="prepare-the-master-server"></a>Preparación del servidor maestro
Estos pasos se deben utilizar para preparar un servidor maestro en los niveles de uso general u optimizado para memoria.

El parámetro `azure.replication_support` debe establecerse en **REPLICA** en el servidor maestro. Cuando se cambia este parámetro, es necesario reiniciar el servidor para que el cambio surta efecto.

1. En Azure Portal, seleccione el servidor de Azure Database for PostgreSQL existente para utilizar como servidor maestro.

2. En el menú izquierdo, seleccione **Parámetros del servidor**.

3. Busque el parámetro `azure.replication_support`.

   ![Búsqueda del parámetro azure.replication_support](./media/howto-read-replicas-portal/azure-replication-parameter.png)

4. Establezca el valor del parámetro `azure.replication_support` en **REPLICA**. Haga clic en **Guardar** para guardar los cambios.

   ![Establezca el parámetro en REPLICA y guarde los cambios](./media/howto-read-replicas-portal/save-parameter-replica.png)

5. Después de guardar los cambios, recibirá una notificación:

   ![Guardado de la notificación](./media/howto-read-replicas-portal/parameter-save-notification.png)

6. Reinicie el servidor para aplicar los cambios. Para aprender a reiniciar un servidor, consulte [Reinicio de un servidor de Azure Database for PostgreSQL mediante Azure Portal](howto-restart-server-portal.md).


## <a name="create-a-read-replica"></a>Creación de una réplica de lectura
Para crear una réplica de lectura, siga estos pasos:

1.  Seleccione el servidor de Azure Database for PostgreSQL existente para utilizar como servidor maestro. 

2.  En el menú del servidor, en **CONFIGURACIÓN**, seleccione **Replicación**.

   Si no ha establecido el parámetro `azure.replication_support` en **REPLICA** en un servidor maestro de uso general u optimizado para memoria y no ha reiniciado el servidor, recibirá una notificación. Complete estos pasos antes de crear la réplica.

3.  Seleccione **Agregar réplica**.

   ![Adición de una réplica](./media/howto-read-replicas-portal/add-replica.png)

4.  Escriba un nombre para la réplica de lectura. Seleccione **Aceptar** para confirmar la creación de la réplica.

   ![Asignación de un nombre a la réplica](./media/howto-read-replicas-portal/name-replica.png) 

Las réplicas se crean con la misma configuración de servidor que el servidor maestro. Después de crear una réplica, se pueden cambiar varias configuraciones independientemente del servidor maestro: generación de proceso, núcleos virtuales, almacenamiento y período de retención de copia de seguridad. El plan de tarifa también se puede modificar independientemente, excepto desde el nivel Básico.

> [!IMPORTANT]
> Antes de actualizar la configuración de un servidor maestro con nuevos valores, actualice la configuración de las réplicas a valores iguales o mayores. Esta acción garantiza que la réplica puede hacer frente a los cambios realizados en el servidor maestro.

Después de crear la réplica de lectura, puede verla en la ventana **Replicación**:

![Visualización de la nueva réplica en la ventana Replicación](./media/howto-read-replicas-portal/list-replica.png)
 

## <a name="stop-replication"></a>Detención replicación
Puede detener la replicación entre un servidor maestro y una réplica de lectura.

> [!IMPORTANT]
> Después de detener la replicación en un servidor maestro y en una réplica de lectura, este proceso no se puede deshacer. La réplica de lectura se convierte en un servidor independiente que admite operaciones de lectura y escritura. Este servidor independiente no puede volver a convertirse en una réplica.

Para detener la replicación entre un servidor maestro y una réplica de lectura desde Azure Portal, siga estos pasos:

1.  En Azure Portal, seleccione el servidor maestro de Azure Database for PostgreSQL.

2.  En el menú del servidor, en **CONFIGURACIÓN**, seleccione **Replicación**.

3.  Seleccione el servidor de réplica para el que desea detener la replicación.

   ![Selección de la réplica](./media/howto-read-replicas-portal/select-replica.png)
 
4.  Seleccione **Detener replicación**.

   ![Selección de Detener replicación](./media/howto-read-replicas-portal/select-stop-replication.png)
 
5.  Seleccione **Aceptar** para detener la replicación.

   ![Confirmación de la detención de la replicación](./media/howto-read-replicas-portal/confirm-stop-replication.png)
 

## <a name="delete-a-master-server"></a>Eliminación de un servidor maestro
Para eliminar un servidor maestro, se usan los mismos pasos que para eliminar un servidor de Azure Database for PostgreSQL independiente. 

> [!IMPORTANT]
> Cuando se elimina un servidor maestro, la replicación se detiene en todas las réplicas de lectura. Las réplicas de lectura se convierten en servidores independientes que ahora admiten tanto lectura como escritura.

Para eliminar un servidor de Azure Portal, siga estos pasos:

1.  En Azure Portal, seleccione el servidor maestro de Azure Database for PostgreSQL.

2.  Abra la página **Introducción** del servidor. Seleccione **Eliminar**.

   ![En la página Introducción del servidor, selección de Eliminar para eliminar el servidor maestro](./media/howto-read-replicas-portal/delete-server.png)
 
3.  Escriba el nombre del servidor maestro que desea eliminar. Seleccione **Eliminar** para confirmar la eliminación del servidor maestro.

   ![Confirmación de la eliminación del servidor maestro](./media/howto-read-replicas-portal/confirm-delete.png)
 

## <a name="delete-a-replica"></a>Eliminación de una réplica
Puede eliminar una réplica de lectura similar a cómo eliminar un servidor maestro.

- En Azure Portal, abra la página **Introducción** para la réplica de lectura. Seleccione **Eliminar**.

   ![En la página Introducción de la réplica, selección de Eliminar para eliminar la réplica](./media/howto-read-replicas-portal/delete-replica.png)
 
También puede eliminar la réplica de lectura desde la ventana **Replicación** siguiendo estos pasos:

1.  En Azure Portal, seleccione el servidor maestro de Azure Database for PostgreSQL.

2.  En el menú del servidor, en **CONFIGURACIÓN**, seleccione **Replicación**.

3.  Seleccione la réplica de lectura que desea eliminar.

   ![Selección de la réplica para eliminar](./media/howto-read-replicas-portal/select-replica.png)
 
4.  Seleccione **Eliminar réplica**.

   ![Selección de Eliminar réplica](./media/howto-read-replicas-portal/select-delete-replica.png)
 
5.  Escriba el nombre de la réplica que quiere eliminar. Seleccione **Eliminar** para confirmar la eliminación de la réplica.

   ![Confirmación de la eliminación de la réplica](./media/howto-read-replicas-portal/confirm-delete-replica.png)
 

## <a name="monitor-a-replica"></a>Supervisión de una réplica
Están disponibles dos métricas para supervisar las réplicas de lectura.

### <a name="max-lag-across-replicas-metric"></a>Métrica de retraso máximo entre réplicas
La métrica **Max Lag Across Replicas** (Retraso máximo entre réplicas) muestra el retardo en bytes entre la réplica con mayor retardo y el servidor maestro. 

1.  En Azure Portal, seleccione el servidor maestro de Azure Database for PostgreSQL.

2.  Seleccione **Métricas**. En la ventana de **métricas**, seleccione **Max Lag Across Replicas** (Retraso máximo entre réplicas).

    ![Supervisión del retraso máximo entre réplicas](./media/howto-read-replicas-portal/select-max-lag.png)
 
3.  En **Agregación**, seleccione **Máx**.


### <a name="replica-lag-metric"></a>Métrica de retraso de réplica
La métrica **Replica Lag** (Retraso entre réplicas) muestra el tiempo desde la última transacción reproducida en esta réplica. Si no se produce ninguna transacción en el servidor maestro, la métrica refleja este retardo de tiempo.

1.  En Azure Portal, seleccione la réplica de lectura de Azure Database for PostgreSQL.

2.  Seleccione **Métricas**. En la ventana de **métricas**, seleccione **Replica Lag** (Retraso entre réplicas).

   ![Supervisión del retraso de réplica](./media/howto-read-replicas-portal/select-replica-lag.png)
 
3.  En **Agregación**, seleccione **Máx**. 
 
## <a name="next-steps"></a>Pasos siguientes
Más información sobre las [réplicas de lectura en Azure Database for PostgreSQL](concepts-read-replicas.md).