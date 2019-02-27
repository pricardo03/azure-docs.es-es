---
title: Conjuntos de datos de Azure Data Factory Mapping Data Flow
description: Azure Data Factory Mapping Data Flow tiene compatibilidad con conjunto de datos específicos
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/14/2019
ms.openlocfilehash: 36ca5e07adf79de77ac4ab4149ff8e96a1dece8d
ms.sourcegitcommit: 4bf542eeb2dcdf60dcdccb331e0a336a39ce7ab3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2019
ms.locfileid: "56408755"
---
# <a name="mapping-data-flow-datasets"></a>Conjuntos de datos de Mapping Data Flow

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Los conjuntos de datos son una construcción de Data Factory que definen la forma de los datos con los que trabaja en la canalización. En Data Flow, para los datos de nivel de fila y columna, es necesario definir los conjuntos de datos con gran precisión. Los conjuntos de datos usados en canalizaciones de flujo de control no requieren conocer los datos en la misma profundidad.

Los conjuntos de datos de las transformaciones del origen y del receptor de Data Flow se usan para definir el esquema de datos básico. Si no tiene ningún esquema en los datos, puede establecer el desfase de esquema para el origen y el receptor. Si se define el esquema desde el conjunto de datos, tendrá los tipos de datos relacionados, los formatos de datos, la ubicación del archivo y la información de conexión del servicio vinculado asociado.

## <a name="dataset-types"></a>Tipos de conjuntos de datos

Actualmente en Data Flow encontrará cuatro tipos de conjunto de datos:

* Azure SQL DB
* Azure SQL DW
* Parquet
* Texto delimitado

Los conjuntos de datos de Data Flow separan el *tipo* de origen del tipo de conexión del servicio vinculado. Por lo general, en Data Factory se elige el tipo de conexión (Blob, ADLS, etc.) y, a continuación, se define el tipo de archivo en el conjunto de datos. Dentro de Data Flow, seleccionará los tipos de origen, que se pueden asociar a distintos tipos de conexión del servicio vinculado.

![Opciones de transformación del origen](media/data-flow/dataset1.png "orígenes")

## <a name="data-flow-compatible-datasets"></a>Conjuntos de datos compatibles con Data Flow

Al crear un nuevo conjunto de datos, hay una casilla llamada "Data Flow Compatible" (Compatible con Data Flow) en la esquina superior derecha del panel. Al hacer clic en ese botón, se filtrarán solo los conjuntos de datos que se pueden utilizar con Data Flow. 

## <a name="import-schemas"></a>Importación de esquemas

Al importar el esquema de conjuntos de datos de Data Flow, verá el botón Importar esquema. Al hacer clic en ese botón, verá dos opciones: importar desde el origen o importar desde un archivo local. En la mayoría de los casos, importará el esquema directamente desde el origen. Sin embargo, si tiene un archivo de esquema existente (archivo Parquet o CSV con encabezados), puede señalar a ese archivo local y Data Factory definirá el esquema en función de ese archivo de esquema.

