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
ms.openlocfilehash: 68ac603964593892dfcbc3488b4e6f2c91a0eb92
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2018
ms.locfileid: "50238281"
---
# <a name="global-data-distribution-with-azure-cosmos-db"></a>Distribución de datos global con Azure Cosmos DB

Muchas de las aplicaciones actuales se ejecutan o esperan ejecutarse a escala planetaria. Siempre están disponibles y al acceso de usuarios de todo el mundo. Administrar la distribución global de los datos que estas aplicaciones utilizan mientras se brinda un alto rendimiento y una alta disponibilidad es un problema complicado. Cosmos DB es un servicio de base de datos distribuida de manera global que se diseñó desde el principio para superar estos desafíos.

Cosmos DB es un servicio fundamental de Azure que está disponible en todas las [regiones de Azure](https://azure.microsoft.com/global-infrastructure/regions/) de manera predeterminada. Microsoft opera centros de datos de Azure en más de 50 regiones en todo el mundo y sigue expandiéndose para satisfacer las necesidades cada vez más grandes de los clientes. Microsoft trabaja con el servicio Cosmos DB todo el día, todos los días, por lo que el usuario puede centrarse en sus aplicaciones. Cuando se crea una cuenta de Cosmos DB, es el usuario quien decide en qué región o regiones se debe implementar.

Los clientes de Cosmos DB pueden configurar sus bases de datos para que se distribuyan de manera global y estén disponibles en cualquier parte de 1 a más de 50 regiones de Azure. Para disminuir la latencia, debe colocar los datos lo más cerca posible de la ubicación del usuario, por lo que decidir en cuántas regiones y en qué regiones ejecutarse depende del alcance global de la aplicación y de la ubicación de los usuarios. Cosmos DB replica sin problemas todos los datos dentro de la cuenta de Cosmos a todas las regiones configuradas. Cosmos DB proporciona una sola imagen de sistema de los contenedores y la base de datos de Cosmos, para que la aplicación pueda leer y escribir de manera local. Con Cosmos DB, puede agregar o quitar las regiones asociadas con su cuenta en cualquier momento. La aplicación no necesita estar en pausa ni volver a implementarse y sigue teniendo alta disponibilidad para atender a los datos en todo momento, gracias a las funcionalidades de hospedaje múltiple que proporciona el servicio.

## <a name="key-benefits-of-global-distribution"></a>Ventajas clave de distribución global

**Compilación de aplicaciones activas/activas globales de manera sencilla**: con la funcionalidad de arquitectura multimaestro, cada región es grabable (además de legible), lo que permite una escalabilidad de escritura elástica ilimitada, 99,999 % de disponibilidad de lectura y escritura, en todo el mundo, y lecturas y escrituras rápidas garantizadas atendidas en menos de 10 milisegundos, en el percentil 99.  

Con las API de hospedaje múltiple de Azure Cosmos DB, la aplicación siempre sabe cuál es la región más cercana y envía las solicitudes a esa región. La región más cercana se identifica sin ningún cambio de configuración. Como agrega y quita regiones desde la cuenta de Cosmos DB, no es necesario volver a implementar la aplicación y esta sigue teniendo alta disponibilidad.

**Compilación de aplicaciones de alta capacidad de respuesta**: la aplicación se puede diseñar fácilmente para que realice lecturas y escrituras en tiempo real, con latencias en milisegundos de un dígito, en todas las regiones que eligió para la base de datos.  De manera interna, Azure Cosmos DB controla la replicación de datos entre regiones de una manera que garantiza el nivel de coherencia elegido para la cuenta de Cosmos.

Muchas aplicaciones se beneficiarán de las mejoras de rendimiento que se incluyen con la capacidad de realizar escrituras en varias regiones (locales). Algunas aplicaciones, como las que requieren coherencia fuerte, preferirán canalizar todas las escrituras a una sola región. Cosmos DB admite ambas configuraciones, tanto para una región como para varias regiones.

**Compilación de aplicaciones de alta disponibilidad**: ejecutar una base de datos en varias regiones mejora la disponibilidad de la base de datos. Si hay alguna región no disponible, otras regiones administrarán automáticamente las solicitudes de la aplicación. Azure Cosmos DB ofrece el 99,999 % de disponibilidad de lectura y escritura para las bases de datos de varias regiones.

**Continuidad empresarial durante interrupciones regionales**: Azure Cosmos DB admite la [conmutación automática por error](how-to-manage-database-account.md#enable-automatic-failover-for-your-cosmos-account) durante una interrupción regional. Además, durante una interrupción regional, Cosmos DB sigue manteniendo sus SLA de rendimiento, coherencia, disponibilidad y latencia. Para ayudar a garantizar que toda la aplicación tiene una alta disponibilidad, Azure Cosmos DB ofrece la API de conmutación por error manual para simular una interrupción regional. Con esta API, puede realizar maniobras de continuidad empresarial regulares y estar preparado.

**Escalabilidad global de lectura y escritura**: con la funcionalidad de arquitectura multimaestro, puede escalar de manera elástica el rendimiento de lectura y escritura en todo el mundo. La funcionalidad de arquitectura multimaestro garantiza que el rendimiento que la aplicación configura en un contenedor o una base de datos de Azure Cosmos DB se entrega en todas las regiones y lo protegen [SLA con respaldo financiero](https://aka.ms/acdbsla).

**Varios modelos de coherencia bien definidos**: el protocolo de replicación de Azure Cosmos DB está diseñado para ofrecer cinco modelos de coherencia bien definidos, prácticos e intuitivos, cada uno con un equilibrio claro entre coherencia y rendimiento. Estos modelos de coherencia le permiten compilar aplicaciones correctas distribuidas correctamente con facilidad.

## <a id="Next Steps"></a>Pasos siguientes

Lea más sobre la distribución global en estos artículos:

* [Distribución global en segundo plano](global-dist-under-the-hood.md)
* [Configuración de los clientes para el hospedaje múltiple](how-to-manage-database-account.md#configure-clients-for-multi-homing)
* [Incorporación o eliminación de regiones de una base de datos](how-to-manage-database-account.md#addremove-regions-from-your-database-account)
* [Creación de una directiva personalizada de resolución de conflictos para cuentas de API SQL](how-to-manage-conflicts.md#create-a-custom-conflict-resolution-policy)