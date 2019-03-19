---
title: Transformación del origen del flujo de datos de asignación de Azure Data Factory
description: Transformación del origen del flujo de datos de asignación de Azure Data Factory
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/12/2019
ms.openlocfilehash: 20491981cb02e428ff4114b9456d74b0de651be8
ms.sourcegitcommit: dd1a9f38c69954f15ff5c166e456fda37ae1cdf2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/07/2019
ms.locfileid: "57569032"
---
# <a name="mapping-data-flow-source-transformation"></a>Transformación del origen del flujo de datos de asignación

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

La transformación del origen configura un origen de datos que se va a usar para introducir datos en el flujo de datos. Puede tener más de una transformación de origen en un único flujo de datos. Siempre comenzar a diseñar flujos de datos con una transformación de origen.

> [!NOTE]
> Cada flujo de datos requiere al menos una transformación del origen. Agregue tantos orígenes adicionales como sea necesario para completar las transformaciones de datos. Puede combinar esos orígenes con una transformación de combinación o unión. Al depurar el flujo de datos en las sesiones de depuración, se leerán los datos del origen mediante la configuración de muestreo o los límites de código fuente de depuración. Sin embargo, se escribirá ningún dato a un receptor hasta que se ejecuta el flujo de datos de una actividad de flujo de datos de canalización. 

![Opciones de transformación del origen](media/data-flow/source.png "origen")

Cada transformación del origen de flujo de datos debe asociarse con exactamente un conjunto de datos de Data Factory. El conjunto de datos define la forma y la ubicación de los datos para escribir o leer desde. Puede usar caracteres comodín y el archivo de listas en el origen para que funcione con más de un archivo a la vez al usar orígenes de archivo.

## <a name="data-flow-staging-areas"></a>Áreas de almacenamiento provisional del flujo de datos

El flujo de datos funciona con conjuntos de datos de "almacenamiento provisional" que están todos en Azure. Estos conjuntos de datos de flujo de datos se usan en el almacenamiento provisional de datos para realizar las transformaciones de datos. Data Factory tiene acceso a casi 80 conectores nativos diferentes. Para incluir datos de esos otros orígenes en el flujo de datos, almacene primero esos datos en una de esas áreas de almacenamiento provisional del conjunto de datos del flujo de datos mediante la actividad de copia.

## <a name="options"></a>Opciones

### <a name="allow-schema-drift"></a>Permitir el desfase de esquema
Si las columnas de origen van a cambiar con frecuencia, seleccione la opción para permitir el desfase de esquema. Esta opción permitirá que todos los campos entrantes del origen fluyan hasta el receptor a través de las transformaciones.

### <a name="validate-schema"></a>Validar esquema

![Origen público](media/data-flow/source1.png "origen público 1")

Si la versión de entrada del origen de datos no coincide con el esquema definido, la ejecución del flujo de datos generará un error.

### <a name="sampling"></a>muestreo
Use el muestreo para limitar el número de filas del origen.  Esto es útil para probar o muestreo de datos desde el origen con fines de depuración.

## <a name="define-schema"></a>Definición del esquema

![Transformación del origen](media/data-flow/source2.png "origen 2")

Para los tipos de archivo de origen que no están fuertemente tipados (es decir, archivos sin formato frente a archivos Parquet), debe definir los tipos de datos para cada campo aquí en la transformación del origen. Posteriormente, puede cambiar los nombres de columna en una transformación de selección y los tipos de datos en una transformación de columna derivada. 

![Transformación de origen](media/data-flow/source003.png "tipos de datos")

Para los orígenes fuertemente tipada, puede modificar los tipos de datos en una transformación seleccione posteriores. 

### <a name="optimize"></a>Optimizar

![Particiones de origen](media/data-flow/sourcepart.png "creación de particiones")

En la pestaña "Optimize" (Optimizar) de la transformación del origen, verá un tipo de partición adicional llamado "Source" (Origen). Esta solo se ilumina cuando ha seleccionado como origen una base de datos SQL de Azure. Esto se debe a que ADF querrá paralelizar las conexiones para ejecutar consultas grandes en el origen de la base de datos SQL de Azure.

La creación de particiones de los datos en el origen de la base de datos SQL es opcional, pero es útil en el caso de consultas de gran tamaño. Tiene dos opciones:

### <a name="column"></a>Columna

En la tabla de origen, seleccione una columna por la que crear una partición. También debe establecer el número máximo de conexiones.

### <a name="query-condition"></a>Condición de consulta

Opcionalmente, puede elegir crear una partición de las conexiones según una consulta. Con esta opción, simplemente coloque el contenido de un predicado WHERE. Por ejemplo, año > 1980

## <a name="source-file-management"></a>Administración de archivos de origen
![Nueva configuración de origen](media/data-flow/source2.png "Nueva configuración")

* Ruta de acceso de comodín para seleccionar una serie de archivos de la carpeta de origen que coincidan con un patrón. Esto invalidará cualquier archivo que haya establecido en la definición del conjunto de datos.
* Lista de archivos Igual que un conjunto de archivos. Elija un archivo de texto que se cree con una lista de archivos de ruta de acceso relativa para procesar.
* La columna para almacenar el nombre de archivo almacenará el nombre del archivo del origen en una columna de los datos. Escriba aquí un nombre nuevo para almacenar la cadena de nombre de archivo.
* Tras finalizar, puede elegir no hacer nada con el archivo de origen después de que se ejecuta el flujo de datos o bien eliminar o mover los archivos de origen. Las rutas de acceso para mover los archivos de origen son rutas de acceso relativas.

### <a name="sql-datasets"></a>Conjuntos de datos de SQL

Cuando se use Azure SQL DB o Azure SQL DW como origen, dispondrá de opciones adicionales.

* Consulta: escriba una consulta SQL para el origen. Al definir una consulta se invalidará cualquier tabla que haya elegido en el conjunto de datos. Tenga en cuenta que aquí no se admiten cláusulas Order By.

* Tamaño de lote: escriba un tamaño de lote para fragmentar datos grandes en lecturas con tamaño de lote.

> [!NOTE]
> La configuración de la operación de archivo solo se ejecutará cuando el flujo de datos se ejecute desde una ejecución de canalización (depuración o ejecución de canalización) mediante la actividad de ejecución de flujo de datos de una canalización. Las operaciones de archivo NO se ejecutan en modo de depuración de flujo de datos.

### <a name="projection"></a>Proyección

![Proyección](media/data-flow/source3.png "Projection")

Al igual que los esquemas en los conjuntos de datos, la proyección de origen define las columnas de datos, los tipos de datos y los formatos de datos de los datos de origen. Si tiene un archivo de texto sin un esquema definido, haga clic en la opción "Detect Data Type" (Detectar el tipo de dato) para pedir a ADF que intente muestrear e inferir los tipos de datos. Puede establecer los formatos de datos predeterminados para la detección automática con el botón "Define Default Format"(Definir formato predeterminado). Puede modificar los tipos de datos de columna en una posterior transformación de columna derivada. Los nombres de columna se pueden modificar mediante la transformación de selección.

![Formatos predeterminados](media/data-flow/source2.png "Default formats")

## <a name="next-steps"></a>Pasos siguientes

Comience a crear la transformación de datos con [Columna derivada](data-flow-derived-column.md) y [Selección](data-flow-select.md).
