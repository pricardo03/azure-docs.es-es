---
title: Administración de réplicas de lectura en Azure Portal para Azure Database for PostgreSQL
description: Aprenda a administrar réplicas de lectura de Azure Database for PostgreSQL desde Azure Portal.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 04/01/2019
ms.openlocfilehash: bf1fb1c1343173949ecb6348284cb537282b277b
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "59787670"
---
# <a name="create-and-manage-read-replicas-from-the-azure-portal"></a>Creación y administración de réplicas de lectura en Azure Portal

En este artículo, obtendrá información sobre cómo crear y administrar las réplicas de lectura en el servicio Azure Database for PostgreSQL mediante Azure Portal. Para más información acerca de las réplicas de lectura, consulte la [introducción](concepts-read-replicas.md).


## <a name="prerequisites"></a>Requisitos previos
Un [servidor de Azure Database for PostgreSQL](quickstart-create-server-database-portal.md) que se usará como servidor maestro.

## <a name="prepare-the-master-server"></a>Preparación del servidor maestro
Estos pasos se deben utilizar para preparar un servidor maestro en los niveles de uso general u optimizado para memoria. El servidor maestro está preparado para la replicación, establezca el parámetro azure.replication_support. Cuando se cambia el parámetro de replicación, un reinicio del servidor es necesario para que el cambio surta efecto. En el portal de Azure, estos dos pasos están encapsulados en un solo botón, **habilitar la compatibilidad con replicación**.

1. En Azure Portal, seleccione el servidor de Azure Database for PostgreSQL existente para utilizar como servidor maestro.

2. En la barra lateral del servidor, bajo **configuración**, seleccione **replicación**.

3. Seleccione **habilitar la compatibilidad con replicación**. 

   ![Habilitar la compatibilidad con la replicación](./media/howto-read-replicas-portal/enable-replication-support.png)

4. Confirme que desea habilitar la compatibilidad con la replicación. Esta operación reiniciará el servidor maestro. 

   ![Confirmar la compatibilidad de habilitar la replicación](./media/howto-read-replicas-portal/confirm-enable-replication.png)
   
5. Recibirá dos notificaciones de Azure portal una vez completada la operación. Hay una notificación para actualizar el parámetro de servidor. Hay otra notificación para el reinicio del servidor que sigue inmediatamente.

   ![Habilitar notificaciones de éxito:](./media/howto-read-replicas-portal/success-notifications-enable.png)

6. Actualice la página de portal de Azure para actualizar la barra de herramientas de replicación. Ahora puede crear las réplicas de lectura para este servidor.

   ![Barra de herramientas actualizada](./media/howto-read-replicas-portal/updated-toolbar.png)
   
Habilitar la compatibilidad de replicación es una operación única por el servidor maestro. Un **deshabilitar la compatibilidad con la replicación** botón se proporciona para su comodidad. No se recomienda deshabilitar la compatibilidad con la replicación, a menos que esté seguro de que nunca creará una réplica en este servidor principal. No se puede deshabilitar la compatibilidad con la replicación mientras el servidor maestro tiene réplicas existentes.


## <a name="create-a-read-replica"></a>Creación de una réplica de lectura
Para crear una réplica de lectura, siga estos pasos:

1. Seleccione el servidor de Azure Database for PostgreSQL existente para utilizar como servidor maestro. 

2. En la barra lateral del servidor, bajo **configuración**, seleccione **replicación**.

3. Seleccione **Agregar réplica**.

   ![Adición de una réplica](./media/howto-read-replicas-portal/add-replica.png)

4. Escriba un nombre para la réplica de lectura. Seleccione **Aceptar** para confirmar la creación de la réplica.

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

1. En Azure Portal, seleccione el servidor maestro de Azure Database for PostgreSQL.

2. En el menú del servidor, en **CONFIGURACIÓN**, seleccione **Replicación**.

3. Seleccione el servidor de réplica para el que desea detener la replicación.

   ![Selección de la réplica](./media/howto-read-replicas-portal/select-replica.png)
 
4. Seleccione **Detener replicación**.

   ![Selección de Detener replicación](./media/howto-read-replicas-portal/select-stop-replication.png)
 
5. Seleccione **Aceptar** para detener la replicación.

   ![Confirmación de la detención de la replicación](./media/howto-read-replicas-portal/confirm-stop-replication.png)
 

## <a name="delete-a-master-server"></a>Eliminación de un servidor maestro
Para eliminar un servidor maestro, se usan los mismos pasos que para eliminar un servidor de Azure Database for PostgreSQL independiente. 

> [!IMPORTANT]
> Cuando se elimina un servidor maestro, la replicación se detiene en todas las réplicas de lectura. Las réplicas de lectura se convierten en servidores independientes que ahora admiten tanto lectura como escritura.

Para eliminar un servidor de Azure Portal, siga estos pasos:

1. En Azure Portal, seleccione el servidor maestro de Azure Database for PostgreSQL.

2. Abra la página **Introducción** del servidor. Seleccione **Eliminar**.

   ![En la página Introducción del servidor, selección de Eliminar para eliminar el servidor maestro](./media/howto-read-replicas-portal/delete-server.png)
 
3. Escriba el nombre del servidor maestro que desea eliminar. Seleccione **Eliminar** para confirmar la eliminación del servidor maestro.

   ![Confirmación de la eliminación del servidor maestro](./media/howto-read-replicas-portal/confirm-delete.png)
 

## <a name="delete-a-replica"></a>Eliminación de una réplica
Puede eliminar una réplica de lectura similar a cómo eliminar un servidor maestro.

- En Azure Portal, abra la página **Introducción** para la réplica de lectura. Seleccione **Eliminar**.

   ![En la página Introducción de la réplica, selección de Eliminar para eliminar la réplica](./media/howto-read-replicas-portal/delete-replica.png)
 
También puede eliminar la réplica de lectura desde la ventana **Replicación** siguiendo estos pasos:

1. En Azure Portal, seleccione el servidor maestro de Azure Database for PostgreSQL.

2. En el menú del servidor, en **CONFIGURACIÓN**, seleccione **Replicación**.

3. Seleccione la réplica de lectura que desea eliminar.

   ![Selección de la réplica para eliminar](./media/howto-read-replicas-portal/select-replica.png)
 
4. Seleccione **Eliminar réplica**.

   ![Selección de Eliminar réplica](./media/howto-read-replicas-portal/select-delete-replica.png)
 
5. Escriba el nombre de la réplica que quiere eliminar. Seleccione **Eliminar** para confirmar la eliminación de la réplica.

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

1. En Azure Portal, seleccione la réplica de lectura de Azure Database for PostgreSQL.

2. Seleccione **Métricas**. En la ventana de **métricas**, seleccione **Replica Lag** (Retraso entre réplicas).

   ![Supervisión del retraso de réplica](./media/howto-read-replicas-portal/select-replica-lag.png)
 
3. En **Agregación**, seleccione **Máx**. 
 
## <a name="next-steps"></a>Pasos siguientes
Más información sobre las [réplicas de lectura en Azure Database for PostgreSQL](concepts-read-replicas.md).