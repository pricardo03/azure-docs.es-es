---
title: Escalado de un grupo de servidor de Hiperescala (Citus) en Azure Database for PostgreSQL
description: Ajuste la memoria del grupo de servidores, el disco y los recursos de CPU para tratar un aumento de la carga
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.topic: conceptual
ms.date: 12/17/2019
ms.openlocfilehash: bec2a40d8cf5fb178418ec6bb59a52a0bfe3eb8c
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2019
ms.locfileid: "75453054"
---
# <a name="scale-a-hyperscale-citus-server-group"></a>Escalado de un grupo de servidores Hiperescala (Citus)

Azure Database for PostgreSQL:Hiperescala (Citus) proporciona escalado de autoservicio para tratar el aumento de la carga. Azure Portal permite agregar nuevos nodos de trabajo y aumentar los núcleos virtuales de los nodos existentes.

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

## <a name="increase-vcores"></a>Aumentar los núcleos virtuales

Además de agregar nodos nuevos, puede aumentar las capacidades de los nodos existentes. Actualmente, esta característica se encuentra en versión preliminar: para solicitar más núcleos virtuales para los nodos del grupo de servidores, [póngase en contacto con el Soporte técnico de Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

## <a name="next-steps"></a>Pasos siguientes

Más información sobre el grupo de servidores en [opciones de rendimiento](concepts-hyperscale-configuration-options.md).
