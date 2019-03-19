---
title: Hacer que expiren datos en Azure Cosmos DB con período de vida
description: Con TTL, Microsoft Azure Cosmos DB ofrece la posibilidad de que los documentos se purguen automáticamente del sistema tras un período de tiempo.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/14/2018
ms.author: mjbrown
ms.reviewer: sngun
ms.openlocfilehash: f9dec5b3aeb951316985c965de70a372f55b8225
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/07/2019
ms.locfileid: "57549194"
---
# <a name="time-to-live-in-azure-cosmos-db"></a>Período de vida en Azure Cosmos DB 

Mediante el "período de vida" o TTL, Azure Cosmos DB proporciona la capacidad de eliminar automáticamente elementos de un contenedor después de un determinado período de tiempo. De forma predeterminada, puede establecer el período de vida en el nivel de contenedor e invalidar el valor en cada elemento. Después de establecer el TTL en el nivel de contenedor o de elemento, Azure Cosmos DB eliminará automáticamente estos elementos cuando haya pasado el período de tiempo seleccionado tras la hora de la última modificación. El valor del período de vida se configura en segundos. Cuando se configura el TTL, el sistema elimina automáticamente los elementos expirados en función del valor de TTL, a diferencia de una operación de eliminación que se emite explícitamente desde la aplicación cliente.

## <a name="time-to-live-for-containers-and-items"></a>Período de vida para contenedores y elementos

El período de vida se establece en segundos y se interpreta como una delta desde el momento en que se modificó por última vez un elemento. Puede establecer el período de vida en un contenedor o en un elemento dentro del contenedor:

1. **Período de vida en un contenedor** (se establece mediante `DefaultTimeToLive`):

   - Si no existe (o está establecido en NULL), los elementos no expiran automáticamente.

   - Si existe y el valor se ha establecido en "-1", es igual a infinito y los documentos no expiran de forma predeterminada.

   - Si existe y el valor se ha establecido en un número ("n"), los elementos expiran "n" segundos después de la última modificación.

2. **Período de vida en un elemento** (se establece mediante `ttl`):

   - Esta propiedad es aplicable solo si está presente el valor `DefaultTimeToLive` y no está establecido en NULL para el contenedor primario.

   - Si está presente, invalida el valor de `DefaultTimeToLive` del contenedor primario.

## <a name="time-to-live-configurations"></a>Configuraciones de período de vida

* Si el TTL se establece en "n" en un contenedor, los elementos de ese contenedor expirarán después de n segundos.  Si hay elementos en el mismo contenedor que tengan su propio período de vida establecido en -1 (lo que indica que no expiran) o si algunos elementos han invalidado la configuración del período de vida con un número distinto, estos elementos expirarán según el valor de TTL configurado. 

* Si no se establece el TTL en un contenedor, el período de vida en un elemento de este contenedor no tiene ningún efecto. 

* Si el TTL en un contenedor se establece en -1, un elemento de este contenedor que tenga el período de vida establecido en n, expirará después de n segundos, y los elementos restantes no expirarán. 

La eliminación de elementos en función del TTL es gratuita. No hay ningún costo adicional (es decir, no se consume ninguna RU adicional) cuando se elimina el elemento como resultado de la expiración de TTL.

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre cómo configurar el período de vida en los siguientes artículos:

* [Cómo configurar el período de vida](how-to-time-to-live.md)
