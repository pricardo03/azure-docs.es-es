---
title: Administración de réplicas de lectura en Azure Portal para Azure Database for PostgreSQL
description: En este artículo se describe cómo administrar las réplicas de lectura de Azure Database for PostgreSQL en Azure Portal.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 02/01/2019
ms.openlocfilehash: 37150f67e29dae0357c978cfaea9abeebeef428c
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/04/2019
ms.locfileid: "55691412"
---
# <a name="how-to-create-and-manage-read-replicas-in-the-azure-portal"></a>Creación y administración de réplicas de lectura en Azure Portal

> [!IMPORTANT]
> La característica de réplica de lectura está en versión preliminar pública.


En este artículo, obtendrá información sobre cómo crear y administrar las réplicas de lectura en el servicio Azure Database for PostgreSQL mediante Azure Portal. Para más información sobre las réplicas de lectura, [lea la documentación de los conceptos](concepts-read-replicas.md).

## <a name="prerequisites"></a>Requisitos previos
- Un [servidor de Azure Database for PostgreSQL](quickstart-create-server-database-portal.md) que se usará como servidor maestro.

## <a name="prepare-the-master-server"></a>Preparación del servidor maestro
Este paso de preparación del servidor maestro se aplica solo a los servidores de propósito general y optimizados para memoria.

El parámetro **azure.replication_support** debe establecerse en RÉPLICA en el servidor maestro. Si cambia este parámetro, será necesario reiniciar el servidor para que el cambio surta efecto.

1. En Azure Portal, seleccione el servidor de Azure Database for PostgreSQL existente que desee utilizar como servidor maestro.

2. Seleccione **Parámetros del servidor** en el menú de la izquierda.

3. Busque **azure.replication_support**.

   ![Azure Database for PostgreSQL: azure.replication_support](./media/howto-read-replicas-portal/azure-replication-parameter.png)

4. Establezca **azure.replication_support** en RÉPLICA. **Guarde** el cambio.

   ![Azure Database for PostgreSQL: RÉPLICA y guardar](./media/howto-read-replicas-portal/save-parameter-replica.png)

5. Una vez completada la operación, recibirá una notificación.

   ![Azure Database for PostgreSQL: notificación de la operación](./media/howto-read-replicas-portal/parameter-save-notification.png)

6. Reinicie el servidor para aplicar el cambio después de guardarlo. Consulte [la documentación de reinicio](howto-restart-server-portal.md) para obtener información sobre cómo reiniciar un servidor.


## <a name="create-a-read-replica"></a>Creación de una réplica de lectura
Para crear réplicas de lectura, puede seguir estos pasos:
1.  Seleccione el servidor de Azure Database for PostgreSQL existente que desea utilizar como servidor maestro. 

2.  Seleccione Replicación en el menú, en CONFIGURACIÓN.

   Si no ha establecido **azure.replication_support** en RÉPLICA en el servidor maestro optimizado para memoria o de propósito general, y lo reinicia, verá un mensaje que le indica que lo haga. Hágalo antes de continuar con la creación.

3.  Seleccione Agregar réplica.

   ![Azure Database for PostgreSQL: agregar réplica](./media/howto-read-replicas-portal/add-replica.png)

4.  Escriba un nombre para el servidor de réplica y seleccione Aceptar para confirmar la creación de la réplica.

   ![Azure Database for PostgreSQL: denominar la réplica](./media/howto-read-replicas-portal/name-replica.png) 

> [!IMPORTANT]
> Las réplicas de lectura se crean con la misma configuración de servidor que el servidor maestro. Después de crear una réplica, puede cambiar el plan de precios (excepto hacia Básico y desde Básico), la generación de proceso, los núcleos virtuales, el almacenamiento y el período de retención de copia de seguridad de forma independiente del servidor maestro.

> [!IMPORTANT]
> Antes de actualizar la configuración del servidor maestro con nuevos valores, se debe actualizar la configuración de las réplicas con valores iguales o mayores. Si intenta hacer esto, se producirá un error. De esta forma, se tiene la garantía de que las réplicas están al día con los cambios realizados en el servidor maestro. 


Una vez creado el servidor de réplica, puede verlo en la ventana Replicación.

![Azure Database for PostgreSQL: nueva réplica](./media/howto-read-replicas-portal/list-replica.png)
 

## <a name="stop-replication"></a>Detención replicación

> [!IMPORTANT]
> La detención la replicación en un servidor es irreversible. Una vez que detenida, la replicación entre un servidor maestro y una réplica no se puede deshacer. Después, el servidor de réplica se convierte en un servidor independiente que admite operaciones de lectura y escritura. Este servidor no puede volver a convertirse en una réplica.

Para detener la replicación entre un servidor maestro y una réplica desde Azure Portal, siga estos pasos:
1.  En Azure Portal, seleccione el servidor maestro de Azure Database for PostgreSQL.

2.  Seleccione Replicación en el menú, en CONFIGURACIÓN.

3.  Seleccione el servidor de réplica para el que desea detener la replicación.

   ![Azure Database for PostgreSQL: seleccionar réplica](./media/howto-read-replicas-portal/select-replica.png)
 
4.  Seleccione Detener replicación.

   ![Azure Database for PostgreSQL: seleccionar la detección de la replicación](./media/howto-read-replicas-portal/select-stop-replication.png)
 
5.  Para confirmar que desea detener la replicación, haga clic en Aceptar.

   ![Azure Database for PostgreSQL: confirmar la detección de la replicación](./media/howto-read-replicas-portal/confirm-stop-replication.png)
 

## <a name="delete-a-master"></a>Eliminación de un servidor maestro

> [!IMPORTANT]
> Al eliminar un servidor maestro, se detiene la replicación en todos los servidores de réplica. Los servidores de réplica se convierten en servidores independientes que ahora admiten tanto lectura como escritura.
Para eliminar un servidor maestro, hay que seguir los mismos pasos que con un servidor de Azure Database for PostgreSQL independiente. Para eliminar un servidor desde Azure Portal, lleve a cabo los siguientes pasos:

1.  En Azure Portal, seleccione el servidor maestro de Azure Database for PostgreSQL.

2.  En la página Información general, seleccione Eliminar.

   ![Azure Database for PostgreSQL: eliminar el servidor](./media/howto-read-replicas-portal/delete-server.png)
 
3.  Escriba el nombre del servidor maestro y seleccione Eliminar para confirmar la eliminación.

   ![Azure Database for PostgreSQL: confirmar la eliminación](./media/howto-read-replicas-portal/confirm-delete.png)
 

## <a name="delete-a-replica"></a>Eliminación de una réplica
Para eliminar una réplica de lectura, puede seguir los mismos pasos que para eliminar el servidor maestro anteriormente. En primer lugar, abra la página Información general de la réplica y seleccione Eliminar.

   ![Azure Database for PostgreSQL: eliminar la réplica](./media/howto-read-replicas-portal/delete-replica.png)
 
Como alternativa, puede eliminarla desde la ventana Replicación.
1.  En Azure Portal, seleccione el servidor maestro de Azure Database for PostgreSQL.

2.  Seleccione Replicación en el menú, en CONFIGURACIÓN.

3.  Seleccione el servidor de réplica que desea eliminar. 

   ![Azure Database for PostgreSQL: seleccionar réplica](./media/howto-read-replicas-portal/select-replica.png)
 
4.  Seleccione Eliminar réplica.

   ![Azure Database for PostgreSQL: seleccionar réplica para eliminar](./media/howto-read-replicas-portal/select-delete-replica.png)
 
5.  Escriba el nombre de la réplica y seleccione Eliminar para confirmar la eliminación.

   ![Azure Database for PostgreSQL: confirmar la eliminación de la rÉplica](./media/howto-read-replicas-portal/confirm-delete-replica.png)
 

## <a name="monitor-a-replica"></a>Supervisión de una réplica
### <a name="max-lag-across-replicas"></a>Retraso máximo entre réplicas
La métrica **Retraso máximo entre réplicas** muestra el retardo en bytes entre la réplica con mayor retardo y el servidor maestro. 

1.  En Azure Portal, seleccione el **servidor maestro** de Azure Database for PostgreSQL.

2.  Seleccione Métricas. En la ventana de métricas, seleccione **Max Lag Across Replicas** (Retraso máximo entre réplicas).

    ![Azure Database for PostgreSQL: supervisar el retraso máximo entre réplicas](./media/howto-read-replicas-portal/select-max-lag.png)
 
3.  Seleccione **Max** (Máximo) como agregación. 

### <a name="replica-lag"></a>Retraso entre réplicas
La métrica de **Replica Lag** (Retraso entre réplicas) muestra el tiempo desde la última transacción reproducida en esta réplica. Si no se produce ninguna transacción en el servidor maestro, la métrica refleja este retardo de tiempo.

1.  En Azure Portal, seleccione el servidor de **réplica** de Azure Database for PostgreSQL.

2.  Seleccione Métricas. En la ventana de métricas, seleccione **Replica Lag** (Retraso entre réplicas).

   ![Azure Database for PostgreSQL: supervisar el retraso entre réplicas](./media/howto-read-replicas-portal/select-replica-lag.png)
 
3.  Seleccione **Max** (Máximo) como agregación. 
 
## <a name="next-steps"></a>Pasos siguientes
- Más información sobre las [réplicas de lectura en Azure Database for PostgreSQL](concepts-read-replicas.md).