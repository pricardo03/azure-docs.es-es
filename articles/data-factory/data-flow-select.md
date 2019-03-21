---
title: Transformación Selección de Azure Data Factory Mapping Data Flow
description: Transformación Selección de Azure Data Factory Mapping Data Flow
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/12/2019
ms.openlocfilehash: bc83b41067d587adce41658a2c4b3d68969750ba
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/23/2019
ms.locfileid: "56729345"
---
# <a name="azure-data-factory-mapping-data-flow-select-transformation"></a>Transformación Selección de Azure Data Factory Mapping Data Flow

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Use esta transformación para la selección de columnas (reducir el número de columnas) o para crear alias de nombres de columnas y secuencias.

La transformación Selección permite crear un alias de una secuencia completa, o de las columnas de esa secuencia, asignarles nombres diferentes (alias) y, después, hacer referencia a esos nuevos nombres en el flujo de datos. Esta transformación resulta útil en escenarios de autocombinación. Para implementar una autocombinación en ADF Data Flow, se toma una secuencia, se crea una la rama con "Nueva rama" e, inmediatamente después, se agrega una transformación "Selección". Ahora, la secuencia tiene un nuevo nombre que puede usar para combinarla con la secuencia original y crear una autocombinación:

![Autocombinación](media/data-flow/selfjoin.png "Self-join")

En el diagrama anterior, la transformación Selección se encuentra en la parte superior. Lo que está haciendo es asignar a la secuencia original el alias "OrigSourceBatting". En la transformación Combinación resaltada a continuación, puede ver que usamos esta secuencia con el alias establecido en Selección como combinación de la derecha, lo que nos permite hacer referencia a la misma clave tanto en el lado derecho como el izquierdo de la combinación interna.

También puede usar Selección como una manera de anular la selección de columnas del flujo de datos. Por ejemplo, si tiene 6 columnas definidas en el receptor, pero solo desea elegir tres específicas para transformarlas y enviarlas al receptor, puede seleccionar solo esas tres con la transformación Selección.

> [!NOTE]
> Debe desactivar "Seleccionar todo" para seleccionar solo columnas específicas.

Opciones

El valor predeterminado de "Selección" es incluir todas las columnas de entrada y mantener los nombres originales. Para crear un alias de la secuencia, puede establecer el nombre de la transformación Selección.

Para crear alias de columnas individuales, desactive la opción "Seleccionar todo" y use la asignación de columnas en la parte inferior.

![Transformación Selección](media/data-flow/select001.png "Select Alias")
