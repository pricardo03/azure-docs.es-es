---
title: Transformación del receptor del flujo de datos de asignación de Azure Data Factory
description: Transformación del receptor del flujo de datos de asignación de Azure Data Factory
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/03/2019
ms.openlocfilehash: 381dc2f9f6d3a074af00ba047472719c086f5811
ms.sourcegitcommit: 4bf542eeb2dcdf60dcdccb331e0a336a39ce7ab3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2019
ms.locfileid: "56408415"
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

### <a name="output-settings"></a>Configuración de salida

Al sobrescribir, se trunca la tabla si existe, luego se vuelve a crear y se cargan los datos. Al anexar se insertan las nuevas filas. Si la tabla con el nombre de tabla del conjunto de datos no existe en el ADW de destino, el flujo de datos crea la tabla y luego carga los datos.

Si anula la selección de asignación automática, puede asignar manualmente los campos a la tabla de destino.

![Opciones de ADW del receptor](media/data-flow/adw2.png "receptor de ADW")

#### <a name="field-mapping"></a>Asignación de campos

En la pestaña de asignación de la transformación del receptor, puede asignar las columnas de entrada (izquierda) al destino (derecha). Cuando recibe los flujos de datos en los archivos, ADF siempre escribe nuevos archivos en una carpeta. Cuando realiza la asignación a un conjunto de datos de base de datos, puede elegir generar una nueva tabla con este esquema (establezca "Save Policy" [Guardar directiva] en "Overwrite" [Sobrescribir]) o insertar nuevas filas en una tabla existente y asignan los campos al esquema existente.

Puede usar la selección múltiple en la tabla de asignación para vincular varias columnas con un solo clic, desvincular varias columnas o asignar varias filas al mismo nombre de columna.

![Asignación de campos](media/data-flow/multi1.png "varias opciones")

Si quiere restablecer las asignaciones de columnas, presione el botón "Remap" (Volver a asignar) y así restablecer las asignaciones.

![Conexiones](media/data-flow/maxcon.png "Conexiones")

### <a name="updates-to-sink-transformation-for-adf-v2-ga-version"></a>Actualizaciones de la transformación del receptor para la versión V2 GA de ADF

![Opciones del receptor](media/data-flow/sink1.png "Receptor uno")

![Opciones de receptor](media/data-flow/sink2.png "Receptores")

* Con los receptores, ahora están disponibles las opciones para permitir desfase de esquema y validar esquema. De esta manera, puede indicar a ADF que acepte completamente las definiciones de esquema (desfase de esquema) o que genere un error en el receptor si el esquema cambia (validar esquema).

* Borre la carpeta. ADF truncará el contenido de la carpeta del receptor antes de escribir los archivos de destino en esa carpeta de destino.

* Opciones de nombre de archivo

   * Valor predeterminado: Permitir que Spark nombre los archivos según los valores predeterminados de PART.
   * Patrón: escriba un nombre para los archivos de salida.
   * Por partición: escriba un nombre de archivo por partición.
   * Como datos de columna: establezca el archivo de salida en el valor de una columna.

> [!NOTE]
> Las operaciones de archivos solo se ejecutarán cuando se ejecute la actividad de ejecución del flujo de datos, y no mientras se está en el modo de depuración del flujo de datos.

Con los tipos de receptor de SQL, puede establecer:

* Truncar tabla
* Volver a crear tabla (quitar o crear)
* Tamaño de lote para grandes cargas de datos. Escriba un número para depositar las escrituras en fragmentos.

![Asignación de campos](media/data-flow/sql001.png "Opciones de SQL")

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha creado el flujo de datos, agregue una [actividad de ejecución de flujo de datos a la canalización](https://docs.microsoft.com/azure/data-factory/concepts-data-flow-overview).
