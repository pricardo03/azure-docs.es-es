---
title: Transformación Selección de flujo de datos de asignación
description: Transformación Selección de flujo de datos de asignación de Azure Data Factory
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 03/08/2020
ms.openlocfilehash: 2d04de420f743e4fef4cff4bd2912559dae0886a
ms.sourcegitcommit: e6bce4b30486cb19a6b415e8b8442dd688ad4f92
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/09/2020
ms.locfileid: "78934184"
---
# <a name="mapping-data-flow-select-transformation"></a>Transformación Selección de Mapping Data Flow


Use esta transformación para la selección de columnas (reducir el número de columnas), para crear alias de nombres de columnas y secuencias y para reordenar las columnas.

## <a name="how-to-use-select-transformation"></a>Uso de la transformación Selección
La transformación Selección permite crear un alias de una secuencia completa, o de las columnas de esa secuencia, asignarles nombres diferentes (alias) y, después, hacer referencia a esos nuevos nombres en el flujo de datos. Esta transformación resulta útil en escenarios de autocombinación. Para implementar una autocombinación en ADF Data Flow, se toma una secuencia, se crea una la rama con "Nueva rama" e, inmediatamente después, se agrega una transformación "Selección". Ahora, la secuencia tiene un nuevo nombre que puede usar para combinarla con la secuencia original y crear una autocombinación:

![Autocombinación](media/data-flow/selfjoin.png "Autocombinación")

En el diagrama anterior, la transformación Selección se encuentra en la parte superior. Lo que está haciendo es asignar a la secuencia original el alias "OrigSourceBatting". En la transformación Combinación resaltada a continuación, puede ver que usamos esta secuencia de alias de selección como combinación de la derecha, lo que nos permite hacer referencia a la misma clave tanto en el lado derecho como el izquierdo de la combinación interna.

También puede usar Selección como una manera de anular la selección de columnas del flujo de datos. Por ejemplo, si tiene 6 columnas definidas en el receptor, pero solo desea elegir tres específicas para transformarlas y enviarlas al receptor, puede seleccionar solo esas tres con la transformación Selección.

![Transformación Selección](media/data-flow/newselect1.png "Seleccionar alias")

## <a name="options"></a>Opciones
* El valor predeterminado de "Selección" es incluir todas las columnas de entrada y mantener los nombres originales. Para crear un alias de la secuencia, puede establecer el nombre de la transformación Selección.
* Para crear alias de columnas individuales, desactive la opción "Seleccionar todo" y use la asignación de columnas en la parte inferior.
* Elija Skip Duplicates (Omitir duplicados) para eliminar las columnas duplicadas de los metadatos de entrada o salida.

![Omitir duplicados](media/data-flow/select-skip-dup.png "Omitir duplicados")

* Si elige omitir duplicados, los resultados estarán visibles en la pestaña Inspect (Inspeccionar). ADF mantendrá la primera repetición de la columna y verá que se han quitado de su flujo todas las repeticiones posteriores de la misma columna.

> [!NOTE]
> Para borrar las reglas de asignación, pulse el botón **Restablecer**.

## <a name="mapping"></a>Asignación
De forma predeterminada, la transformación Selección asignará automáticamente todas las columnas, lo que pasará a través de todas las columnas entrantes con el mismo nombre de la salida. El nombre del flujo de salida que se establece en Configuración de Selección definirá un nuevo nombre de alias para el flujo. Si mantiene Selección establecido en mapa automático, puede asignar un alias a todo el flujo con todas las columnas iguales.

![Reglas de la transformación Selección](media/data-flow/rule2.png "Asignación basada en reglas")

Si desea asignar un alias, quitar, cambiar el nombre o reordenar las columnas, antes debe desactivar "mapa automático". De forma predeterminada, verá una regla predeterminada que se ha especificado como "Todas las columnas de entrada". Puede dejar esta regla en vigor si tiene intención de permitir siempre que todas las columnas entrantes se asignen al mismo nombre en la salida.

Sin embargo, si desea agregar reglas personalizadas, tendrá que hacer clic en "Agregar asignación". La asignación de campos le proporcionará una lista de los nombres de columna de entrada y de salida para asignar y crear un alias. Elija "asignación basada en reglas" para crear reglas de coincidencia de patrones.

## <a name="rule-based-mapping"></a>Asignación basada en reglas
Al elegir la asignación basada en reglas, se indica a ADF que evalúe la expresión coincidente para que coincida con las reglas de patrón de entrada y que defina los nombres de los campos salientes. Puede agregar cualquier combinación de asignaciones basadas en campos y en reglas. A continuación, el ADF genera los nombres de campo en el entorno en tiempo de ejecución en función de los metadatos entrantes del origen. Puede ver los nombres de los campos generados durante la depuración y mediante el panel de vista previa de los datos.

En la [documentación acerca de los patrones de columnas](concepts-data-flow-column-pattern.md) hay más detalles acerca de la coincidencia de patrones.

### <a name="use-rule-based-mapping-to-parameterize-the-select-transformation"></a>Uso de la asignación basada en reglas para parametrizar la transformación Seleccionar
Puede parametrizar la asignación de campos en la transformación Seleccionar mediante la asignación basada en reglas. Use la palabra clave ```name``` para comprobar los nombres de columna de entrada con un parámetro. Por ejemplo, si tiene un parámetro de flujo de datos denominado ```mycolumn```, puede crear una única regla de transformación de selección que siempre asigne el nombre de columna que establezca en ```mycolumn``` a un nombre de campo de esta manera:

```name == $mycolumn```

## <a name="next-steps"></a>Pasos siguientes
* Después de usar Selección para cambiar el nombre de las columnas, reordenarlas y crear alias, use la [transformación del receptor](data-flow-sink.md) para enviar los datos a un almacén de datos.
