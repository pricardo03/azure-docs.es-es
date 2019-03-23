---
title: Transformación del receptor del flujo de datos de asignación de Azure Data Factory
description: Transformación del receptor del flujo de datos de asignación de Azure Data Factory
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/03/2019
ms.openlocfilehash: a56f391aa76bd1216fd51d516adb836a2093bcba
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/22/2019
ms.locfileid: "58371146"
---
# <a name="mapping-data-flow-sink-transformation"></a>Transformación del receptor del flujo de datos de asignación

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

![Opciones de receptor](media/data-flow/windows1.png "receptor 1")

Al finalizar la transformación del flujo de datos, puede recibir los datos transformados en un conjunto de datos de destino. En la transformación del receptor, puede elegir la definición del conjunto de datos que quiere usar para los datos de salida de destino. Puede tener tantas transformaciones del receptor como precise el flujo de datos.

Una práctica común para justificar los cambios en los datos de entrada y el desfase de esquema es recibir los datos de salida en una carpeta sin un esquema definido en el conjunto de datos de salida. Puede justificar además todos los cambios de columna en los orígenes si selecciona la opción "Allow Schema Drift" (Permitir el desfase de esquema) en el origen y, luego, asigna automáticamente todos los campos en el receptor.

Si la recepción tiene lugar en un conjunto de datos, puede elegir sobrescribir o anexar el flujo de datos o darlo por erróneo.

También puede elegir la asignación automática para recibir todos los campos de entrada. Si prefiere elegir los campos que quiere recibir en el destino, o si le gustaría cambiar los nombres de los campos en el destino, elija desactivar la asignación automática y luego haga clic en la pestaña de asignación para asignar los campos de salida:

![Opciones del receptor](media/data-flow/sink2.png "receptor 2")

## <a name="output-to-one-file"></a>Salida a un solo archivo
En los tipos de receptor de Azure Blob Storage o Data Lake, la salida de los datos transformados se genera en una carpeta. Spark genera los archivos de datos de salida con particiones según el esquema de partición que se va a usar en la transformación del receptor. Para establecer el esquema de partición, puede hacer clic en la pestaña "Optimize" (Optimizar). Si quiere que ADF combine la salida en un único archivo, haga clic en el botón de radio "Single Partition" (Partición única).

![Opciones del receptor](media/data-flow/opt001.png "opciones del receptor")

## <a name="field-mapping"></a>Asignación de campos

En la pestaña de asignación de la transformación del receptor, puede asignar las columnas de entrada (izquierda) al destino (derecha). Cuando recibe los flujos de datos en los archivos, ADF siempre escribe nuevos archivos en una carpeta. Cuando realiza la asignación a un conjunto de datos de base de datos, puede elegir generar una nueva tabla con este esquema (establezca "Save Policy" [Guardar directiva] en "Overwrite" [Sobrescribir]) o insertar nuevas filas en una tabla existente y asignan los campos al esquema existente.

Puede usar una selección múltiple en la tabla de asignación para vincular varias columnas con un solo clic, desvincular varias columnas o asignar varias filas en el mismo nombre de columna.

Cuando desee siempre toman el conjunto de campos entrantes y asignarlas a un destino como-, establezca la opción "Permitir la desviación en el esquema".

![Asignación de campos](media/data-flow/multi1.png "varias opciones")

Si quiere restablecer las asignaciones de columnas, presione el botón "Remap" (Volver a asignar) y así restablecer las asignaciones.

![Opciones del receptor](media/data-flow/sink1.png "Receptor uno")

![Opciones de receptor](media/data-flow/sink2.png "Receptores")

* Con los receptores, ahora están disponibles las opciones para permitir desfase de esquema y validar esquema. De esta manera, puede indicar a ADF que acepte completamente las definiciones de esquema (desfase de esquema) o que genere un error en el receptor si el esquema cambia (validar esquema).

* Borre la carpeta. ADF truncará el contenido de la carpeta del receptor antes de escribir los archivos de destino en esa carpeta de destino.

## <a name="file-name-options"></a>Opciones de nombre de archivo

   * Valor predeterminado: Permitir que Spark nombre los archivos según los valores predeterminados de PART.
   * Patrón: Especifique un patrón para los archivos de salida. Por ejemplo, "préstamos [n]" creará loans1.csv, loans2.csv,...
   * Por partición: escriba un nombre de archivo por partición.
   * Como datos de columna: establezca el archivo de salida en el valor de una columna.

> [!NOTE]
> Las operaciones de archivos solo se ejecutarán cuando se ejecute la actividad de ejecución del flujo de datos, y no mientras se está en el modo de depuración del flujo de datos.

## <a name="database-options"></a>Opciones de base de datos

* Permitir insert, update, delete, realiza una operación Upsert. El valor predeterminado es permitir que las inserciones. Si desea eliminar filas, upsert o update, primero debe agregar una transformación de fila alter a las filas de la etiqueta a esas acciones específicas. Si se desactiva "Permitir inserción" dejará de ADF inserten nuevas filas del origen.
* Truncar una tabla (elimina todas las filas de la tabla de destino antes de completar el flujo de datos)
* Vuelva a crear tabla (realiza colocar o creación de la tabla de destino antes de completar el flujo de datos)
* Tamaño de lote para grandes cargas de datos. Escriba un número de depósito de escrituras en fragmentos
* Habilitar el almacenamiento provisional: Esto indicará ADF para utilizar Polybase al cargar el almacén de datos de Azure como el conjunto de datos de receptor

> [!NOTE]
> En el flujo de datos, puede pedir ADF para crear una nueva definición de tabla en la base de datos de destino mediante el establecimiento de un conjunto de datos en la transformación de receptor que tiene un nuevo nombre de tabla. En el conjunto de datos SQL, haga clic en "Editar" debajo del nombre de tabla y escriba un nuevo nombre de tabla. A continuación, en la transformación de receptor, activar "Permitir la desviación en el esquema". Seth la configuración de "Importación de esquema" en ninguno.

![Esquema de la transformación de origen](media/data-flow/dataset2.png "esquema SQL")

![Opciones de receptor de SQL](media/data-flow/alter-row2.png "opciones de SQL")

> [!NOTE]
> Al actualizar o eliminar filas en el receptor de la base de datos, debe establecer la columna de clave. Este modo, la fila Alter es capaz de determinar la fila única de DML.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha creado el flujo de datos, agregue una [actividad de ejecución de flujo de datos a la canalización](concepts-data-flow-overview.md).
