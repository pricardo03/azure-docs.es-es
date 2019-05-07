---
title: Distribución global de los datos con Azure Cosmos DB
description: Obtenga más información sobre replicación geográfica, arquitectura multimaestro, conmutación por error y recuperación de datos a escala mundial con bases de datos globales de Azure Cosmos DB, un servicio de base de datos con varios modelos distribuido globalmente.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/18/2019
ms.openlocfilehash: b90e495e0901c0caa14d9451f365e17f6c075e2b
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65070802"
---
# <a name="global-data-distribution-with-azure-cosmos-db---overview"></a>Introducción a la distribución de datos global con Azure Cosmos DB

Las aplicaciones actuales deben estar siempre en línea y tener una alta capacidad de respuesta. Para lograr baja latencia y alta disponibilidad, las instancias de estas aplicaciones deben implementarse en centros de datos que están cerca de sus usuarios. Estas aplicaciones se implementan normalmente en varios centros de datos y se denominan "distribuidas globalmente". Las aplicaciones distribuidas globalmente necesitan una base de datos distribuida globalmente que puede replicar de forma transparente los datos en cualquier lugar del mundo para permitir que las aplicaciones funcionen en una copia de los datos que están cerca de los usuarios. 

Azure Cosmos DB es un servicio de base de datos distribuida globalmente que está diseñado para proporcionar baja latencia, escalabilidad elástica del rendimiento, semántica bien definida para la coherencia de datos y alta disponibilidad. En resumen, si la aplicación necesita tiempo de respuesta garantizado de rápido en cualquier lugar del mundo, si es necesario para estar siempre en línea y necesita escalabilidad ilimitada y elástico de rendimiento y almacenamiento, debe compilar la aplicación en Azure Cosmos DB.

Puede configurar sus bases de datos para que se distribuyan de manera global y estén disponibles en cualquiera de las regiones de Azure. Para reducir la latencia, coloque los datos cerca de dónde están los usuarios. Elegir las regiones requeridas depende del alcance global de la aplicación y de dónde se encuentran los usuarios. COSMOS DB replica los datos de forma transparente a todas las regiones asociadas con su cuenta de Cosmos. Proporciona una sola imagen de sistema de los contenedores y la base de datos de Azure Cosmos globalmente distribuida, para que la aplicación pueda leer y escribir de manera local. 

Con Azure Cosmos DB, puede agregar o quitar las regiones asociadas con su cuenta en cualquier momento. La aplicación no necesita pausarse o volver a implementarse para agregar o quitar una región. Sigue siendo la alta disponibilidad constantemente debido a las capacidades de hospedaje múltiple que proporciona el servicio de forma nativa.

![Topología de implementación de alta disponibilidad](./media/distribute-data-globally/deployment-topology.png)

## <a name="key-benefits-of-global-distribution"></a>Ventajas clave de distribución global

**Compilación de aplicaciones globales activo/activo.** Con el protocolo de replicación de varios maestros novel, todas las regiones admite lecturas y escrituras. También permite la capacidad de varios maestra:

- Elástico ilimitado de escritura y la escalabilidad de lectura. 
- 99,999 % de disponibilidad de lectura y escritura en todo el mundo.
- Garantía de lecturas y escrituras atendidas en menos de 10 milisegundos en el percentil 99.

Mediante el uso del hospedaje múltiple de Azure Cosmos DB API, la aplicación es compatible con la región más cercana y puede enviar solicitudes a esa región. La región más cercana se identifica sin ningún cambio de configuración. Como agregar y quitar regiones a y desde su cuenta de Azure Cosmos, la aplicación no necesita volver a implementar o en pausa, sigue siendo la alta disponibilidad en todo momento.

**Creación de aplicaciones con alta capacidad de respuesta.** La aplicación puede realizar casi en tiempo real lecturas y escrituras en todas las regiones que eligió para la base de datos. Internamente, Azure Cosmos DB controla la replicación de datos entre regiones con garantías de nivel de coherencia del nivel que ha seleccionado.

**Creación de aplicaciones de alta disponibilidad.** Ejecución de una base de datos en varias regiones en todo el mundo aumenta la disponibilidad de una base de datos. Si alguna región no está disponible, otras regiones gestionan automáticamente las solicitudes de la aplicación. Azure Cosmos DB ofrece el 99,999 % de disponibilidad de lectura y escritura para las bases de datos de varias regiones.

**Mantenimiento de la continuidad empresarial durante interrupciones regionales.** Azure Cosmos DB admite [conmutación por error automática](how-to-manage-database-account.md#automatic-failover) durante una interrupción regional. Durante una interrupción regional, Azure Cosmos DB sigue manteniendo sus SLA de rendimiento, coherencia, disponibilidad y latencia. Para ayudar a asegurarse de que toda la aplicación tiene alta disponibilidad, Cosmos DB ofrece una API para simular una interrupción regional de conmutación por error manual. Con esta API, puede realizar maniobras de continuidad empresarial regulares.

**Escalabilidad global del rendimiento de lectura y escritura.** Puede permitir que todas las regiones ser grabable y escalar lecturas y escrituras en todo el mundo. El rendimiento que la aplicación se configura en una base de datos de Azure Cosmos o un contenedor garantiza que se entregan en todas las regiones asociadas con su cuenta de Azure Cosmos. El rendimiento aprovisionado garantizado por [SLA con respaldo financiero](https://aka.ms/acdbsla).

**Elección entre varios modelos de coherencia bien definidos.** El protocolo de replicación de Azure Cosmos DB ofrece cinco modelos de coherencia prácticos, intuitivos y bien definidos. Cada modelo tiene un equilibrio entre coherencia y rendimiento. Use estos modelos de coherencia para compilar con facilidad las aplicaciones distribuidas globalmente.

## <a id="Next Steps"></a>Pasos siguientes

Lea más sobre la distribución global en estos artículos:

* [Distribución global en segundo plano](global-dist-under-the-hood.md)
* [Configuración de una arquitectura multimaestro en las aplicaciones](how-to-multi-master.md)
* [Configuración de los clientes para el hospedaje múltiple](how-to-manage-database-account.md#configure-multiple-write-regions)
* [Incorporación o eliminación de regiones de una cuenta de Azure Cosmos DB](how-to-manage-database-account.md#addremove-regions-from-your-database-account)
* [Creación de una directiva de resolución de conflictos personalizada para cuentas de API de SQL](how-to-manage-conflicts.md#create-a-custom-conflict-resolution-policy)