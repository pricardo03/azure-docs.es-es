---
title: 'Escalado de una instancia de Azure Database for PostgreSQL: grupo de servidor de Hiperescala (Citus)'
description: Ajuste la memoria del grupo de servidores, el disco y los recursos de CPU para tratar un aumento de la carga
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.topic: conceptual
ms.date: 9/17/2019
ms.openlocfilehash: 6053ba37bf330f6b59e291dade822a5ca9de8c85
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73492303"
---
# <a name="scale-a-hyperscale-citus-server-group"></a>Escalado de un grupo de servidores Hiperescala (Citus)

Azure Database for PostgreSQL:Hiperescala (Citus) proporciona escalado de autoservicio para tratar el aumento de la carga. Azure Portal permitir agregar nuevos nodos de trabajo y aumentar la capacidad de los nodos existentes.

## <a name="add-worker-nodes"></a>Incorporación de nodos de trabajo

Para agregar nodos, vaya a la pestaña **Configurar** en el grupo de servidores de Hiperescala (Citus).  Arrastre el control deslizante de **Números de nodo de trabajo** para cambiar el valor.

![Controles deslizantes de recursos](./media/howto-hyperscale-scaling/01-sliders-workers.png)

Haga clic en el botón **Guardar** para que el valor cambiado surta efecto.

> [!NOTE]
> Una vez que se ha aumentado y guardado, el número de nodos de trabajo no se puede reducir con el control deslizante.

### <a name="rebalance-shards"></a>Reequilibrio de particiones

Para aprovechar los nodos recién agregados, tiene que reequilibrar la tabla distribuida [particiones de base de datos](concepts-hyperscale-distributed-data.md#shards), lo que significa mover algunas particiones de los nodos existentes a las nuevas. En primer lugar, compruebe que los trabajos nuevos hayan finalizado correctamente el aprovisionamiento. Luego, inicie el reequilibrador de particiones conectándose al nodo coordinador de clústeres con psql y ejecute:

```sql
SELECT rebalance_table_shards('distributed_table_name');
```

La función `rebalance_table_shards` vuelve a equilibrar todas las tablas del grupo [colocación](concepts-hyperscale-colocation.md)de la tabla nombrada en su argumento. Por lo tanto, no tiene que llamar a la función para cada tabla distribuida, simplemente llámela en una tabla representativa de cada grupo de ubicación.

## <a name="increase-vcores-or-storage-space"></a>Aumento del espacio de almacenamiento o núcleos virtuales

Además de agregar nodos nuevos, puede aumentar las capacidades de los nodos existentes. Vaya a la pestaña **Configurar** en el grupo de servidores Hiperescala (Citus) y arrastre el control deslizante de **Núcleos virtuales** y **Almacenamiento** para cambiar estos valores para todos los nodos de trabajo. Asegúrese de hacer clic en **Guardar** para aplicar los cambios.

## <a name="next-steps"></a>Pasos siguientes

Más información sobre el grupo de servidores en [opciones de rendimiento](concepts-hyperscale-configuration-options.md).
