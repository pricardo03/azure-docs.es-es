---
title: 'Escalado de una instancia de Azure Database for PostgreSQL: grupo de servidor de Hiperescala (Citus)'
description: Ajuste la memoria del grupo de servidores, el disco y los recursos de CPU para tratar un aumento de la carga
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.topic: conceptual
ms.date: 9/17/2019
ms.openlocfilehash: a69b4988a5ee835381de986edaa5899b09aa9e4e
ms.sourcegitcommit: 55f7fc8fe5f6d874d5e886cb014e2070f49f3b94
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2019
ms.locfileid: "71263019"
---
# <a name="scale-a-hyperscale-citus-server-group"></a>Escalado de un grupo de servidores Hiperescala (Citus)

Azure Database for PostgreSQL:Hiperescala (Citus) proporciona escalado de autoservicio para tratar el aumento de la carga. Azure Portal facilita la incorporación de nuevos nodos de trabajo.

Para ello, vaya a la pestaña **Configurar** en el grupo de servidores de Hiperescala (Citus).
Arrastre el control deslizante de **Números de nodo de trabajo** para cambiar el valor.

![Controles deslizantes de recursos](./media/howto-hyperscale-scaling/01-sliders-workers.png)

Haga clic en el botón "Guardar" para que el valor cambiado surta efecto.

> [!NOTE]
> Una vez que se ha aumentado y guardado, el número de nodos de trabajo no se puede reducir con el control deslizante.
>
> Además, los núcleos virtuales y el almacenamiento todavía no se pueden ajustar en el coordinador o los trabajos con esta interfaz de usuario. Abra una incidencia de soporte técnico si necesita escalar el proceso en los nodos de coordinador o de trabajo.

Para aprovechar los nodos recién agregados, tiene que reequilibrar la tabla distribuida [particiones de base de datos](concepts-hyperscale-distributed-data.md#shards), lo que significa mover algunas particiones de los nodos existentes a las nuevas. Para iniciar el reequilibrador de particiones, conéctese al nodo coordinador de clústeres con psql y ejecute:

```sql
SELECT rebalance_table_shards('distributed_table_name');
```

La función `rebalance_table_shards` vuelve a equilibrar todas las tablas del grupo [colocación](concepts-hyperscale-colocation.md)de la tabla nombrada en su argumento. Por lo tanto, no tiene que llamar a la función para cada tabla distribuida, simplemente llámela en una tabla representativa de cada grupo de ubicación.

## <a name="next-steps"></a>Pasos siguientes

Más información sobre el grupo de servidores en [opciones de rendimiento](concepts-hyperscale-configuration-options.md).
