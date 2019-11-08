---
title: Transformación Alterar fila de flujo de datos de asignación de Azure Data Factory
description: Cómo actualizar un destino de base de datos mediante la transformación Alterar fila de flujo de datos de asignación de Azure Data Factory
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 03/12/2019
ms.openlocfilehash: 7a782c62165aa6f2641c2ebe8e4600198ec373c5
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73486202"
---
# <a name="azure-data-factory-alter-row-transformation"></a>Transformación Alterar fila de Azure Data Factory

Use la transformación Alterar fila para establecer directivas de inserción, eliminación, actualización y upsert en las filas. Puede agregar las condiciones de uno a varios como expresiones. Estas condiciones se deben especificar en orden de prioridad, ya que cada fila se marcará con la directiva correspondiente a la primera expresión coincidente. Cada una de esas condiciones puede dar lugar a la inserción, actualización, eliminación o upsert de una fila (o filas). La transformación Alterar fila puede generar tanto acciones DDL como DML en la base de datos.



![Configuración de alteración de fila](media/data-flow/alter-row1.png "Configuración de alteración de fila")

> [!NOTE]
> Las transformaciones Alter Row (Alterar fila) solo funcionarán en los receptores de base de datos o CosmosDB del flujo de datos. Las acciones que asigne a las filas (inserción, actualización, eliminación, upsert) no se producirán durante las sesiones de depuración. Debe agregar una tarea de ejecución de flujo de datos a una canalización y usar la depuración de la canalización o desencadenadores para aplicar las directivas de alteración de fila en las tablas de base de datos.

## <a name="indicate-a-default-row-policy"></a>Indicación de una directiva de fila predeterminada

Cree una transformación Alterar fila y especifique una directiva de fila con una condición de `true()`. Cada fila que no cumpla alguna de las expresiones definidas previamente se marcará para la directiva de fila especificada. De forma predeterminada, cada fila que no cumple ninguna expresión condicional se marcará para `Insert`.

![Directiva Alterar fila 1](media/data-flow/alter-row4.png "Directiva Alterar fila 1")

> [!NOTE]
> Para marcar todas las filas con una directiva, puede crear una condición para dicha directiva y especificar la condición como `true()`.

## <a name="view-policies"></a>Visualización de directivas

Active el modo de depuración de flujo de datos para ver los resultados de las directivas de alteración de fila en el panel Vista previa de los datos. La ejecución de la transformación Alterar fila en el modo de depuración de flujo de datos no producirá acciones DDL ni DML con respecto a su destino. Para que esas acciones tengan lugar, ejecute el flujo de datos dentro de una actividad de ejecución de flujo de datos en una canalización.

![Directivas de alteración de fila](media/data-flow/alter-row3.png "Directivas de alteración de fila")

Le permitirá comprobar y ver el estado de cada fila en función de sus condiciones. Hay representaciones de icono para cada acción de inserción, actualización, eliminación y upsert que tendrá lugar en el flujo de datos, que indica qué acción se llevará a cabo cuando se ejecute el flujo de datos dentro de una canalización.

## <a name="sink-settings"></a>Configuración del receptor

Debe tener un tipo de receptor de base de datos para que Alterar fila funcione. En la configuración del receptor, debe establecer cada acción correspondiente a las condiciones de Alterar fila que se van a permitir.

![Receptor de alteración de fila](media/data-flow/alter-row2.png "Receptor de alteración de fila")

El comportamiento predeterminado en ADF Data Flow con los receptores de base de datos es insertar las filas. Si desea permitir también las actualizaciones, operaciones upsert y eliminaciones, también debe activar estos cuadros en el receptor para permitir las acciones.

> [!NOTE]
> Si las inserciones, actualizaciones u operaciones upsert modifican el esquema de la tabla de destino en el receptor, se producirá un error en el flujo de datos. Para modificar el esquema de destino en la base de datos, debe elegir la opción "Volver a crear la tabla" en el receptor. De esta forma se quita la tabla y se vuelve a crear con la nueva definición de esquema.

## <a name="next-steps"></a>Pasos siguientes

Después de la transformación Alterar fila, puede desear [recibir los datos en un almacén de datos de destino](data-flow-sink.md).
