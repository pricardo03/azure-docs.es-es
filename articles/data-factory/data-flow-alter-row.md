---
title: Transformación Alterar fila de Azure Data Factory Mapping Data Flow
description: Cómo actualizar un destino de base de datos mediante la transformación Alterar fila de Azure Data Factory Flow
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 03/12/2019
ms.openlocfilehash: f0ac5bb36079983b10e4d86cc776bd4e5ee6817d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65520157"
---
# <a name="azure-data-factory-alter-row-transformation"></a>Transformación Alterar fila de Azure Data Factory

Use la transformación Alterar fila para establecer directivas de inserción, eliminación, actualización y upsert en las filas. Puede agregar las condiciones de uno a varios como expresiones. Cada una de esas condiciones puede dar lugar a la inserción, actualización, eliminación o upsert de una fila (o filas). La transformación Alterar fila puede generar tanto acciones DDL como DML en la base de datos.

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

![Configuración de Alterar fila](media/data-flow/alter-row1.png "Alter Row Settings")

> [!NOTE]
> Las transformaciones Alterar fila solo funcionarán en los receptores de base de datos del flujo de datos. Las acciones que asigne a las filas (inserción, actualización, eliminación, upsert) no se producirán durante las sesiones de depuración. Debe agregar una tarea de ejecución de flujo de datos a una canalización y usar la depuración de la canalización o desencadenadores para aplicar las directivas de alteración de fila en las tablas de base de datos.

## <a name="view-policies"></a>Visualización de directivas

Cambie el modo de depuración de flujo de datos para activarlo y, a continuación, vea los resultados de las directivas de alteración de fila en el panel Vista previa de los datos. La ejecución de la transformación Alterar fila en el modo de depuración de flujo de datos no producirá acciones DDL ni DML con respecto a su destino. Para que esas acciones tengan lugar, ejecute el flujo de datos dentro de una actividad de ejecución de flujo de datos dentro de una canalización.

![Directivas de alteración de fila](media/data-flow/alter-row3.png "Alter Row Policies")

Le permitirá comprobar y ver el estado de cada fila en función de sus condiciones. Hay representaciones de icono para cada acción de inserción, actualización, eliminación y upsert que tendrá lugar en el flujo de datos, que indica qué acción se llevará a cabo cuando se ejecute el flujo de datos dentro de una canalización.

## <a name="sink-settings"></a>Configuración del receptor

Debe tener un tipo de receptor de base de datos para que Alterar fila funcione. En la configuración del receptor, debe establecer cada acción para que se permita.

![Receptor Alterar fila](media/data-flow/alter-row2.png "Alter Row Sink")

El comportamiento predeterminado en ADF Data Flow con los receptores de base de datos es insertar las filas. Si desea permitir también las actualizaciones, operaciones upsert y eliminaciones, también debe activar estos cuadros en el receptor para permitir las acciones.

> [!NOTE]
> Si las inserciones, actualizaciones u operaciones upsert modifican el esquema de la tabla de destino en el receptor, se producirá un error en el flujo de datos. Para modificar el esquema de destino en la base de datos, debe elegir la opción "Volver a crear la tabla" en el receptor. De esta forma se quita la tabla y se vuelve a crear con la nueva definición de esquema.

## <a name="next-steps"></a>Pasos siguientes

Después de la transformación Alterar fila, puede desear [recibir los datos en un almacén de datos de destino](data-flow-sink.md).
