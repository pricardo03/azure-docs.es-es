---
title: Transformación Agregar de Azure Data Factory Mapping Data Flow
description: Transformación Agregar de Azure Data Factory Data Flow
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/01/2019
ms.openlocfilehash: 7b488b243c0520befb6b5470598f460b5a759fed
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/23/2019
ms.locfileid: "56730043"
---
# <a name="azure-data-factory-mapping-data-flow-aggregate-transformation"></a>Transformación Agregar de Azure Data Factory Mapping Data Flow

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

En la transformación Agregar se definen las agregaciones de columnas de las secuencias de datos. En el Generador de expresiones, puede definir distintos tipos de agregaciones (por ejemplo, SUM, MIN, MAX, COUNT, etc.) y crear un nuevo campo en la salida que incluya estas agregaciones con campos de agrupación opcionales.

![Opciones de la transformación Agregar](media/data-flow/agg.png "aggregate 1")

## <a name="group-by"></a>Agrupar por
(Opcional) Elija una cláusula Agrupar por para la agregación y use el nombre de una columna existente o un nombre nuevo. Use "Agregar columna" para agregar más cláusulas group-by y haga clic en el cuadro de texto junto al nombre de columna para abrir el Generador de expresiones. Seleccione solo una columna o una combinación de columnas o expresiones para su agrupación.

## <a name="the-aggregate-column-tab"></a>Pestaña Agregar columna 
(Obligatorio) Elija la pestaña Agregar columna para generar las expresiones de agregación. Puede elegir una columna existente para sobrescribir el valor con la agregación o crear un nuevo campo con el nuevo nombre para la agregación. La expresión que vaya a usar para la agregación se escribirá en el cuadro de la derecha, junto al selector de nombre de columna. Al hacer clic en ese cuadro de texto, se abrirá el Generador de expresiones.

![Opciones de la transformación Agregar](media/data-flow/agg2.png "aggregator")

## <a name="data-preview-in-expression-builder"></a>Vista previa de datos en el Generador de expresiones

En el modo de depuración, el Generador de expresiones no puede generar vistas previas de los datos con las funciones de agregado. Para ver las vistas previas de los datos con las transformaciones Agregar, cierre el Generador de expresiones y vea el perfil de datos en el diseñador de flujos de datos.
