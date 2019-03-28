---
title: Conjuntos de datos de Azure Data Factory Mapping Data Flow
description: Azure Data Factory de asignación de flujo de datos tiene compatibilidad de conjunto de datos específico
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/14/2019
ms.openlocfilehash: efb82c57a5620ef3eace8b39f6f27f2286202f84
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2019
ms.locfileid: "58521846"
---
# <a name="mapping-data-flow-datasets"></a>Conjuntos de datos de Mapping Data Flow

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Los conjuntos de datos son una construcción de Data Factory que definen la forma de los datos con los que trabaja en la canalización. En Data Flow, para los datos de nivel de fila y columna, es necesario definir los conjuntos de datos con gran precisión. Los conjuntos de datos usados en canalizaciones de flujo de control no requieren conocer los datos en la misma profundidad.

Conjuntos de datos de flujo de datos se utilizan en transformaciones de origen y receptor. Se utilizan para definir los esquemas de datos básicos. Si no tiene ningún esquema en los datos, puede establecer el desfase de esquema para el origen y el receptor. Si se define el esquema desde el conjunto de datos, tendrá los tipos de datos relacionados, los formatos de datos, la ubicación del archivo y la información de conexión del servicio vinculado asociado. Los metadatos de los conjuntos de datos aparecerá en la transformación de origen como el origen "Proyección". El esquema del conjunto de datos representa el tipo de datos físico y la forma mientras la proyección en la transformación de origen representa la representación del flujo de datos de los datos con los nombres definidos y tipos.

## <a name="dataset-types"></a>Tipos de conjuntos de datos

Actualmente en Data Flow encontrará cuatro tipos de conjunto de datos:

* Azure SQL DB
* Azure SQL DW
* Parquet (de ADLS y Blob)
* Texto delimitado (de ADLS y Blob)

Conjuntos de datos del flujo de datos independientes el *tipo de origen* desde el *tipo de conexión de servicio vinculado*. Por lo general, en Data Factory se elige el tipo de conexión (Blob, ADLS, etc.) y, a continuación, se define el tipo de archivo en el conjunto de datos. Dentro de Data Flow, seleccionará los tipos de origen, que se pueden asociar a distintos tipos de conexión del servicio vinculado.

![Opciones de transformación del origen](media/data-flow/dataset1.png "orígenes")

## <a name="data-flow-compatible-datasets"></a>Conjuntos de datos compatibles con Data Flow

Al crear un nuevo conjunto de datos, hay una casilla llamada "Data Flow Compatible" (Compatible con Data Flow) en la esquina superior derecha del panel. Al hacer clic en ese botón, se filtrarán solo los conjuntos de datos que se pueden utilizar con Data Flow. 

## <a name="import-schemas"></a>Importación de esquemas

Al importar el esquema de conjuntos de datos de Data Flow, verá el botón Importar esquema. Al hacer clic en ese botón, verá dos opciones: importar desde el origen o importar desde un archivo local. En la mayoría de los casos, importará el esquema directamente desde el origen. Sin embargo, si tiene un archivo de esquema existente (archivo Parquet o CSV con encabezados), puede señalar a ese archivo local y Data Factory definirá el esquema en función de ese archivo de esquema.

## <a name="create-new-table"></a>Creación de una nueva tabla

En el flujo de datos, puede pedir ADF para crear una nueva definición de tabla en la base de datos de destino mediante el establecimiento de un conjunto de datos en la transformación de receptor que tiene un nuevo nombre de tabla. En el conjunto de datos SQL, haga clic en "Editar" debajo del nombre de tabla y escriba un nuevo nombre de tabla. A continuación, en la transformación de receptor, activar "Permitir la desviación en el esquema". Seth la configuración de "Importación de esquema" en ninguno.

![Esquema de la transformación de origen](media/data-flow/dataset2.png "esquema SQL")

## <a name="choose-your-type-of-data-first"></a>Elija el tipo de datos en primer lugar

### <a name="delimited-text"></a>Texto delimitado

En el conjunto de datos de texto delimitado, establecerá el delimitador para controlar cualquier únicos delimitadores ('\t 'para TSV,',' para CSV, ' |'...) o usar varios caracteres de delimitador. Establezca el botón de alternancia de fila de encabezado y, a continuación, pasa a la transformación de origen para detectar automáticamente los tipos de datos. Si usa un conjunto de datos de texto delimitado por llevar datos en un receptor, seleccione una carpeta de destino. En la configuración del receptor, puede definir el nombre de los archivos de salida.

### <a name="parquet"></a>Parquet

Use Parquet como el tipo de conjunto de datos de almacenamiento provisional preferido en flujos de datos ADF. Parquet almacena metadatos enriquecidos esquema junto con los datos.

### <a name="database-types"></a>Tipos de base de datos

Puede seleccionar Azure SQL DB o Azure SQL DW.

Para los demás tipos de conjunto de datos ADF, utilice la actividad de copia para organizar los datos. Hay una plantilla de ADF en la Galería de plantillas para ayudarle a crear este patrón.

![Copiar ensayo](media/data-flow/templatedf.png "copiar ensayo")

## <a name="choose-your-connection-type"></a>Elija el tipo de conexión

Si usas Parquet o texto delimitado por conjuntos de datos, a continuación, puede seleccionar la ubicación de los datos: ADLS o Blob.

## <a name="next-steps"></a>Pasos siguientes

Empiece por [crear un nuevo flujo de datos](data-flow-create.md) y agregue una transformación de origen. A continuación, configure el conjunto de datos para el origen.

Use la [actividad de copia](copy-activity-overview.md) para recuperar datos de cualquier ADF origen de datos y colocarlos en etapa en ADLS o Blob para acceso de flujo de datos.

