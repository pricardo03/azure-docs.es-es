---
title: Trabajo con la biblioteca de procesadores de fuente de cambios en Azure Cosmos DB
description: Uso de la biblioteca de procesadores de fuente de cambios de Azure Cosmos DB.
author: rafats
manager: kfile
ms.service: cosmos-db
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 11/06/2018
ms.author: rafats
ms.openlocfilehash: 9d427a8001112e4994597b86579d85156f94a870
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2018
ms.locfileid: "51628904"
---
# <a name="using-the-azure-cosmos-db-change-feed-processor-library"></a>Uso de la biblioteca de procesadores de fuente de cambios de Azure Cosmos DB

La [biblioteca de procesadores de fuente de cambios de Azure Cosmos DB](sql-api-sdk-dotnet-changefeed.md) le ayuda a distribuir el procesamiento de eventos entre varios consumidores. Esta biblioteca simplifica la lectura de los cambios a través de las particiones y varios subprocesos que trabajan en paralelo.

La ventaja principal de la biblioteca de procesadores de fuente de cambios es que no tiene que administrar cada partición y token de continuación, ni que sondear manualmente cada contenedor.

La biblioteca de procesadores de fuente de cambios simplifica la lectura de los cambios a través de las particiones y varios subprocesos que trabajan en paralelo. Administra automáticamente la lectura de los cambios a través de las particiones que usan un mecanismo de concesión. Como puede ver en la imagen siguiente, si inicia dos clientes que usan la biblioteca de procesadores de fuente de cambios, dividen el trabajo entre ellos. A medida que continúe aumentando el número de clientes, siguen dividiéndose el trabajo entre ellos.

![Uso de la biblioteca de procesadores de fuente de cambios de Azure Cosmos DB](./media/change-feed-processor/change-feed-output.png)

El cliente de la izquierda se inició en primer lugar y comenzó la supervisión de todas las particiones; a continuación, se inició el segundo cliente y el primero dejó ir a algunas de las concesiones para el segundo cliente. Se trata de una manera eficaz de distribuir el trabajo entre distintas máquinas y clientes.

Si tiene dos funciones de Azure sin servidor que supervisan el mismo contenedor y usan la misma concesión, las dos funciones pueden obtener documentos diferentes, según el modo en que la biblioteca de procesadores decida procesar las particiones.

## <a name="implementing-the-change-feed-processor-library"></a>Implementación de la biblioteca de procesadores de fuente de cambios

Hay cuatro componentes principales de la implementación de la biblioteca de procesadores de fuente de cambios: 

1. **El contenedor supervisado:** El contenedor supervisado tiene los datos a partir de los cuales se genera la fuente de cambios. Todas las inserciones y cambios realizados en el contenedor supervisado se reflejan en la fuente de cambios del contenedor.

1. **El contenedor de concesión:** El contenedor de concesión coordina el procesamiento de la fuente de cambios entre varios trabajadores. Se usa un contenedor independiente para almacenar las concesiones con una concesión por partición. Resulta ventajoso almacenar este contenedor de concesión en una cuenta diferente con la región de escritura más cerca de donde se está ejecutando el procesador de fuente de cambios. Un objeto de concesión contiene los siguientes atributos:

   * Propietario: Especifica el host que posee la concesión.

   * Continuación: Especifica la posición (token de continuación) en la fuente de cambios para una partición determinada.

   * Marca de tiempo: Última vez que se actualizó la concesión; la marca de tiempo se puede usar para comprobar si la concesión se considera expirada.

1. **Host de procesador:** Cada host determina cuántas particiones se van a procesar según la cantidad de otras instancias de host que tienen concesiones activas.

   * Cuando se inicia un host, adquiere concesiones para equilibrar la carga de trabajo en todos los hosts. Un host renueva periódicamente concesiones, por lo que las concesiones permanecen activas.

   * Un host establece puntos de control en el último token de continuación para su concesión para cada lectura. Para garantizar la seguridad de simultaneidad, un host comprueba ETag para cada actualización de concesión. También se admiten otras estrategias de puntos de control.

   * Tras el cierre, un host libera todas las concesiones pero mantiene la información de continuación, por lo que puede reanudar la lectura más adelante desde el punto de control almacenado.

   Actualmente, el número de hosts no puede ser mayor que el número de particiones (concesiones).

1. **Consumidores:** Los consumidores, o trabajadores, son subprocesos que realizan el procesamiento de fuentes de cambios iniciado por cada host. Cada host de procesador puede tener varios consumidores. Cada consumidor lee la fuente de cambios de la partición a la que se ha asignado y notifica a su host los cambios y las concesiones expiradas.

Para comprender mejor cómo funcionan estos cuatro elementos del procesador de fuente de cambios juntos, echemos un vistazo a un ejemplo en el diagrama siguiente. La colección supervisada almacena documentos y usa "City" como clave de partición. Vemos que la partición azul contiene documentos con el campo "City" de "A-E", etc. Hay dos hosts, cada uno con dos consumidores, que leen las cuatro particiones en paralelo. Las flechas muestran a los consumidores leyendo un punto específico de la fuente de cambios. En la primera partición, el azul más oscuro representa los cambios no leídos, mientras que el azul claro representa los cambios ya leídos de la fuente de cambios. Los hosts utilizan la colección de concesión para almacenar un valor de "continuación" para realizar un seguimiento de la posición de lectura actual para cada consumidor.

![Ejemplo de procesador de fuente de cambios](./media/change-feed-processor/changefeedprocessor.png)

### <a name="change-feed-and-provisioned-throughput"></a>Fuente de cambios y rendimiento aprovisionado

Se le cobrarán las RU consumidas, puesto que la entrada y la salida de datos de contenedores de Cosmos siempre consumen RU. Se le cobrarán las RU consumidas por el contenedor de concesión.

## <a name="additional-resources"></a>Recursos adicionales

* [Biblioteca de procesadores de fuente de cambios de Azure Cosmos DB](sql-api-sdk-dotnet-changefeed.md)
* [Paquete de NuGet](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.ChangeFeedProcessor/)
* [Ejemplos adicionales en GitHub](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/ChangeFeedProcessor)

## <a name="next-steps"></a>Pasos siguientes

Ahora puede obtener más información acerca de las fuentes de cambios en los siguientes artículos:

* [Introducción a la fuente de cambios](change-feed.md)
* [Maneras de leer la fuente de cambios](read-change-feed.md)
* [Uso de la fuente de cambios con Azure Functions](change-feed-functions.md)