---
title: Transformación Alterar fila de Azure Data Factory Mapping Data Flow
description: Cómo actualizar un destino de base de datos mediante la transformación Alterar fila de Azure Data Factory Flow
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 03/12/2019
ms.openlocfilehash: e2cd69d5977b8ad1d9be2a71a006579fe3abfd23
ms.sourcegitcommit: 47b00a15ef112c8b513046c668a33e20fd3b3119
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/22/2019
ms.locfileid: "69971247"
---
# <a name="azure-data-factory-alter-row-transformation"></a>Transformación Alterar fila de Azure Data Factory

Use la transformación Alterar fila para establecer directivas de inserción, eliminación, actualización y upsert en las filas. Puede agregar las condiciones de uno a varios como expresiones. Estas condiciones se deben especificar en orden de prioridad, ya que cada fila se marcará con la directiva correspondiente a la primera expresión coincidente. Cada una de esas condiciones puede dar lugar a la inserción, actualización, eliminación o upsert de una fila (o filas). La transformación Alterar fila puede generar tanto acciones DDL como DML en la base de datos.

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

![Configuración de Alterar fila](media/data-flow/alter-row1.png "Alter Row Settings")

> [!NOTE]
> Las transformaciones Alterar fila solo funcionarán en los receptores de base de datos del flujo de datos. Las acciones que asigne a las filas (inserción, actualización, eliminación, upsert) no se producirán durante las sesiones de depuración. Debe agregar una tarea de ejecución de flujo de datos a una canalización y usar la depuración de la canalización o desencadenadores para aplicar las directivas de alteración de fila en las tablas de base de datos.

## <a name="indicate-a-default-row-policy"></a>Indicación de una directiva de fila predeterminada

Cree una transformación Alterar fila y especifique una directiva de fila con una condición de `true()`. Cada fila que no cumpla alguna de las expresiones definidas previamente se marcará para la directiva de fila especificada. De forma predeterminada, cada fila que no cumple ninguna expresión condicional se marcará para `Insert`.

![Directiva Alterar fila 1](media/data-flow/alter-row4.png "Alter row one policy")

> [!NOTE]
> Para marcar todas las filas con una directiva, puede crear una condición para dicha directiva y especificar la condición como `true()`.

## <a name="view-policies"></a>Visualización de directivas

Active el modo de depuración de flujo de datos para ver los resultados de las directivas de alteración de fila en el panel Vista previa de los datos. La ejecución de la transformación Alterar fila en el modo de depuración de flujo de datos no producirá acciones DDL ni DML con respecto a su destino. Para que esas acciones tengan lugar, ejecute el flujo de datos dentro de una actividad de ejecución de flujo de datos en una canalización.

![Directivas de alteración de fila](media/data-flow/alter-row3.png "Alter Row Policies")

Le permitirá comprobar y ver el estado de cada fila en función de sus condiciones. Hay representaciones de icono para cada acción de inserción, actualización, eliminación y upsert que tendrá lugar en el flujo de datos, que indica qué acción se llevará a cabo cuando se ejecute el flujo de datos dentro de una canalización.

## <a name="sink-settings"></a>Configuración del receptor

Debe tener un tipo de receptor de base de datos para que Alterar fila funcione. En la configuración del receptor, debe establecer cada acción correspondiente a las condiciones de Alterar fila que se van a permitir.

![Receptor Alterar fila](media/data-flow/alter-row2.png "Alter Row Sink")

El comportamiento predeterminado en ADF Data Flow con los receptores de base de datos es insertar las filas. Si desea permitir también las actualizaciones, operaciones upsert y eliminaciones, también debe activar estos cuadros en el receptor para permitir las acciones.

> [!NOTE]
> Si las inserciones, actualizaciones u operaciones upsert modifican el esquema de la tabla de destino en el receptor, se producirá un error en el flujo de datos. Para modificar el esquema de destino en la base de datos, debe elegir la opción "Volver a crear la tabla" en el receptor. De esta forma se quita la tabla y se vuelve a crear con la nueva definición de esquema.

## <a name="next-steps"></a>Pasos siguientes

Después de la transformación Alterar fila, puede desear [recibir los datos en un almacén de datos de destino](data-flow-sink.md).
