---
title: 'Transformación de origen en Mapping Data Flow: Azure Data Factory | Microsoft Docs'
description: Obtenga información sobre cómo configurar una transformación de origen en la asignación de Data Flow.
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 09/06/2019
ms.openlocfilehash: c3c24e9dc674ac29c8ca4d0d445cc3f572cda71e
ms.sourcegitcommit: 11265f4ff9f8e727a0cbf2af20a8057f5923ccda
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/08/2019
ms.locfileid: "72029210"
---
# <a name="source-transformation-for-mapping-data-flow"></a>Transformación de origen de asignación de Data Flow 



Una transformación de origen configura el origen de datos para el flujo de datos. Al diseñar flujos de datos, el primer paso será siempre configurar una transformación de origen. Para agregar un origen, haga clic en el cuadro **Agregar origen** en el lienzo de Data Flow.

Cada flujo de datos requiere al menos una transformación de origen, pero puede agregar tantos orígenes como sea necesario para completar las transformaciones de datos. Puede combinar esos orígenes con una transformación de combinación, búsqueda o unión.

Cada transformación de origen se asocia exactamente con un conjunto de datos de Data Factory. El conjunto de datos define la forma y la ubicación de los datos que quiere escribir o leer. Si va a utilizar un conjunto de datos basado en archivos, puede usar caracteres comodín y listas de archivos en el origen para trabajar con más de un archivo a la vez.

## <a name="supported-connectors-in-mapping-data-flow"></a>Conectores compatibles en Mapping Data Flow

Mapping Data Flow sigue un enfoque de extracción, carga y transformación (ELT) y funciona con conjuntos de datos de un *almacenamiento provisional* que están todos en Azure. Actualmente, se pueden usar los siguientes conjuntos de datos en una transformación de origen:
    
* Azure Blob Storage
* Azure Data Lake Storage Gen1
* Azure Data Lake Storage Gen2
* Azure SQL Data Warehouse
* Azure SQL Database

Azure Data Factory tiene acceso a más de 80 conectores nativos. Para incluir datos de esos otros orígenes en el flujo de datos, use la herramienta de actividad de copia para cargar esos datos en una de las áreas de almacenamiento provisional compatibles.

## <a name="source-settings"></a>Configuración de origen

Una vez que haya agregado un origen, configúrelo mediante la pestaña **Configuración de origen**. Aquí puede elegir o crear el conjunto de datos al que apunta el origen. También puede seleccionar las opciones de muestreo y esquema para los datos.

![Pestaña Configuración de origen](media/data-flow/source1.png "Source settings tab")

**Desfase de esquema:** El [desfase de esquema](concepts-data-flow-schema-drift.md) es la capacidad de Data Factory de administrar de forma nativa los esquemas flexibles en los flujos de datos sin necesidad de definir explícitamente los cambios en las columnas.

* Si las columnas de origen van a cambiar con frecuencia, seleccione la casilla **Allow schema drift** (Permitir el desfase de esquema). Esta opción permite que todos los campos de origen entrantes fluyan hasta el receptor a través de las transformaciones.

* Al elegir **Infer drifted column types** (Inferir tipos de columnas desfasadas), se indica a Data Factory que detecte y defina los tipos de datos para cada nueva columna detectada. Con esta característica desactivada, todas las columnas desfasadas serán del tipo cadena.

**Validate schema:** (Validar esquema) Si Validar esquema está seleccionado, el flujo de datos no se podrá ejecutar si los datos de origen entrantes no coinciden con el esquema definido del conjunto de datos.

**Skip line count:** (Número de líneas para omitir) Este campo especifica el número de líneas que se van a omitir al principio del conjunto de datos.

**Muestreo:** Habilite el muestreo para limitar el número de filas del origen. Use esta configuración al probar o muestrear datos del origen con fines de depuración.

Para validar si el origen está configurado correctamente, active el modo de depuración y capture una vista previa de los datos. Para más información, consulte [Modo de depuración](concepts-data-flow-debug-mode.md).

> [!NOTE]
> Cuando se activa el modo de depuración, el valor del límite de filas de la configuración de depuración sobrescribirá el valor de muestreo en el origen durante la vista previa de los datos.

## <a name="file-based-source-options"></a>Opciones de origen basado en archivos

Si usa un conjunto de datos basado en archivos como, por ejemplo, Azure Blob Storage o Azure Data Lake Storage, la pestaña **Source options** (Opciones de origen) le permite administrar cómo lee los archivos el origen.

![Source options (Opciones de origen)](media/data-flow/sourceOPtions1.png "Source options")

**Ruta de acceso de comodín:** El uso de un patrón de caracteres comodín indicará a ADF que recorra todos los archivos y carpetas que coincidan en una única transformación del origen. Se trata de una manera eficaz de procesar varios archivos en un único flujo. Agregue varios patrones de coincidencia de caracteres comodín con el signo + que aparece al desplazar el puntero sobre el patrón de caracteres comodín existente.

En el contenedor de origen, elija una serie de archivos que coincidan con un patrón. Solo se puede especificar un contenedor en el conjunto de datos. La ruta de acceso con carácter comodín, por tanto, también debe incluir la ruta de acceso de la carpeta de la carpeta raíz.

Ejemplos de caracteres comodín:

* ```*``` Representa cualquier conjunto de caracteres.
* ```**``` Representa el anidamiento recursivo de directorios.
* ```?``` Reemplaza un carácter.
* ```[]``` Coincide con uno de los caracteres entre corchetes.

* ```/data/sales/**/*.csv``` Obtiene todos los archivos csv que se encuentra en /data/sales.
* ```/data/sales/20??/**``` Obtiene todos los archivos del siglo XX.
* ```/data/sales/2004/*/12/[XY]1?.csv``` Obtiene todos los archivos csv de diciembre de 2004 que comienzan con X o Y precedido por un número de dos dígitos.

**Partition Root Path:** (Ruta de acceso de la raíz de la partición) Si tiene carpetas con particiones en el origen de archivo con un formato ```key=value``` (por ejemplo, year=2019), puede asignar el nivel superior del árbol de carpetas de la partición a un nombre de columna del flujo de datos.

En primer lugar, establezca un comodín que incluya todas las rutas de acceso que sean carpetas con particiones y, además, los archivos de hoja que desee leer.

![Configuración del archivo de origen de la partición](media/data-flow/partfile2.png "Configuración del archivo de la partición")

Use el valor de Partition Root Path (Ruta de acceso de la raíz de la partición) para definir cuál es el nivel superior de la estructura de carpetas. Cuando vea el contenido de los datos mediante una vista previa, verá que ADF agregará las particiones resueltas que se encuentran en cada uno de los niveles de las carpetas.

![Ruta de acceso de la raíz de la partición](media/data-flow/partfile1.png "Vista previa de Ruta de acceso de la raíz de la partición")

**Lista de archivos**: Se trata de un conjunto de archivos. Cree un archivo de texto que incluya una lista de archivos de ruta de acceso relativa para procesar. Apunte a este archivo de texto.

**Column to store file name:** (Columna para almacenar el nombre de archivo) Almacene el nombre del archivo de origen en una columna de los datos. Escriba aquí el nombre de una nueva columna para almacenar la cadena de nombre de archivo.

**After completion:** (Tras finalizar) Elija no hacer nada con el archivo de origen después de que se ejecute el flujo de datos o bien elimine o mueva el archivo de origen. Las rutas de acceso para mover los archivos de origen son relativas.

Para mover archivos de origen a otra ubicación posterior al procesamiento, primero seleccione "Mover" para la operación de archivo. A continuación, establezca el directorio "from". Si no usa ningún carácter comodín para la ruta de acceso, la configuración de "from" será la misma carpeta que la carpeta de origen.

Si tiene una ruta de acceso de origen con un comodín, su sintaxis será como esta:

```/data/sales/20??/**/*.csv```

puede especificar "from" como

```/data/sales```

y "to" como

```/backup/priorSales```

En este caso, todos los subdirectorios cuyo origen se encuentra en /data/sales se mueven a /backup/priorSales.

> [!NOTE]
> Las operaciones de archivo solo se ejecutan cuando el flujo de datos se inicia desde una ejecución de canalización (depuración o ejecución de canalización) que usa la actividad de ejecución de Data Flow de una canalización. Las operaciones de archivo *no* se ejecutan en modo de depuración de Data Flow.

**Filter by last modified:** (Filtrar últimos modificados) Puede filtrar los archivos que desea procesar especificando un intervalo de fechas de la última vez que se modificaron. Todas las horas están en formato UTC. 

### <a name="add-dynamic-content"></a>Incorporación de contenido dinámico

Todas las configuraciones de origen se pueden especificar como expresiones mediante el [lenguaje de expresiones de transformación de Mapping Data Flow](data-flow-expression-functions.md). Para agregar contenido dinámico, haga clic o desplace el puntero en los campos del panel de configuración. Haga clic en el hipervínculo para **agregar contenido dinámico**. Con esto se iniciará el generador de expresiones, donde puede establecer los valores de forma dinámica mediante expresiones, valores literales estáticos o parámetros.

![Parámetros](media/data-flow/params6.png "Parámetros")

## <a name="sql-source-options"></a>Opciones de origen de SQL

Si el origen está en SQL Database o SQL Data Warehouse, hay configuración adicional específica de SQL disponible en la pestaña **Source Options** (Opciones de origen). 

**Entrada:** Seleccione si desea señalar el origen en una tabla (equivale a ```Select * from <table-name>```) o escribir una consulta SQL personalizada.

**Consultar** Si selecciona Consulta en el campo de entrada, escriba una consulta SQL para el origen. Esta configuración invalidará cualquier tabla que haya elegido en el conjunto de datos. Las cláusulas **Ordenar por** no se admiten aquí, pero puede establecer una instrucción SELECT FROM completa. También puede usar las funciones de tabla definidas por el usuario. **select * from udfGetData()** es un UDF in SQL que devuelve una tabla. Esta consulta genera una tabla de origen que puede usar en el flujo de datos.

**Tamaño del lote**: escriba un tamaño de lote para fragmentar datos grandes en lecturas.

**Nivel de aislamiento**: El valor predeterminado para los orígenes de SQL en Mapping Data Flow es Lectura no confirmada. Puede cambiar el nivel de aislamiento aquí a uno de estos valores:
* Read Committed
* Read Uncommitted
* Repeatable Read
* Serializable
* Ninguno (ignorar el nivel de aislamiento)

![Nivel de aislamiento](media/data-flow/isolationlevel.png "Nivel de aislamiento")

## <a name="projection"></a>Proyección

Al igual que los esquemas en los conjuntos de datos, la proyección de un origen define las columnas, los tipos y los formatos de datos de los datos de origen. Para la mayoría de los tipos de conjuntos de datos, como SQL y Parquet, la proyección en un origen se corrige para que refleje el esquema definido en un conjunto de datos. Cuando los archivos de origen no están fuertemente tipados (por ejemplo, archivos sin formato en lugar de archivos Parquet), puede definir los tipos de datos de cada campo en la transformación de origen.

![Configuración de la pestaña Proyección](media/data-flow/source3.png "Proyección")

Si el archivo de texto no tiene ningún esquema definido, seleccione **Detectar tipo de datos** para que Data Factory muestree e infiera los tipos de datos. Seleccione **Definir formato predeterminado** para detectar automáticamente los formatos de datos predeterminados. 

Puede modificar los tipos de datos de columna en una transformación de columna derivada de un nivel inferior. Use una transformación de selección para modificar los nombres de columna.

## <a name="optimize-the-source-transformation"></a>Optimización de la transformación de origen

En la pestaña **Optimizar** de la transformación de origen, es posible que vea un tipo de partición de **origen**. Esta opción solo está disponible cuando el origen es Azure SQL Database. Esto se debe a que Data Factory intenta establecer conexiones en paralelo para ejecutar consultas grandes en el origen de SQL Database.

![Configuración de la partición de origen](media/data-flow/sourcepart3.png "creación de particiones")

No es necesario que cree particiones de los datos en el origen de SQL Database, pero las particiones son útiles para consultas grandes. Puede basar la partición en una columna o una consulta.

### <a name="use-a-column-to-partition-data"></a>Uso de una columna para crear particiones de datos

En la tabla de origen, seleccione una columna en la que crear una partición. También debe establecer el número de particiones.

### <a name="use-a-query-to-partition-data"></a>Uso de una consulta para crear particiones de datos

Puede elegir crear una partición de las conexiones según una consulta. Escriba el contenido de un predicado WHERE. Por ejemplo, escriba año > 1980.

Para más información sobre la optimización en Mapping Data Flow, consulte la [pestaña Optimización](concepts-data-flow-overview.md#optimize).

## <a name="next-steps"></a>Pasos siguientes

Comience a compilar una [transformación de columna derivada](data-flow-derived-column.md) y una [transformación de selección](data-flow-select.md).
