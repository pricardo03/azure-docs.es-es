---
title: Hacer que expiren datos en Azure Cosmos DB con período de vida
description: Con TTL, Microsoft Azure Cosmos DB ofrece la posibilidad de que los documentos se purguen automáticamente del sistema tras un período de tiempo.
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/21/2019
ms.author: rimman
ms.reviewer: sngun
ms.openlocfilehash: 692e0ec575904ff0a70b8c73268d2df62e776bb6
ms.sourcegitcommit: 59fd8dc19fab17e846db5b9e262a25e1530e96f3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2019
ms.locfileid: "65978788"
---
# <a name="time-to-live-ttl-in-azure-cosmos-db"></a>Período de vida (TTL) en Azure Cosmos DB 

Con **período de vida** o TTL, Azure Cosmos DB proporciona la capacidad de eliminar automáticamente elementos de un contenedor después de un determinado período de tiempo. De forma predeterminada, puede establecer el período de vida en el nivel de contenedor e invalidar el valor en cada elemento. Después de establecer el TTL en el nivel de contenedor o de elemento, Azure Cosmos DB eliminará automáticamente estos elementos cuando haya pasado el período de tiempo seleccionado tras la hora de la última modificación. El valor del período de vida se configura en segundos. Cuando se configura el TTL, el sistema eliminará automáticamente los elementos expirados en función del valor TTL, sin necesidad de una operación de eliminación explícitamente emitido por la aplicación cliente.

## <a name="time-to-live-for-containers-and-items"></a>Período de vida para contenedores y elementos

El tiempo de vida se establece en segundos, y se interpreta como una diferencia desde el momento en que se modificó por última vez un elemento. Puede establecer el período de vida en un contenedor o en un elemento dentro del contenedor:

1. **Período de vida en un contenedor** (se establece mediante `DefaultTimeToLive`):

   - Si no existe (o está establecido en NULL), los elementos no expiran automáticamente.

   - Si existe y el valor se establece en "-1", equivale a infinito y elementos no caducan de forma predeterminada.

   - Si existe y el valor se establece en un número *"n"* : los elementos expirarán *"n"* segundos después de su última hora de modificación.

2. **Período de vida en un elemento** (se establece mediante `ttl`):

   - Esta propiedad es aplicable solo si está presente el valor `DefaultTimeToLive` y no está establecido en NULL para el contenedor primario.

   - Si está presente, invalida el valor de `DefaultTimeToLive` del contenedor primario.

## <a name="time-to-live-configurations"></a>Configuraciones de período de vida

* Si se establece el TTL *"n"* en un contenedor, a continuación, los elementos de ese contenedor expirará después *n* segundos.  Si hay elementos en el mismo contenedor que tienen su propio tiempo de live, se establece en -1 (lo que indica que no caduquen) o si el período de vida de configuración con un número diferente invalidar algunos elementos, estos elementos expiran según su propio valor TTL configurado. 

* Si no se establece el TTL en un contenedor, el período de vida en un elemento de este contenedor no tiene ningún efecto. 

* Si el TTL en un contenedor se establece en -1, un elemento de este contenedor que tenga el período de vida establecido en n, expirará después de n segundos, y los elementos restantes no expirarán. 

La eliminación de elementos en función del TTL es gratuita. No hay ningún costo adicional (es decir, no se consume ninguna RU adicional) cuando se elimina el elemento como resultado de la expiración de TTL.

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre cómo configurar el período de vida en los siguientes artículos:

* [Cómo configurar el período de vida](how-to-time-to-live.md)
