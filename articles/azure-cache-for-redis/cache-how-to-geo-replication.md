---
title: Configuración de replicación geográfica para Azure Cache for Redis | Microsoft Docs
description: Obtenga información sobre cómo replicar las instancias de Azure Cache for Redis en distintas regiones geográficas.
services: cache
documentationcenter: ''
author: yegu-ms
manager: jhubbard
editor: ''
ms.assetid: 375643dc-dbac-4bab-8004-d9ae9570440d
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache
ms.devlang: na
ms.topic: article
ms.date: 03/06/2019
ms.author: yegu
ms.openlocfilehash: 4254175955c3560c7bd0fdd08c6b60c318238b76
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "57991573"
---
# <a name="how-to-configure-geo-replication-for-azure-cache-for-redis"></a>Configuración de replicación geográfica para Azure Cache for Redis

Replicación geográfica proporciona un mecanismo para vincular dos instancias de Azure Cache for Redis de nivel Premium. Una memoria caché se elige como la caché vinculada principal y la otra como la caché vinculada secundaria. La caché vinculada secundaria pasa a ser de solo lectura, por lo que los datos escritos en la caché principal se replican en la caché vinculada secundaria. Esta funcionalidad se puede usar para replicar una caché en varias regiones de Azure. En este artículo se proporciona una guía para configurar la replicación geográfica para las instancias de Azure Cache for Redis de nivel Premium.

## <a name="geo-replication-prerequisites"></a>Requisitos previos de la replicación geográfica

Para configurar la replicación geográfica entre dos cachés, se deben cumplir los siguientes requisitos:

- Ambas cachés están [nivel Premium](cache-premium-tier-intro.md) almacena en caché.
- Ambas cachés están en la misma suscripción de Azure.
- La caché vinculada secundaria es el mismo tamaño de caché o un tamaño de caché mayor que la caché vinculada principal.
- Se crean las memorias caché y en un estado de ejecución.

Algunas características no son compatibles con replicación geográfica:

- No se admite la persistencia con replicación geográfica.
- Se admite la agrupación en clústeres si ambas cachés tienen la agrupación en clústeres habilitada y tienen el mismo número de particiones.
- Se admiten las memorias caché en la misma red virtual.
- Las cachés de distintas redes virtuales son compatibles con advertencias. Consulte [puedo usar la replicación geográfica con cachés en una red virtual?](#can-i-use-geo-replication-with-my-caches-in-a-vnet) para obtener más información.

Una vez que se configura la replicación geográfica, se aplican las siguientes restricciones al par de cachés vinculadas:

- La caché vinculada secundaria es de solo lectura, porque puede leer de ella, pero no puede escribir datos. 
- Se quitarán los datos existentes en la caché vinculada secundaria antes de la vinculación. Sin embargo, quitan los datos replicados permanecen en la caché vinculada secundaria si la replicación geográfica es posterior.
- No se puede [escala](cache-how-to-scale.md) ninguna de las cachés, mientras que las cachés vinculadas.
- No se puede [cambiar el número de particiones](cache-how-to-premium-clustering.md) si la memoria caché tiene la agrupación en clústeres habilitada.
- No puede habilitar la persistencia en ninguna de las cachés.
- También puede [exportar](cache-how-to-import-export-data.md#export) de ninguna de las cachés.
- No se puede [importación](cache-how-to-import-export-data.md#import) en la caché vinculada secundaria.
- No se puede eliminar la caché vinculada o el grupo de recursos que los contiene, hasta que la desvincule las cachés. Para más información, vea [¿Por qué no se pudo realizar la operación al intentar eliminar la memoria caché vinculada?](#why-did-the-operation-fail-when-i-tried-to-delete-my-linked-cache)
- Si las memorias caché se encuentran en regiones diferentes, los costos de salida de red se aplican a los datos se mueven entre regiones. Para más información, consulte el artículo sobre [cuánto cuesta replicar datos entre regiones de Azure](#how-much-does-it-cost-to-replicate-my-data-across-azure-regions).
- Conmutación automática por error no se produce entre la caché vinculada principal y secundaria. ¿Para obtener más información y obtener información sobre cómo conmutar por error una aplicación cliente, consulte [cómo funciona la conmutación por error a la caché vinculada secundaria?](#how-does-failing-over-to-the-secondary-linked-cache-work)

## <a name="add-a-geo-replication-link"></a>Agregar un vínculo de replicación geográfica

1. Para vincular dos cachés para la replicación geográfica, el primer clic **georreplicación** desde el menú de recursos de la memoria caché que vaya a ser el principal vinculado caché. A continuación, haga clic en **Agregar vínculo de replicación de caché** desde el **georreplicación** hoja.

    ![Agregar vínculo](./media/cache-how-to-geo-replication/cache-geo-location-menu.png)

2. Haga clic en el nombre de la caché secundaria deseada desde la **cachés compatibles** lista. Si la caché secundaria no se muestra en la lista, compruebe que la [requisitos previos de la replicación geográfica](#geo-replication-prerequisites) se cumplen para la caché secundaria. Para filtrar las cachés por región, haga clic en la región en el mapa para mostrar solo esas cachés en el **cachés compatibles** lista.

    ![Cachés compatibles de replicación geográfica](./media/cache-how-to-geo-replication/cache-geo-location-select-link.png)
    
    También puede iniciar el proceso de vinculación o ver los detalles de la caché secundaria mediante el menú contextual.

    ![Menú contextual de replicación geográfica](./media/cache-how-to-geo-replication/cache-geo-location-select-link-context-menu.png)

3. Haga clic en **Vincular** para vincular las dos cachés y comenzar el proceso de replicación.

    ![Vincular cachés](./media/cache-how-to-geo-replication/cache-geo-location-confirm-link.png)

4. Puede ver el progreso del proceso de replicación en la hoja **Replicación geográfica**.

    ![Estado de la vinculación](./media/cache-how-to-geo-replication/cache-geo-location-linking.png)

    También puede consultar el estado de la vinculación en la hoja de **información general** para ambas cachés, la principal y la secundaria.

    ![Estado de cachés](./media/cache-how-to-geo-replication/cache-geo-location-link-status.png)

    Una vez que se completa el proceso de replicación, el **estado del vínculo** cambia a **Correcto**.

    ![Estado de cachés](./media/cache-how-to-geo-replication/cache-geo-location-link-successful.png)

    La caché vinculada principal sigue estando disponible para su uso durante el proceso de vinculación. La caché vinculada secundaria no está disponible hasta que se complete el proceso de vinculación.

## <a name="remove-a-geo-replication-link"></a>Quitar un vínculo de replicación geográfica

1. Para quitar el vínculo entre dos cachés y detener la replicación geográfica, haga clic en **Unlink caches** (Desvincular cachés) en la hoja **Replicación geográfica**.
    
    ![Desvincular cachés](./media/cache-how-to-geo-replication/cache-geo-location-unlink.png)

    Una vez que se completa el proceso de desvinculación, la caché secundaria queda disponible tanto para lecturas como para escrituras.

>[!NOTE]
>Cuando se quita el vínculo de replicación geográfica, los datos replicados de la caché vinculada principal se mantienen en la caché secundaria.
>
>

## <a name="geo-replication-faq"></a>Preguntas más frecuentes sobre la replicación geográfica

- [¿Puedo usar la replicación geográfica con una caché de nivel Estándar o Básico?](#can-i-use-geo-replication-with-a-standard-or-basic-tier-cache)
- [¿La caché está disponible para usarla durante los procesos de vinculación o desvinculación?](#is-my-cache-available-for-use-during-the-linking-or-unlinking-process)
- [¿Puedo vincular más de dos cachés?](#can-i-link-more-than-two-caches-together)
- [¿Puedo vincular dos cachés de distintas suscripciones de Azure?](#can-i-link-two-caches-from-different-azure-subscriptions)
- [¿Puedo vincular dos cachés de tamaños distintos?](#can-i-link-two-caches-with-different-sizes)
- [¿Puedo usar la replicación geográfica con agrupación en clústeres habilitada?](#can-i-use-geo-replication-with-clustering-enabled)
- [¿Puedo usar la replicación geográfica con cachés en una VNET?](#can-i-use-geo-replication-with-my-caches-in-a-vnet)
- [¿Qué es la programación de replicación para la replicación geográfica de Redis?](#what-is-the-replication-schedule-for-redis-geo-replication)
- [¿Cuánto tiempo tarda la replicación geográfica?](#how-long-does-geo-replication-replication-take)
- [¿Se garantiza el punto de recuperación de replicación?](#is-the-replication-recovery-point-guaranteed)
- [¿Puedo usar PowerShell o la CLI de Azure para administrar la replicación geográfica?](#can-i-use-powershell-or-azure-cli-to-manage-geo-replication)
- [¿Cuánto cuesta replicar datos entre regiones de Azure?](#how-much-does-it-cost-to-replicate-my-data-across-azure-regions)
- [¿Por qué no se pudo realizar la operación al intentar eliminar la memoria caché vinculada?](#why-did-the-operation-fail-when-i-tried-to-delete-my-linked-cache)
- [¿Qué región se debe usar para la caché vinculada secundaria?](#what-region-should-i-use-for-my-secondary-linked-cache)
- [¿Cómo funciona la conmutación por error a la caché vinculada secundaria?](#how-does-failing-over-to-the-secondary-linked-cache-work)

### <a name="can-i-use-geo-replication-with-a-standard-or-basic-tier-cache"></a>¿Puedo usar la replicación geográfica con una caché de nivel Estándar o Básico?

No, la replicación geográfica solo está disponible para las cachés de nivel Premium.

### <a name="is-my-cache-available-for-use-during-the-linking-or-unlinking-process"></a>¿La caché está disponible para usarla durante los procesos de vinculación o desvinculación?

- Cuando el vínculo, la caché vinculada principal sigue estando disponible mientras se completa el proceso de vinculación.
- Cuando el vínculo, la caché vinculada secundaria no está disponible hasta que se complete el proceso de vinculación.
- Al desvincular, ambas cachés siguen estando disponibles mientras se completa el proceso de desvinculación.

### <a name="can-i-link-more-than-two-caches-together"></a>¿Puedo vincular más de dos cachés?

No, solo puede vincular dos cachés juntos.

### <a name="can-i-link-two-caches-from-different-azure-subscriptions"></a>¿Puedo vincular dos cachés de distintas suscripciones de Azure?

No, ambas cachés deben estar en la misma suscripción de Azure.

### <a name="can-i-link-two-caches-with-different-sizes"></a>¿Puedo vincular dos cachés de tamaños distintos?

Sí, siempre que la caché vinculada secundaria sea mayor que la caché vinculada principal.

### <a name="can-i-use-geo-replication-with-clustering-enabled"></a>¿Puedo usar la replicación geográfica con agrupación en clústeres habilitada?

Sí, siempre que ambas cachés tengan la misma cantidad de particiones.

### <a name="can-i-use-geo-replication-with-my-caches-in-a-vnet"></a>¿Puedo usar la replicación geográfica con cachés en una VNET?

Sí, se admite la replicación geográfica de cachés en Vnet con advertencias:

- Se admite la replicación geográfica entre cachés de la misma VNET.
- También se admite la replicación geográfica entre cachés en distintas redes virtuales.
  - Si las redes virtuales están en la misma región, puede conectarlas mediante [emparejamiento de VNET](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) o un [conexión de puerta de enlace VPN de VNET a VNET](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways#V2V).
  - Si las redes virtuales están en distintas regiones, replicación geográfica emparejamiento no se admite el uso de la red virtual debido a una restricción con equilibradores de carga interno básico. Para obtener más información acerca de las restricciones de emparejamiento de red virtual, consulte [restricciones y requisitos de red Virtual - emparejamiento:](https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-peering#requirements-and-constraints). La solución recomendada es usar una conexión de puerta de enlace VPN de VNET a VNET.

Uso de [esta plantilla de Azure](https://azure.microsoft.com/resources/templates/201-redis-vnet-geo-replication/), puede implementar rápidamente dos cachés con replicación geográfica en una red virtual conectado con una conexión de puerta de enlace VPN de VNET a VNET.

### <a name="what-is-the-replication-schedule-for-redis-geo-replication"></a>¿Qué es la programación de replicación para la replicación geográfica de Redis?

La replicación es continua y asincrónica y no sucede en una programación específica. Todas las escrituras realizadas en el servidor principal se replican de forma inmediata y de forma asincrónica en la base de datos secundaria.

### <a name="how-long-does-geo-replication-replication-take"></a>¿Cuánto tiempo tarda la replicación geográfica?

La replicación es incremental, asincrónica y continua y el tiempo empleado no difiera mucho de la latencia entre regiones. En determinadas circunstancias, la caché secundaria posible que deba realizar una sincronización completa de los datos desde el servidor principal. El tiempo de replicación en este caso es depende del número de factores como: carga en la caché principal, el ancho de banda de red disponible y la latencia de entre regiones. Hemos encontrado tiempo de replicación para un par de replicación geográfica de 53 GB completa puede ser cualquier valor entre 5 y 10 minutos.

### <a name="is-the-replication-recovery-point-guaranteed"></a>¿Se garantiza el punto de recuperación de replicación?

Para las memorias caché en un modo de replicación geográfica, persistencia está deshabilitada. Si un par de replicación geográfica se ha desvinculado, como una conmutación por error iniciada por el cliente, la caché vinculada secundaria mantiene sus datos sincronizados hasta ese punto de tiempo. En tales situaciones no se garantiza que ningún punto de recuperación.

Para obtener un punto de recuperación, [exportar](cache-how-to-import-export-data.md#export) de ninguna de las cachés. Más adelante, puede [importación](cache-how-to-import-export-data.md#import) en la caché vinculada principal.

### <a name="can-i-use-powershell-or-azure-cli-to-manage-geo-replication"></a>¿Puedo usar PowerShell o la CLI de Azure para administrar la replicación geográfica?

Sí, se puede administrar la replicación geográfica mediante Azure portal, PowerShell o CLI de Azure. Para obtener más información, consulte el [documentos de PowerShell](https://docs.microsoft.com/powershell/module/az.rediscache/?view=azps-1.4.0#redis_cache) o [documentos con la CLI de Azure](https://docs.microsoft.com/cli/azure/redis/server-link?view=azure-cli-latest).

### <a name="how-much-does-it-cost-to-replicate-my-data-across-azure-regions"></a>¿Cuánto cuesta replicar datos entre regiones de Azure?

Cuando usa la replicación geográfica, los datos de la caché vinculada principal se replican a la caché vinculada secundaria. No hay ningún cargo por la transferencia de datos si las dos cachés vinculadas están en la misma región. Si las dos cachés vinculadas se encuentran en regiones diferentes, el cargo por transferencia de datos es el costo de salida de la red de datos que se transfieren a través de cualquier región. Para más información, consulte [Detalles de precios de ancho de banda](https://azure.microsoft.com/pricing/details/bandwidth/).

### <a name="why-did-the-operation-fail-when-i-tried-to-delete-my-linked-cache"></a>¿Por qué no se pudo realizar la operación al intentar eliminar la memoria caché vinculada?

Replicación geográfica de las memorias caché y sus grupos de recursos no se puede eliminar mientras vinculado hasta que quite el vínculo de replicación geográfica. Si intenta eliminar el grupo de recursos que contiene una o las dos cachés vinculadas, se eliminan los otros recursos del grupo de recursos, pero el grupo de recursos permanece en el estado `deleting` y las cachés en el grupo de recursos permanecen en el estado `running`. Para eliminar el grupo de recursos y las cachés vinculadas dentro de él, desvincular las cachés, como se describe en [quitar un vínculo de replicación geográfica](#remove-a-geo-replication-link).

### <a name="what-region-should-i-use-for-my-secondary-linked-cache"></a>¿Qué región se debe usar para la caché vinculada secundaria?

En general, se recomienda para que la caché esté en la misma región de Azure que la aplicación que accede a ella. Para las aplicaciones con las regiones principales y de reserva independientes, se recomienda que las memorias caché principales y secundarias existen en esas mismas regiones. Para más información sobre regiones emparejadas, consulte el artículo sobre [procedimientos recomendados: regiones emparejadas de Azure](../best-practices-availability-paired-regions.md).

### <a name="how-does-failing-over-to-the-secondary-linked-cache-work"></a>¿Cómo funciona la conmutación por error a la caché vinculada secundaria?

No se admite la conmutación automática por error entre regiones de Azure para las cachés de replicación geográfica. En un escenario de recuperación ante desastres, los clientes deben mostrar la pila de toda la aplicación de manera coordinada en su región de copia de seguridad. Lo que permite decidir los componentes de aplicación individual cuándo cambiar a sus copias de seguridad en sus propios puede afectar negativamente al rendimiento. Una de las ventajas claves de Redis es que es un almacén de latencia muy baja. Si la aplicación del cliente principal está en una región distinta a su memoria caché, el tiempo de ida y vuelta se ha agregado tenga un impacto apreciable en el rendimiento. Por este motivo, hemos evitar la conmutación por error automáticamente debido a problemas transitorios de disponibilidad.

Para iniciar una conmutación por error iniciada por el cliente, desvincule primero las memorias caché. A continuación, cambie el cliente de Redis para utilizar el punto de conexión de la caché secundaria (anteriormente vinculada). Cuando se desvinculen las dos cachés, la caché secundaria vuelve a una memoria caché de lectura y escritura normal y acepta solicitudes directamente desde los clientes de Redis.

## <a name="next-steps"></a>Pasos siguientes

Más información sobre el [nivel Premium de Azure Cache for Redis](cache-premium-tier-intro.md).
