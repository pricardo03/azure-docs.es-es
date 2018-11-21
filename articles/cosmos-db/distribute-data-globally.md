---
title: Distribución de datos global con Azure Cosmos DB | Microsoft Docs
description: Obtenga más información sobre replicación geográfica, arquitectura multimaestro, conmutación por error y recuperación de datos a escala mundial con bases de datos globales de Azure Cosmos DB, un servicio de base de datos con varios modelos distribuido globalmente.
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 10/26/2018
ms.author: mjbrown
ms.openlocfilehash: c90450fa4cc35b460198f5a351a965aee4ea4f4b
ms.sourcegitcommit: 542964c196a08b83dd18efe2e0cbfb21a34558aa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2018
ms.locfileid: "51636422"
---
# <a name="build-globally-distributed-applications-with-azure-cosmos-db"></a>Creación de aplicaciones distribuidas globalmente con Azure Cosmos DB

Muchas de las aplicaciones actuales requieren se trabajen en múltiples centros de datos. Estas aplicaciones se conocen como aplicaciones distribuidas globalmente. Estas aplicaciones siempre están disponibles y accesibles para usuarios de todo el mundo. Administrar la distribución global de los datos que estas aplicaciones utilizan mientras se ofrece baja latencia, una escalabilidad elástica de rendimiento y una alta disponibilidad en todo el mundo es un problema complicado. Azure Cosmos DB es un servicio de base de datos distribuido globalmente que está diseñado para proporcionar baja latencia, escalabilidad elástica del rendimiento, semántica bien definida para la coherencia de datos y alta disponibilidad. En resumen, si la aplicación necesita un tiempo de respuesta rápido garantizado en cualquier parte del mundo, si requiere estar siempre en línea y necesita escalabilidad ilimitada y elástica de rendimiento y almacenamiento, debería considerar la posibilidad de crear aplicaciones con Azure Cosmos DB.

Azure Cosmos DB es un servicio fundamental de Azure que está disponible en todas las [regiones de Azure](https://azure.microsoft.com/global-infrastructure/regions/) de manera predeterminada. Microsoft opera centros de datos de Azure en más de 54 regiones en todo el mundo y sigue expandiendo la presencia regional para satisfacer las necesidades cada vez más grandes de los clientes. Cuando se crea una cuenta de Azure Cosmos, es el usuario quien decide en qué región o regiones se debe implementar. Microsoft trabaja con el servicio Azure Cosmos DB todo el día y todos los días, por lo que el usuario puede centrarse en sus aplicaciones.

Puede configurar sus bases de datos para que se distribuyan de manera global y estén disponibles en cualquiera de las regiones de Azure. Para reducir la latencia, debe colocar los datos en la ubicación más cercana a la de los usuarios. Elegir las regiones requeridas depende del alcance global de la aplicación y de dónde se encuentran los usuarios. Azure Cosmos DB replica sin problemas los datos dentro de la cuenta a todas las regiones asociadas con la cuenta. Proporciona una sola imagen de sistema de los contenedores y la base de datos de Azure Cosmos globalmente distribuida, para que la aplicación pueda leer y escribir de manera local. Con Azure Cosmos DB, puede agregar o quitar las regiones asociadas con su cuenta en cualquier momento. La aplicación no necesita volver a pausarse o implementarse para agregar o quitar una región. Sigue ofreciendo una alta disponibilidad en todo momento debido a las funcionalidades de hospedaje múltiple que el servicio ofrece.

## <a name="key-benefits-of-global-distribution"></a>Ventajas clave de distribución global

**Compilación de aplicaciones activas/activas globales**: con la funcionalidad de arquitectura multimaestro, cada región es grabable (además de legible). La característica multimaestro también garantiza una escalabilidad de escritura elástica ilimitada, 99,999 % de disponibilidad de lectura y escritura en todo el mundo y lecturas y escrituras rápidas garantizadas atendidas en menos de 10 milisegundos, en el percentil 99.  

Con las API de hospedaje múltiple de Azure Cosmos DB, la aplicación sabe cuál es la región más cercana y envía las solicitudes a esa región. La región más cercana se identifica sin ningún cambio de configuración. Como agrega y quita regiones desde la cuenta de Azure Cosmos DB, no es necesario volver a implementar la aplicación y esta sigue teniendo alta disponibilidad.

**Compilación de aplicaciones de alta capacidad de respuesta**: la aplicación se puede diseñar fácilmente para que realice lecturas y escrituras en tiempo real, con latencias en milisegundos de un dígito, en todas las regiones que eligió para la base de datos.  De manera interna, Azure Cosmos DB controla la replicación de datos entre regiones de una manera que garantiza el nivel de coherencia elegido para la cuenta de Azure Cosmos.

Muchas aplicaciones se beneficiarán de las mejoras de rendimiento que se incluyen con la capacidad de realizar escrituras en varias regiones (locales). Algunas aplicaciones que requieren coherencia fuerte prefieren canalizar todas las escrituras a una sola región. Para admitir estas aplicaciones, Azure Cosmos DB admite configuraciones tanto para una única región como para varias regiones.

**Compilación de aplicaciones de alta disponibilidad**: ejecutar una base de datos en varias regiones aumenta la disponibilidad de la base de datos. Si hay alguna región no disponible, otras regiones administrarán automáticamente las solicitudes de la aplicación. Azure Cosmos DB ofrece el 99,999 % de disponibilidad de lectura y escritura para las bases de datos de varias regiones.

**Continuidad empresarial durante interrupciones regionales**: Azure Cosmos DB admite la [conmutación automática por error](how-to-manage-database-account.md#automatic-failover) durante una interrupción regional. Además, durante una interrupción regional, Azure Cosmos DB sigue manteniendo sus SLA de rendimiento, coherencia, disponibilidad y latencia. Para ayudar a garantizar que toda la aplicación tiene una alta disponibilidad, Azure Cosmos DB ofrece la API de conmutación por error manual para simular una interrupción regional. Con esta API, puede realizar maniobras de continuidad empresarial regulares.

**Escalabilidad global de lectura y escritura**: con la funcionalidad de arquitectura multimaestro, puede escalar de manera elástica el rendimiento de lectura y escritura en todo el mundo. La funcionalidad de arquitectura multimaestro garantiza que el rendimiento que la aplicación configura en un contenedor o una base de datos de Azure Cosmos DB se entrega en todas las regiones y lo protegen [SLA con respaldo financiero](https://aka.ms/acdbsla).

**Varios modelos de coherencia bien definidos**: el protocolo de replicación de Azure Cosmos DB está diseñado para ofrecer cinco modelos de coherencia bien definidos, prácticos e intuitivos. Cada modelo de coherencia tiene un equilibrio entre coherencia y rendimiento. Estos modelos de coherencia le permiten compilar aplicaciones distribuidas globalmente con facilidad.

## <a id="Next Steps"></a>Pasos siguientes

Lea más sobre la distribución global en estos artículos:

* [Distribución global en segundo plano](global-dist-under-the-hood.md)
* [Configuración de los clientes para el hospedaje múltiple](how-to-manage-database-account.md#configure-clients-for-multi-homing)
* [Incorporación o eliminación de regiones de una cuenta de Azure Cosmos](how-to-manage-database-account.md#addremove-regions-from-your-database-account)
* [Creación de una directiva personalizada de resolución de conflictos para cuentas de API SQL](how-to-manage-conflicts.md#create-a-custom-conflict-resolution-policy)