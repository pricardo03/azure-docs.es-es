---
title: 'Transformación Agregar en Mapping Data Flow: Azure Data Factory | Microsoft Docs'
description: Aprenda cómo agregar datos a escala en Azure Data Factory con la transformación Agregar en Mapping Data Flow.
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/01/2019
ms.openlocfilehash: 0201cbdd05cd8aae4afb92b459bf58fb5ff6a142
ms.sourcegitcommit: 11265f4ff9f8e727a0cbf2af20a8057f5923ccda
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/08/2019
ms.locfileid: "72026979"
---
# <a name="aggregate-transformation-in-mapping-data-flow"></a>Transformación Agregar en Mapping Data Flow 



En la transformación Agregar se definen las agregaciones de columnas de las secuencias de datos. Mediante el Generador de expresiones, puede definir diferentes tipos de agregaciones, tales como SUM, MIN, MAX y COUNT, que se puede agrupar por columnas calculadas o existentes.

## <a name="group-by"></a>Agrupar por
Seleccione una columna existente o cree una nueva columna calculada para usarla como una cláusula Agrupar por para la agregación. Para usar una columna existente, seleccione la columna que desee en la lista desplegable. Para crear una nueva columna calculada, mantenga el mouse sobre la cláusula y haga clic en "Columna calculada". Se abrirá el [Generador de expresiones de Data Flow](concepts-data-flow-expression-builder.md). Una vez creada la columna calculada, escriba el nombre de la columna de salida en el campo "Name as" (Nombre como). Si desea agregar una cláusula Agrupar por adicional, mantenga el mouse sobre una cláusula existente y haga clic en "+".

![Configuración de Agrupar por de la transformación Agregar](media/data-flow/agg.png "Aggregate transformation group by settings")

> [!NOTE]
> Una cláusula Agrupar por es opcional en una transformación Agregar.

## <a name="aggregate-column"></a>Columna de agregación 
Elija la pestaña "Agregados" para generar las expresiones de agregación. Puede elegir una columna existente y sobrescribir el valor con la agregación o crear un nuevo campo con un nuevo nombre. La expresión de agregación se escribirá en el cuadro de la derecha, junto al selector de nombre de columna. Para editar la expresión, haga clic en el cuadro de texto para abrir el Generador de expresiones. Para incorporar una agregación adicional, mantenga el mouse sobre una expresión existente y haga clic en "+" para crear una nueva columna de agregación o un [patrón de columnas](concepts-data-flow-column-pattern.md).

![Configuración de agregación de la transformación Agregar](media/data-flow/agg2.png "Aggregate transformation aggregate settings")

> [!NOTE]
> Cada expresión de agregación debe contener al menos una función de agregado.

> [!NOTE]
> En el modo Depurar, el Generador de expresiones no puede generar vistas previas de los datos con las funciones de agregado. Para ver las vistas previas de los datos para las transformaciones Agregar, cierre el Generador de expresiones y vea los datos a través de la pestaña "Vista previa de los datos".

## <a name="reconnect-rows-and-columns"></a>Reconexión de filas y columnas
Las transformaciones Agregar son muy parecidas a las consultas de selección de agregado de SQL. Las columnas que no estén incluidas en la cláusula Group by o en las funciones de agregado no fluirán a través de la salida de la transformación Agregar. Si quiere incluir otras columnas con la salida de las filas agregadas, debe realizar una de estas dos acciones:

* Usar una función de agregado para incluir esa columna adicional, como Last() o First().
* Volver a unir las columnas antes del agregado mediante el [patrón Self Join](https://mssqldude.wordpress.com/2018/12/20/adf-data-flows-self-join/).

## <a name="next-steps"></a>Pasos siguientes

* Definir un agregación basada en ventanas mediante la [transformación Ventana](data-flow-window.md).
