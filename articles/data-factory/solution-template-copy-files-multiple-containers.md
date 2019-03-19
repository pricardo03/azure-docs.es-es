---
title: Copiar archivos de varios contenedores con Azure Data Factory | Microsoft Docs
description: Obtenga información sobre cómo usar una plantilla de solución para copiar archivos de varios contenedores con Azure Data Factory.
services: data-factory
documentationcenter: ''
author: dearandyxu
ms.author: yexu
ms.reviewer: douglasl
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 11/1/2018
ms.openlocfilehash: a52729adf8d6df3f4e44e561b45b854db433628c
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/07/2019
ms.locfileid: "57543430"
---
# <a name="copy-files-from-multiple-containers-with-azure-data-factory"></a>Copia de archivos de varios contenedores con Azure Data Factory

En este artículo se describe una plantilla de solución que puede usar para copiar archivos de varios contenedores entre los archivos de almacenamiento. Por ejemplo, puede usar para migrar su lago de datos de AWS S3 a Azure Data Lake Store. O bien, podría usar la plantilla para replicar desde una cuenta de almacenamiento de blobs de Azure a otra.

> [!NOTE]
> Si desea copiar archivos desde un único contenedor, es más eficaz utilizar la [la herramienta Copy Data](copy-data-tool.md) para crear una canalización con actividad de copia única. La plantilla en este artículo es más que necesita para ese escenario simple.

## <a name="about-this-solution-template"></a>Acerca de esta plantilla de solución

Esta plantilla enumera los contenedores desde el almacén de almacenamiento de origen. A continuación, se copia esos contenedores en el almacén de destino.

La plantilla contiene tres actividades:
- **GetMetadata** examina el almacén de almacenamiento de origen y obtiene la lista de contenedores.
- **ForEach** Obtiene la lista de contenedores desde el **GetMetadata** actividad y, a continuación, recorre en iteración la lista y pasa cada contenedor a la actividad de copia.
- **Copia** copia cada contenedor desde el almacén de almacenamiento de origen en el almacén de destino.

La plantilla define dos parámetros:
- *SourceFilePath* es la ruta de acceso de la tienda de origen de datos, donde puede obtener una lista de los contenedores. En la mayoría de los casos, la ruta de acceso es el directorio raíz, que contiene varias carpetas de contenedor. El valor predeterminado para este parámetro es `/`.
- *DestinationFilePath* es la ruta de acceso donde se copiarán los archivos en el almacén de destino. El valor predeterminado para este parámetro es `/`.

## <a name="how-to-use-this-solution-template"></a>Uso de esta plantilla de solución

1. Vaya a la **copiar varios contenedores de archivos entre los archivos de almacenamiento** plantilla. Crear un **New** conexión con el almacén de almacenamiento de origen. El almacén de almacenamiento de origen es donde van a copiar los archivos de varios contenedores de.

    ![Creación de una nueva conexión con el origen](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image1.png)

2. Crear un **New** conexión a su almacén de almacenamiento de destino.

    ![Creación de una nueva conexión con el destino](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image2.png)

3. Seleccione **usar esta plantilla**.

    ![Uso de esta plantilla](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image3.png)
    
4. Verá la canalización, como en el ejemplo siguiente:

    ![Visualización de la canalización](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image4.png)

5. Seleccione **depurar**, escriba el **parámetros**y, a continuación, seleccione **finalizar**.

    ![Ejecución de la canalización](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image5.png)

6. Revise el resultado.

    ![Revisión del resultado](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image6.png)

## <a name="next-steps"></a>Pasos siguientes

- [Copia masiva de una base de datos mediante el uso de una tabla de control con Azure Data Factory](solution-template-bulk-copy-with-control-table.md)

- [Copiar archivos de varios contenedores con Azure Data Factory](solution-template-copy-files-multiple-containers.md)