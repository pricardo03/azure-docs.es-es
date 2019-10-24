---
title: Configuración de una transformación de receptor en la característica de flujo de datos de asignación de Azure Data Factory
description: Aprenda a configurar una transformación de receptor en el flujo de datos de asignación.
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/03/2019
ms.openlocfilehash: 5fc9262dd53f390dbc43646626cc324d8655f1de
ms.sourcegitcommit: bb65043d5e49b8af94bba0e96c36796987f5a2be
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2019
ms.locfileid: "72387772"
---
# <a name="sink-transformation-for-a-data-flow"></a>Transformación del receptor para un flujo de datos



Una vez que haya transformado un flujo de datos, puede recibir los datos en un conjunto de datos de destino. En la transformación del receptor, elija una definición de conjunto de datos para los datos de salida de destino. Puede tener tantas transformaciones del receptor como precise el flujo de datos.

Para justificar los cambios en los datos de entrada y el desfase de esquema, reciba los datos de salida en una carpeta sin un esquema definido en el conjunto de datos de salida. También puede seleccionar **Allow schema drift** (Permitir el desfase de esquema) en el origen para justificar los cambios de columna en los orígenes. Después, asigne automáticamente todos los campos en el receptor.

![Opciones de la pestaña Sink (Receptor), incluida la opción Auto Map (Asignación automática)](media/data-flow/sink1.png "receptor 1")

Para recibir todos los campos de entrada, active **Asignación automática**. Para elegir los campos que se van a recibir en el destino, o para cambiar los nombres de los campos en el destino, desactive la opción **Asignación automática**. Después, abra la pestaña **Asignación** para asignar los campos de salida.

![Opciones de la pestaña Mapping (Asignación)](media/data-flow/sink2.png "receptor 2")

## <a name="output"></a>Output 
En los tipos de receptor de Azure Blob Storage o Data Lake Storage, genere la salida de los datos transformados en una carpeta. Spark genera los archivos de datos de salida con particiones según el esquema de partición que se usa en la transformación del receptor. 

Puede establecer el esquema de partición en la pestaña **Optimizar**. Si quiere que Data Factory combine la salida en un solo archivo, seleccione **Partición única**.

![Opciones de la pestaña Optimize (Optimizar)](media/data-flow/opt001.png "opciones de receptor")

## <a name="field-mapping"></a>Asignación de campos
En la pestaña **Asignación** de la transformación del receptor, puede asignar las columnas de entrada de la izquierda a los destinos de la derecha. Cuando recibe los flujos de datos en los archivos, Data Factory siempre escribe nuevos archivos en una carpeta. Cuando se asigna un conjunto de datos de base de datos, se eligen las opciones de operaciones de la tabla de la base de datos para insertar, actualizar, upsert o eliminar.

![La pestaña Mapping (Asignación)](media/data-flow/sink2.png "Receptores")

En la tabla de asignación, puede usar la selección múltiple para vincular varias columnas, desvincular varias columnas o asignar varias filas al mismo nombre de columna.

Para asignar siempre el conjunto de campos entrante a un destino tal cual y aceptar completamente las definiciones de esquema flexibles, seleccione **Allow schema drift** (Permitir el desfase de esquema).

![Pestaña Mapping (Asignación), en la que se muestran campos asignados a columnas del conjunto de datos](media/data-flow/multi1.png "varias opciones")

Para restablecer las asignaciones de columna, seleccione **Re-map** (Reasignar).

![Pestaña Sink (Receptor)](media/data-flow/sink1.png "Receptor uno")

Seleccione **Validar esquema** para que se genere un error en el receptor si el esquema cambia.

Seleccione **Clear the folder** (Borrar la carpeta) para truncar el contenido de la carpeta del receptor antes de escribir los archivos de destino en esa carpeta de destino.

## <a name="rule-based-mapping"></a>Asignación basada en reglas
Al desactivar la asignación automática, tendrá la opción de agregar una asignación basada en columnas (asignación fija) o basada en reglas. La asignación basada en reglas le permitirá escribir expresiones con coincidencia de patrones. 

![Asignación basada en reglas](media/data-flow/rules4.png "Asignación basada en reglas")

Al elegir la asignación basada en reglas, se indica a ADF que evalúe la expresión coincidente para que coincida con las reglas de patrón de entrada y que defina los nombres de los campos salientes. Puede agregar cualquier combinación de asignaciones basadas en campos y en reglas. A continuación, el ADF genera los nombres de campo en el entorno en tiempo de ejecución en función de los metadatos entrantes del origen. Puede ver los nombres de los campos generados durante la depuración y mediante el panel de vista previa de los datos.

Los detalles sobre la coincidencia de patrones se encuentran en la [documentación del patrón de columnas](concepts-data-flow-column-pattern.md).

## <a name="file-name-options"></a>Opciones de nombre de archivo

Configure la nomenclatura de los archivos: 

   * **Valor predeterminado**: permita que Spark nombre los archivos según los valores predeterminados de PART.
   * **Patrón**: escriba un patrón para los archivos de salida. Por ejemplo, **loans[n]** creará loans1.csv, loans2.csv, etc.
   * **Por partición**: escriba un nombre de archivo por partición.
   * **Como datos de columna**: establezca el archivo de salida en el valor de una columna.
   * **Salida en un solo archivo**: con esta opción, ADF combinará los archivos de salida con particiones en un solo archivo con nombre. Para usar esta opción, el conjunto de datos debe resolverse en un nombre de carpeta. Además, tenga en cuenta que esta operación de combinación probablemente produzca un error en función del tamaño del nodo.

> [!NOTE]
> Las operaciones de archivo solo comienzan cuando se ejecuta la actividad de ejecución de Data Flow. No se inician en modo de depuración de Data Flow.

## <a name="database-options"></a>Opciones de base de datos

Seleccione los valores de configuración de la base de datos:

![Pestaña Settings (Configuración), en la que se muestran las opciones del receptor de SQL](media/data-flow/alter-row2.png "Opciones SQL")

* **Método de actualización**: el valor predeterminado es permitir las inserciones. Borre **Allow insert** (Permitir insertar) si quiere dejar de insertar nuevas filas del origen. Para realizar operaciones de actualización, upsert o eliminación de filas, primero agregue una transformación de alteración de fila a fin de etiquetar las filas para esas acciones. 
* **Volver a crear tabla**: coloque o cree la tabla de destino antes de que el flujo de datos finalice.
* **Truncar tabla**: quite todas las filas de la tabla de destino antes de que el flujo de datos finalice.
* **Tamaño del lote**: Escriba un número para depositar las escrituras en fragmentos. Use esta opción para grandes cargas de datos. 
* **Permitir almacenamiento provisional**: use PolyBase al cargar Azure Data Warehouse como conjunto de datos receptor.
* **Scripts SQL anteriores y posteriores**: escriba scripts de SQL de varias líneas que se ejecutarán antes (preprocesamiento) y después (procesamiento posterior) de que los datos se escriban en la base de datos del receptor.

![Scripts previos y posteriores al procesamiento de SQL](media/data-flow/prepost1.png "Scripts de procesamiento SQL")

> [!NOTE]
> En Data Flow, puede indicarle a Data Factory que cree una definición de tabla en la base de datos de destino. Para crear la definición de tabla, establezca un conjunto de datos en la transformación del receptor que tenga un nuevo nombre de tabla. En el conjunto de datos de SQL, debajo del nombre de la tabla, seleccione **Editar** y escriba un nuevo nombre. Después, en la transformación del receptor, active **Allow schema drift** (Permitir el desfase de esquema). Establezca **Importar esquema** en **Ninguno**.

![Configuración del conjunto de datos SQL, en la que se muestra dónde se debe editar el nombre de la tabla](media/data-flow/dataset2.png "Esquema SQL")

> [!NOTE]
> Al actualizar o eliminar filas en el receptor de base de datos, debe establecer la columna de clave. Esta opción permite que la transformación de alteración de fila determine la fila única en la biblioteca de movimiento de datos (DML).

## <a name="next-steps"></a>Pasos siguientes
Ahora que ha creado el flujo de datos, agregue una [actividad de Data Flow a la canalización](concepts-data-flow-overview.md).
