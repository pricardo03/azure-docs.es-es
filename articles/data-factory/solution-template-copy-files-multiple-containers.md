---
title: Copia de archivos de varios contenedores con Azure Data Factory | Microsoft Docs
description: Aprenda a usar una plantilla de solución para copiar archivos de varios contenedores con Azure Data Factory.
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
ms.openlocfilehash: aa5f32594c295ab6a8e60af8359370f64f75a72d
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/08/2019
ms.locfileid: "55966816"
---
# <a name="copy-files-from-multiple-containers-with-azure-data-factory"></a>Copia de archivos de varios contenedores con Azure Data Factory

La plantilla de solución que se describe en este artículo le ayuda a copiar archivos desde varios archivos, contenedores o cubos de unos almacenamientos de archivos a otros. Por ejemplo, puede querer migrar su instancia de Data Lake de AWS S3 a Azure Data Lake Store. O tal vez desee replicar todo el contenido de una cuenta de Azure Blob Storage a otra cuenta de Azure Blob Storage. Esta plantilla está diseñada para estos casos de uso.

Si desea copiar archivos desde un único contenedor o cubo, es más eficaz utilizar la **herramienta Copy Data** para crear una canalización con una sola actividad Copy. Esta plantilla es más de lo que necesita para este sencillo caso de uso.

## <a name="about-this-solution-template"></a>Acerca de esta plantilla de solución

Esta plantilla enumera los contenedores desde el almacén de almacenamiento de origen y luego copia cada uno de los contenedores del almacenamiento de origen al almacén de destino. 

La plantilla contiene tres actividades:
-   Una actividad **GetMetadata** para examinar el almacenamiento de origen y obtener la lista de contenedores.
-   Una actividad **ForEach** para obtener la lista de contenedores desde la actividad **GetMetadata** y, a continuación, recorrer en iteración la lista y pasar cada contenedor a la actividad Copy.
-   Una actividad **Copy** para copiar cada contenedor desde el almacén de almacenamiento de origen al almacén de destino.

La plantilla define dos parámetros:
-   El parámetro *SourceFilePath* es la ruta de acceso del almacén de origen de datos, donde puede obtener una lista de los contenedores o cubos. En la mayoría de los casos, la ruta de acceso es el directorio raíz, que contiene varias carpetas de contenedor. El valor predeterminado para este parámetro es `/`.
-   El parámetro *DestinationFilePath* es la ruta de acceso donde se copiarán los archivos en el almacén de destino. El valor predeterminado para este parámetro es `/`.

## <a name="how-to-use-this-solution-template"></a>Uso de esta plantilla de solución

1. Vaya a la plantilla **Copy multiple files containers between File Stores**(Copiar varios contenedores de archivos entre almacenes de archivos) y cree una **nueva conexión** a su almacén de almacenamiento de origen. El almacén de almacenamiento de origen es el lugar donde se van a copiar los archivos de varios contenedores o cubos.

    ![Creación de una nueva conexión con el origen](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image1.png)

2. Cree una **nueva conexión** con el almacén de almacenamiento de destino.

    ![Creación de una nueva conexión con el destino](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image2.png)

3. Haga clic en **Use this template** (Usar esta plantilla).

    ![Uso de esta plantilla](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image3.png)
    
4. Verá la canalización disponible en el panel, como se muestra en el ejemplo siguiente:

    ![Visualización de la canalización](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image4.png)

5. Haga clic en **Depurar**, escriba los **Parámetros** y haga clic en **Finalizar**.

    ![Ejecución de la canalización](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image5.png)

6. Revise el resultado.

    ![Revisión del resultado](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image6.png)

## <a name="next-steps"></a>Pasos siguientes

- [Introducción al servicio Azure Data Factory](introduction.md)
